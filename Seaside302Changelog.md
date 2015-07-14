# Introduction #
Seaside 3.0.2 is a bug fix release for Seaside 3.0. There never was an official 3.0.1 release.


# Details #

The following bugs were fixed:
  * [#606](https://github.com/SeasideSt/Seaside/issues/606)) WAKomEncoded fails serving CSS files from WAExternalFileLibrary
  * [#41](https://github.com/SeasideSt/Seaside/issues/41)) Provide simple email abstraction
  * [#602](https://github.com/SeasideSt/Seaside/issues/602)) Seaside's scriptaculous in place editor is bugged
  * [#600](https://github.com/SeasideSt/Seaside/issues/600)) BlockClosure>>processHttp sends deprecated #fixTemps
  * [#610](https://github.com/SeasideSt/Seaside/issues/610)) Welcome should display a warning if author initials are not set
  * [#612](https://github.com/SeasideSt/Seaside/issues/612)) subscript out of bounds when encoding a single 0 character to UTF-8

In addition the following improvements were made:
  * added the possibility for new items to CTReport
  * GRCodecStream >> #isStream added
  * removed #hash, #= and #isDictionary from GRSmallDictionary
  * added a rule detecting the use of "self class hash", which does not work in GemStone
  * fixed findTokens: rule
  * updated to jQuery 1.4.4
  * updated to jQuery UI 1.8.6
  * do not include fragment in AJAX request URLs
  * fixes and improvements to the jQuery UI auto complete dialog
  * made script generator on WABuilder configurable
  * fix MNU in uploads due to missing content type in Chrome
  * WAPathConsumer >> #peekToEnd added
  * added WAUrl >> #withoutFragment
  * made the halo class configurable, like in morphic
  * added an experimental space tally to /status
  * The tool tips in the /config application update themselves depending on the state
  * fixed the WideString method JQDevelopmentLibrary>>jQueryJs
