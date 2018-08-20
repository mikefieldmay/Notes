## React - Error Boundary

Error boundary is a React 16 concept. The idea is to wrap a component in a higher level component that may catch any errors that are thrown.

```
// ErrorBoundary.js
import React, { Component } from 'react';

class ErrorBoundary extends Component {

    state = {
        hasError: false,
        errorMessage: ''
    }

    componentDidCatch = (error, info) => {
        this.setState({hasError: true, errorMessage: error})
    }

    render() {
        if (this.state.hasError) {
            return <h1>{this.state.errorMessage}</h1>
        } else {
            return this.props.children;
        }
        
    }
}

export default ErrorBoundary;

// App.js

return <ErrorBoundary
        key={person.id}><Person 
        name={person.name} 
        age={person.age}
        click={() => {this.deletePersonHandler(index)}}
        changed={(event) => this.nameChangedHandler(event, person.id)}
        
        /></ErrorBoundary>

```
Your code will still error whilst in development mode, but when you ship it to production if an error occurs within the Person component, the error will display but not cause the program to break. You should only use error boundaries if it makes sense. Cases where it might fail.


