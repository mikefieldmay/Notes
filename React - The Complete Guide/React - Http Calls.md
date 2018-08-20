## React Http calls

Typically in React the React frontend and the server will need to communicate sometimes. You rarely retreive a new html page, it is more likely you'll be receiving data in the form of Json.
In React you have to options. You can use a javascript feature (XMLHTTP), or you can use a third party library to make it easier.
We'll be using axios.

`npm install --save axios`

In React we cause side-effects in the `componentDidMount` lifecycle hook.
Basic http request to update state:
```
componentDidMount() {
        axios.get('https://jsonplaceholder.typicode.com/posts') // returns a promise
            .then(response => {
                this.setState({posts: response.data})
            });
    }

```

Basic data transformation:
```
componentDidMount() {
      axios.get('https://jsonplaceholder.typicode.com/posts') // returns a promise
          .then(response => {
              const posts = response.data.slice(0, 4);
              const updatedPosts = posts.map(post => {
                  return {
                      ...post,
                      author: 'Mike'
                  }
              })
              this.setState({posts: updatedPosts});
          });
  }
  ```

Making a request for a specific item:
```
FullPost.js
...
state = {
        loadedPost: null
    }
    componentDidUpdate() {
        if (this.props.id) {
            if (!this.state.loadedPost || this.state.loadedPost && this.state.loadedPost.id !== this.props.id) { // this checks that the post is new, and stops the component from endlessly re-rendering
                axios.get('/posts/' + this.props.id)
                .then((res) => {
                    this.setState({loadedPost: res.data})
                });
            }
        }
    }
...
```

Deleting:
```
deletePostHandler = () => {
    axios.delete('/posts/' + this.props.id)
        .then((res) => {
            console.log(res);
        })
}
```

### Interceptors
We can use interceptors in order to manipulate any outgoing requests made with axios. Furthermore we can set default
```
// Index.js
...
import axios from './axios';

axios.interceptors.request.use((request) => {
    console.log(request);
    return request;
}, error => {
    console.log(error);
    return Promise.reject(error);
})

// axios.defaults.baseURL = "https://jsonplaceholder.typicode.com";
axios.defaults.headers.common['Authorization'] = 'AUTH_TOKEN';

axios.interceptors.response.use(response => {
    return response;
}, error => {
    console.log(error);
    return Promise.reject(error);
})
...
```

Creating an instance of axios:
When we use axios, we use it globally. We can get around this by creating a specific instance of it.
We can configure this instance with specific parameters. 
```
// axios.js

import axios from 'axios';

const instance = axios.create({
    baseURL: "https://jsonplaceholder.typicode.com"
})

export default instance;

```
