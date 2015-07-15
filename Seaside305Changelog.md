# Introduction #
Seaside 3.0.5 is bug fix release for Seaside 3.0.


# Details #

The following bugs were fixed:
  * Pharo 1.2 support
  * Seaside applications no longer handle prefetch requests from Firefox
  * [#596](https://github.com/SeasideSt/Seaside/issues/596): uptime reporting on /status may be wrong (thanks Sven)
  * [#648](https://github.com/SeasideSt/Seaside/issues/648): WARequest >> #rawBody is empty
  * [#587](https://github.com/SeasideSt/Seaside/issues/587): clean up WAComancheRequestConverter >> #requestBodyFor:
  * [#651](https://github.com/SeasideSt/Seaside/issues/651): JQPost appends arguments onto url
  * jQuery 1.6.1
  * update to jQuery UI 1.8.13

# Upgrading #
## Pharo Users ##
If you're on Pharo 1.1 or 1.0 and don't use Metacello you additionally need to load Grease-Pharo11-Core.

## jQuery Users ##
Check you the [jQuery 1.6.1 changelog](http://blog.jquery.com/2011/05/12/jquery-1-6-1-released/).
