  * WARequestHandler refactorings
  * Canvas renderer is default, the old renderer is deprecated
  * New, canvas like header api
  * File libraries
  * Some tools have been cleaned up
  * New image map api
  * Many new XHTML elements
  * WAAnchorTag >> #text: has been deprecated (use #with: instead)
  * Removed WATagBrush >> #tooltip: (use #title: instead)
  * A deprecation api to make all these deprecations possible
  * Ability to open a debugger on the rendering stack of an element
  * Pretty printer fixes and improvements (including comments)
  * Cookie fixes
  * Convenience methods in WARequest
  * Many bug fixes
  * Class and method comments
  * And more
  
## Migrate from 2.6 to 2.7

### Must

  * every component that uses the old default `WAHtmlBuilder` must do so explicitly by returning it in `#rendererClass`
  * evaluate `WADispatcher resetAll`
  * use `WATagBrush >> #title:` instead of `WATagBrush >> #tooltip:`
  * move to new image map api, check all senders of `#imageMap`, see `WAImageMapTag` for details

### Should:

  * move to new header api, modify implementors of `#updateRoot:`
  * remove the implemtors of #rendererClass that return `WAHtmlCanvas`
  * migrate to `WAHtmlCanvas`
  * migrate from `WAScriptLibrary` and `WAStyleLibrary` to `WAFileLibrary` (check class comment of `WAFileLibrary`)
  * use `WAAnchorTag >> #with:` instead of `WAAnchorTag >> #text:`
  * check for deprecated message sends
  * use `#heading:level:` instead of `#heading:;level:`
  * replace users of `WAChangePassword`, `WAEditDialog`, `WAEmailConfirmation`, `WAGridDialog`, `WALoginDialog`, `WANoteDialog`
