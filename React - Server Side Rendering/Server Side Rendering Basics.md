## Server Side rendering

### Why use Server Side Rendering?

The way a traditional React App works on an initial page load. When we hit the main page we the page loads the index.html. The browser then requests the javascript file and the page loads. The React app will then boot, potentially request data from the backend. We make 3 requests before a page will even load. 
Page load is incredibly important. It has a massive effect on users. SSR helps us load a page as fast as possible for users.
With SSR we return an HTML document with a large amount of content. The content is immediately visible to the user.
Our server will receive the request, load the React app up in memory, fetch any data that is required and then generate a html document with that app. After this document is shipped we then load the js bundle into the app and then re-renders the page as a React application on the page.
