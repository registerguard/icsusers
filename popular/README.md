# “Popular” utility page

<p><img width="100%" src="https://cloud.githubusercontent.com/assets/218624/4328853/af971128-3f8c-11e4-881f-c050420bccfa.gif" alt="A happy user"></a></p>

This folder contains example code used to generate a “most popular” utility page, powered by [NEWSCYCLE Solutions’](http://www.newscyclesolutions.com/) `##class(dt.cms.trace.Popular).getPopular()` (a.k.a. `<dti:popular></dti:popular>`) code.

## Demo

Check out a [live example](http://registerguard.com/csp/cms/sites/rg/pages/popular/stories.csp).

## Dependencies …

Some of the below methods are drag/drop while others will require modifications to work on your system.

### … as [Gists](https://gist.github.com/):

Method | Description
--- | ---
[`byline()`](https://gist.github.com/mhulse/c493894e109a210f6ef4) | Author byline.
[`cmsPubTracking()`](https://gist.github.com/mhulse/33bb98b29f08bc1e2a10) | Earliest possible “published to web” date.
[`headline()`](https://gist.github.com/mhulse/59ede19115960ee5a716) | Story headline.
[`label()`](https://gist.github.com/mhulse/382b0f46c3ad5f8a8357) | Get human-readable “label” for a section.
[`runDate()`](https://gist.github.com/mhulse/2bf966495e5da2ab2612) | Get story’s run date.

## Notes

* The `label()` method depends on a “Guides” database with this structure:

 ![gides](https://cloud.githubusercontent.com/assets/218624/4327344/5c6a64a0-3f78-11e4-81c2-6d54d6308dca.png)

## Questions?

<micky@registerguard.com>
