# Components Deep Dive

Components are the core building block, and 'the' feature of the React library.

## A better Project strucuture

What should go into it's own component, and what should go into the higher component.

```
const person = (props) => {
    return (
        <div className={classes.Person}>
            <p onClick={props.click}>I'm {props.name} and I am {props.age} years old </p>
            <p>{props.children}</p>
            <input type="text" onChange={props.changed} value={props.name}/>
            // we could potentially turn input into a component if we thought we
            // were gonna have many inputs across the site
        </div>
        )
}
```
In our App js file we do a lot of things. Typically, components that are involved with state shouldn't be involved with the UI rendering. The render method should be lean. We may want to create a PersonList or Persons component to be responsible for the rendering of the people. We wcould also outsource our cockpit to it's own component.

## Functional components vs Stateful/Container Components.

You should try to create functional components as often as possible. These components have a narrow focus and a clear responsibility. They are only for display. They don't manage state. We only want a to have a few places in the application where the state changes. In a big application, it's very difficult to find where state changes
are, if every component has access to state.
One of the fundamental React principles is to have containers which handle state and to use class components as little as possible. If you get a central point and a good split. The root component of features should be containers.

In Stateful componenents we can access state and lifecycle hooks. In stateless components we cannot access either.
Only use stateful if you need to access state.

## Component lifecycle

When React renders a component for us it runs through various lifecycles which can be accessed within a *Stateful* component.
The methods are:
Component Lifecycle - Creation
- constuctor(): Instatiates component and passes any props into a component. Call `super(props)` Do set up state, don't CauseSide-effects such as calling out to a web server.
- componentWillMount(): Exists for historic reasons. Do: Update staet, last minute optimization. Don't set up side effects.
- render():
Gives react an idea of what the component should look like. Prepare and structure your JSX code.
- IT THEN RENDERS CHILD COMPONENTS
- componentDidMount(): Do Cause Side Effects, Don't update state.

Component Lifecycle - Update (triggered by props)
- componentWillReceiveProps(): Do sync the local state of the component to the props. Don't Cause side effects
- shouldComponentUpdate(): May cancel updating process. You can return true or false to decide whether a component updates or not. Do Decide whether to continue, don't cause side effects
- componentWillUpdate(): Do sync state to props. Don't Cause Side-effects.
- render()
- UPDATES ALL CHILD COMPONENT PROPS
- componentDidUpdate(): Here you can cause side effects, but you shouldn't update state

Component Lifecycle - Update ()
- componentDidCatch():
- componentWillUnmount(): This is executed right before a component leaves the DOM

Component Lifecycle - Update (triggered by internal/state change)
- shouldComponentUpdate(): Do decide whether to continue, do not cause side effects
- componentWillUpdate(): Sync state to props, don't cause side effects
- render()
- UpdateChildComponentProps
- ComponentDidUpdate(): Do cause side effects, don't update state

## Performance gains with PureComponents

React will still call the render method even if nothing has changed in the DOM. It won't cause the page to re-render, but calling the `render()` method on a larger application can cause performance issues. We can add methods in the should update component that checks if any of the props have changed. If we want to make a shallow comparison of objects we don't have to implement `shouldComponentUpdate()` we could inherit from PureComponent instead of a regular component.
You shouldn't use pure components everywhere. You should only use them where updates may not be required. Maybe have a few at a higher level, but you may take a performance hit. Some strategically placed pure components makes sense though.

It can be a good idea to have a PureComponent as a container to components that are rendered underneath.

## Reacts DOM updating strategically
`render()` doesn't render to the DOM. It creates a suggestion of what the HTML should look like on the DOM. React compares virtual DOMs. It compares the old DOM with the re-rendered DOM. The virtual DOM is faster than the real DOM. If react does find changes it updates the virtual DOM. It only changes the DOM in places where change happens. If there are no differences it doesn't update the real DOM. Accessing the DOM is really slow.

## Returning adjacent elements

React 16 only

