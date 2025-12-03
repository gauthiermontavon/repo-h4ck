# GraphQL API vulnerabilities

## Finding endpoints
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

