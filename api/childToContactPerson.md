# Child to contact person

This API allows to control the relationship between a contact person and a child. The relationship between them is "many to many", meaning a child can have multiple contact people, and a contact person can have multiple children for whom s/he can be contacted.

## Getting the contact people for a child

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/child/id/8a870782-9f56-11e5-940b-d381b14746db/contactPeople
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sun, 13 Dec 2015 17:51:05 GMT
Content-Length: 159

[{"relationship":"father","contactPerson":{"id":"407422f0-9f89-11e5-b6fd-b71df7a39c3f","firstName":"Papa","lastName":"Test","tenantCanonicalName":"platform"}}]
```

### Request

Send a `GET` request to `/api/v0/child/id/$CHILD_ID/contactPeople` where `$CHILD_ID` is the UUID identifying the child.

### Response

A JSON list, following the following structure:

```
[
  {
    "relationship": "string",
    "contactPerson": { ContactPerson object }
  },
  ...
]
```

Field name             | Type          | Nullable | Description
---------------------- | ------------- | -------- | -----------
`relationship`         | String        | No       | The relationship of the contact person to the child, e.g. "father", "grandparent"...
`contactPerson`        | ContactPerson | No       | The contact person object. For the JSON structure, see the contact person API docs.

No errors are returned when you request a UUID that does not identify a child, you just get back an empty list.

## Getting the children for a contact person

```
curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/contactPerson/id/407422f0-9f89-11e5-b6fd-b71df7a39c3f/children
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sun, 13 Dec 2015 18:36:58 GMT
Content-Length: 152

[{"id":"8a870782-9f56-11e5-940b-d381b14746db","firstName":"Voornaam","lastName":"Achternaam","birthDate":"2001-09-26","tenantCanonicalName":"platform"}]
```

### Request

Send a `GET` request to `/api/v0/contactPerson/id/$CONTACT_PERSON_ID/children` where `$CONTACT_PERSON_ID` is the UUID identifying the contact person.

### Response

A JSON list, following the following structure:

```
[
  { Child object },
  ...
]
```

For the structure of the child object, see the child API docs. No errors are returned when you request a UUID that does not identify a child, you just get back an empty list.

## Adding a relationship between a child and contact person

```
curl -i -X POST -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"contactPersonId": "407422f0-9f89-11e5-b6fd-b71df7a39c3f", "relationship": "father"}' http://localhost:9000/api/v0/child/id/8a870782-9f56-11e5-940b-d381b14746db/contactPeople
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sun, 13 Dec 2015 21:10:40 GMT
Content-Length: 56

{"message":"Successfully registered","status":"success"}
```

```
curl -i -X POST -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"contactPersonId": "407422f0-9f89-11e5-b6fd-b71df7a39c3f", "relationship": "father"}' http://localhost:9000/api/v0/child/id/8a870782-9f56-11e5-940b-d381b14746db/contactPeople
HTTP/1.1 409 Conflict
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sun, 13 Dec 2015 21:10:42 GMT
Content-Length: 88

{"message":"This contact person is already registered for this child.","status":"error"}
```

```
curl -i -X POST -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"contactPersonId": "407422f0-9f89-11e5-b6fd-b71df7a39c3f", "relationship": "father"}' http://localhost:9000/api/v0/child/id/8a870782-9f56-11e5-940b-d381b14746de/contactPeople
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sun, 13 Dec 2015 21:11:44 GMT
Content-Length: 130

{"message":"Non-existant child or contact person, or child/contact person does not belong to the current tenant","status":"error"}
```


## Removing the relationship between a person and a contact person

```
$ curl -i -X DELETE -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"contactPersonId": "407422f0-9f89-11e5-b6fd-b71df7a39c3f", "relationship": "father"}' http://localhost:9000/api/v0/child/id/8a870782-9f56-11e5-940b-d381b14746db/contactPeople/407422f0-9f89-11e5-b6fd-b71df7a39c3f
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_OKEN
Date: Sun, 13 Dec 2015 19:48:44 GMT
Content-Length: 47

{"message":"Deleted 1 rows","status":"success"}
```

