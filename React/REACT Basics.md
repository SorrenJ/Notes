
## Hooks


functions that begin with use. can be utilized to invoke different features of React into your functional components


### useState

 if a component is rerendered, we lose all the information we were trying to capture. The `useState` hook gives functional components the ability to use and manage state.

The only parameter the hook uses is the initial state.

It returns two values:

1. the state; and
2. a function to update the state.

```jsx
import { useState } from "react";

function SpecialsMenu() {
  // currentSpecial is the variable we can call to reference state
  // setCurrentSpecial is the function used to set a new state
  const [currentSpecial, setCurrentSpecial] = useState("Pizza");
  const modifySpecialToTacos = () => {
    setCurrentSpecial("Tacos");
  };
  return (
    <div>
      <h1>Today's special is:</h1>
      <h2>{currentSpecial}</h2>
      <button onClick={modifySpecialToTacos}>We want tacos!</button>
    </div>
  );
}
```

Upon clicking on the button, it will trigger the `modifySpecialToTacos` function. The `modifySpecialToTacos` function will run the `setState` function `setCurrentSpecial`, which modifies the state to “Tacos”.

This allows us to save data in between renders.


### useEffect

 `useEffect` cannot fetch information, however it can invoke functions when the component is rendered. It “listens” for changes, it can even listen for when a state is modified.


## State

State enables React components to respond dynamically to user interactions, data changes, and other events. When a component's state changes, React automatically re-renders it to update the user interface to reflect the new state.

State encompasses the key parts of our UI that change based on user input. State allows us to retain information across multiple renders. To store state, we need to use a feature of React called "hooks" - specifically, the `useState` hook.

```jsx
import React, { useState } from "react";
```


```jsx
function Application(props) {
  const [count, setCount] = useState(0);

  return (
    <main>
      <Button onClick={() => setCount(count + 1)}>Increment</Button>
      <h1>Button was clicked {count} times.</h1>
    </main>
  );
}
```

The `useState` function receives an optional argument which is the default value for the state. `useState` returns an array. The first element (count) of the array is the current value for the state. The second element (setcount) is a function that can update the state and cause a render.


The `useState` hook uses array destructuring syntax to return two different values. The pair of values returned in the array are related. One is used to _get_ a value (a "getter) and the other is used to _set_ a value (a "setter").


### Implementing default value to useState

```jsx
  function SomeComponent(){
    const [state, setState] = useState("Some word");
    //...
  }  
```


### Setting a state

```jsx
  setState("Some other word")
```

Calling `setState` like this will update the value of the state to `"Some other word"` in the next rendered version of our app. Since we need to wait for the next rendered version, that means that `setState` works asynchronously.


### Referencing an already created function

```jsx
  const handleClick = (event) => setState(event.target.value ? event.target.value : "");

  <button onClick={handleClick} />

  // instead of

  <button onClick={event => setState(event.target.value ? event.target.value : "")} />
```


## Controlled components

When we create components that override HTML form elements to let React control their state, we call them **controlled components**.

```jsx
function DisplayWord(props) {
  const [word, setWord] = useState("");

  return (
    <main>
      <input
        value={word}
        onChange={(event) => setWord(event.target.value)}
        placeholder="Please enter a word"
      />
      <h1>Your word is: {word}.</h1>
    </main>
  );
}
```

the `<input>` element becomes a _controlled component_ when we provide a `value` prop and an `onChange` event handler that can update the value.

Let's consider what is happening at each step of the input cycle:

1. A user types a single character "A" into the `input` element. 
``` js
 value={word}
```
2. The `onChange` event handler is triggered. 

```jsx
 onChange={(event) => setWord(event.target.value)}
```
3. It invokes the `setWord` function to change the state.

``` js 
setWord(event.target.value)}
```
4. When the state changes, React calls the component function.
5. The `useState` call returns the current value which is "A".
``` js
const [word, setWord] = useState("");
```
7. The `input` element has its value set to "A".
8. The browser displays the `"Your word is: {word}."` message as `"Your word is: A."`

As the user types more letters, we run this process for each change to the input. The value of the `word` state always contains the most recent user input. Another benefit is that you can call `setWord("")` (with an empty string) at any point to empty or reset the `<input>`.


## Fragmentation

this is a nono. There are two root elements being returned.

```jsx
return (
  <h1>A heading</h1>
  <p>A paragraph</p>
)
```


It could be fixed by wrapping both elements in a `<div>`:

```jsx
return (
  <div>
    <h1>A heading</h1>
    <p>A paragraph</p>
  </div>
)
```

you may want to consider a `<Fragment>`. Unlike a `<div>`, a React `<Fragment>` does not create any extra DOM nodes, which can clutter up your HTML output and cause problems with your layout.

```jsx
return (
  <React.Fragment>
    <h1>A heading</h1>
    <p>A paragraph</p>
  </React.Fragment>
)
```

```js
import React, { Fragment } from 'react';
```


```jsx
return (
  <Fragment>
    <h1>A heading</h1>
    <p>A paragraph</p>
  </Fragment>
)
```


for tenary conditionals

```jsx
const userLoggedIn = false;

return (
  <Fragment>
    {userLoggedIn ? 
    <>
      <h1>Success!</h1>
      <p>You are logged in.</p>
    </>
    :
    <>
      <h1>Warning!</h1>
      <p>You are not logged in</p>
    </>
    }
  </Fragment>
)
```

## Conditional Rendering

You gotto use ternary operator, if statements don't work.  You cannot `return` an `if` statement! You can only return an **expression** - something that resolves to a value. Moreover, we can't use `if` statements inside JSX.

```jsx
function Hello(props) { 
// CORRECT: Ternary is allowed. One of the two <h1s> will be rendered
  return (props.yourName ? <h1>Hello, {props.yourName}.</h1> : <h1>Sorry, you don't seem to have a name.</h1>);
}
```


### Short Circuit Evaluation

The logical `&&` operator can be used for short-circuit evaluation. It is evaluated left to right, and if the first expression is false, the second expression is not evaluated at all. 

The `&&` operator is a good choice if you want to show something or nothing at all. It can be useful to think of the `&&` operator like an `if` statement.

```jsx
function Hello(props) { 
  return (props.yourName && <h1>Hello {props.yourName}</h1>);
}
```

`{props.yourName} && <h1>Hello {props.yourName}</h1>` is short-circuit evaluated. If `props.yourName` contains a value that is truthy, the `<h1>` will be rendered. 

However, if `props.yourName` is falsy then the JSX expression `<h1>Hello {props.yourName}</h1>` is _not evaluated at all_ and nothing is shown. Any side effects of attempting to render the `<h1>` don't matter, and no error will be thrown.
