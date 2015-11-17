# Sign-up

## Signing up

```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"firstName": "Firstname", "lastName": "Lastname", "email": "hello@example.com", "password": "aoeuaoeu"}' http://localhost:9000/api/v0/auth/signUp
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
X-Auth-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyLVlHVm5ReVNiSWxFTjNOV05QWEZ2V1VrNWtBVGlSc3hKUVd1bUZBNE9sdldVY3lheHB0bjY1UUY2VFRkbXUyMlhncFVQN0pSbTdWaFNxd0V0RG4wYU15aGQwUmw2cThMQWs1Y21HVHcxIiwiaXNzIjoicGxheS1zaWxob3VldHRlIiwiZXhwIjoxNDQ3ODI2NzcwLCJpYXQiOjE0NDc3ODM1NzAsImp0aSI6ImRiZjQ2ZmQyYzM3NDZiOGFhNjg1MzQ5ZTgwYTMwZjcyMzM4N2I1NjIzNDRhYmQwNTQwOTI5MTIyYTU3OWFlN2JiZmYzNzliZTBlMDlkYzJhMzI1ODVlYzM1NDI4ZTFhZjkyZDQyYmQ4ZjlkYmYxYWM2OTNlMDExODFlYmZkNGIxNGFlMTJlZTlkN2FjMDc0YzAxNTgzOGFjMThjMzdhYTE0NjIwYTU2YjRkMTI0YzM2NzdmNTVhZDAyOTJkYjQ0ZTQ1YzBhNDYwZGZjYTk1YzlmNjQ0ZTcxYTllYWJkMzM1NTdhNTMxMzUyNjczYTJmNDQ1MzNkMmY0MGE2YjQ0MWUifQ.8ZOAvkhPs6d2dn7MkKJ0Cr7s6-BLjw87BdSxqxaBpHE
Date: Tue, 17 Nov 2015 18:06:10 GMT
Content-Length: 230

{"userID":"58277516-0a4e-4995-ad41-5d30884c7e54","loginInfo":{"providerID":"credentials","providerKey":"hello@example.com"},"firstName":"Firstname","lastName":"Lastname","fullName":"Firstname Lastname","email":"hello@example.com"}
```

### Request

Send a POST request to `/api/v0/auth/signUp` with an `application/json` body containing a JSON object that has the following structure: `{"firstName": "string", "lastName": "string", "email": "string", "password": "string"}`.

### Response

Status code: 201

The `X-Auth-Token` header, containing a JWT for the newly created user.

An `application/json` body with the following structure: `{"userID": "UUID", "loginInfo": {"providerID": "credentials", "providerKey": "string"}, "firstName": "string", "lastName": "string", "fullName": "string", "email": "string"}`

### Signing up with an email address that is already in use

```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"firstName": "Firstname", "lastName": "Lastname", "email": "hello@example.com", "password": "aoeuaoeu"}' http://localhost:9000/api/v0/auth/signUp
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
Date: Tue, 17 Nov 2015 18:14:37 GMT
Content-Length: 58

{"message":"There already exists a user with this email!"}
```
### Request

Same as signing up, but with an email address that is already in use.

### Response

Code: 400

A JSON object with one key, called `message`, containing an informative error.

