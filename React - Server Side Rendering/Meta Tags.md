# Meta Tags
Meta tags are a standardised way of giving information about your webpage to other websites.
`og:` relates to the open graph protocol. They give applications like facebook, google etc a chance to read some basic information about your app. They will dramatically improve your SSO. (Open Graph)[http://ogp.me/]
```
<html prefix="og: http://ogp.me/ns#">
    <head>
        <title>The Rock (1996)</title> // Title for page in tab
        <meta property="og:title" content="The Rock" />
        <meta property="og:type" content="video.movie" />
        <meta property="og:url" content="http://www.imdb.com/title/tt0117500/" />
        <meta property="og:image" content="http://ia.media-imdb.com/images/rock.jpg" />
        ...
    </head>
    ...
</html>
```

Meta tags are automatically scraped from our page by a service that is looking at it. These tags are only read on the html that is sent by the initial page request.Some services will correctly render the react app, but others will not. We need to make sure when we generate these tags on the server, we send the right tags.

## React Helmet
To do this we use React Helmet. Helmet is a way of injecting data from a React App into the `<Head>` of an application.
```
// page.js
<Helmet>
    <title>{`${this.props.users.length} Users Loaded`}</title>
    <meta property="og:title" content="Users App" />
</Helmet>

// renderer.js

const helmet = Helmet.renderStatic() // detects and objectifies any helmet tags in the jsx
        
return `
    <html>
        <head>
            ${helmet.title.toString()}
            ${helmet.meta.toString()} // will render all helmet meta tags
```

## renderToString vs renderToNodeStream

renderToString: Make API request => build entire HTML doc => Load entire doc response and sent it back
renderToNodeStream: Make API request => Build tiny snippet of HTML doc => send tiny snippet =>build tiny snippet => send tiny snippet.

renderToNodeStream sends a response very quickly to the browser. You are unable to do redirects if you are using the node stream library. As soon as you start sending the response back. It has perfomance benefits, but it prevents the redirect.
