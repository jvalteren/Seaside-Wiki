Traditional web development passes state between pages as parameters in link URLs or hidden form fields. In Seaside, all the links are generated for you. How do you maintain state?

After an action callback finishes, the same component instance gets redisplayed. You can use its instance variables to store user interface state. These instance variables can be initialized in the component’s `#initialize` method. For example, you could add a simple ’counter’ instance variable, and increase it every time the user clicks a certain link:

```smalltalk
initialize
  super initialize.
  counter := 0

renderContentOn: html
  html heading: 'The counter is ', counter asString.
  html anchor
     callback: [ counter := counter + 1 ];
     with: 'increase'
```

## Backtracking

In the example above when you use the back button the state will not be reverted. If you want the state to be reverted you have to either tracking the state of the entire component

```smalltalk
states
  ^ Array with: self
```

Or by only tracking the state of the "counter" instance variable with a `WAValueHolder `

```smalltalk
initialize
  super initialize.
  counter := WAValueHolder with: 0

renderContentOn: html
  html heading: 'The counter is ', counter value asString.
  html anchor
     callback: [ counter value: counter counter + 1 ];
     with: 'increase'

states
  ^ Array with: counter
```
