**Seaside 3.3.0 has not yet been released.**

Seaside 3.3 includes [Grease 1.4](https://github.com/SeasideSt/Grease/wiki/Grease-1.4-Changelog)

# New Features #
- Pharo 7 supported
- Seaside-REST now supports the OPTIONS and PATCH http verbs
- The Seaside-REST-Examples package has been added which contains examples on how to use Seaside REST
- Seaside now generates HTML-style boolean attributes instead of XML-style boolean attributes, `checked` instead of `checked="checked"`
- Many more HTML5 events are now supported
- jQuery binding was updated to 3.3.1 (see https://jquery.com/upgrade-guide/ for how to upgrade)
- jQuery-UI binding was updated to 1.12.1 (see https://jqueryui.com/upgrade-guide/1.12/ for how to upgrade)
- HTML document root now supports directly passing a style sheet or Javascript as a String
- `#initializeCache` can be sent to an application after changing the cache configuration for the changes to take effect
- the MIME types for .sass and .scss are now supported
- `WAEnterpriseAuberginesStrategy` has been added which tracks session similar to Tomcat/Java EE/Servlet with a "JSESSIONID" cookie and a "jsessionid" path parameter so that existing sticky session load balancers can be used
- a mapping for Prototype's `PeriodicalExecuter` has been added
- `#noAutocomplete` is also understood by inputs
- some of the icons of the development tools have been updated to vector graphics so that they should look better on high-DPI screens
- SameSite Strict is used for session cookies

# Issues Resolved #
See the [resolved issues for milestone 3.3](https://github.com/SeasideSt/Seaside/milestone/4?closed=1) for a complete list of issues. Some highlights:

- `WAXmlCanvas builder` now returns a ready to use builder
- `WADynamicVariable` now uses `GRDynamicVariable` which uses the native `DynamicVariable` if supported on the platform
- Render phase continuation and `JSObject` now uses the script generator configured on the application
- `WAErrorHandler` now sets the charSet attribute of the content type
- The `REST` group in the Metacello baseline no longer contains the `Seaside-Component` package
- Expired sessions tracked with cookies should no result in an infinite redirect
- The bundled Prototype JavaScript library has been updated from 1.7.0 to 1.7.3 which fixes multi select lists

# Breaking Changes #

- Removed support for Pharo 1,2,3 and GemStone 2.4 (these were probably broken in 3.2 already)
- When a session has expired no longer is `#expiredRegistryKey` being sent to the response generator. Instead `#handleDefault:` is not sent to the application. If you require the old behavior you should subclass `WAApplication` and override `#handleExpired` with the method below. See https://github.com/SeasideSt/Seaside/issues/916.
```smalltalk
handleExpired: aRequestContext
	
	aRequestContext responseGenerator
		expiredRegistryKey;
		respond
```
- `#initializeMemorySettingsProfileSeaside` has been removed
- jQuery UI `#zIndex:` has been removed use the appendTo option instead https://jqueryui.com/upgrade-guide/1.12/#removed-zindex
- jQuery binding was updated to 3.3.1 (see https://jquery.com/upgrade-guide/ for how to upgrade)
- jQuery-UI binding was updated to 1.12.1 (see https://jqueryui.com/upgrade-guide/1.12/ for how to upgrade)
- the "XHTML" validation tool int the development toolbar has been removed, it is unlikely this ever worked
- SameSite Strict is used for session cookies, that means requests from other pages, including redirects, will no longer get the session cookie. If you rely on the old behavior we implementing you own session tracking strategy.

# Deprecated Features #
- `JSObject >> #timeout:` has been deprecated in favour of `JSObject >> #setTimeout:`. `JQAjaxSetup >> #timeout:` and `JQAjax >> #timeout:` have not been deprecated.
- `WAHtmlRoot` `#beXhtml10Strict`, `#beXhtml10Transitional` and `#beXhtml11` have been deprecated in favor of `#beHtml5`.

# Upgrading #