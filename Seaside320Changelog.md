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
  * WAMutex owner is now a transient value

# Breaking Changes #
  * the type of an input element is no longer in its class
    * you have to change the CSS selector of the input type eg `.checkbox` to `[type='checkbox']`
  * inline JavaScript in XHTML in XML mode will no longer work
  * we no longer ship JavaScript libraries for generating and parsing JSON
  * server adapter authors should send `#rfc6265String` instead of #`oldNetscapeString`, #`rfc2109String`, #`rfc2965String`, #`rfc6265String`
  * Cookie2 support is gone (only Opera ever supported it but doesn't anymore)
  * `WADivTag` has been removed, `WAGenericTag` is used instead, if you have class extensions there you need to move them to `WAGenericTag`
  * parsing an URL with a non-numeric port (eg. `http://www.seaside.st:8x/`) will now signal a `WAInvalidUrlSyntaxError`
  * the following deprecated methods have been removed
    * WARequestCookie class >> #fromString:
    * WAUrl >> #pathString
    * WAPopupAnchorTag >> #name
    * WAPopupAnchorTag >> #name:
  * The metacello configuration `ConfigurationOfSeaside3`'s default group now loads Core,Email,JSON,Javascript,JQuery and JQueryUI groups while it previously only loaded the Core group. This enhances discoverability for newcomers and existing users can still configure the load to only load the groups they need.
  * We store class bindings instead of classes in configuration values. Therefore all configurations have to be registered again, see [#820](https://github.com/SeasideSt/Seaside/issues/820).
  * Dialects with namespaces (eg. VM) need to implement `GRPlatform >> #bindingOf:`, an example implementation can be found in the comments.
  * [#262](https://github.com/SeasideSt/Seaside/issues/262): Creating a new session is O(n)
    * `WASession >> #buildCache` has been renamed to `#createDocumentHandlerCache` and returns a new cache instance
    * `WASession >> #createCache` has been renamed to `#createContinuationCache` and returns a new cache instance
    * `WASession >> #cache` has been renamed to `#documentHandlers`
    * `WARegistry >> #keyFor:` has been removed
    * the session cache is unidirectional and no longer bidirectional
    * `Dictionary >> #valuesCollect:` has been removed

# Bugs Fixed #

The following bugs were fixed:
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
