# Authentication and Session Tracking

You have to find a session tracking strategy that matches your requirements. If none of the existing ones fit the bill you can implement your own. Be aware that anything based on URLs allows users to copy and paste links to other users who get the same session.

We recommend you use the default `WAQueryFieldHandlerTrackingStrategy` plus a (signed or encrypted) cookie for authentication. That way users can have multiple independent sessions open in different tabs and you are not vulnerable to users copy and pasting links.

# TLS

We recommend you use TLS and a front end web server like [Apache HTTP](https://httpd.apache.org) or [nginx](https://nginx.org/en/) that does TLS termination. From a performance point of view we also recommend you serve static resources through the front end web server.
Follow the best practices for configuring that server and TLS.

# Development Tools

We strongly recommend you do not deploy any development tools. This includes the Seaside-Development and Seaside-Welcome packages. We also recommend against deploying Seaside-Tools-Web, if you need them make sure they are protected by strong passwords or better. If possible we recommend you remove the compiler from your production images.
We also recommend you do not deploy any tests.

# XSS and XSRF

If you are using Seaside components and are not parsing URLs you should not have to do anything to protect yourself against XSS and XSRF. For more information see [[Security Features|Security-Features]]

If you're using Seaside-REST from a browser/JavaScript context you need a way to protect yourself against XSRF

# SQL Injection

Refer to the document of your database access technology on how to protect yourself against SQL injection.