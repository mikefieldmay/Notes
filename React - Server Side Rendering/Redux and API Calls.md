# Redux and SSR
## 4 Big challenges
1. Redux needs different configuration on the browser and server.
2. Aspects of the Authentication need to be handled on the server. Normally this is only in the browser. Google uses cookie based autentication, but when rendered on the server we don't have easy access to the cookie data.
3. Need some way to detect when all initial data load action creators are completed on server. The update occurs automatically in the browser, but on the server we need to know the exact instant that the request issued is complete so we can render the component to a string and return it. Biggest challenge around ssr.
4. Need state rehydration on browser.
These 4 challenges are the biggest issue around SSR in React.