_From [Seaside - A Multiple Control Flow Web Application Framework](http://www.iam.unibe.ch/~scg/Archive/Papers/Duca04eSeaside.pdf), Ducasse, Lienhard, Renggli_

Most of today’s frameworks do not support debugging of web applications well. Most display the error and the line number in the web browser only, which makes it very inconvenient to find and fix bugs.

When an unhandled exception occurs (see figure below), a stack trace is shown in the web browser (a) with a link called debug. Clicking this link (1), the developer activates a debugger (b) within the development environment which lets him inspect variables and even modify the code on the fly. In the given example the message printStringAsCents, that is automatically highlighted in the debugger (b), has been spelled wrongly and is fixed (2) by the developer. The debugger now displays the recompiled method (c). During this time, the web browser keeps waiting for the response of the server. When hitting proceed (3), the processing of the request which had caused the error is resumed and the resulting page is finally displayed in the web browser (d).

This feature makes debugging web applications very powerful: there is no manual recompilation and restarting of the web server required. The developer is put right back into the questionable page where he is able to see if he fixed the error properly and is able to continue the testing session.

[debug_loop.png]