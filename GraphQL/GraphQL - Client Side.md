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