## Routing

React itself is only a component framework library. In order to turn it into a fuller framework we use a third party package.
Routing is about being able to show different pages to a user. The idea behind a SPA is to have a single HTML file. We still use a single page, but we use javascript to render different views depending on the path of the url. For that we are going to use a router package. The package has to parse the URL and we also need to configure different paths which the package can read. When we get to the path we should then render the correct JSX code. 

## React-Router

The package we need to install is React-Router-DOM in order to render to the DOM. 
`npm install --save react-router-dom`.
In the index.js or the app.js file we need to wrap our app in a component we get from the react-router-dom package.

```
// App.js
class App extends Component {
  render() {
    return (
      <BrowserRouter>
        <div className="App">
          <Blog />
        </div>
      </BrowserRouter>      
    );
  }
}
```

In our main container comoonent we import the `Route` component from react-router-dom. This is the component that will render the routes.
```
class Blog extends Component {
    render () {
        return (
            <div className="Blog">
                <header>
                    <nav>
                        <ul>
                            <li><Link to="/">Home</Link></li>
                            <li><Link to="/new-post">New Post</Link></li>
                        </ul>
                    </nav>
                </header>
                <Route path="/" exact component={Posts}/> // exact means that the route has to be an exact match. it defaults to false. 
            </div>
        );
    }
}
```

You can access information from the Route in the rendered components props. 
Futhermore, children of a rendered component can know about the route using the withRoute hoc.
```
import React from 'react';
import { withRouter } from 'react-router-dom'

import './Post.css';

const post = (props) => (
    <article className="Post" onClick={props.clicked}>
        <h1>{props.title}</h1>
        <div className="Info">
            <div className="Author">{props.author}</div>
        </div>
    </article>
);

export default withRouter(post); // adds router props to other components. Returns info on the nearest loaded route.
```
The way you right your `<Link to="/">` you can't control whether it's a relative or absolute path. It is always an absolute path.
you can create relative paths by using the below syntax:
```
<Link to={{
    pathname: this.props.match + '/new-post'
}}>
```

You learned about <Link> , you learned about the to  property it uses.

The path you can use in to can be either absolute or relative. 

## Absolute Paths
By default, if you just enter to="/some-path"  or to="some-path" , that's an absolute path. 

Absolute path means that it's always appended right after your domain. Therefore, both syntaxes (with and without leading slash) lead to example.com/some-path .

## Relative Paths
Sometimes, you might want to create a relative path instead. This is especially useful, if your component is already loaded given a specific path (e.g. posts ) and you then want to append something to that existing path (so that you, for example, get /posts/new ).

If you're on a component loaded via /posts , to="new"  would lead to example.com/new , NOT example.com/posts/new . 

To change this behavior, you have to find out which path you're on and add the new fragment to that existing path. You can do that with the url  property of props.match :

<Link to={props.match.url + '/new'}>  will lead to example.com/posts/new  when placing this link in a component loaded on /posts . If you'd use the same <Link>  in a component loaded via /all-posts , the link would point to /all-posts/new .

There's no better or worse way of creating Link paths - choose the one you need. Sometimes, you want to ensure that you always load the same path, no matter on which path you already are => Use absolute paths in this scenario.

Use relative paths if you want to navigate relative to your existing path.

## NavLink

NavLink allows you to add active classes to links. 
```
<NavLink 
    to="/" 
    exact
    activeClassName="active">Home</NavLink> // choose which classname should apply to active class (active by default);
```

## Parsing Query Parameters

You learned how to extract route parameters (=> :id  etc). 

But how do you extract search (also referred to as "query") parameters (=> ?something=somevalue  at the end of the URL)? How do you extract the fragment (=> #something  at the end of the URL)?

Query Params:
You can pass them easily like this:

<Link to="/my-path?start=5">Go to Start</Link> 

or
```
<Link 
    to={‌{
        pathname: '/my-path',
        search: '?start=5'
    }}
    >Go to Start</Link>
```
React router makes it easy to get access to the search string: props.location.search .

But that will only give you something like ?start=5 

You probably want to get the key-value pair, without the ?  and the = . Here's a snippet which allows you to easily extract that information:
```
componentDidMount() {
    const query = new URLSearchParams(this.props.location.search);
    for (let param of query.entries()) {
        console.log(param); // yields ['start', '5']
    }
}
```
URLSearchParams  is a built-in object, shipping with vanilla JavaScript. It returns an object, which exposes the entries()  method. entries()  returns an Iterator - basically a construct which can be used in a for...of...  loop (as shown above).

When looping through query.entries() , you get arrays where the first element is the key name (e.g. start ) and the second element is the assigned value (e.g. 5 ).

Fragment:
You can pass it easily like this:
```
<Link to="/my-path#start-position">Go to Start</Link> 

or

<Link 
    to={‌{
        pathname: '/my-path',
        hash: 'start-position'
    }}
    >Go to Start</Link>
```
React router makes it easy to extract the fragment. You can simply access props.location.hash .

## Navigating programatically

`props` of the router have access to history oobject, which has methods that allow you to navigate around the browsers stack. 
It has forward, backward and push methods which allow you to navigate. You can push routes to the top of the stack to navigate to new pages.

`this.props.history.push({pathname: `/${id}`})`

Push pushes the page onto the stack, but redirect replaces the place on the stack so back will not return you to the previous page.

## Navigation Guards

A typical guard is used when you may not know if a user is authenticated or not and y9ou want to prevent a user from accessing the page. Because everything is javascript you can do something fairly simple like:

```
{ this.state.auth ? <Route path="/new-post" component={NewPost} /> : null}
```

The Redirect component will catch the route if nothing is rendered. You could also add this check in the component will mount lifecycle hook of the conditionally rendered component. 

## Catching unknown routes
```
<Switch>
    { this.state.auth ? <Route path="/new-post" component={NewPost} /> : null}
    <Route path="/posts" component={Posts} />
    <Redirect from="/" to="/posts" />
</Switch>
```
Redirect will catch unknown requests. 

An alternative is to create a component without a path that catches any paths that don't have components:
`<Route render={() => <h1>Not found</h1>}`

## Lazy Loading routes

Lazy loading or code splitting are implemented in React using React Router 4 adn the webpack config. 
It should work in any other decently set up webpack project. 

```
// AsyncComponent.js

import React, { Component } from 'react';

const asyncComponent = (importComponent) => {
    return class extends Component {
        state = {
            component = null
        }

        componentDidMount() {
            importComponent() // promise that resolves to the default export of the imported module
                .then(cmp => {
                    this.setState({component: cmp.default})
                });
        }

        render () {
            const c = this.state.component
            return C ? <C {...this.props} /> : null;
        }
    }
}

// Blog.js

const asyncNewPosts = asyncComponent(() => import('../NewPost/NewPost')) // we dynamically import the contents of new post whihch renders when the import() promise resolves.

```

## Routing and the Server

When a user sends a request to the server, the server renders the index.html page. The issue is, it's the react app that handles the routes which aren't even rendered yet. We need to configure the server ina  way that always forwards requests (even for unknown requests). This is set up in the development server of a create react project. You also need to set the base path for the app router. You can configure that in the BrowserRouter component:
```
<BrowserRouter pathname='/my-app'>
    <div className="App">
        <Blog />
    </div>
    </BrowserRouter>  
```
If you're serving an app from a subdirectory, make sure you setup basename.

React Router Docs: https://reacttraining.com/react-router/web/guides/philosophy