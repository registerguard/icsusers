# “Popular” utility page

This folder contains example code used to generate a “most popular” utility page, powered by [NEWSCYCLE Solutions’](http://www.newscyclesolutions.com/) `##class(dt.cms.trace.Popular).getPopular()` (a.k.a. `<dti:popular></dti:popular>`) code.

## Demo

<p><a href="http://registerguard.com/csp/cms/sites/rg/pages/popular/stories.csp"><img width="100%" src="https://cloud.githubusercontent.com/assets/218624/4343663/7b03f890-405e-11e4-92f5-9bccfd3b9db8.gif" alt="Animated gif demonstration"></a></p>

View the [live page](http://registerguard.com/csp/cms/sites/rg/pages/popular/stories.csp).

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
