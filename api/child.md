# Child endpoint

This JSON REST endpoint allows managing children

## Authentication

This endpoint requires a JSON Web Token. See the authentication documentation for details.

## Requesting a child

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/child/id/d0dae4a2-9f1e-11e5-97b7-cfe975e88820
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Thu, 10 Dec 2015 09:29:49 GMT
Content-Length: 153

{"id":"d0dae4a2-9f1e-11e5-97b7-cfe975e88820","firstName":"Voornaam","lastName":"Achternaam","birthDate":"2001-09-26","tenantCanonicalName":"despeelberg"}
```

### Request

Send a `GET` request to `/api/v0/child/id/$ID`, where `$ID` is the UUID identifying the child.

### Response

A JSON object, following the following structure:

```
{
  "id": "UUID",
  "firstName": "string",
  "lastName": "string",
  "birthDate": "Date as yyyy-mm-dd",
  "tenantId": "UUID"
}
```

The following fields are returned, nullable meaning if it's possible that the key may be absent:

Field name             | Type   | Nullable | Description
---------------------- | ------ | -------- | -----------
`id`                   | UUID   | No       | The UUID identifying the child
`firstName`            | String | No       | First name of the child
`lastName`             | String | No       | Last name of the child
`birthDate`            | Date   | Yes      | Date on which the child was born, in the format `yyyy-mm-dd` (year, month, day)
`tenantCanonicalName`  | String   | No     | The canonical name identifying the tenant to whose organisation this child belongs

## Requesting all children

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/child
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Thu, 10 Dec 2015 09:28:13 GMT
Content-Length: 306

[{"id":"d0dae4a2-9f1e-11e5-97b7-cfe975e88820","firstName":"Voornaam","lastName":"Achternaam","birthDate":"2001-09-26","tenantCanonicalName":"despeelberg"},{"id":"54647116-9f20-11e5-97b7-13a61b2822a7","firstName":"Ander","lastName":"Achternaam","birthDate":"2001-09-26","tenantCanonicalName":"despeelberg"}]
```
### Request

Send a GET request to `/api/v0/child`

### Resonse

Status code: 200

The body has the following structure:

```
[
  {...},
  {...}
]
```

Where each object is a child. The JSON structure for a child is described above.

## Creating a new child

```
$ curl -i -X POST -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"firstName": "Voornaam", "lastName": "Achternaam", "birthDate": "2001-09-26", "tenantCanonicalName": "despeelberg"}'  http://localhost:9000/api/v0/child
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Thu, 10 Dec 2015 09:17:20 GMT
Content-Length: 153

{"id":"d0dae4a2-9f1e-11e5-97b7-cfe975e88820","firstName":"Voornaam","lastName":"Achternaam","birthDate":"2001-09-26","tenantCanonicalName":"despeelberg"}
```

### Request

Send a `POST` request to `/api/v0/child`

The body of your request should be JSON, with the structure specified above. The `id` field should not be set, since it's generated serverside. The header `Content-Type` should be equal to `application/json`.

### Response

#### On successfully created

Response code: 201

The body of the request is the created child.

## Updating a child

```
$ curl -i -X PUT -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"id": "d0dae4a2-9f1e-11e5-97b7-cfe975e88820", "firstName": "Nieuwe", "lastName": "Naam", "birthDate": "2001-09-26", "tenantCanonicalName": "despeelberg"}'  http://localhost:9000/api/v0/child
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Thu, 10 Dec 2015 09:33:03 GMT
Content-Length: 145

{"id":"d0dae4a2-9f1e-11e5-97b7-cfe975e88820","firstName":"Nieuwe","lastName":"Naam","birthDate":"2001-09-26","tenantCanonicalName":"despeelberg"}
```

### Request

Send a PUT request to `/api/v0/child`. The `Content-Type` of your request should be `application/json`. The body of your request should be JSON, in the format specified above. The `id` is required.

### Response

#### On a successful update

Status code: 200

The body of the request is the updated child.

## Deleting a child

TODO