Our components typically have a wrapping element. You mustn't have multiple elements sit next to each other in the return method. An array of JSX elements is okay to return. If you do, each array item needs to have a key. Components that render a list are okay to be returned. If a wrapping element is redundant. We can use higher order components to return adjacent elements:
```
// Aux.js

import React from 'react';

const aux = (props) => props.children;
// this component just returns the children props. It jest returns whatever is between the opening and closing tag

export default aux;

// Cockpit.js

...
return (
        <Aux>
            <h1>Welcome to React</h1>
            <button className={btnClass} onClick={props.clicked} >Switch Name</button>
            <p className={assignedClasses.join(' ')}>
                To get started, edit <code>src/App.js</code> and save to reload.
            </p>
        </Aux>
    )
...
```
Because Aux doesn't actually contain any HTML all it does is return it's children elements. This way we con't create divs we won't need. Sometimes you don't want to create a wrapping div (unless you want to do so for styling reasons). It can also cause issues with flexbox.

If your project uses React 16.2, you can now use a built-in "Aux" component - a so called fragment.

It's actually not called Aux  but you simply use <>  - an empty JSX tag.

So the following code
```
<Aux>
    <h1>First Element</h1>
    <h1>Second Element</h1>
</Aux>
becomes

<>
    <h1>First Element</h1>
    <h1>Second Element</h1>
</>
```
Behind the scenes, it does the same our Aux  component did.

## Understanding Higher Order Components

In our previous example, we use a HOC because wrapped by another component. They may also have some minor functionality aswell:
```
// WithClass.js
import React from 'react';

const withClass = (props) => (
    <div className={props.classes}> // we can pass the class in as props
        { props.children }
    </div>
)

export default withClass;

// App.js

...
<WithClass classes={Classes.App}>
   <button onClick={() => {this.setState({showPersons: true})}}>Show Button</button>
   <Cockpit
     showPersons={this.state.showPersons}
     persons={this.state.persons}
     clicked={this.togglePersonsHandler}/>
   {persons}
 </WithClass>
...
```

There is another way we can do this too:
```
// withClass.js
import React from 'react';

const withClass = (WrappedComponent, className) => {
    return (props) => (
        <div className={className}>
            <WrappedComponent/>
        </div>
    ) // this is valid jsx code
}

export default withClass;

// App.js
...
export default withClass(App, Classes.App);
```

## Passing Unknown PROPS

```
import React, { Component } from 'react';

// const withClass = (WrappedComponent, className) => {
//     return (props) => (
//         <div className={className}>
//             <WrappedComponent {...props}/>
//         </div>
//     ) // this is valid jsx code
// } // return a basic functional component
const withClass = (WrappedComponent, className) => {
    return class WithClass extends Component {
        render() {
            return (
                <div className={className}>
                    <WrappedComponent {...this.props}/>
                </div>
            )
        }
    } // return a stateful component
}

export default withClass;
```

## Using setState correctly

You should only use state in a few selected areas. We create a copy of the state we are changing so we don't mutate the original, we then mutate the copy and assign the copy as a new state.

```
togglePersonsHandler = () => {
   const doesShow = this.state.showPersons
   this.setState({
     showPersons: !doesShow,
     toggleClicked: this.state.toggleClicked + 1 // this is not good.
   })
 }
```
Toggle state is an nchronous method called by react. It could be a case it's called in different areas of the app at the same time. If you're going to access the state inside setState you should use the following method.
```
togglePersonsHandler = () => {
    const doesShow = this.state.showPersons
    this.setState((previousState, props) => {
      return {
        showPersons: !doesShow,
        toggleClicked: this.prevState.toggleClicked + 1
      }
    }) // this is safe as previous state cannot be mutated anywhere else in the app whilst this method is being called
  }
```

## Validating props

In our example `Person.js` we receive 4 important props. We can make sure that we're being passed using prop-types.
- `npm install --save prop-types`
```
class Person extends Component {

    render() {
        return (
            <div className={classes.Person}>
                <p onClick={this.props.click}>I'm {this.props.name} and I am {this.props.age} years old </p>
                <p>{this.props.children}</p>
                <input type="text" onChange={this.props.changed} value={this.props.name}/>
            </div>
        )
    }
}

Person.propTypes = {
    click: PropTypes.func,
    name: PropTypes.string,
    age: PropTypes.number,
    change: PropTypes.func
}; // we create an object that knows what type the props need to be

export default Person;
```

