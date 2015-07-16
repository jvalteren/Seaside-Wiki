Seaside 2.8 is released as of 28 October 2007.

## The big picture

  * rendering speed improvements mostly because of a new encoding architecture
  * memory usage reduction due to less continuations and new backtracking system
  * runs on two new platforms:
      * [Gemstone/S](http://seaside.gemtalksystems.com/)
      * [KernelImage](http://www.comtalk.cz/Squeak/98)
  * comments, more and better tests, documentation
  * tool modularization
  * new style and homepage
  * Dropped the old WAHtmlRenderer API
  * Squeak 3.7 support dropped (needs SeasideSqueak37 compatibility package)
  * improved response streaming (WATask and WABasicAthentication work)

## The new backtracking system

Backtracking is no longer done by sending #registerObjectForBacktracking: or #registerForBacktracking but by implementing the #states selector. This allowed us to simplify and improve a lot of code. It’s also very similar to the existing #children. You can have a look the implementors to see examples.

In Detail

### API

  * improved and simplified WAAnchor (uses URL objects now)
  * improved and simplified WAPopupAnchor
  * made WAImageTag protocol compatible to WAAnchorTag
  * added #optionGroup
  * added WAComponent >> #chooseFrom: convenience method (mostly for tutorials)
  * select tag supports disabled elements
  * two new elements (ins and del)
  * WAHtmlBuilder to conveniently render html from outside of #renderContentOn:

### I18N

  * set accept-charset of forms to the character set of the session
  * bug with multipart forms fixed in WAKomEncoded
  * included Andreas’ utf-8 fastpath into WAKomEncoded
  * WAUrl has different encoding as text content or url attribute value
  * fixed WideString bug in WAFile

### Bugs Fixed

  * fixed WABrowser bugs
  * fixed bug in the profiler that showed a non-printable character -> this bug existed since the beginning of time
  * lots of AJAX related fixes

### Clean Ups

  * removed WAModLisp, use fast_cgi instead -> we do not depend on KomServices anymore
  * removed all references to ScriptingSystem, use Form >> #storeString instead
  * removed WAChangePassword, WAEditDialog, WAEditDialog, WAGridDialog, WALoginDialog, WANoteDialog
  * Sushi Store is now its own package at`Seaside-Examples`

### General

  * WARedirectHandler subclasses can be used to handle session expiry
  * the error walkback displays a list of possible causes
  * lots of small fixes especially related to i10n
  * lots of small improvements
  * lots of xhtml validation related fixes
  * clean up, remove lots of old unused stuff
  * better portability