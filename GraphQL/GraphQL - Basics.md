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

### Connecting different Objects

As in all databases, we may want to have tables that are connected to each other.
```
// database.json

{
    "users": [
        { "id": "23", "firstName": "Samantha", "age": 21, "companyId": "1" },
        { "id": "29", "firstName": "Tomos", "age": 28, "companyId": "2" },
        { "id": "30", "firstName": "Mike", "age": 30, "companyId": "1"  }

    ],

    "companies": [
        { "id": "1", "name": "Apple", "description": "iphone" },
        { "id": "2", "name": "Google", "description": "iphone" }
    ]
}
```

In the above eample the company id refers to the id of the company (duh).
In graphQL we can link these ecamples where we declare the Object Types.

```
const CompanyType = new GraphQLObjectType({
    name: "Company",
    fields: () => ({ // wrapping this in a function gets around the problem of creating circular dependencies
        id: { type: GraphQLString },
        name: { type: GraphQLString },
        description: { type: GraphQLString },
        users: { 
            type: new GraphQLList(UserType),
            resolve: (parentValue, args) => {
                return axios.get(`http://localhost:3000/companies/${parentValue.id}/users`) // parentValue allows us to acess data from the record
                .then(res => res.data) // axios returns our data in an object with a data property. graphQL doesn't care about that so we return the data it actually wants
            }
        }
    })
})

const UserType = new GraphQLObjectType({
    name: "User",
    fields: () => ({
        id: { type: GraphQLString },
        firstName: { type: GraphQLString },
        age: { type: GraphQLInt },
        company: { 
            type: CompanyType,
            resolve: (parentValue, args) => {
                return axios.get(`http://localhost:3000/companies/${parentValue.companyId}`)
                    .then(res => res.data)
            }
        },
    })
}) // pur user type defines the structure of the record that exists in our database

const RootQuery = new GraphQLObjectType({ // our rootQuery iis what points us to a very particular record in our graph
    name: "RootQueryType",
    fields: {
        user: {
            type: UserType,
            args: {id: {type: GraphQLString}}, // the root query expects to receive an id as an argument. That ID is a string
            resolve: (parentValue, args) => {
                return axios.get(`http://localhost:3000/users/${args.id}`)
                    .then(resp => resp.data)
            } // resolve is the function that actually returns the data. We don't really care about the 
            // parentValue, we normally just care about the args arguments.
        },
        company: {
            type: CompanyType,
            args: {id: {type: GraphQLString}},
            resolve: (parentValue, args) => {
                return axios.get(`http://localhost:3000/companies/${args.id}`)
                    .then(resp => resp.data)
            }
        }
    }
})

module.exports = new GraphQLSchema({
    query: RootQuery
})

```

### Mutations

The bread and butter of database operations is CRUD and in graphQL we refer to the changing of database data as mutations.
Mutations can be quite challenging to work with. Our RootQuery is what allows us to read from our database. We will create a Mutations object type.
```
const mutation = new GraphQLObjectType({
    name: "Mutation",
    fields: {
        addUser: {
            type: UserType,
            args: {
                firstName: { type: new GraphQLNonNull(GraphQLString) }, // is a basic required value helper, but does not provide any validation
                age: { type: new GraphQLNonNull(GraphQLInt) },
                companyId: { type: GraphQLString}
            },
            resolve: (parentValue, {firstName, age}) => {
                return axios.post('http://localhost:3000/users/', {
                    firstName,
                    age
                })
                .then(res => res.data)
            }
        }
    }

})

// Calling it in the graphiQL Interface:

mutation {
  addUser(firstName: "mike", age: 13) {
    id,
    firstName,
    age
  } // we need to request data back from the server.
}

```