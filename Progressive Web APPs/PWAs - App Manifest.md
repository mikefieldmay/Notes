### App Manifest

The app manifest is a single file that gives information to the browser about how to display information about the apop differently. It also allows our app to be installed on a mobile device.
If we make our app installable on the homescreen we increase user interaciton with the application as the user doesn't have to type the url in or manage book marks.
In the manifest.json:
```
{
  "name": "Sweaty - Activity Tracker", // This is the long name of the app
  "short_name": "Sweaty", // Short name of the App
  "start_url": "./index.html" // Which page to load on startup on the homescreen
  "scope": ".", // which pages are available in our PWA experience. You could limit it to a certain part of the app but that's not common
  "display": "standalone", // typical choice and doesn't show address bar
  "background_color": "#fff", // color to show when app is loading
  "theme_color": "#3f51b5", // Theme color (e.g on top bar in task switcher)
  "description": "Keep running till you're super sweaty", // Description of app
  "dir": "ltr", // Reacd direction of your app
  "lang": "en-US", // Main language of the app
  "orientation": "portrait-primary", // Set and enforce default orientation
  "icons": [{ // Configure Icons
    "src": "/src/path/to/icon", // Icon Path
    "type": "img/png", // Image Type
    "sizes": "48x48" // Icon size, browser chooses best one for given device
  }],
  "related_applications": [ // this is where we may give users the option to use related apps to open devices e.g if there is a native app for the site
    {
      "platform": "play", 
      "url": "ww.urlofsomething.com/thing",
      "id": "com.example.app1"
    }
  ]
  }
```
For Icons "48x48", "128x128" and "512x512" are important sizes.
In order for the manifest to be used we need to link it to the application. In the app.html:
`<link rel="manifest" href="/manifest.json" />`

