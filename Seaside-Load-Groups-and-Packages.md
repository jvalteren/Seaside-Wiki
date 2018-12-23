Seaside is composed of several parts, often mapping to a separate code package or a Metacello group, so you can load only the parts you need when working on your own Seaside application project. Obviously, the configuration specifies all required dependencies, so that when you want to load the 'JQuery' group, the 'Javascript' group will automatically be loaded as well.

The default load group (i.e. when you do not specify any specific groups to load) loads the most common parts you will need when building a web application. This is perfect when you start out with Seaside. However, at some point in time, you will want to load additional groups or leave out the parts you do not use in your own project.

Here is a complete list of Metacello groups you can use, with a small description of the contents. If you want to know more, check out the Metacello config in the `BaselineOfSeaside3` class. 

* **Core**: This is the core of Seaside you will need in all cases.
* **JSON**: Serialize Smalltalk objects to JSON, deserialize JSON to Smalltalk objects and produce JSON responses in general using a JSON parser and Seaside JSON canvas.
* **REST**: Create a REST API service (See https://github.com/SeasideSt/Seaside/wiki/Seaside-REST)
* **Email**: Support for sending emails
* **Javascript**: Support for generating Javascript code
* **JQuery**: Binding for generating jQuery scripts (http://www.jquery.org)
* **JQueryUI**: Binding for jQuery-UI components (https://jqueryui.com/)
* **Scriptaculous**: Binding for Scriptaculous (https://script.aculo.us/) and prototypejs (http://prototypejs.org/)
* **RSS**: Support for building RSS feeds
* **Security**: Extensions to activate specific security features
* **Seaside-InternetExplorer**: Support for Internet Explorer-specific features
* **Comet**: Polling-based updates (See http://book.seaside.st/book/web-20/comet)
* **Filesystem**: Support for `WAFilelibrary` functionality on disk instead of via the image (i.e. `WAExternalFileLibrary`)
* **Gettext**: i18n solution for Seaside apps (https://www.gnu.org/software/gettext/)
* **Development**: Development tools
* **Slime**: Lint rules 

All the web server adaptors are also contained in separate groups. You need at least one adaptor to serve your Seaside application. We recommend 

* **Zinc**: Default in Pharo (http://zn.stfx.eu/zn/index.html)
* **WebClient**: Squeak only. (http://ss3.gemstone.com/ss/WebClient.html)
* **Swazoo**: http://www.squeaksource.com/@nRFs5PD_3rsW_l2b/z59e7NTB
* **Kom**
* **FastCGI** (Gemstone only)