_From [Seaside - A Multiple Control Flow Web Application Framework](http://www.iam.unibe.ch/~scg/Archive/Papers/Duca04eSeaside.pdf), Ducasse, Lienhard, Renggli_

As Seaside is written in Smalltalk it is based on a very powerful, fully object-oriented language and development environment. In addition to being able to use the tools provided by the environment, Seaside integrates them seamlessly with the web. This makes the platform a versatile and productive environment for web application development.

We start by looking at the debugging facilities before presenting the Seaside specific tools.

# Incremental Programming

Smalltalk’s philosophy of incremental programming in an interactive environment is supported by Seaside. Code can be added and edited while the web application is running and there is neither the need to manually recompile the code nor to restart the server. In many cases this makes it possible to update a system in production on the fly without any outage and without the need to set-up a temporary backup server.

These facilities are also very powerful for debugging. Seaside provides unique debugging features. Read more in [[Debugging Seaside Applications|Debugging-Seaside-Applications]].

# Toolbar

A toolbar that is shown at the bottom of the web-application during the development phase enables the programmer to access additional tools from within the web. Of course, all these tools have been written in Seaside itself:

[[devel_tools.png]]

<dl>
<dt>New Session</dt><dd>starts the application within a new session.</dd>
<dt>Configure</dt><dd>opens a dialog letting the user configure the settings of the application. This includes properties such as where to start a new session, what should happen in case of an exception or if the development toolbar is displayed or not.</dd>
<dt>Toggle Halos</dt><dd>shows or hides the halos, which are discussed in detail in the next paragraph.</dd>
<dt>Profile</dt><dd>shows a detailed report on the computation time that has been consumed while building this page.</dd>
<dt>Memory</dt><dd>Use displays a detailed report on the amount of memory consumed by this application.</dd>
<dt>XHTML</dt><dd>starts an external XML validator on this page.</dd>
<dl>

# Tool support

There are specialized refactoring and code critics tools available for Seaside. Check out the following articles by Lukas Renggli:

OmniBrowser Refactoring Tools
New Refactorings
Code Critics in OB
Slime