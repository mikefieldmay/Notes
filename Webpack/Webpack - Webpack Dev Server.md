## Webpack Dev Server

When we run webpack, we spit out some files and then manually open the index.html and then loads the bundled files.
We are:
- manually running webpack
- files in same directory
- manually open index.HTML

Webpack dev server is a library that acts as an intermediary between our browser and our output. We start it once and it will automatically rebuild our project when the files change. It only updates the module that has changed. We'll also no longer be manually be loading the html. We'll access the dev server which will return the index.html.
The server is something that we don't have much access to. It's more about developing a client side application without doing any server side coding.

`npm install webpack-dev-server`

When webpack dev server sees a change it will automatically rebuild the project, but only the specific file.
Webpack-dev-server will not create the `dist` directory. It internally executes webpack, but stops any files being saved to teh directory. The files only exist in memory. It is only for development and not for production use.
