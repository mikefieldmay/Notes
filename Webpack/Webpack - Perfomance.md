## Code Splitting

Code splitting is one of the big successes of webpack. Certain pages may have more js code than others. When we go to our home page, we don't necessarily need all the javascript. When we navigate to the next page is when we need the extra files.
With webpacks default settings, everything is wrapped together into a bundle.js. We can add configuration to webpack to split this code. The initial load of the application is the most important thing when creating webapps.
When you enable codesplitting webpack changes the way it creates seperate bundles.

```
// index.js
const button = document.createElement('button')
button.innerText = 'Click me'
button.onclick = () => {
    System.import('./image_viewer')
        .then(module => {
            module.default()
        }) // this pulls in a single module of code.
    //Everything imported in image_viewer will be brought in too
}

document.body.appendChild(button)
```
All we need to do is add `import()` or `System.import(deprecated)`.

## React Router

We can use code splitting with react-routing in order to split our bundles up.
You can change you're import config in order to get lazy loading:
```
import React from 'react';
import { Router, Route, IndexRoute, hashHistory } from 'react-router';

import Home from './components/Home';
import ArtistMain from './components/artists/ArtistMain';
import ArtistDetail from './components/artists/ArtistDetail';
import ArtistCreate from './components/artists/ArtistCreate';
import ArtistEdit from './components/artists/ArtistEdit';

const Routes = () => {
  return (
    <Router history={hashHistory}>
      <Route path="/" component={Home}>
        <IndexRoute component={ArtistMain} />
        <Route path="artists/new" component={ArtistCreate} />
        <Route path="artists/:id" component={ArtistDetail} />
        <Route path="artists/:id/edit" component={ArtistEdit} />
      </Route>
    </Router>
  );
};

export default Routes;

// Can be refactored to:

import React from 'react';
import { Router, Route, IndexRoute, hashHistory } from 'react-router';

import Home from './components/Home';
import ArtistMain from './components/artists/ArtistMain';

const componentRoutes = {
  component: Home,
  path: '/',
  indexRoute: { component: ArtistMain },
  childRoutes: [
    {
      path: '/artists/new',
      getComponent(location, cb) {
        System.import('./components/artists/ArtistCreate')
          .then(module => cb(null, module.default));
      }
    },
    {
      path: '/artists/:id',
      getComponent(location, cb) {
        System.import('./components/artists/ArtistEdit')
          .then(module => cb(null, module.default));
      }
    },
    {
      path: '/artists/:id/edit',
      getComponent(location, cb) {
        System.import('./components/artists/ArtistDetail')
          .then(module => cb(null, module.default));
      }
    }
  ]
}

const Routes = () => {
  return (
    <Router history={hashHistory} routes={componentRoutes}/>
  );
};

export default Routes;
```

https://staging.api.kingfisher.com/qa/v1/hip-customer/visualization/guest?vendor=home-profile

https://staging.api.kingfisher.com/qa/v1/hip-customer/visualisation/guest?vendor=home-profile


https://staging.api.kingfisher.com/qa/v1/hip-customer/visualisation/guest
