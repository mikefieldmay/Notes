# Webpack - Basics

## Why is Webpack so popular?

Rich web applications are web apps that have a lot of dynamic features about them.
`Server side templating` is a bit of a legacy application method and we depend on a backend Server that renders the html and sends it to the browser. Relies on teh server.
`Single page applications` send a barebones html doc to the user. It then requires some scripts in and then the javascript runs on the users machine to assemble the full webpage. Relies on js.

In the SPA world we ship out a huge amount of js to the users browser in order for the site to build. Because of this, we need something to handle.
If we end up having a lot of javascript code, we can run into problems. If we store all the code in a handful of files it becomes an absolute nightmare to make changes too and difficult to navigate. To address the idea of huge codebases the idea of javascript modules was born. Rather than having a small amount of huge files, we do the opposite. This can also cause problems though.
If our code has several files we need to be careful in which order the code executes. We need to have a very particular load order. The second issue is that having many js files and loading them over a http connection can be very slow. This is even more prescient on mobiles.
`The purpose of webpack is to take all our js files to bundle them into one output.`
By merging all the files together we solve the issues that were stated above. THis is it's core purpose, but it can do a ton of other stuff aswell.

## Dummy Project

Below is a dummy project. The index file is reliant on the loading of the sum file first. In javascript modules, each module has it's own scope. That's why we need to make a link between files.
Javascript has a few different rulesets for how modules behave. The differing module systems are a source of pain for the js users:
- CommonJs - Uses require and module.exports. Commonjs is implemented by NodeJs.
- AMD - Asyncronous module definition. Uses require and define. Used in asynchronous loading frontend websites.
- Es2015 - Uses import and export. With the es2015 spec this is the way that js has been defined. This is the way javascript is going.

### Installing and configuring webpack

By default, we need to tell webpack what it needs to do within out project. When we run webpack it automatically looks for webpack.cofig.js. There are 2 minimum pieces to the config that webpack needs to run. These are `entry` and `output`. Traditionally the entrypoint is known as index.js. The entry point is a relitve file reference.
Beacause nothing is dependant on index.js it is the entrypoint to the application. The entry file forms the start of the module building process. The output is another object that takes 2 further arguments path and filename. Filename is what the bundled files get called, but conventionally `bundle.js`. The path needs to be an absolute path on the file system. We can use nodejs' path which makes it easier to work out the relative path. Node makes sure the correct path is specified regardless of os.
```
// webpack.cofig.js

const path = require('path')

const config = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js'
    }
};

module.exports = config;
```

We also need to add a script to run webpack.
```
// package.json
{
  "name": "js_modules",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^2.2.0-rc.0"
  }
}

```
We add the script because if you just type webpack in the terminal it runs the global version of webpack. If we set up a script it ensures that the version of webpack ran is the webpack that's saved inside of the node_modules directory. When you install a module globally you can only have one version installed. Global modules makes it difficult to deal with version control. Locally installed versions help to deal with those sorts of dependencies.

Webpack builds often look like this in the terminal:
```
Hash: 635ba2e9515dbe2fbcbd
Version: webpack 2.2.0-rc.0
Time: 49ms
    Asset     Size  Chunks             Chunk Names
bundle.js  2.83 kB       0  [emitted]  main // the bundle size is greater than the sum of it's constituent parts.
   [0] ./src/sum.js 50 bytes {0} [built]
   [1] ./src/index.js 75 bytes {0} [built]
```

At the top of the `bundle.js` file it creates a variable called myModules. It contains two functions that wrap the code that was specified in our two modules. It specifies the entrypoint of the app as an index. It then looks in the array of modules to find the module at that index and executes it. Inside of the module it then references the `required` files as the modules place in the array.

```
// bundle.js (simplified)

var myModules = [
    function() {
        const sum = (a,b) => a + b;
        return sum;
    },
    function() {
        const sum = myModules[0]();
        const total = sum(10, 5);
        console.log(total);
    }
]

var entryPointIndex = 1;
myModules[entryPointIndex]();
```
