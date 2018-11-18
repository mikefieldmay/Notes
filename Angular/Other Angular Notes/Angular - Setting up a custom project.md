## Setting up a custom project

To start an Angular project, create the project folder and type the command
`npm init`.
We should then create a folder structure:
```
src
 - index.html
 - polyfills.ts
 - app
  - main.ts
  - app.module.ts
  - app-routing.module.ts
  - app.component.html
  - app.component.class
  - home.component.ts
  - users
    - users.component.html
    - users.component.ts
    - users.component.css
    - users.module.ts
    - users-routing.ts

```

## tsconfig file

```
{
    "compilerOptions": {
        "moduleResolution": "node", // how typescript will analyse the imports
        "emitMetadataDecorator": true, // we are using decorators
        "experimentalDecorators": true, // we are using decorators
        "target": "es5", // es5 to run in all browsers
        "lib": [ // determines which features it is aware of
            "es5",
            "dom"
        ]
    }
}
```

## webpack
```
webpack.config.common.js // used all the time

```
