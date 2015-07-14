# graphql-sequelize

## Installation

`$ npm install --save-dev graphql-sequelize`

graphql-sequelize assumes you have graphql and sequelize installed.

## Resolve helpers

A helper for resolving graphql queries targeted at Sequelize models or associations.

```js
import resolver from 'graphql-sequelize';

let User = sequelize.define('user', {
  name: Sequelize.STRING
});

let userType = new GraphQLObjectType({
  name: 'User',
  description: 'A user',
  fields: {
    id: {
      type: new GraphQLNonNull(GraphQLInt),
      description: 'The id of the user.',
    },
    name: {
      type: GraphQLString,
      description: 'The name of the user.',
    }
  }
});

let schema = new GraphQLSchema({
  query: new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
      user: {
        type: userType,
        // args will automatically be mapped to `where`
        args: {
          id: {
            description: 'id of the user',
            type: new GraphQLNonNull(GraphQLInt)
          }
        },
        resolve: resolver(User)
      }
    }
  })
});

let schema = new GraphQLSchema({
  query: new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
      users: {
        // The resolver will use `findOne` or `findAll` depending on whether the field it's used in is a `GraphQLList` or not.
        type: new GraphQLList(userType),
        resolve: resolver(User)
      }
    }
  })
});
```