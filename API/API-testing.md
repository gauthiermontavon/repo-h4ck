# API testing

## 1. Identify endpoints / API Documentation
- Javascript files can contain call to API endpoints
- change method & content-type/media type
- GET / POST / PUT / PATCH / DELETE
- hidden endpoints
  - Burp intruder payload to find similiar ` PUT /api/user/update` with wordlists based on commen API naming
- hidden parameters

 ## 2. Mass assignement vulnerabilities

 ```
PATCH /api/users/
{
    "username": "wiener",
    "email": "wiener@example.com",

}
```
```
GET /api/users/123
{
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com",
    "isAdmin": "false"
}
```
 ```
PATCH /api/users/
{
    "username": "wiener",
    "email": "wiener@example.com",
    "isAdmin": "true"

}
```

## 3. Server-side parameter pollution
- browser make request `GET /userSearch?name=peter&back=/home`
- server queries internal API `GET /users/search?name=peter&publicProfile=true`
### Truncated query string - Pollution (URL-encoded# : %23):
- browser make request `GET /userSearch?name=peter%23foo&back=/home`
- server `GET /users/search?name=peter#foo&publicProfile=true`
  - if peter is returned, query truncated => publicProfil auto add by server removed. May be able to exploit to return non-public users
  - if invalid name, no truncated
 
### Injecting invalid parameters - Pollution (URL-encoded & : %26):
- browser make request `GET /userSearch?name=peter%26foo=xyz&back=/home`
- server `GET /users/search?name=peter&foo=xyz&publicProfile=true`
  - if peter is returned, additionnal parameter is parsed and pass to server
### Injecting valid parameters - Pollution
- browser make request `GET /userSearch?name=peter%26email=foo&back=/home`
- server `GET /users/search?name=peter&email=foo&publicProfile=true`
  - if peter is returned, additionnal parameter is parsed and pass to server
### Overriding existing parameters - Pollution
- browser make request `GET /userSearch?name=peter%26name=admin&back=/home`
- server `GET /users/search?name=peter&name=admin&publicProfile=true`
  - PHP parses the last only
  - ASP.net combines both ==> Invalud usernam
  - Node.js parses the first only,

 
