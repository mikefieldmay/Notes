### Creating a React App from nothing.

## Webpack

Webpack is a bundler that concats and optimizes files as well as hooking in plugins that allow us to compile modern javascript code as well as other types of files.

Webpack translates:
- js to the bundle.js file
- css to the bundle.css file
- pngs and jpegs to optimised pngs

Webpack always needs:
- at least one entry point (usually our app.js file). It will analyze this files dependencies starting with this root entry file. It bundles these files in to one correctly ordered concatenated output. It uses file type dependant transformations to mutate code with the assistance of loaders. We also use plugins which take the concatenated file and apply some general transformations to the bundled code. 

## Basic workflow requirements
- compile next gen javascript features
- handle jsx
- CSS auto prefixing
- Support image imports
- Optimize code