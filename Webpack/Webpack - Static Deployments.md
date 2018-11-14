## Webpack Static Deployment

By default, webpack only puts out static assets. Static assets refer to anything that isn't hosted by a server. It creates a built application that works entirely in the users browser, but doesn't make any backend calls. You can deploy your assets from a custom server. When we think about webpack deployment we consider whether we use a static (frontend only) application, or whether we need a backend.
Static hosts include: 
- Github pages
- Amazon S3
- Digital Ocean
- MS Azure
- surge

Server Based Providers:
- Amazon EC2
- Amazon ELB
- Digital Ocean
- Heroku
- MS Azure

Some are completely free. 

### Webpack Modifier
We need to add an extra plugin like so:

```
new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV) 
    }) // We use several files that use the NODE_ENV eniro variable. 
      // It is a window scoped variable. If it is set to production, React will behave differently.
      // it tells react not to do such a large amount of error checking. We make this available in the output by using the define plugin
```
This above plugin lets us set the environment when we run the build command.
`"build": "NODE_ENV=production npm run clean && webpack -p",` in the package json will build webpack for production which involves stripping ou all the unnessecary characters and creates a smaller webpack bundle. 