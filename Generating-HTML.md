Every Seaside component needs to know how to display itself as HTML. Usually, it does so by sending a message to a WARenderCanvas instance that returns an instance of a subclass of WATagBrush. You can then call methods of the instance of a subclass of `WATagBrush`. For example, `#heading` returns an instance of `WAHeadingTag`, `#level1` sets the the tag to a level one heading, and `#with:` will set the heading tag’s contents. A simple `renderContentOn:` might look like this:

```smalltalk
renderContentOn: html
   html heading level1; with: 'Hello World!'.
```

# Simplified Tags

Most of the time you will set a tag’s contents by calling `#with:` on that tag but `WARenderCanvas` provides a convenient way to create a tag and set its content with one method for most tags. The above example created a h1 tag that contains the string "Hello World!". The simple way to do this is by calling `#heading:`

```smalltalk
renderContentOn: html
   html heading: 'Hello World!'.
```

# Nested Tags

When a tag’s content includes nested tags instead of just text, you can pass a block in instead. The block will be evaluated, and any html generated within the scope of the block will be nested within the tag. For example, part of the heading might want to be within an `<strong>` tag:

```smalltalk
renderContentOn: html
   html heading: [
      html text: 'Hello, '.
      html strong: 'wide'.
      html text: ' world!' ]
```

This would produce `<h1>Hello, <strong>wide</strong> world!</h1>`. As you can see, if you just want to output text to the document without any surrounding tag, use `#text:`.

These conventions for content are applied uniformly throughout the methods implemented by the subclasses of `WATagBrush`. Here’s a more complex example, that builds a table with two rows and two columns:

```smalltalk
renderContentOn: html
   html table: [
      html tableRow: [
         html tableData: [ html strong: 'Name' ].
         html tableData: person name ].
      html tableRow: [
         html tableData: [ html strong: 'Age' ].
         html tableData: person age ] ]
```

# Attributes

Attributes can be set by calling the appropriate method on a tag before calling `#with:` to set it’s contents. The following creates `<div id="some-dom-id" class="some-dom-class">Styled and identified text</div>`.

```smalltalk
renderContentOn: html
   html div 
      id: 'some-dom-id'; 
      class: 'some-dom-class'; 
      with: 'Styled and identified text'
```

It’s important what #with: be called after setting any attributes. Not doing so will result in your tag being output without the attributes that follow the call to #with:.

# Links and Forms

For more information on WARenderCanvas, including how to use it to generate links, forms, and form inputs, see [[Links, Forms and Callbacks|Links,-Forms-and-Callbacks]].