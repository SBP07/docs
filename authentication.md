# Authentication

## JWT based authentication

[JSON Web Tokens](http://jwt.io/) are used for the API.

### Rationale

Read more [here](https://auth0.com/blog/2014/01/07/angularjs-authentication-with-cookies-vs-token/), but JWT's were chosen because there are no CORS problems, they are not vulnerable to CSRF attacks, statelessness, and library support.

### Requesting a token

```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"email": "toye.thomas@gmail.com", "password": "password_goes_here", "rememberMe": true}' http://localhost:9000/api/v0/auth/jwt/signIn
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Mon, 16 Nov 2015 13:33:43 GMT
Content-Length: 692

{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyLVFGT2dVZmhPSTVSU1h0RGlFWDBrMU1QVE5XdGRYMXp2RHhWaUxLSlliM0RcL3d1VjJ0RG9TYTcwMWpkckhMdmlTNTc5WU4xb2VKS1dPbStpbU82QUtnRDVTXC81QVhDeDltd09MN1JJUHB3bk9vVGc9PSIsImlzcyI6InBsYXktc2lsaG91ZXR0ZSIsImV4cCI6MTQ1MDI3MjgyMywiaWF0IjoxNDQ3NjgwODIzLCJqdGkiOiI4NDRjMDdlMmVmNmJjYjU0OGM5NzRlMTkyODBmZDgwODVlYjVmZjgyMmViMmU0YjA5NDZmNzJlODcyYWU2OTFmMTQxY2JlZGExYmU2ZmNiNTRmN2FmNjFjY2U5NTAzNDZjMzE3ZWVlZjEzOTYxYmMxOGRmMDUyNDFiNTY0YTM1NDUzOThlZjMzYzI0YzJlNzFlZGRlMzU3YjNmNzU3NThlY2Q3OGJiYzUxYzk0OGU2M2EwYWQ1YjIxZjIxZjRjM2U2ZDFiM2MzZDE1MTg2ODI0NDAzZDFhY2Y4NzdiODA5YTE4MmMyYzQyYmI1Y2JiMzYwNDIwNmZmM2YyOGIzMDc4In0.FURScIHd7S5YfsA-T-FH0yfnWV4TbGn5z4mUiFI7A_w"}
```

#### Request

Send a POST request to `/api/v0/auth/jwt/signIn` with an `application/json` body containing a JSON object that has the following structure: `{"email": "string", "password": "string", "rememberMe": boolean}`.

#### Response

Status code: 200

A JSON object with one key, called `token`, containing the JWT token.

### Requesting a token when using a wrong password

```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"email": "toye.thomas@gmail.com", "password": "wrong_password", "rememberMe": true}' http://localhost:9000/api/v0/auth/jwt/signIn 
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8
Date: Mon, 16 Nov 2015 13:49:45 GMT
Content-Length: 34

{"message":"Invalid credentials!"}
```

#### Request

Same as requesting a token, but wrong credentials.

#### Response

Status code: 401

A JSON object with one key, called `message`, containing an informative error.

### Requesting an API resource

```
$ curl -i -X GET -H "X-Auth-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyLVFGT2dVZmhPSTVSU1h0RGlFWDBrMU1QVE5XdGRYMXp2RHhWaUxLSlliM0RcL3d1VjJ0RG9TYTcwMWpkckhMdmlTNTc5WU4xb2VKS1dPbStpbU82QUtnRDVTXC81QVhDeDltd09MN1JJUHB3bk9vVGc9PSIsImlzcyI6InBsYXktc2lsaG91ZXR0ZSIsImV4cCI6MTQ1MDI3MjgyMywiaWF0IjoxNDQ3NjgwODIzLCJqdGkiOiI4NDRjMDdlMmVmNmJjYjU0OGM5NzRlMTkyODBmZDgwODVlYjVmZjgyMmViMmU0YjA5NDZmNzJlODcyYWU2OTFmMTQxY2JlZGExYmU2ZmNiNTRmN2FmNjFjY2U5NTAzNDZjMzE3ZWVlZjEzOTYxYmMxOGRmMDUyNDFiNTY0YTM1NDUzOThlZjMzYzI0YzJlNzFlZGRlMzU3YjNmNzU3NThlY2Q3OGJiYzUxYzk0OGU2M2EwYWQ1YjIxZjIxZjRjM2U2ZDFiM2MzZDE1MTg2ODI0NDAzZDFhY2Y4NzdiODA5YTE4MmMyYzQyYmI1Y2JiMzYwNDIwNmZmM2YyOGIzMDc4In0.FURScIHd7S5YfsA-T-FH0yfnWV4TbGn5z4mUiFI7A_w" http://localhost:9000/api/v0/auth/jwt/test
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyLWNpK1lnQk0wK2RFajRsZ05LREtTYWkzVU9mSFBwejNuanhRZUxweVRaQlp1R3l4R0NhcHhSSFFzRzFYNjAxMXBjUlI1M0M0RmowbzRFZlA3aG1lVlhQdlBxekhOc0xvZ2JXc1QzUklYQUw3TmRRPT0iLCJpc3MiOiJwbGF5LXNpbGhvdWV0dGUiLCJleHAiOjE0NTAyNzI4MjMsImlhdCI6MTQ0NzY4MjIwNCwianRpIjoiODQ0YzA3ZTJlZjZiY2I1NDhjOTc0ZTE5MjgwZmQ4MDg1ZWI1ZmY4MjJlYjJlNGIwOTQ2ZjcyZTg3MmFlNjkxZjE0MWNiZWRhMWJlNmZjYjU0ZjdhZjYxY2NlOTUwMzQ2YzMxN2VlZWYxMzk2MWJjMThkZjA1MjQxYjU2NGEzNTQ1Mzk4ZWYzM2MyNGMyZTcxZWRkZTM1N2IzZjc1NzU4ZWNkNzhiYmM1MWM5NDhlNjNhMGFkNWIyMWYyMWY0YzNlNmQxYjNjM2QxNTE4NjgyNDQwM2QxYWNmODc3YjgwOWExODJjMmM0MmJiNWNiYjM2MDQyMDZmZjNmMjhiMzA3OCJ9.iqPX0COFPMiAVjx_swkA6zB7HDjhDYt-MAbH4UE4oNA
Date: Mon, 16 Nov 2015 13:56:44 GMT
Content-Length: 39

{"message":"Successful authentication"}
```

#### Request

Make a GET request to a protected API endpoint (the test endpoint `/api/v0/auth/jwt/test` is used in this example), with the `X-Auth-Token` header set to the JWT token.

#### Response

Status code: 200

The body depends on the endpoint you requested.

### Requesting an API resource when not authenticated

#### Request

Same as requesting an API resource, but with a wrong or missing JWT token.

#### Response

Status code: 401

