# React - The complete guide

## What, Why and How?

React is a javascript library for building user interfaces. React apps run in the browser. Things happen instantly because we don't have to wait for a server response. 
The user interface is built up of React components. Splitting our website into componentsallow us to build each component as contained pieces of code. We can also easily re-use components. React components can be thought of as custom html elements. 
React creates components, React DOM renders these components to the real DOM.

```
function Person(props) {
  return (
    <div className="person">
      <h1>{props.name}</h1>
      <p>Your Age: {props.age}</p>
    </div>
  ); // this syntax is jsx, not html, and allows us to write html in javascript
}

var app = (
  <div>
    <Person name="Max" age="28" />
    <Person name="Mike" age="27" />
  </div>
)

ReactDOM.render(app, document.querySelector('#app')) // lets us render a javascript function to the real DOM. 
```

UI State becomes difficult to manage with Vanilla javascript in bigger applications. 
It allows us to focus on business logic. It is maintained by a big community, Framework Creators probably write better code than we could. It has a huge ecosystem, active community and high performance. 

## Two Kinds of applications

Single page applications:
- Only ONE HTML Page, Content is re-rendered on the client. 
- Every component is a react component and is managed by a root react component.
- Typically only ONE `ReactDOM.render()` call.

Multiple Page Applications:
- Multiple HTML Pages, Content is rendered on the Server. 
- Most Pages are just plain html and css, although we may dump in widgets. 
- One `ReactDOM.render()` call per widget

# React core concepts

## Using a build workflow

We need a fairly complex build workflow for in order to:
- Optimize our code by shipping code that's as small as possible.
- Use Next-Gen javascript features that makes code cleaer, easier to read, less error prone and faster.
- Be more productive (using css prefixes across browsers, linting)

To this we  need:
- A dependancy management tool `npm` or `yarn`. Dependencies are third-party librarieswe use in our apps. 
- We need a bundler (Webpack) in order to write modular code, but that is compiled into a single file. 
- Use compiler that can change Next-gen javascript into standard javascript that is usable by all browsers. We can tie it in with Webpack.
- Use a development server to test files locally. 

## Create React App

Create React App is the recommended way of creating React apps favoured by the React community. To installCreate React APP we run:
`npm install - g create-react-app`.
`create-react-app name-of-app` Creates a new react app and installs all the dependencies it needs. 

## Component basics

Components are essentially custom html elements. 
Typically you have one root component, and react renders all the other components from within that main component. 
```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();
```

We can create components by creating classes that extend from Component.
```
import React, { Component } from 'react';
import './App.css';

class App extends Component { // everything extends from component
  render() { 
    return (
      <div className="App">
        <header className="App-header">
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    ); // all react components need a render method to render the html to the screen.
  }
}

export default App;
```
N.B you always need to render the html (or jsx as it actually is) to the DOM. 

You can also create jsx in the following way:

```
import React, { Component } from 'react';
import './App.css';

class App extends Component { // everything extends from component
  render() { 
    return React.createElement('div', null, 'h1', "Hi I'm a react app") The last two arguments are strings. If we want to pass in a new element we call creatElement in a createElement call.
    // we can pass in class names in the settings as an object instead of null. 
    // This method takes element, then settings and then children
  }
}

export default App;
```
We always need to import React as the `React.createElement()` method is what actually compiles the code.

## JSX restrictions

Because jsx isn't html, just javascript oit has certain limitations. 
```
class App extends Component {
  render() {
    return (
      <div className="App"> 
        <header className="App-header">
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}
```
You can't use class as it is a restricted name in javascript. React uses className instead.
All jsx returned from the `render` method must contain one root element. In React 16 you can return adjacent elements, but it is best practice to nest everything in one root element.

## Creating a component
In React it is convention to create a folder to contain your component, and these are given a capital letter. 
In it's simplest form, a component is just a function that returns jsx.
```
// Person.js
import React from 'react';

const person = () => { // a function tends to be lowercase whilst a class is uppercase
    return <p>I'm a person</p>
}

export default person;

// App.js

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
        <Person />
      </div>
    );
  }
}

export default App;
```
The advantages of creating components is that we can focus on small seperate pieces of code that become e to maintain, re-use and configure.

### Outputting dynamic content in components

We can output dynamic content in jsx.
Values are passed in in `{}`:
```
import React from 'react';

const person = () => { 
    return <p>I'm a person and I am {Math.floor(Math.random() * 30)} years old </p>
}

export default person;
```
We can pass variables into a React component with props (properties).
```
// App.js
...
<Person name="Max" age="28"/>
<Person name="Mike" age="27">My Hobbies: Eating</Person>
<Person name="Jeff" age="20"/>
...

// Person.js

const person = (props) => { // an argument is always passed into a function that renders html. React calls it props by convention
    return <p>I'm {props.name} and I am {props.age} years old </p>
}
```

We can also access content between the opening and closing of the tag by using the children keyword:
```
const person = (props) => { // a function tends to be lowercase whilst a class is uppercase
    return (
        <div>
            <p>I'm {props.name} and I am {props.age} years old </p>
            <p>{props.children}</p> // paragraph is still rendered on all components, but is empty
        </div>)
}
```

### Props and state
`props`  and `state`  are CORE concepts of React. Actually, only changes in props  and/ or state  trigger React to re-render your components and potentially update the DOM in the browser (a detailed look at how React checks whether to really touch the real DOM is provided in section 6).

