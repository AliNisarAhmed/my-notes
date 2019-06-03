# React Concepts

## Element vs Component

#### React Element
  - *describes* what you want to see on the screen,
  -  it describes a UI element
  -  Object representation of a DOM node 

In order to create our object representation of a DOM node (aka a React element), we can use React’s `createElement` method.
```js
const element = React.createElement(
  'div',
  {id: 'login-btn'},
  'Login'
);
```
`createElement` takes in three arguments. The first is a tag name string (div, span, etc), the second is any attributes you want the element to have, the third is the contents or the children of the element, in this case, the text “Login”. The createElement invocation above is going to return an object that looks like this.
```js
{
  type: 'div',
  props: {
    children: 'Login',
    id: 'login-btn'
  }
}
```

When it’s rendered to the DOM (using ReactDOM.render), we’ll have a new DOM node that looks like this,

`<div id='login-btn'>Login</div>`

#### React Component 
A component is a function or a Class which optionally accepts input and returns a React element.

```js
function Button ({ onLogin }) {
  return React.createElement(
    'div',
    {id: 'login-btn', onClick: onLogin},
    'Login'
  )
}
```

The process of replacing `React.createElement` calls with the object that it returns is called **Reconciliation** in React and it’s triggered every time `setState` or `ReactDOM.render` is called.

There’s an often un-talked about abstraction layer between JSX and what’s actually going on in React land”. This abstraction layer is that JSX is always going to get compiled to `React.createElement` invocations (typically) via Babel.
