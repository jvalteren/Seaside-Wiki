# Introduction #
Seaside 3.1 contains the following changes.
  * The way session are tracked has been made customizable. The following strategies are supported out of the box:
    * query fields
    * cookies of the browser supports it, query fields otherwise
    * _new_: cookies only
    * _new_: cookies for browsers and IPs for web crawlers, inspired by [Crawler Session Manager Valve](http://www.tomcatexpert.com/blog/2011/05/18/crawler-session-manager-valve) for Tomcat
    * _new_: ssl session id, needs special server support, right now only works with AJP
  * All the deprecated methods and classes have been removed.
  * Session continuations have been made subclasses of WARequestHandler. Those who have created their own continuation subclasses may need to make adjustments to their classes.
  * New WAPluggableActionContinuation introduced (see [#680](https://github.com/SeasideSt/Seaside/issues/680)) for details). Those subclassing WAActionPhaseContinuation probably want to be subclasses WACallbackProcessingActionContinuation now instead.
  * Removed WAMain and its subclasses. Applications now specify an initial continuation instead. Those subclassing WAMain should consider implementing a subclass of WASessionContinuation instead.
  * `WAPopupAnchorTag>>#name:` has been replaced by `WAPopupAnchorTag>>#windowTitle:`
  * JSON support is now in Seaside-JSON-Core, #jsonOn: methods get a JSON canvas instead of a stream as an argument
  * Support for the HTML 5 "multiple" attribute has been added. Text input and file upload tags now understand the `#multipleValuesCallback:` message. The value passed to the callback will be a sequenceable collection.
  * Document handlers (created by one of the `#document:` methods) are now stored in their respective session instead as a session in the application. This means that the number of "sessions" used when using document handlers goes down and there's less pressure on the session cache. Also the document handlers now time out when their session times out. However the size of the individual session now goes up when using document handlers. The document handlers of a session can no longer be accessed concurrently because they are behind the session lock. In addition it makes the code in `WAApplication` simpler because it only cares about sessions.
  * The `#resetIfPossible` method has been added to `WAResponse`. It clears a partially rendered response if that's possible (not streaming server is used).
  * The optional `#flush` method has been added to `WAResponse`. This allows programmatic flushing of a partially rendered response. Out of the box this is used by comet and a new render phase continuation that flushes once the HTML header has been rendered. Currently only the Comanche adaptor supports this.
  * `WARestfulComponentFilter` allows applications to easily switch from session-less parts to session-based parts.
  * add `#dontDestroy` to `WARequestContext` to prevent object destruction during debugging

# Bugs Fixed #
  * [#111](https://github.com/SeasideSt/Seaside/issues/111): 	jumpTo instance variable on WASession
  * [#244](https://github.com/SeasideSt/Seaside/issues/244)) : 	WABatchedList>>hasMultiplePages
  * [#325](https://github.com/SeasideSt/Seaside/issues/325):	Force the use of cookies
  * [#364](https://github.com/SeasideSt/Seaside/issues/364): 	add always redirect option to WARegistry
  * [#453](https://github.com/SeasideSt/Seaside/issues/453): 	document path consumer
  * [#500](https://github.com/SeasideSt/Seaside/issues/500): 	WADebugErrorHandler>>unableToResumeResponse provides invalid instructions
  * [#591](https://github.com/SeasideSt/Seaside/issues/591):	WAComboResponse - a combined buffered / streaming response
  * [#592](https://github.com/SeasideSt/Seaside/issues/592):	investigate tracking sessions by SSL session id
  * [#626](https://github.com/SeasideSt/Seaside/issues/626): 	Allow platforms to implement custom encoders for speed
  * [#636](https://github.com/SeasideSt/Seaside/issues/636): 	expected exception behavior WAWalkbackErrorHandler not portable (and not ANSI compliant)
  * [#645](https://github.com/SeasideSt/Seaside/issues/645): 	WAPopupAnchorTag overrides #name: of WAAnchorTag with different semantics
  * [#663](https://github.com/SeasideSt/Seaside/issues/663): 	Remove default nil option from WAListAttribute
  * [#676](https://github.com/SeasideSt/Seaside/issues/676): 	response generators have to reset the response before generating a new one
  * [#679](https://github.com/SeasideSt/Seaside/issues/679):   Make SessionContinuations into subclasses of RequestHandler
  * [#680](https://github.com/SeasideSt/Seaside/issues/680):   Add a pluggable action phase continuation
  * [#681](https://github.com/SeasideSt/Seaside/issues/681):   Get rid of WAMain and subclasses
  * [#697](https://github.com/SeasideSt/Seaside/issues/697):	Within a WAComponent I've repeated implemented #children to return a collection that contains a reference to self. Causing infinite recursion
  * [#699](https://github.com/SeasideSt/Seaside/issues/699): 	GRPharoPlatform>>#write:toFile:inFolder: uses CrLfFileStream in seaside 3.0.6.3
  * [#707](https://github.com/SeasideSt/Seaside/issues/707):	don't use #document:mimeType: when mime type is nil
  * [#708](https://github.com/SeasideSt/Seaside/issues/708):	JQDialog>>#buttons:
  * [#709](https://github.com/SeasideSt/Seaside/issues/709):	move document handlers to session
  * [#712](https://github.com/SeasideSt/Seaside/issues/712):	Pretty printing is broken for JavaScript expressions
  * [#716](https://github.com/SeasideSt/Seaside/issues/716):	WARequestHandler>>#preferenceAt: has no corresponding WARequestHandler>>#preferenceAt: ifAbsent:
  * [#717](https://github.com/SeasideSt/Seaside/issues/717): 	WAMemoryItem>>sizeOfObject
  * [#718](https://github.com/SeasideSt/Seaside/issues/718): 	Make profile tree more useful
  * [#722](https://github.com/SeasideSt/Seaside/issues/722): 	add support for the "multiple" attribute
  * [#724](https://github.com/SeasideSt/Seaside/issues/724):	add .pushState to ajaxifier
  * [#726](https://github.com/SeasideSt/Seaside/issues/726): 	Rework JSON and JavaScript escaping
  * [#727](https://github.com/SeasideSt/Seaside/issues/727): 	walkback only works for exceptions in callback phase
  * [#728](https://github.com/SeasideSt/Seaside/issues/728): 	add support for new HTTP status codes
  * [#730](https://github.com/SeasideSt/Seaside/issues/730): 	Float infinity and nan don't produce correct JSON representations
  * [#731](https://github.com/SeasideSt/Seaside/issues/731): 	utf-16 broken
  * [#732](https://github.com/SeasideSt/Seaside/issues/732): 	Add flushing render phase continuation
  * [#733](https://github.com/SeasideSt/Seaside/issues/733): 	multibyte characters broken when flushing a WAComboResponse
  * [#735](https://github.com/SeasideSt/Seaside/issues/735):	Problem with WAFileMetadataLibrary>>#deployFiles
  * [#736](https://github.com/SeasideSt/Seaside/issues/736): 	implement #basicNextPutAll: on pseudo streams
  * [#737](https://github.com/SeasideSt/Seaside/issues/737): 	Update WAScreenshot to work with Pharo 2.0
  * [#738](https://github.com/SeasideSt/Seaside/issues/738): 	Set encoding for Seaside-REST responses
  * [#740](https://github.com/SeasideSt/Seaside/issues/740):	Improper use of #replaceAll:with: (@ WAFileMetadataLibrary)
  * [#741](https://github.com/SeasideSt/Seaside/issues/741): 	#includesSubString: deprecated in Pharo 2.0
  * [#742](https://github.com/SeasideSt/Seaside/issues/742): 	Generated javascripts are not directly serialized on the canvas stream (leading to slow performance?)
  * [#743](https://github.com/SeasideSt/Seaside/issues/743): 	WAVNCController still uses Project which is gone from Pharo-1.4
  * [#747](https://github.com/SeasideSt/Seaside/issues/747): 	Fix HTML 5 support
  * [#749](https://github.com/SeasideSt/Seaside/issues/749): 	Wrong handling of urls encoded in UTF8
  * [#751](https://github.com/SeasideSt/Seaside/issues/751):	WAAccept>>fromString: assumes the only parameter to a media-range is 'q'
  * [#754](https://github.com/SeasideSt/Seaside/issues/754):	#asJson produces strings with non-valid escape sequences "\0", "\a" and "\x.."
  * [#755](https://github.com/SeasideSt/Seaside/issues/755):	JSJsonParser removes whitespace at the start of string literals
  * [#756](https://github.com/SeasideSt/Seaside/issues/756): 	WACanvasBrushTest>>testCanvasWithLineBreaksGemStoneIssue289 uses non-portable selectors
  * [#759](https://github.com/SeasideSt/Seaside/issues/759): 	application/json recognized as a binary mime type
  * [#760](https://github.com/SeasideSt/Seaside/issues/760): 	addAllFilesIn: is broken in Pharo 20
  * [#762](https://github.com/SeasideSt/Seaside/issues/762): 	WAUrl decodePercent: 'abc%' fails
  * [#769](https://github.com/SeasideSt/Seaside/issues/769): 	more Slime rule for collections
  * [#770](https://github.com/SeasideSt/Seaside/issues/770): 	ScaledDecimal rendering support
  * [#772](https://github.com/SeasideSt/Seaside/issues/772): 	WAResponse >>#doNotCache doesn't send full Cache-Control header

# Changes #
## Users ##
  * replaced `WARenderCanvas` with `WAHtmlCanvas` (already available in 3.0)
  * `WASelectionDateTable` no longer adds border
  * `WASelectionDateTable` no longer adds CSS style
  * `#jsonOn:` now has a JsonCanvas as an argument
  * `#jsonOn:` is in Seaside-JSON-Core
  * `JSJsonParser` was renamed to `WAJsonParser`
  * `WACookie >> #oldNetscapeStringWithoutKey` is now in Seaside-Adaptors-Comanche
  * `WAUrl` >> #pathString is deprecated, use either `#pathStringUnencoded` or `#pathStringEncodedWith:`
  * The default doctype is now html5

## Server Adaptors ##
  * if your server supports access to the SSL session id implement `#sslSessionIdFor:`
  * optionally you can implement `#resetIfPossible` and `#flush`
  * instead of using `WARequestCookie class >> #fromString:` you should use `WARequestCookie class >> #fromString:codec:`, this has also been added to the latest versions of Seaside 3.0

## Porters ##
  * `GRPlatform >> #includesUnsafeUrlCharacter:` and `#includesUnsafeXmlCharacter:` are no longer used. They are replaced by `GRPlatform >> #xmlEncoderOn:`, `#urlEncoderOn:`, `#urlEncoderOn:codec:`, and `#jsonEncoderOn:` which allow you to answer custom implementations. You don't have to do this, the old implementations are provided as default implementations.
  * There is a new `#sourceFileEncoding` method on `GRPlatform` that is used by the file library to save the conent of uploaded files into methods. It defaults to ISO-8859-1 which is what was hard coded in Seaside 3.0.

## Dependencies ##
  * `Seaside-JSON-Core` along `Seaside-Pharo-JSON-Core` is now in the Seaside 3.1 repository
  * `#callback:json:` was moved from `JQuery-Core` to `JQuery-JSON` which depends on `Seaside-JSON-Core`
  * `Seaside-HTML5` has been merged into `Seaside-Canvas` and `Seaside-Core`
