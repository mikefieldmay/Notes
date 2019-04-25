# Webpack - loaders

The core purpose of web pack is to bundle all the js into one file. Anything else is a nice extra.

## Loaders
Loaders are used to allow to do preprocessing on our code. Loaders are commonly used to pre-process our js and also css and html.

### Babel Loader
Babel loader transpiles modern es code into es5 which is supported by everything.
To set babel up basically comes in 3 modules:
- babel-loader: tells babel how to work with webpack.
- babel-core: knows how to take in codem parse it, and generate some output files
- babel-preset-env: Ruleset for telling babel exactly which pieces of ES2015/6/7 syntax to look for, and how to turn it into ES5 code.
We want to make sure babel is only applied to js files in the project. Webpack 2 refers to `loaders` as `modules` and each `loader` is a `rule`.

```
// webpack.config.js
const config = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js'
    },
    module: {
        rules: [
            {
                use: 'babel-loader', // load babel
                test: /\.js$/ // apply to all js files
            }
        ]
    }
};

// .babelrc
{
    "presets": ["babel-preset-env"] // whenever babel is loaded use this
}
```

This is all you need to compile js to ES5

### CSS Loaders
We can import css files into javascript files.
In order to do so though we need to add a couple of packages to our webpack. We need:
- css-loader: knows how to deal with CSS imports
- style-loader: takes CSS imports and adds them to the HTML document

Webpack doesn't modify our html document directly. The css loader pushes the raw contents of our css into the bundle file. It takes the contents of that file, turns it into a string and then appends it to the head tag.
It is faster to load our css in a seperate file. In order to get our webpack to spit out a different file we need to install another library: `extract-text-webpack-plugin`. This takes all the text out and puts it into a different file.

```
const path = require('path');
const ExtractTextPlugin = require('extract-text-webpack-plugin')

const config = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js'
    },
    module: {
        rules: [
            {
                use: 'babel-loader',
                test: /\.js$/
            },
            {
                loader: ExtractTextPlugin.extract({
                    loader: 'css-loader'
                }), // we're making sure our css is seperated out into a seperate file.
                test: /\.css$/
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin('style.css') // anything grabbed by the loader will be exporteed to style.css
    ]
};

module.exports = config;
```

### Image loaders
We are going to add another loader that will let us add images to our webpack. The two loaders we're gonna use are:
- image-webpack-loader: is going to compress the images
- url-loader: will take the result and if it's large is will split it to a different directory. If it's small it'll be added to our bundle.js file
```
const path = require('path');
const ExtractTextPlugin = require('extract-text-webpack-plugin')

const config = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js',
        publicPath: 'build/' // URL loader emits the URL of the file with output.publicPath prepended to the url.
        // this is used by any loader that uses a file path approach
    },
    module: {
        rules: [
            {
                use: 'babel-loader',
                test: /\.js$/
            },
            {
                loader: ExtractTextPlugin.extract({
                    loader: 'css-loader'
                }), // we're making sure our css is seperated out into a seperate file.
                test: /\.css$/
            },
            {
                test: /\.(jpe?g|png|gif|svg)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: { limit: 40000 } // limit defined as 40000. If the image is larger than 40000 bytes, seperate it out
                    }, // we can add options to loaders by passing them in as objects
                    'image-webpack-loader'
                ]
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin('style.css') // anything grabbed by the loader will be exporteed to style.css
    ]
};

module.exports = config;
```
