Unlike some low level frameworks Seaside offers built in protection against many common web application vulnerabilities.

# Attacks #
This is how Seaside protects you against common attacks against your web application.

## Session Fixation ##
Session fixation is not possible because client supplied session ids are ignored when no matching session is found. Review the implementors of `#noHandlerFoundForKey:in:context:`.

Further information:
  * https://www.owasp.org/index.php/Session_fixation

## Cross-site Scripting (XSS) ##
The Seaside templating engine "the render canvas" escapes all output by default. It therefore adopts a safe by default policy. Special effort has to be taken to render values without escaping. Such places can easily be found and audited by looking at all the senders of `#html:`.

Further information:
  * https://www.owasp.org/index.php/XSS
  * https://en.wikipedia.org/wiki/Cross-site_scripting
  * https://wonko.com/post/html-escaping

## Cross-Site Request Forgery (CSRF) ##
Seaside uses a [capability based](https://en.wikipedia.org/wiki/Capability-based_security) security model where only handles to actions are handed to the client. These handles are bound to a state snapshot (continuation). The state snapshots are identified by a random number which is session specific and acts like a CSRF token.

It is important that this only applies to "traditional" usage of Seaside with sessions and components, this does not apply to Seaside-REST where Seaside provides no built-in protection against CSRF.

Further information:
  * https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)

## Response Splitting ##
Seaside prevents response splitting does by not allowing CR or LF values in HTTP headers.

Further information:
  * https://www.owasp.org/index.php/Response_Splitting
  * https://en.wikipedia.org/wiki/HTTP_response_splitting
  * https://nealpoole.com/blog/2011/01/http-response-splitting-on-reddit-com/

## Malicious File Execution ##
Since Seaside is not executing files or templates it is not vulnerable to malicious file execution.

Further information:
  * https://www.owasp.org/index.php/Top_10_2007-A3

# Features #
In addition to the protections against the attacks above Seaside offers the following security related features.

## Capabilities ##
Seaside uses a [capability based](https://en.wikipedia.org/wiki/Capability-based_security) security model where only handles to actions are handed to the client. These handles are bound to a state snapshot (continuation).

## Strict Transport Security (STS) ##
Seaside offers the built in `WAStrictTransportSecurityFilter` that adds a `Strict-Transport-Security` and redirects to the HTTPS version of the current URL.

Further information:
 * https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security

## DoS ##
Seaside offers limited protection against DoS-attacks by offering the option to limit the amount of session.

In general Seaside has not been throughly reviewed to make sure it is not vulnerable to:

 * hash collisions
 * request headers (body size)

Further information:
 * https://en.wikipedia.org/wiki/Denial-of-service_attack
 * https://www.owasp.org/index.php/Denial_of_Service

## Arbitrary Code Execution ##
Seaside does not interpret or execute data sent by the client. However some Smalltalk dialects have what is essentially an implementation of `eval()` in the form of `Object class >> #readFrom:`. You have to review that you never pass user input to this method either directly or indirectly, eg. in the form of `Boolean class >> #readFrom:`

# Further Information #
  * [Open Web Application Security Project (OWASP)](https://www.owasp.org/)
  * [Ruby On Rails Security Guide](https://guides.rubyonrails.org/security.html)
  * [4 HTTP Security headers you should always be using](https://www.ibuildings.nl/node/234)