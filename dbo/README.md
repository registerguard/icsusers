# System modification notes

The Google Groups thread that kicked off our mapping fix can be viewed here (login required):

[UNS query: Best way to convert for non-UNS (7.7.3) system?](https://groups.google.com/d/msg/dti-lightning/_plqfeGgiOA/y2zO6bMVpnwJ)

Prompted by the above thread, Joy P. contacted us directly with these detailed instructions:

> There are a few things that we need to make this work.

> 1. The `dbo.FileHeader` class needs to be available in the `CMS` namespace.
2. The `dbo.ForeignDbLink` class needs to be available in the `CMS` namespace.
3. The data for `dbo.FileHeader` needs to be available in the `CMS` namespace.
4. The data for `dbo.ForeignDbLink` needs to be available in the `CMS` namespace.

> We need to create the `dbo.FileHeader` class for the `CMS` namespace. I exported the class from `NEWSMEDIA` and then imported it into `CMS`. Then I removed the triggers, and added the `READONLY` parameter to the class definition. I have attached [this class (`dbo.FileHeader.xml`)](./dbo.FileHeader.xml) for your convenience. Do not change the `READONLY parameter and try to write to this table from the `CMS` namespace.

> There is already a version of the `dbo.ForeignDbLink` class in the CMS namespace. This class only defines properties and indexes, and you should only use the `CMS` version of this table as a read-only table. 

> Now that we have classes that support the structure of the data that we need to read, we need to provide the data. We do this by creating global mappings for the `CMS` namespace in the System Management Portal (SMP): <kbd>Home</kbd> `>` <kbd>Configuration</kbd> `>` <kbd>Namespaces</kbd> `>` <kbd>Global Mappings</kbd>.

> There should already be a global mapping for `dbo.FileHeader*` that reads data from the NEWSMEDIADATA database. We need to create a mapping for `dbo.ForeignDbLink`. Here is a screenshot that shows the values you should use:

> [![image001](https://cloud.githubusercontent.com/assets/218624/4333091/7da40130-3fdc-11e4-8c61-69a0e61a0d15.png)](https://cloud.githubusercontent.com/assets/218624/4333091/7da40130-3fdc-11e4-8c61-69a0e61a0d15.png)

> Click <kbd>OK</kbd>, then verify that you have both global mappings:

> [![image002](https://cloud.githubusercontent.com/assets/218624/4333100/976d9900-3fdc-11e4-866c-5da9a336f3d4.png)](https://cloud.githubusercontent.com/assets/218624/4333100/976d9900-3fdc-11e4-866c-5da9a336f3d4.png)

> Then click <kbd>Save Changes</kbd>.

> Don't ask me why we already have the class for `dbo.ForeignDbLink` but not the mapping for its data, and we have the mapping for `dbo.FileHeader` but not the class. I don't know who did this.

> You will only need to import the `dbo.FileHeader` class into the `CMS` namespace on your `NEWS` instance, but you will need to create the global mapping on all servers, including ECP servers.  If you upgrade to a later `7.x` release at some point, you will need to re-import the `dbo.FileHeader` class after the upgrade.  The mappings will be preserved through an upgrade to a newer `7.x` release.

> You may want to re-use this technique to make other data easily available in `CMS`, but be careful with that. I’m not saying that you can’t do this to make any other data accessible from `CMS`, but it would probably be good to have me quickly review the change to make sure it will not cause problems with future upgrades.

> **TIP 1:**  When writing sql queries, always prefix the table name with the schema (e.g. `dbo.Story` instead of `Story`). This will make it easier when you eventually upgrade to UNS because you just need to search on “`dbo.`” to find a bunch of sql statements that you need to modify to be work with `UNS`.

> **TIP 2:** When writing a query that uses mapped data, tag the table references with comments to make it easy to find that code and update it correctly for `UNS`:

> ```sql
SELECT cmspic.ID, cmspic.fileHeaderId
FROM %IGNOREINDICES "i_foreignid" dt_cms_schema.CMSPictureMapping cmspm INNER JOIN dt_cms_schema.CMSPicture cmspic ON cmspic.ID = cmspm.CMSPicture
INNER JOIN dbo.FileHeader fh ON fh.fileHeaderId = cmspic.fileHeaderId                                                                                                              // @@UNS:  change dbo.FileHeader to dt_newsmedia.FileHeader          
INNER JOIN dbo.ForeignDbLink fdbl ON fdbl.fileHeaderId = cmspic.fileHeaderId AND fdbl.foreignidField = 'story.storyId'                // @@UNS: change dbo.ForeignDbLink to dt_newsmedia.ForeignDbLink
AND fdbl.foreignid = ?
INNER JOIN dt_cms_schema.CMSPictureVersion cmspv ON cmspv.ID = cmspic.TheCMSPictureVersion WHERE cmspm.position = ?
AND fh.priorityId != ?
ORDER BY fdbl.viewOrder

> Now when you need to update this code to work with `UNS`, you will find this query by searching on “`dbo.`” and modify the code by the following rules:

> 1. If no namespace change occurred before the code is executed (i.e. the code executes against the `CMS` namespace), replace “`dbo.`” with “`dt_news`”.
2. If you have an upgrade tag (`@@UNS: …`), modify the schema reference according to the comment.
3. If there was a namespace change, then replace “`dbo.`” with one of “`dt_ad.`”, “`dt_admedia.`”, “`dt_locations.`”, “`dt_newsmedia.`” depending on which namespace the code is executing against.

> P.S.: This is a small glimpse of how much easier it is to write queries in UNS when you don't have to switch namespace to get different pieces of data that you need.
