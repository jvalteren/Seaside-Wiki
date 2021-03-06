# Features #
  * new session cache with better performance and scalability
    * Supports both idle and absolute timeouts as recommended by [OWASP Session Management](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet#Automatic_Session_Expiration)
    * access by key is fast (O(1) average case O(n) worst case)
    * reaping expired sessions is proportional (O(n)) to the number of expired session and independent of the total number of sessions (O(1))
    * creating a new session independent of the total number of sessions
    * custom implementation for GemStone/S based on `RcKeyValueDictionary`
  * WAUrl parsing and serialization is context dependent
  * In Pharo the web based class browser will use Nautilus if available (Pharo 2+).
  * Ajax file uploads (jQuery serialization callback)

## GemStone/S related improvements
  * dedicated cache implementation
  * configurations hold on to bindings instead of classes
  * no longer uses a fork of Seaside-Core, Seaside-Session and Seaside-Environment

# Issues Resolved #

  * [#615](https://github.com/SeasideSt/Seaside/issues/615): Depricate WARenderContext >> #callbackAt:
  * [#706](https://github.com/SeasideSt/Seaside/issues/706): Remove WADivTag
  * [#763](https://github.com/SeasideSt/Seaside/issues/763): Add support for srcset on img and source tag
  * [#777](https://github.com/SeasideSt/Seaside/issues/777): Make TestCase >> #fail SLime rule
  * [#778](https://github.com/SeasideSt/Seaside/issues/778): remove JSON libraries
  * [#790](https://github.com/SeasideSt/Seaside/issues/790): WAUrl parsing and serialization needs to be context dependent
  * [#791](https://github.com/SeasideSt/Seaside/issues/791): Replace Set-Cookie2 with RFC 6265
  * [#796](https://github.com/SeasideSt/Seaside/issues/796): Add support for Forwarded header
  * [#807](https://github.com/SeasideSt/Seaside/issues/807): MNU RBArgumentNode>>key with parameterized Seaside 3.1 REST Method
  * [#809](https://github.com/SeasideSt/Seaside/issues/809): Use HTML 5 meta tag for charset
  * [#810](https://github.com/SeasideSt/Seaside/issues/810): Provide a dedicated security package
  * [#815](https://github.com/SeasideSt/Seaside/issues/815): fix WABrowser in Pharo 4
  * [#816](https://github.com/SeasideSt/Seaside/issues/816): WAHtmlAttributes >> #addClass: allocates too much
  * [#819](https://github.com/SeasideSt/Seaside/issues/819): JQAjax does not have a json: callback
  * [#812](https://github.com/SeasideSt/Seaside/issues/812): Input elements should not generate a class by default
  * [#820](https://github.com/SeasideSt/Seaside/issues/820): Configurations should not hold on to classes
  * [#822](https://github.com/SeasideSt/Seaside/issues/822): Drop clever CDATA trick
  * [#824](https://github.com/SeasideSt/Seaside/issues/824):   GRPackage>>#resolveWith: contains non-portable usage of String>>indexOfSubCollection:startingAt:
  * [#825](https://github.com/SeasideSt/Seaside/issues/825): Remove Methods deprecated in 3.1
  * [#827](https://github.com/SeasideSt/Seaside/issues/827): GRPlatform >> #deprecationExceptionSet should use ExceptionSet
  * [#828](https://github.com/SeasideSt/Seaside/issues/828): replace TimeStamp with DateAndTime
  * [#830](https://github.com/SeasideSt/Seaside/issues/830):    	(ENH) Javascript Condition with else decoration (JSConditionElse)
  * [#833](https://github.com/SeasideSt/Seaside/issues/833): WARequest>>destroy doesn't set the body' to nil
  * [#837](https://github.com/SeasideSt/Seaside/issues/837): No able to view / compile Pharo 3 / Pharo 4 code in Seaside
  * [#838](https://github.com/SeasideSt/Seaside/issues/838): Usage of class-side #initialize is not portable
  * [#842](https://github.com/SeasideSt/Seaside/issues/842): add the ability to send an arbitrary string back along with the passenger id
  * [#843](https://github.com/SeasideSt/Seaside/issues/843): add #basicStop to WATestServerAdapter
  * [#846](https://github.com/SeasideSt/Seaside/issues/846): Allow the usage of IE9 for HTML5Shiv support
  * [#851](https://github.com/SeasideSt/Seaside/issues/851): WASession >> #unregister does signal an error
  * [#863](https://github.com/SeasideSt/Seaside/issues/863): Scriptaculous scripts are still strings when the should be script objects

# Breaking Changes #
  * the type of an input element is no longer in its class
    * you have to change the CSS selector of the input type eg `.checkbox` to `[type='checkbox']`
  * inline JavaScript in XHTML in XML mode will no longer work
  * we no longer ship JavaScript libraries for generating and parsing JSON
  * server adapter authors should send `#rfc6265String` instead of #`oldNetscapeString`, #`rfc2109String` or #`rfc2965String`, there is a Slime transformation rule available
  * Cookie2 support is gone (only Opera ever supported it but doesn't anymore)
  * `WADivTag` has been removed, `WAGenericTag` is used instead, if you have class extensions there you need to move them to `WAGenericTag`
  * parsing an URL with a non-numeric port (eg. `http://www.seaside.st:8x/`) will now signal a `WAInvalidUrlSyntaxError`
  * the following deprecated methods have been removed
    * `WARequestCookie class >> #fromString:`
    * `WAUrl >> #pathString`
    * `WAPopupAnchorTag >> #name`
    * `WAPopupAnchorTag >> #name:`
  * The metacello configuration `ConfigurationOfSeaside3`'s default group now loads Core,Email,JSON,Javascript,JQuery and JQueryUI groups while it previously only loaded the Core group. This enhances discoverability for newcomers and existing users can still configure the load to only load the groups they need.
  * We store class bindings instead of classes in configuration values. Therefore all configurations have to be registered again, see [#820](https://github.com/SeasideSt/Seaside/issues/820).
  * the default server manager needs to be reset with: `WAServerManager instVarNamed: 'default' put: nil`. You need to stop the servers first and register them afterwards again. You need to do this only if you upgrade an existing image.
  * Dialects with namespaces (eg. VM) need to implement `GRPlatform >> #bindingOf:`, an example implementation can be found in the comments.
  * [#262](https://github.com/SeasideSt/Seaside/issues/262): Creating a new session is O(n)
    * `WASession >> #buildCache` has been renamed to `#createDocumentHandlerCache` and returns a new cache instance
    * `WASession >> #createCache` has been renamed to `#createContinuationCache` and returns a new cache instance
    * `WASession >> #cache` has been renamed to `#documentHandlers`
    * `WARegistry >> #keyFor:` has been removed
    * the session cache is unidirectional and no longer bidirectional
    * `Dictionary >> #valuesCollect:` has been removed
  *  In Seaside 3.0, the argument of `addLoadScript:`,`addLoadScriptFirst:` may be a string. As from 3.1 it should always be a `JSObject` (sub)instance. This was not clearly documented when 3.1 was released, therefore we repeat this here.

# Upgrading #
We recommend loading the code into a new image. If that's not possible for you try the following procedure:
```smalltalk
WAServerManager default stopAll.
WAServerManager instVarNamed: 'default' put: nil`.
"register your sever adapter"
WAEnvironment reloadApplications
"register your applications again, remove what you don't need"
```