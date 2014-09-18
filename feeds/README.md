# “Popular” JSON/P feed

This folder contains example code used to generate a [JSON/P](http://json-jsonp-tutorial.craic.com/index.html) feed, powered by [NEWSCYCLE Solutions’](http://www.newscyclesolutions.com/) `##class(dt.cms.trace.Popular).getPopular()` (a.k.a. `<dti:popular></dti:popular>`) code.

## JSON structure

The `popular.csp` CSP page generates JSON with this basic structure:

```json
{
	"stories": [
		{
			"byline": "Billy Johnson",
			"category": "Crime",
			"count": 1,
			"deck": "Lorem ipsum dolor sit amet",
			"headline": "The quick brown fox jumps over the lazy dog",
			"path": "\/foo\/permalink.html",
			"published": "2014-09-16 15:48:01",
			"section": "local",
			"section_link": "\/foo\/",
			"server": "http://site.com"
		}
	]
}
```

Check out a [live example](http://registerguard.com/csp/cms/sites/rg/feeds/popular.csp).

## Options

The code can be manipulated via these URL [query string](http://en.wikipedia.org/wiki/Query_string) ([`$get()`](http://docs.intersystems.com/cache20091/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fget)) parameters:

Parameter | Description | Example
--- | --- | ---
`callback` | JSONP callback name. | `http://site.com/popular.csp?callback=foo`
`page` | Page number for pagination. | `http://site.com/popular.csp?page=3`
`per` | Items per page. | `http://site.com/popular.csp?per=15`
`publication` | Site publication (default is `rg`). | `http://site.com/popular.csp?publication=rg`
`section` | Site section (default is entire publication). | `http://site.com/popular.csp?section=sports`

For details, please read through the [source code](./popular.csp).

## Dependencies

A list of custom code dependencies follow.

### [`##class(custom.rg.Pick).pics()`](https://github.com/registerguard/pick):

> Get pictures attached to a story and order them by vieworder (+ a whole lot more!).

> For use with DTI's ContentPublisher which is powered by Intersystem's Caché.

If you can’t install the above code, you could remove it from [`popular.csp`](./popular.csp) and/or replace it with your own version.

### Custom:

You’ll most likely have to replace this code with your own custom version:

Method | Description
--- | ---
`getimagesize()` | Width and height of image stream.
`meta()` | Extracts caption from image stream.
`cmsPubTracking()` | Earliest possible “published to web” date.
`byline()` | Author byline.
`catName()` | Sub/category name from `subCategoryId`.
`subHeadline()` | Story deck.
`headline()` | Story headline.
`uri()` |  Story URL.

**Note:** All code listed above is available upon request: <micky@registerguard.com>

## Notes

* The code defaults to JSON; JSONP is an option if you utilize the `callback` query string parameter.
* The maximum number of results is hard-coded to `100`; you can change this number by editing the `page("max")` delcaration/initialization.
* If/when the requested page is out of range, the code will return a response status of `404 Not Found` (needed if using [Infinite Scroll](https://github.com/paulirish/infinite-scroll)).
* Response is cached for 10 minutes and varies by `page` and `per` query string parameters.
* The `getPopular()` method will grab results from entire publication; use `section` query string parameter to target a specific section.
* The default `publication` is `rg`; you can change this value by editing the `page("publication")`  delcaration/initialization.
* Code tested on 7.7.3 Lighting, Cache for Windows (x86-64) 2009.1.5 (Build 901_0_11112U) Thu Dec 8 2011 18:33:08 GMTST.
