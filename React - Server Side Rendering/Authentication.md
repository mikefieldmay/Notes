# Authentication

Authentication is a big problem with Server Side rendering. 
Authentication can traditionally be thought of as a contract between a browser and an api. The browser sends a token (jwt/cookie). Browser => Server => Api. Our server will need knowledge of who is trying to access the api and it then needs to fool the API to think that it's coming from the browser. Cookies are automatically attached to each HTTP request by the browser. When we get a cookie set from our our app, if we try and access a different domain, we no longer pass the cookie.

## Using a Proxy

We are going to set a proxy on the server. Rather than sending a request directly from the browser to our API, we will set a proxy up on the renderer server to handle requests from the browser. The proxy will then communicate that proxy back to the server. The browser will think that the cookie will have been issued by the Render server and as such it will attach the cookie to it. The render server will then manually add the cookie to it's api requests to the server. 