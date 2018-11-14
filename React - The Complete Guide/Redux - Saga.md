# Redux Saga

Redux Saga is a package that helps us deal with asynchronous code in our applications.
`npm install --save redux-saga`.

We use redux to deal with side-effects that don't directly affect the redux store.
```
// saga/auth.js

import { put } from 'redux-saga/effects'; // put dispatches an action.

import * as actionTypes from '../actions/actionTypes';

export function * logout(action) {
    yield localStorage.removeItem('token'); // yield essentially says execute this function, but wait for it to finish
    yield localStorage.removeItem('expirationDate');
    yield localStorage.removeItem('userId');
    yield put(
        {type: actionTypes.AUTH_LOGOUT}
    );
} // a star after the function turns it into a generator. It is a function that can be paused and executed incrementally. 
//This can be used to wait for async code to finish. 

```