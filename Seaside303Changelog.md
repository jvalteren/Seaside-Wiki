# Introduction #
Seaside 3.0.3 is closed, but not yet released bug fix release for Seaside 3.0.


# Details #

The following bugs were fixed:
  * [#607](https://github.com/SeasideSt/Seaside/issues/607): 	No adapter can be stared if two stopped adapters are on the same port
  * [#609](https://github.com/SeasideSt/Seaside/issues/609): 	Consider not using #subStrings: in WAUrl>>addToPath:
  * [#611](https://github.com/SeasideSt/Seaside/issues/611):	Auto Reload of a page and memory consumption
  * [#619](https://github.com/SeasideSt/Seaside/issues/619):	error detected on WATemporaryFile
  * [#621](https://github.com/SeasideSt/Seaside/issues/621): 	error uploading file
  * [#618](https://github.com/SeasideSt/Seaside/issues/618): 	Port number in server adaptors

In addition the following improvements were made:
  * Reaping of expired sessions now happens every _n_ (default 10) session creations instead of every _n_ session accesses. This is back to the behavior of Seaside 2.8. If you want the behavior of previous Seaside 3 releases use `WARetrievalIntervalReapingStrategy`. `WARetrievalIntervalReapingStrategy` reaps on access as well as creation (_new_). Already registered applications will not be migrated and continue to use `WARetrievalIntervalReapingStrategy`. To use the new `WAAccessIntervalReapingStrategy` they have to be registered again or the configuration of the cache has to be updated.
  * Upload streaming of the Comanche adaptor can be configured in the control panel
