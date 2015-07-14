# Introduction #
Seaside 3.1.1 is a bug fix release for Seaside 3.1.

# Details #

The following changes occurred:
  * Ported to Gemstone 3.1
  * Slime is now available on Squeak, Pharo1.4, Pharo2.0 and Pharo3.0
  * Gettext is now included in the ConfigurationOfSeaside3
  * DateAndTime>>jsonOn: now prints the ISO8601 representation in a string
  * [#783](https://github.com/SeasideSt/Seaside/issues/783)): 	Don’t require nesting #object: in #value: in JSON arrays

# Bugs Fixed #

The following bugs were fixed:
  * [#776](https://github.com/SeasideSt/Seaside/issues/776)): 	Testcase>>#fail is not portable
  * [#779](https://github.com/SeasideSt/Seaside/issues/779)): 	WAVersion uploader broken in 3.1
  * [#780](https://github.com/SeasideSt/Seaside/issues/780)): 	Fixes for the web tools in Pharo3
  * [#781](https://github.com/SeasideSt/Seaside/issues/781)): 	Also catch platform deprecation signals
  * [#784](https://github.com/SeasideSt/Seaside/issues/784)):   Some status code messages are missing from WAResponse class>>initializeStatusMessages
  * [#785](https://github.com/SeasideSt/Seaside/issues/785)): 	Better support recaching in restful filters and handlers
  * [#786](https://github.com/SeasideSt/Seaside/issues/786)): 	Bug in WAAbstractFileLibrary
  * [#787](https://github.com/SeasideSt/Seaside/issues/787)): 	WAUrl relies on not portable Stream semantics
  * [#788](https://github.com/SeasideSt/Seaside/issues/788)): 	Screenshot application does not work on Pharo 2.0+
  * [#789](https://github.com/SeasideSt/Seaside/issues/789)):   Running WAFilelibraryTest on Pharo3 leaves Seaside-Tests-Core package in a dirty state
  * [#792](https://github.com/SeasideSt/Seaside/issues/792)): 	Issue encoding a non-ascii character preceded by an xml-unsafe one
