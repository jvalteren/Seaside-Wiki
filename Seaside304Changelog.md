# Introduction #
Seaside 3.0.4 is a bug fix release for Seaside 3.0.


# Details #

The following bugs were fixed:
  * [#9](https://github.com/SeasideSt/Seaside/issues/9):    Add switch to WAFileLibrary such that #/ also generates static paths
  * [#356](https://github.com/SeasideSt/Seaside/issues/356): Add functional test for WAValidationDecoration
  * [#462](https://github.com/SeasideSt/Seaside/issues/462): Have convenience methods for http status codes on WAResponse (thanks Avi Shefi)
  * [#482](https://github.com/SeasideSt/Seaside/issues/482): speedup WAEncoder
  * [#522](https://github.com/SeasideSt/Seaside/issues/522): WAFileLibrary generated methods don't have timestamps
  * [#542](https://github.com/SeasideSt/Seaside/issues/542): implement Strict Transport Security (thanks Avi Shefi)
  * [#608](https://github.com/SeasideSt/Seaside/issues/608): Configuration Lookup is Slow (thanks Avi Shefi)
  * [#620](https://github.com/SeasideSt/Seaside/issues/620): WAFileLibrary>>name answers a Symbol, but used in places where String should be used
  * [#624](https://github.com/SeasideSt/Seaside/issues/624): Changing assigned configuration partens of an application does not correctly update the /config UI (thanks Avi Shefi)
  * [#625](https://github.com/SeasideSt/Seaside/issues/625): DNU RRComponent class >> #canBeRoot
  * [#629](https://github.com/SeasideSt/Seaside/issues/629): Seaside is broken on Swazoo 2.3
  * [#630](https://github.com/SeasideSt/Seaside/issues/630): Add WANoReapingStrategy
  * [#634](https://github.com/SeasideSt/Seaside/issues/634): Add GC settings from Pharo
  * [#632](https://github.com/SeasideSt/Seaside/issues/632): prevent response splitting
  * [#637](https://github.com/SeasideSt/Seaside/issues/637): Add option to make a file library send an X-Sendfile header instead of the the response body. (thanks Avi Shefi)
  * [#639](https://github.com/SeasideSt/Seaside/issues/639): merge WAHtmlCanvas and WARenderCanvas (thanks Avi Shefi)
  * [#641](https://github.com/SeasideSt/Seaside/issues/641): Halo source view incorrectly displaying entity characters
  * [#643](https://github.com/SeasideSt/Seaside/issues/643): load 3.0.4 on top of 3.0.3 results in tests hitting WAAttributeNotFound
  * [#646](https://github.com/SeasideSt/Seaside/issues/646): GRPharoPlatform>>isIpAddress broken
