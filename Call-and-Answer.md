Other topics have covered the lifecycle of an individual component: when it gets to be its turn to render itself, it uses a WARenderCanvas to [[generate HTML|Generating-HTML]], registering callbacks for its links and forms along the way. When the next request comes in, some of those callbacks will be executed, the [[state|Maintaining-State]] of the component or application will be changed, and it’ll render itself again.

But what happens if, partway through a callback, the application needs some further information from the user? For example, let’s say the callback is in response to the user clicking a "change background color" link on a the configuration page of their hosted blog service. The callback would like to oblige - it knows exactly which attribute of which object to set - but first it needs to know what color to change it to. It has to show the user a color picking page. In Seaside, a component does this by sending `#call:`.

The `#call:` method works like this: it takes a new component instance as a parameter. It swaps the current (receiver) component out for the new component: where the receiver used to be on the page, you will now see argument. This may mean that the user sees a completely different page (if the receiver was at the root of the component tree), or it may be relatively minor (if the receiver was just one of many [[embedded components|Embedding-Subcomponents]]). It then suspends the current process and displays the updated page to the user.

The new component will now be generating HTML, registering callbacks, and evaluating those callbacks when the use submits requests. In our example, this means, say, generating links for various colors. At some point, the user will do something (clicking on a color) that will cause a callback to run that will send the `#answer:` message to the new component. This is where the magic happens:

The value passed to `#answer:` will be returned from the original send to `#call:`, and the program will continue from there.

So our callback for the "change background color" link might look like this:

```smalltalk
changeBackgroundColor
  |color|
  color := self call: ColorPicker new.
  blog backgroundColor: color
```

What would happen is this: at the point of the send to `#call:`, an HTTP response would go back to the user showing the ColorPicker. The user would click on a color link, which would send a new HTTP request, which would invoke a callback somewhere in ColorPicker. This would cause a new Color instance to be created and sent to `#answer:`. That same color instance would then be returned from `#call:`, assigned to "color", and set as the blog’s background color. The changeBackgroundColor callback would then end, trigging another HTTP response, but with the config page now swapped back in for the color picker.

There’s nothing saying the callback has to end after one `#call:` - you can string as many together as you want. This makes it very easy to define complex workflows, since you can use all of the normal Smalltalk control structures (loops, if statements, temporary variables) when stringing together a long sequence of pages.

What’s more, the back button will still work. If, in the example above, the user decides they don’t like their choice, and hit the back button to go back to the color picker, choosing a different color, the send to `#call:` will return a second time, with a different value, and the program will continue as before.

It’s good style in Seaside to structure your application so that as many of your components as possible simply `#answer:` a value rather than calling any others; this helps keep coupling loose and components reusable. For example, that ColorPicker might be used in tens of different situations in the blog application, or even in tens of other applications. Because it has a simple `#call:`/`#answer:` interface, it doesn’t have to know anything about where it’s being used - just like a subroutine.