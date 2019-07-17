# GraphQl Client Side

GraphQL is an emerging technology and as such there are many new and emerging ways of interacting with GraphQL in the front-end. In GraphQL we send a request to the server that is an object with the following shape:
```
{
    operationName: null,
    query: "",
    variables: null
}
```
Our graphQL client needs to be a bonding layer between react and our GraphQL server. 

The 3 main clients that are used are: 
1. Lokka - As simple as possible. Basic queries, mutations, some simple caching.
2. Apollo Client - Produced by the same guys as Meteor.js. Good balance between features and complexity.
3. Relay - Amazingly performant for mobile. By far the most insanely complex.

We will stick with express-graphql as a server as it's structure follows how facebook believes graphQl should be written on the server. Apollo Server has it's own implementation and it is prone to having large changes that introduce big changes.

### Getting data
Apollo client uses a very similar structure to Redux when added to an application:
```
// 
// In App.js

import React from 'react';
import ReactDOM from 'react-dom';
import ApolloClient from 'apollo-client'; // Apollo client is agnostic to what frontend uses it
import { ApolloProvider } from 'react-apollo'; // apollo provider creas a bridge to react from React to Apollo Client
import { SongList } from './components/SongList'

const client = new ApolloClient({}); // assumes that the graphql server is on the graphql route

const Root = () => {
  return (
    <ApolloProvider client={client}>
      <SongList />
    </ApolloProvider>
  )
};

ReactDOM.render(
  <Root />,
  document.querySelector('#root')
);

// In Songlist.js

import React, { Component } from 'react';
import gql from 'graphql-tag';
import { graphql } from 'react-apollo';

class SongListBase extends Component {
    render() {
        return (
            <div>
                Song List
            </div>
        );
    }
}

const query = gql`
    {
        songs {
            title
        }
    }
` // defines the query, not executes it

export const SongList = graphql(query)(SongListBase)

```

### Mutating data

In order to pass dynamic data to a query we need to use a GraphQL concept called Query Variables:
```
    mutation MutationName($title: String) {
        definedMutation(title: $title) {
            id
            title
        }
    }
```
Query Variables can be passed in by turning the mutation into a sort of function. We define the variable name and then type it. When we've defined this variable we can access it within the appliction. 