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
$ curl -i -H "Content-Type: application/json" -X POST -d '{"email": "hello@example.com", "password": "wrong_password", "rememberMe": true}' http://localhost:9000/api/v0/auth/jwt/signIn
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8
Date: Tue, 08 Dec 2015 11:44:39 GMT
Content-Length: 51

{"message":"Invalid credentials!","status":"error"}
```

#### Request

Same as requesting a token, but wrong credentials.

#### Response

Status code: 401

A JSON object with two keys, one called `message`, containing an informative error, the other `status`, with value `error`.

### Requesting an API resource

```
$ curl -i -X GET -H "X-Auth-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyLWJ2Sm9NRGtXRDhDNWlMTEdzYlpxdUhiTnR0MFJzVEFSZE4wOGZGdVRTZUtzNTNPV0I3R09Pd2tqa2RlK01YQzhMeEZ2UTFRYXBUV1VkTm83ZXo3NVBJNnNmdzVoZ2pxekJFR2xtSjI4IiwiaXNzIjoicGxheS1zaWxob3VldHRlIiwiZXhwIjoxNDUyMTY3MTc2LCJpYXQiOjE0NDk1NzUxNzYsImp0aSI6IjI0NGE4M2NhZThjZTUyOTVmYTJjZGZiOThlMWQ0MzljMTZlNDJiZTJiZGJmNDBiYjU3OWRhMjVjYjg3ZDI5NGRkZWY0NTk3YTc1MjIwMDJiYzY4NmQ0OGViZDUyMDA5OTYxNDZkN2M0MGEzMzkxMjEzMjlkNmJkNmU1ZmJmM2MwNWNmM2QyZDJlNDYyODMyYjkxMjdkNmRhNDk5YjBjNmVkMjhiYjg1NjdlMDVhNGVkNmNkZTg1N2YwNDU2NDY0MWQ4N2E3NDhhYzJiOTIzMGJjNDU1ZDM3NDc1YTZlODZmZjRhN2ViOWE2ODQwOTdhMmE2MDUwZTBmNGU3NDNiM2YifQ._SYsIKN_bdqLC8IQQcw0_2nqGQ0vZWnIol1o5j7J2cg" http://localhost:9000/api/v0/auth/jwt/test
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyLVdjN0VweTFkNzdxMlBSSWdjWjZsMDBPVWsrZGRXQnlFSXI2STNPMmVqQWIwMEl6R2xCV2tYWEF0bEdlcGRyU1MxSVB4SkxFQlNxakxsU1RMSmk1N0l6OTFyOTF4eGR0ZzJNcTJQdWtFIiwiaXNzIjoicGxheS1zaWxob3VldHRlIiwiZXhwIjoxNDUyMTY3MTc2LCJpYXQiOjE0NDk1NzUyMDgsImp0aSI6IjI0NGE4M2NhZThjZTUyOTVmYTJjZGZiOThlMWQ0MzljMTZlNDJiZTJiZGJmNDBiYjU3OWRhMjVjYjg3ZDI5NGRkZWY0NTk3YTc1MjIwMDJiYzY4NmQ0OGViZDUyMDA5OTYxNDZkN2M0MGEzMzkxMjEzMjlkNmJkNmU1ZmJmM2MwNWNmM2QyZDJlNDYyODMyYjkxMjdkNmRhNDk5YjBjNmVkMjhiYjg1NjdlMDVhNGVkNmNkZTg1N2YwNDU2NDY0MWQ4N2E3NDhhYzJiOTIzMGJjNDU1ZDM3NDc1YTZlODZmZjRhN2ViOWE2ODQwOTdhMmE2MDUwZTBmNGU3NDNiM2YifQ._ayqlZlj2gEp8cNUe7pMI6ZbLAumLdKwXiIsZ8xrSbQ
Date: Tue, 08 Dec 2015 11:46:48 GMT
Content-Length: 344

{"message":"Authentication successful","identity":{"userID":"6c20630e-9d9b-11e5-891c-e34dca1742dd","loginInfo":{"providerID":"credentials","providerKey":"admin@example.com"},"firstName":"Global","lastName":"Admin","fullName":"Global Admin","email":"admin@example.com","roles":["normaluser"],"tenantCanonicalName":"platform"},"status":"success"}
```

#### Request

Make a GET request to a protected API endpoint (the test endpoint `/api/v0/auth/jwt/test` is used in this example), with the `X-Auth-Token` header set to the JWT token.

#### Response

Status code: 200

The body depends on the endpoint you requested. In the example, the JWT test endpoint is used, which returns data pertaining to the logged-in user.

### Requesting an API resource when not authenticated

```
$ curl -i -X GET http://localhost:9000/api/v0/auth/jwt/test
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8
Date: Tue, 08 Dec 2015 11:48:44 GMT
Content-Length: 63

{"message":"Not authenticated, access denied","status":"error"}
```

#### Request

Same as requesting an API resource, but with a wrong or missing JWT token.

#### Response

Status code: 401

A JSON object with two keys, one called `message`, containing an informative error, the other `status`, with value `error`.