It's a good way of letting other people you work with know what you need to be using, but it can only be used in components that extend class.
These are available proptypes: https://reactjs.org/docs/typechecking-with-proptypes.html

## Using References ref

Ref can be extremely valuable when focusing on text inputs. You should use refs for focus and media playbacks, not style and other things.

```
componentDidMount() {
        this.inputElement.focus()
    }

    render() {
        return (
            <div className={classes.Person}>
                <p onClick={this.props.click}>I'm {this.props.name} and I am {this.props.age} years old </p>
                <p>{this.props.children}</p>
                <input
                    ref={(inp) => {
                        this.inputElement = inp;
                    }} // only available in stateful components
                    type="text"
                    onChange={this.props.changed}
                    value={this.props.name}/>
            </div>
        )
    }
```

## More on the Ref API

In React 16.3 there are updated methods for element references.
```
lass Person extends Component {

    constructor(props) {
        super(props);
        this.inputElement = React.createRef(); // create ref creates the reference
    }

    componentDidMount() {
        this.inputElement.current.focus() // we need to use current
    }

    render() {
        return (
            <div className={classes.Person}>
                <p onClick={this.props.click}>I'm {this.props.name} and I am {this.props.age} years old </p>
                <p>{this.props.children}</p>
                <input
                    ref={this.inputElement} we pass this to the ref
                    type="text"
                    onChange={this.props.changed}
                    value={this.props.name}/>
            </div>
        )
    }
}

```

We are also able to use forwarded references. We can get a reference for a component from outside a component. it is useful for HOCs.
```
const withClass = (WrappedComponent, className) => {
    const WithClass = class extends Component {
        render() {
            return (
                <div className={className}>
                    <WrappedComponent ref={this.props.forwardedRef} {...this.props}/>
                </div>
            )
        }
    }
    return React.forwardRef((props, ref) => {
       return <WithClass {...props} forwardedRef={ref} />
    })
}


export default withClass;

```

## The Context API

A great tool for passing global state around your app. Sometimes you will have global state that you want to pass around to many components.
```
// App.js
...
export const AuthContext = React.createContext(false) // context works with providers and consumers
...
    return (
      <Aux>
          <button onClick={() => {this.setState({showPersons: true})}}>Show Button</button>
          <Cockpit
            showPersons={this.state.showPersons}
            persons={this.state.persons}
            clicked={this.togglePersonsHandler}
            login={this.loginHandler}/>
          <AuthContext.Provider value={this.state.authenticated}> // context provider
            {persons}
          </AuthContext.Provider>
      </Aux>
    ); // provides context to all child components
  }
}

export default withClass(App, Classes.App);

// Person.js

render() {
        return (
            <div className={classes.Person}>
                 <AuthContext.Consumer>
                    {auth => auth ? <p>I'm authenticated</p> : null }
                </AuthContext.Consumer> // context consumer

                <p onClick={this.props.click}>I'm {this.props.name} and I am {this.props.age} years old </p>
                <p>{this.props.children}</p>
                <input
                    ref={this.inputElement}
                    type="text"
                    onChange={this.props.changed}
                    value={this.props.name}/>
            </div>
        )
    }
```

## Updated Lifecycle hooks

React 16.3 discourages the use of componentWillMount, componentWillUpdate and componentWillReceiveProps. They are discouraged because they are often used incorrectly. Their not that useful, and they can call issues.
- `static getDerivedStateFromProps(nextProps, previousState)` This lifecycle hook is called whenever props get updated and you can then update your state. You shouldn't use this often as your state shouldn't be coupled to your props. It is a method that must be static. It is called before render and mount.
- `getSnapshotBeforeUpdate()` gives you a chance to grab access to the DOM before it updates. It executes right before componentDidUpdate is done. You can save before the DOM changes. You could save the scrolling position in this lifecycle and reinstate in `componentDidUpdate()`

State & Lifecycle: https://reactjs.org/docs/state-and-lifecycle.html
PropTypes: https://reactjs.org/docs/typechecking-with-proptypes.html
Higher Order Components: https://reactjs.org/docs/higher-order-components.html
Refs: https://reactjs.org/docs/refs-and-the-dom.html
