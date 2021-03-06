[Seaside](http://www.seaside.st) provides a layered set of abstractions over HTTP and HTML that let you build highly interactive web applications quickly, reusably and maintainably. It is based on Smalltalk, a proven and robust language that is implemented by  different vendors. Seaside includes:

  * [[Programmatic XHTML generation|Generating-html]].  A lot of markup is boilerplate: the same patterns of lists, links, forms and tables show up on page after page. Seaside has a rich API for generating HTML that lets you abstract these patterns into convenient methods rather than pasting the same sequence of tags into templates every time.

  * [[Callback-based request handling|Links,-Forms-and-Callbacks]]. Why should you have to come up with a unique name for every link and form input on your page, only to extract them from the URL and request fields later?  Seaside automates this process by letting you associate blocks, not names, with inputs and links, so you can think about objects and methods instead of ids and strings.

  * [[Embedded components|Embedding-Subcomponents]]. Stop thinking a whole page at a time; Seaside lets you build your UI as a tree of individual, stateful component objects, each encapsulating a small part of a page. Often, these can be used over and over again, within and between applications - nearly every application, for example, needs a way to present a batched list of search results, or a table with sortable columns, and Seaside includes components for these out the box.

  * [[Modal session management|Call-and-Answer]]. What if you could express a complex, multi-page workflow in a single method?  Unlike servlet models which require a separate handler for each page or request, Seaside models an entire user session as a continuous piece of code, with natural, linear control flow. In Seaside, components can call and return to each other like subroutines; string a few of those calls together in a method, just as if you were using console I/O or opening modal dialog boxes, and you have a workflow. And yes, the back button will still work.

  * Portability. Seaside has been successfully ported to many dialects and servers helping you to avoid vendor lock-in.

Seaside also has good support for [[CSS and Javascript|CSS-and-Javascript]], excellent [[web-based development tools|Development-Tools]] and [[debugging support|Debugging-Seaside-Applications]], a rich [[configuration and preferences|Configuration-and-Preferences]] framework, and more.

If you would like to contribute, please visit [Seaside's contributors page](https://github.com/SeasideSt/Seaside/blob/master/CONTRIBUTING.md).