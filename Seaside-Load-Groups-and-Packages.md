**work in progress**


Seaside is composed of several parts, often mapping to a separate code package or a Metacello group, so you can load only the parts you need when working on your own Seaside application project.

The default load configuration (i.e. when you do not specify any specific parts) loads the most common parts you will need when building a web application. This is perfect when you start out with Seaside. However, at some point in time, you will want to load additional parts or leave out unused parts in your own project.

Here is a complete list of Metacello groups you can use. If you want to know more, check out the `BaselineOfSeaside3` class.

Core
JSON
REST
Email
Javascript
JQuery
JQueryUI
RSS
Scriptaculous
Security
Seaside-InternetExplorer
Examples
Comet
Comet-Examples
Filesystem
Gettext
Development
Slime
Tests
Examples

Adaptors:
- Swazoo
- Kom
- Zinc
- FastCGI