#### Props

`props`  allow you to pass data from a parent (wrapping) component to a child (embedded) component.

Example:

```
AllPosts Component:

const posts = () => {
    return (
        <div>
            <Post title="My first Post" />
        </div>
    );
}
```
Here, title  is the custom property (prop ) set up on the custom Post  component. We basically replicate the default HTML attribute behavior we already know (e.g. <input type="text">  informs the browser about how to handle that input).
```
Post Component:

const post = (props) => {
    return (
        <div>
            <h1>{props.title}</h1>
        </div>
    );
}
```
The Post  component receives the props  argument. You can of course name this argument whatever you want - it's your function definition, React doesn't care! But React will pass one argument to your component function => An object, which contains all properties you set up on <Post ... /> .

{props.title}  then dynamically outputs the title  property of the props  object - which is available since we set the title  property inside AllPosts  component (see above).

#### State

Whilst props allow you to pass data down the component tree (and hence trigger an UI update), state is used to change the component, well, state from within. Changes to state also trigger an UI update.

Example:
```

NewPost Component:

class NewPost extends Component { // state can only be accessed in class-based components!
    state = {
        counter: 1
    };  
 
    render () { // Needs to be implemented in class-based components! Needs to return some JSX!
        return (
            <div>{this.state.counter}</div>
        );
    }
}
```
Here, the NewPost  component contains state . Only class-based components can define and use state . You can of course pass the state  down to functional components, but these then can't directly edit it.

state  simply is a property of the component class, you have to call it state  though - the name is not optional. You can then access it via this.state  in your class JSX code (which you return in the required render()  method).

Whenever state  changes (taught over the next lectures), the component will re-render and reflect the new state. The difference to props  is, that this happens within one and the same component - you don't receive new data (props ) from outside!

## Binding to events

We can bind to DOM events which allow us to execute functions:
Here is the full list of available bindings: https://reactjs.org/docs/events.html#supported-events
```
...

 switchNameHandler = () => {
    console.log("I got clicked baby!!")
  } // this maintains the this keyword when transpiled into es5
    

  render() {
    return (
      <div className="App">
        <header className="App-header">
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <button onClick={this.switchNameHandler} >Switch Name</button>  // don't add () when adding it to the code or it will execute when rendered
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
...
```

## Changing state
We should not mutate state directly e.g:
```
 switchNameHandler = () => {
    this.state.persons[0].name = "Maximillian";
  } 
```

We should use the setState method that is defined in the Component. 
```
switchNameHandler = () => {
    this.setState({persons: [
      { name: "Maximillion", age: 28 },
      { name: "Mike", age: 27 },
      { name: "Jeff", age: 29 }
    ]})
  } 
```
Set state merges the new state over the top of the old state.
React watches out for changes in state and props and then updates the existing DOM in all areas it needs to. 

## Functional (stateless) vs class (stateful) components
We should use the functional form of components as often as possible. Simple components are very clear about what they do. They do not manipulate your application state. This is really important. Most parts of your application shouldn't change your application state. State should only really be handled in specific components known as containers. There's usually a few components that are stateful, but mostly they are stateless. 

## Passing method references between components

If we want to call a method that changes state in a stateless component we need to pass method references. 
You can pass a method as props. 
```
// app.js

switchNameHandler = (newName) => {
    this.setState({persons: [
      { name: newName, age: 28 },
      { name: "Mike", age: 27 },
      { name: "Jeff", age: 29 }
    ]})
  } 
  nameChangedHandler = (event) => {
    this.setState({persons: [
      { name: "Max", age: 28 },
      { name: event.target.value, age: 27 },
      { name: "Jeff", age: 29 }
    ]})
  }
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <button onClick={() => this.switchNameHandler("Maximillion")} >Switch Name</button>  // If you want to pass an argument you can pass an anonymous function that returns a function being clicked
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
        <Person 
          name={this.state.persons[0].name} 
          age={this.state.persons[0].age}
          click={this.switchNameHandler.bind(this, "Max!")} 
          changed={this.nameChangedHandler}/> // you could also bind this and then the value you're passing in.
        <Person name={this.state.persons[1].name} age={this.state.persons[1].age}>My Hobbies: Eating</Person>
        <Person name={this.state.persons[2].name} age={this.state.persons[2].age}/>
      </div>
    );
  }
}
// Bind is the more common use

// Person.js

...
const person = (props) => { // a function tends to be lowercase whilst a class is uppercase
    return (
        <div onClick={props.click}>
            <p>I'm {props.name} and I am {props.age} years old </p>
            <p>{props.children}</p>
            <input type="text" onChange={props.changed} value={props.name}/> // TWO WAY DATA BINDING!
        </div>)
}
...
```

## Adding styling

Css classes in React are global by default. By default no files are included in the build script so css files need imported into files. Webpack allows us to import css into javascript. They are dynamically injected into the html at run time and also adds automatically pre-fix it to work in as many browsers as possible.s
`import './Person.css';`

### Inline styling
You can use inline styling aswell, but this means you don't leverage the full css features.

```
const style = {
      backgroundColor:' white',
      font: 'inherit',
      border: '1px solid blue',
      padding: '8px',
      cursor: 'pointer'
    }
    return (
      <div className="App">
        <header className="App-header">
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <button style={style} onClick={() => this.switchNameHandler("Maximillion")} >Switch Name</button> 
        <p className="App-intro">
```