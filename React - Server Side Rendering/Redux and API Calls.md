# Redux and SSR
## 4 Big challenges
1. Redux needs different configuration on the browser and server.
2. Aspects of the Authentication need to be handled on the server. Normally this is only in the browser. Google uses cookie based autentication, but when rendered on the server we don't have easy access to the cookie data.
3. Need some way to detect when all initial data load action creators are completed on server. The update occurs automatically in the browser, but on the server we need to know the exact instant that the request issued is complete so we can render the component to a string and return it. Biggest challenge around ssr.
4. Need state rehydration on browser.
These 4 challenges are the biggest issue around SSR in React.

### Loading data on the server side
Traditionally in a React Component, you make API calls for data this way data this way:
1. Entire App Rendered
2. Hit the `componentDidMount` lifecycle hook
3. Call the action
4. Make the API data request
5. Wait for response
6. Get data back and populate store
7. Re-render component

The problem wqith SSR is that the lifecycle methods aren't called as there isn't enough time for the component to be re-rendered. Component Did Mount lifecycle methods are never invoked on the server.

The solution to the problem is not without issues, but is the common solution to this SSR problem.
We will:
1. Figure out what components would have rendered based on URL
2. all a `loadData` method attached to each of these components
3. Wait for a response
4. Somehow detect the requests are complete
5. Render the app with the collected data
6. Send result to browser

PROS:
- Only have to render the app one time
- Makes data loading requirement of each component clear
CONS:
- requires tons of extra code. React router currently has to render to know which components to show.

The approach isn't an elegant solution which is why SSR isn't 100% recommended for every application as it requires a lot of extra work and changes the structure of a react application.

React-router-config is a sub package of React Router. It is used mainly for SSR. The bad news is we need to change how we display our routes. We no longer use the `<Route exact path='/'>` component, we instead create a route object where our routes are defined.
```
{
    path: '/',
    exact: true,
    component: Home
}
```

We will need to add a `loadData` function to the component that needs the data. This will define what data the component needs to render. In our `Routes.js` file for each path we will define the component to show and the loadData function that says what datat hte component needs.
In our `server/index.js` file we take the incoming request path and figure out what components will be shown, then use the components load data function.

### Client Side Hydration:
1. Server fetches Redux data
2. Page Rendered on Server
3. Store dumps it's state into the HTML template
4. Page Html sent to the browser
5. Client bundle.js sent to browser
6. Bundle creates it's client side redux store
7. Client side store is initialised with state that was dumped in the page
8. Page rendered with store from client side redux

### XSS Attacks

By default, React protects against xss attacks. However, by hydratin the client by dumping a load of state straight into the component, we are potentially vulnerable.