**Seaside 3.3.0 has not yet been released.**

Seaside 3.3 includes [Grease 1.3](https://github.com/SeasideSt/Grease/wiki/Grease-1.3-Changelog)

# Features #
- Pharo 7 64bit support
- Seaside-REST supports OPTIONS
- Seaside-REST supports PATCH
- a lot more HTML5 events are supported
- HTML root supports directly passing a style sheet or Javascript as a String
- Seaside now generates HTML-stlye boolean attributes instead of XML-style boolean attributes, `checked` instead of `checked="checked"`
- `#initializeCache` can be sent to an application after changing the cache configuration for the changes to take effect
- the MIME types for .sass and .scss are now supported
- the Seaside-REST-Examples package has been added which contains examples on how to use Seaside REST
- `WAEnterpriseAuberginesStrategy` has been added which tracks session similar to Tomcat/Java EE/Servlet with a "JSESSIONID" cookie and a "jsessionid" path parameter so that existing sticky session load balancers can be used
- a mapping for Prototype's `PeriodicalExecuter` has been added
- the bundled Prototype JavaScript library has been updated from 1.7.0 to 1.7.3 which fixes multi select lists
- `#noAutocomplete` is also be understood by inputs
- some of the icons of the development tools have been updated to vector graphics so that they should look better on high-DPI screens

# Issues Resolved #
[Issues resolved](https://github.com/SeasideSt/Seaside/milestone/4?closed=1)

- `WAXmlCanvas builder` now returns a ready to use builder
- render phase continuation and `JSObject` now uses the script generator configured on the application
- `WAErrorHandler` now sets the charSet attribute of the content type
- loading the `REST` Metacello group no longer loads Seaside-Component
- expired sessions tracked with cookies should no result in an infinite redirect

# Breaking Changes #

- Removed GemStone 2.4 support
- When a session has expired no longer is `#expiredRegistryKey` being sent to the response generator. Instead `#handleDefault:` is not sent to the application. If you require the old behavior you should subclass `WAApplication` and override `#handleExpired` with the method below. See https://github.com/SeasideSt/Seaside/issues/916.
```smalltalk
handleExpired: aRequestContext
	
	aRequestContext responseGenerator
		expiredRegistryKey;
		respond
```
- `#initializeMemorySettingsProfileSeaside` has been removed
- jQuery UI `#zIndex:` has been removed use the appendTo option instead https://jqueryui.com/upgrade-guide/1.12/#removed-zindex
- the "XHTML" validation tool int the development toolbar has been removed, it is unlikely this ever worked

# Deprecated Features #
 * `JSObject >> #timeout:` has been deprecated in favour of `JSObject >> #setTimeout:`. `JQAjaxSetup >> #timeout:` and `JQAjax >> #timeout:` have not been deprecated.
 * `WAHtmlRoot` `#beXhtml10Strict`, `#beXhtml10Transitional` and `#beXhtml11` have been deprecated in favor of `#beHtml5`.

# Upgrading #