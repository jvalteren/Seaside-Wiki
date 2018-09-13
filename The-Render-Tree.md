The current state of the UI of a Seaside application is represented by a tree of `WAPresenter` instances. The root of this tree is held onto by the current `WARenderLoop`. The render loop spins in a tight loop, iterating once for each response/request pair. Each iteration involves walking the tree several times, for these purposes:

1. Updating the current URL. Any presenter in the tree may wish to append extra information to the current URL, either to make bookmarks more meaningful when revisted in the future (once the current session state has disappeared), or to affect behavior in the present (eg, by adding fragments to the URL that refer to named anchors in the current document).

   In this pass, each presenter will be sent `#updateUrl:`, and passed an instance of `WAUrl`. Most presenters will not take advantage of this, which is fine, and so the default implementation is empty.

2. Rendering content to the response. The actual message sent to each item in the tree is `#renderWithContext:`, which is passed a `WARenderingContext`, but this is a fairly low level method, and probably won’t need to be overridden. In its default implementation, it creates a new renderer around the context (usually `WAHtmlCanvas`, but overridable via #rendererClass), and sends `#renderAllOn:` passing the renderer as an argument. This, in turn, sends #renderContentOn:.

3. Processing the request. This is actually two passes, #processRequestInputs:context: and #processRequestActions:context:. Each gets passed in a WARequest and a WARenderingContext as arguments. By default, each presenter extracts from the rendering context any of the callbacks that it caused to be registered during #renderContentOn:, and then invokes any that appear in the request. #processRequestInputs:context: only invokes value callbacks (for form inputs, etc), whereas #processRequestActions:context: invokes action callbacks, from links and submit buttons. This ensures that all of the values are set before any actions occur.

In Seaside 2.3, all of the callbacks were processed at once, rather than by the individual items in the tree. This change makes it possible for items in the tree to control the request processing of items below themselves, for example by wrapping a special exception handler around them (useful for validation, eg), or by preventing them from processing the request at all (useful for authentication and similar). To easily enable this kind of thing, both request processing passes go through #processRequest:do:, which takes the WARequest as its first argument and the block as its second. The default implementation simply evaluates the block, which then continues the walk down the tree; overriders may put conditional evaluation around evaluating it based on the request, or evaluate it inside an exception handler, etc. See WATransaction or WABasicAuthentication for examples.

The tree is a somewhat unusual "shape". The skeleton is made up of instances of WAComponent, a subclass of WAPresenter. Each of these must implement the #children method, which returns a (possibly empty) collection of its subcomponents. In the simplest case, walking the tree is simply a depth first traversal: from the root component, through its children, recursively to their children, and so on.

However, this basic case can be extended in two ways. The first is by "decorating" components. Each WAComponent can have any number of instances of WADecoration (another subclass of WAPresenter) attached to it. When walking the tree, all of a component’s decorations are visited before it is. This means that, for the purpose of tree traversal, a component is "below" its decorations: they can render content that encloses it, or affect its request processing.

At the implementation level, a `WAComponent` has a (possibly unused) pointer to its first (outermost) decoration. Decorations have a "next" pointer to the next decoration or, for the last/innermost decoration, to the component itself.

So a simple tree might look like this:

```
                x ------> y ----> z --
                |  next      next    |
     decoration |                    | next
                |                    |
root --------> A <-------------------
   |   child    |
   |            | child
   |            C
   B
```

The root has two children, A and B. A has one child, C, and B has none. A also has three decorations, x, y, and z. The traversal will be in this order: (root, (x, y, z, A, ©), (B)). The most important thing to note about this is that A’s decorations get visited **before** A itself does. Also note that, as indicated by the parentheses, A and C are both "controlled" by the x-y-z decorations, whereas the root and B are not.

The second way the tree is extended is a special case of the first, which is through delegation. When a component is sent `#call:` with another component, it adds a special WADelegation decoration with the new component referenced as the "delegate". `WADelegation` is unusual in that it adjusts the tree traversal to follow the "delegate" pointer instead of the "next" pointer. This means that, for as long as that decoration is present, the component being decorated and all of its children will be obscured by the delegate - they won’t appear in any tree traversals. When the delegate is sent #answer:, the Delegation decoration is removed and the original component becomes visible again.

So, adding a delegation "d" with a component "D" to the above tree, we would get:

```
                                        delegate
                x ------> y -------> d ----------> D
                |  next       next   |
                |                    | next
                |                    |
     decoration |                    z
                |                    | next
root --------> A <-------------------
   |   child    |
   |            | child
   |            C
   B
```

The traversal order is now (root, (x, y, d, D), (B)). Nothing "under" d (including the decoration z and the components A and C) will get rendered. However, x and y, which are before d in the decoration list, do get visited (and have control over D and its children).

Why are x and y before d, and z after? It’s up to the individual decoration. Some decorations (usually those that affect request processing, rather than those that render content) will wish to stay active during delegation, and some will not. Those that wish to come before delegations in the chain should implement `#isGlobal` to return true.