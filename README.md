Go graphql example
---

### Playground and test
```
go run server.go
```

The playground will be available in: http://localhost:8080

To create the skeleton like this repo, you can follow the steps (The blog post in the references was used to get this simple version):
### Init go mod
```
go mod init github.com/girorme
```

### gqlgen install & init
```
go get github.com/99designs/gqlgen
go run github.com/99designs/gqlgen init
```

### Schema definition
The commands above will generate some files, including the `graph\schema.graphqls` which contains the graphql shcema for the API.

Example
```graphql
# in the schema we will be doing Query and Mutations
schema { 
  query: Query
  mutation: Mutation
}

# These are the two queries we will be doing
type Query {
  characters: [Character!]!
  search(name: String!): Character
}

# This is a mutation we will be doing
type Mutation {
  add(character: CharacterInput!): Character!
}

# just a input type for our mutation
input CharacterInput {
  name: String!
  likes: Int!
}

# character schema
type Character {
  id: ID!
  name: String!
  likes: Int!
}
```

Delete `graph/schema.resolvers.go` and in `graph/resolver.go` add the following comment on Resolver struct:

```go
//go:generate go run github.com/99designs/gqlgen
type Resolver struct{}
```

This will auto-generate the code for our Schema that we have written in `schema.graphqls`.

Now in the `graph` folder type:
```
$ go generate
```

> tip: If any error occurs with missing depencies, you can fix with:
```
go env -w GOFLAGS=-mod=mod
```
Ref: https://github.com/golang/go/issues/44129#issuecomment-854975677

After the generate command now you can see the `models_gen.go` file with the generated structs and json tags.

Also in `schema.resolvers.go` we have the methods not implemented based on graphql schema created by us in previous steps (In this repo you'll find some implementation based on the post listed in the end of this readme) 

Reference
---
- https://levelup.gitconnected.com/graphql-server-api-in-golang-601e41408d03