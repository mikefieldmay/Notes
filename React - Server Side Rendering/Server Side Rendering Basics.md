## Server Side rendering

### Why use Server Side Rendering?

The way a traditional React App works on an initial page load. When we hit the main page we the page loads the index.html. The browser then requests the javascript file and the page loads. The React app will then boot, potentially request data from the backend. We make 3 requests before a page will even load. 
Page load is incredibly important. It has a massive effect on users. SSR helps us load a page as fast as possible for users.
With SSR we return an HTML document with a large amount of content. The content is immediately visible to the user.
Our server will receive the request, load the React app up in memory, fetch any data that is required and then generate a html document with that app. After this document is shipped we then load the js bundle into the app and then re-renders the page as a React application on the page.


### Project Structure

When creating the project structure it is important to seperate the business logic and data layer from the SSR View layer. By splitting our backend into two seperate tiers we also will havbe a much easier time scaling our application in the future. If we keep the rendering server in one location then it's easy to scale out the box. SSR is not perfect. Running React on the server is slow. Because it is slow, if we seperate our rendering server, we can then concentrate on upping hte rendering servers.

### Starting a project

The two main problems we need to initially solve are:
1. Running JSX on the server: We run webpack on all our server side code and then bundle it and run the bundle as our server.
2. Need to turn components into HTML: use the `react-dom/server` packages' `renderToString` function to render the react to html on the request.

### SSR vs Universal Javascript vs Isomorphic Javascript

Server Side rendering refers to HTML being generated on the server. With Universal and Isomorphic javascript,some of the code executed on the server runs on the browser.

We never want to pull any server side code into the browser. To do this we split our app into 2 bundles. We have a server bundle (so that we can render react html on the server side) and the client bundle (that contains the logic for the react spa app). `client.js` is only intended to run on the client side and `index.js` is only ever called on the server. 

### Steps in an SSR application

1. App rendered in the servier into some div in the template.
2. Rendered App sent to the users Browser.
3. Browser renders HTML file on screen and then loads the client bundle.
4. Client bundle boots up. From this point on everything works as a regular React app.
5. We manually render the React app a second time into the 'same' div.
6. React renders our app on the client side and compares the new HTML to what already exists in the document.
7. React 'takes over' the existing rendered app, binds event handlers etc.

### Routing

There will be 2 tiers of Routing in the application. Tier 1 is the express Route handler. The second tier is React Router.
Express will delegate 100% of requests to React Router. For anything that displays HTML we will make sure that React Router is in charge of what displays on screen. 
In a normal React App:
1. Browser requests '/users'
2. Express handler of `app.get(*)` responds
3. Express sends down `index.html`
4. Express sends down bundle.js
5. React boots up, React Router boots up
6. Browser Router looks at the url in the address bar and renders some route

Browser Router only looks at the address bar which could be an issue as when we render on the server we don't have an address bar. In order to fix this, we will use the Static Router for rendering routes on the server side.
Our initially render will use the Static Router and when the React app boots up we'll use the Browser Router.
The ssr and the html need to have an identical structure or React will throw warnings at you.
