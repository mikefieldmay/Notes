### GraphQL
## RestfulRouting Primer

Restful routing is a given collection of records on a server, there should be a uniform URL and HTTP request method used to utilize that collection of records.

URL requests and CRUD methods form the basis of our interactions with the server.
When we start to nest data even further, things get even weirder.
When we start to gather lots of data about users it no longer becomes obvious as to the best way to store that data. We may end up with multiple collections which link data through associations.
We may end up having to fire large amounts of requests to get just basic data for a page display. 

The RESTful routing conventions tend to break down when we work with highly related data. We either end up with too many endpoints, or that we start to have many highly customised routes. We also end up with an excess of data because we may be dramatically over-serving the data returned.

GraphQL wants to solve some of the big issues around restful routing as well as the over serving of data.

## What is GraphQL

A graph is a data structure that contains nodes (such as database tables/records/documents) and their relations which are referred to as edges. When we have organised our data into a graph we can query it with GraphQL.
GraphQL we tell to look for a very particular record and then crawl through the associated data. 
```
query {
    user(id: "23") {
        users {
            company {
                name
            }
        }
    }
}
```

### Basic setup

In order to create a GraphQl app with Express we need to install express, graphql, express-graphql and lodash.
```
// server.js

// schema.js
const graphql = require('graphql');

const {
    GraphQLObjectType, // lets us create GraphQL objects
    GraphQLString,
    GraphQLInt
} = graphql

const UserType = new GraphQLObjectType({
    name: "User",
    fields: {
        id: {
            type: GraphQLString // the type of value that this field is
        },
        firstName: {
            type: GraphQLString
        },
        age: {
            type: GraphQLInt
        } 
    }
})
```