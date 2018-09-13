# Links

A component should be able to do more than present information to the user, it should be able to react to things the user does as well. One thing a user might do is click on a link.

In Seaside, any time you generate a link, you can associate a callback block with it. The block will get invoked whenever the user clicks on that particular link. You do this by creating an anchor tag by calling the method `#anchor` and calling the `#callback:` method on the resulting anchor. The argument to `#callback:` should be a code block that will be run when the user clicks the link. For example, this method will generate a "Beep!" link that will cause the Smalltalk image to beep:

```smalltalk
renderContentOn: html
   html anchor
       callback: [ Smalltalk beep ];
       with: 'Beep!'
```

It’s interesting to consider what happens if you have a loop that generates multiple links. What would the following code do?

```smalltalk
renderContentOn: html
   1 to: 10 do: [ :index |
       html anchor
          callback: [ index inspect ];
          with: index.
       html space ]
```

It will generate a series of links, labelled from 1 to 10. Because the action blocks capture the current value of the i variable, each one will have a distinct callback: clicking on the "1" link will evaluate 1 inspect, on the "2" link will evaluate 2 inspect, and so on. The block closure is being used to maintain all of the interesting state for the link.

# Forms

You can use the `#form` method to generate a form. Form inputs work similarly to links: they use blocks, rather than names, to react to user input. One of the simplest form input tag objects is `WATextInputTag`. To create a `WATextInputTag` you call `#textInput` and can set the resulting text input tag’s callback using `#callback:`. For example, you might set up a form that looked like this:

```smalltalk
renderContentOn: html
   html form: [
      html text: 'Name: '.
      html textInput
         value: person name;
         callback: [ :value | person name: value ].
      html break.
      html submitButton ]
```

The text input will show the current value of person name, and when the submit button is pressed whatever text the user has entered will be sent to `#name:`.

Objects who subclass `WAFormInputTag` are all intended to be used inside a form. For example, `#textArea` returns an instance of `WATextAreaTag`, `#checkbox` returns an instance of `WACheckboxTag` that expects a boolean value, and #select returns an instance of `WASelectTag`.

All of these also can use a target/selector form instead of value/callback. The example above could also be written like this:

```smalltalk
renderContentOn: html
   html form: [
       html text: 'Name: '.
       html textInput on: #name of: person.
       html break.
       html submitButton ]
```

Submit buttons are treated, by and large, exactly like links: they can be given a zero-argument action block, and if a particular button is used to submit the form, its action block will be evaluated. For example:

```smalltalk
renderContentOn: html
   html form: [
      html heading: 'Continue?'.
      html submitButton
         callback: [ self continue ];
         with: 'Yes'.
      html space.
      html submitButton
         callback: [ self return ];
         with: 'No' ]
```

As with links, the block closures will capture the appropriate state, so that it’s easy to have loops containing form inputs:

```smalltalk
renderContentOn: html
   html form: [
      self lotsOfPersons do: [ :each |
         html textInput
            value: each name;
            callback: [ :value | each name: value ].
         html horizontalRule ].
      html submitButton ]
```

For more examples look at `#renderContentOn:` in the `WAInputElementContainer` class.