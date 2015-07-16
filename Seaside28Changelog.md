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

## Migrate from 2.7 to 2.8

### Must

  * [[Seaside 2.6 to 2.7|Seaside27Changelog.md]]
  * stop your server (e.g. `WAKom`) and evaluate `WADispatcher resetAll`
  * migrate to `WARenderCanvas`
  * backtrack state by implementing `#states` (check all senders of `#registerObjectForBacktracking:` and `#registerForBacktracking`), `WAStateHolders` do not do any backtracking anymore
  * move to new header api, modify `#updateRoot:`. `#linkToStyle` becomes `#stylesheet` `#url`, `#linkToScript` becomes `#javascript` `#url:`, check all implementors of `#updateRoot:`
  * use `WAAnchorTag >> #with:` instead of `WAAnchorTag >> #text:`
  * replace users of `WAChangePassword`, `WAEditDialog`, `WAEmailConfirmation`, `WAGridDialog`, `WALoginDialog`, `WANoteDialog`
  * every component that implements `#initialize` must send this message also to super (use SLint)
  * if you use Squeak 3.7 load SeasideSqueak37 after Seaside
  * if you use Squeak do not load into an image that has an earlier version of Seaside already loaded
  * For `WAImageTag` use the composed `#document:mimeType:fileName:` selector (or a short version of it) instead of a cascade of selector parts
  * if you loaded an intermediate version of Seaside 2.8 Seaside2.8a1-lr.404 removes `WACookieSession`, you should go back to subclassing `WASession`
  * migrate from `WAScriptLibrary` and `WAStyleLibrary` to `WAFileLibrary` (check class comment of `WAFileLibrary`)
  * the "base path" configuration option has been replaced by "Server Path"

### Should

  * use `#heading level:`; with: instead of `#heading:level:`
  * check for deprecated message sends
  * remove the implementors of `#rendererClass` that return `WAHtmlCanvas`
  * use `WAValueHolder` if you just need an object that wraps a value instead of `ValueHolder` or `WAStateHolder`
  * `WAUrl >> #scheme:` expects a `String` and no longer a `Symbol`

### Rewrite Rules

Boris Popov has some rewrite rules to ease migration Moving from [2.7 to 2.8](http://leftshore.wordpress.com/2007/07/06/moving-from-27-to-28/)
