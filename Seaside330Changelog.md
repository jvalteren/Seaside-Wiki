**Seaside 3.3.0 has not yet been released.**

Seaside 3.3 includes [Grease 1.3](https://github.com/SeasideSt/Grease/wiki/Grease-1.3-Changelog)

# Features #
- Seaside-REST supports OPTIONS
- Seaside-REST supports PATCH
- a lot more HTML5 events are supported
- HTML root supports directly passing a style sheet or Javascript as a String
- Seaside now generates HTML-stlye boolean attributes instead of XML-style boolean attributes, `checked` instead of `checked="checked"`
- `#initializeCache` can be sent to an application after changing the cache configuration for the changes to take effect
- the bundled Prototype.JS has been updated from 1.7.0 to 1.7.3 which fixes multi select lists
- the mime types for .sass and .scss are not supported
- the Seaside-REST-Examples package has been added which contains examples on how to use Seaside REST
- `WAEnterpriseAuberginesStrategy` has been added which tracks session similar to Tomcat/Java EE/Servlet with a "JSESSIONID" cookie and a "jsessionid" path parameter so that existing sticky session load balancers can be used.

# Issues Resolved #
[Issues resolved](https://github.com/SeasideSt/Seaside/milestone/4?closed=1)

- `WAXmlCanvas builder` now returns a ready to use builder
- render phase continuation and `JSObject` now uses the script generator configured on the application
- WAErrorHandler now sets the charSet
- loading the `REST` Metacello group no longer loads Seaside-Component

# Breaking Changes #

- When a session has expired no longer is #expiredRegistryKey being sent to the response generator. Instead #handleDefault: is not sent to the application. If you require the old behavior you should subclass `WAApplication` and override `#handleExpired` with the method below. See https://github.com/SeasideSt/Seaside/issues/916.
```
handleExpired: aRequestContext
	
	aRequestContext responseGenerator
		expiredRegistryKey;
		respond
```
- #initializeMemorySettingsProfileSeaside has been removed

# Deprecated Features #
 * `JSObject >> #timeout:` has been deprecated in favour of `JSObject >> #setTimeout:`. `JQAjaxSetup >> #timeout:` and `JQAjax >> #timeout:` have not been deprecated.

# Upgrading #