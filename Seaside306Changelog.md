# Introduction #
Seaside 3.0.6 is a bug fix release for Seaside 3.0.


# Details #

The following bugs were fixed:
  * jQuery 1.6.2
  * jQuery UI 1.8.16
  * added some mssing jQuery 1.6 methods (thanks Nick)
  * changed JQAutocomplete use of #onComplete: to #onSuccess: as JQGetJson no longer supports #onComplete: (thanks Nick)
  * when unable to proceed from a debugger the HTML would only be rendered in Comanche
  * [#572](https://github.com/SeasideSt/Seaside/issues/572): 	add #removeDelegation convenience (thanks John)
  * [#617](https://github.com/SeasideSt/Seaside/issues/617):	update WAPerformanceFunctionalTest for Cog (thanks Gerd)
  * [#631](https://github.com/SeasideSt/Seaside/issues/631): 	update /status for Cog VM (thanks Michael)
  * [#642](https://github.com/SeasideSt/Seaside/issues/642): 	WATagBrush and WAPrettyPrintedDocument inconsistencies
  * [#653](https://github.com/SeasideSt/Seaside/issues/653): 	WARequestContext>>newDocument assumes #handler is not nil
  * [#654](https://github.com/SeasideSt/Seaside/issues/654): 	Add support for the disabled option to JQButton (thanks Jan)
  * [#655](https://github.com/SeasideSt/Seaside/issues/655): 	WAMimeType>>fromString: cannot handle a wildcard
  * [#656](https://github.com/SeasideSt/Seaside/issues/656): 	Browser broken on Welcome Page
  * [#658](https://github.com/SeasideSt/Seaside/issues/658): 	Seaside-Email should use request handler for configuration look up
  * [#659](https://github.com/SeasideSt/Seaside/issues/659): 	WAApplication>>handleFiltered: DNU when request isPrefetch (thanks Nick)
  * [#660](https://github.com/SeasideSt/Seaside/issues/660): 	support path parameters in WAUrl
  * [#662](https://github.com/SeasideSt/Seaside/issues/662): 	Refactor GRDelayedSend to use composition and reduce duplicate
  * [#671](https://github.com/SeasideSt/Seaside/issues/671):	Seaside-Tests-Core-Documents breaks dependencies
  * [#672](https://github.com/SeasideSt/Seaside/issues/672):	SequenceableCollection>>#endsWith: is not portable
  * [#673](https://github.com/SeasideSt/Seaside/issues/673):	WAHeaderFieldsTest>>#testCrLf uses non-portable method
  * [#674](https://github.com/SeasideSt/Seaside/issues/674): 	WASwazooAdaptor decodes an already decoded URI
  * [#677](https://github.com/SeasideSt/Seaside/issues/677): 	"self.opener is null" when opening sytle editor
  * [#678](https://github.com/SeasideSt/Seaside/issues/678): 	an iframe should be more like a pop up
  * [#682](https://github.com/SeasideSt/Seaside/issues/682): 	JQButton >> #icons and #icons: seem inconsistent
  * [#683](https://github.com/SeasideSt/Seaside/issues/683): 	self call: self infinite recursion detection (thanks Sean)
  * deprecate WATagBrush>>#onEnter:
  * deprecate WADivTag>>#clear
  * WARequestHandler class >> #push:while: has been deprecated for #push:during:

The following features were added:
  * The iframe tag now has a `#callback:` method that allows you to switch the session presenter. This has the advantage that the script generator and all the libraries are preserved. See `WAIframeFunctionalTest` for an example.
