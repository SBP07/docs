# Child endpoint

This JSON REST endpoint allows managing children

## Authentication

This endpoint requires a JSON Web Token. See the authentication documentation for details.

## Requesting a child

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/child/id/5ff72774-8e3d-11e5-8bf8-570d2cd7e3cc
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyLXpIamJyWjd1OFo1eHV0SU1XZDE2YjNkNGNEM0FtTFZIYng3S1ZrVGE5M1dSaXNUVDI0UWdGOXBzS3hWODRLelwvWng1a1ByaERzbW4ybUVPWDAzcmZvSW89IiwiaXNzIjoicGxheS1zaWxob3VldHRlIiwiZXhwIjoxNDQ3OTIzOTEyLCJpYXQiOjE0NDc4ODMwMDcsImp0aSI6IjIxM2Y5YjEzOGI2M2FiYThiZWUxYzIxM2M3NTc5NTBlNzFmZjAwMzkyYzM4NDkyMDlhZmQ0NDJiOTk1NDhlYWZkYjNmZmIwNzkwMzRmN2E4ZmYwNmI5MTg3ZjAzNjJjMjAzMjk3ODgyODkwMjdkN2QwMDJiNWNmMzM5YzVjNGM5NDBkZTc2MDViMjlhOGQ5ZWUxMTk0MGU4NDMxZjhhZjZiZDE1MmEzZmFhZDNiMTgwZGRjZDhhNjQ0MDcxMDBlNTFhOWEwYWYxMDI1NDA2OWY4ZjU4ZWI2MTg0YmFjMjg0ODRjM2FiYTRkYmY3NTUzNTEzMTU0MWYxYjZkMjI1NmEifQ.oT91l5d_W7pWIJZoE6iMSk2QIk9J2xoT_P_UzyCwqZI
Date: Wed, 18 Nov 2015 21:43:27 GMT
Content-Length: 249

{"id":"5ff72774-8e3d-11e5-8bf8-570d2cd7e3cc","firstName":"Voornaam","lastName":"Achternaam","birthDate":"2000-10-20","address":{"street":"Straat","number":"666A","zipCode":7777,"city":"Weireldstad"},"tenantId":"76d211e8-8e38-11e5-a47a-f7ee3b59f6c0"}
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
  "address": {
    "street": "string",
    "number": "string",
    "zipCode": integer,
    "city": "string"
  },
  "tenantId": "UUID"
}
```

The following fields are returned, nullable meaning if it's possible that the key may be absent:

Field name  | Type   | Nullable | Description
----------- | ------ | -------- | -----------
`id`        | UUID   | No       | The UUID identifying the child
`firstName` | String | No       | First name of the child
`lastName`  | String | No       | Last name of the child
`birthDate` | Date   | Yes      | Date on which the child was born, in the format `yyyy-mm-dd` (year, month, day)
`address`   | Object | Yes      | The address on which information pertaining to the child should be sent
`tenantId`  | UUID   | No       | The UUID identifying the tenant to whose organisation this child belongs

The structure of the address key is as follows:

Field name  | Type    | Nullable | Description
----------- | ------- | -------- | -----------
`street`    | String  | No       | The street, e.g. "Baker Street"
`number`    | String  | No       | The house number, e.g. "221B"
`zipCode`   | Integer | No       | The city's zip code, e.g. 9000
`city`      | String  | No       | The name of the city, e.g. "Gent"

While the `address` key of the child object is nullable, it's subkeys are not, meaning that the key can be absent, but if it is present, all the keys should be set.

## Requesting all children

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/child         
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyLURzRlJTa0t1cFBwa0JWd3BuMHNwUnJcL3VYNDlGZFlLOGxEXC9RelVhRUlkN1hiOHE4N2F5ZEZEVHg4ZXF4eDU5MzFRZUQrNkU2MVBCbktcL3RcLzQzRWRcLzFvPSIsImlzcyI6InBsYXktc2lsaG91ZXR0ZSIsImV4cCI6MTQ0NzkyMzkxMiwiaWF0IjoxNDQ3ODgyNDI5LCJqdGkiOiIyMTNmOWIxMzhiNjNhYmE4YmVlMWMyMTNjNzU3OTUwZTcxZmYwMDM5MmMzODQ5MjA5YWZkNDQyYjk5NTQ4ZWFmZGIzZmZiMDc5MDM0ZjdhOGZmMDZiOTE4N2YwMzYyYzIwMzI5Nzg4Mjg5MDI3ZDdkMDAyYjVjZjMzOWM1YzRjOTQwZGU3NjA1YjI5YThkOWVlMTE5NDBlODQzMWY4YWY2YmQxNTJhM2ZhYWQzYjE4MGRkY2Q4YTY0NDA3MTAwZTUxYTlhMGFmMTAyNTQwNjlmOGY1OGViNjE4NGJhYzI4NDg0YzNhYmE0ZGJmNzU1MzUxMzE1NDFmMWI2ZDIyNTZhIn0.CMEADmHpMNBxODQ7bLPo11ggHNFMYq1y9qRB2A9NCNc
Date: Wed, 18 Nov 2015 21:33:49 GMT
Content-Length: 1336

[{"id":"8a497ec8-8e29-11e5-a268-6f46ce2c3943","firstName":"Voornaam","lastName":"Achternaam","birthDate":"2015-11-18","address":null,"tenantId":"4dde3a60-8ddd-11e5-a92c-7b365fa7719e"},{"id":"54ccf2e2-8e2a-11e5-931d-df9c2f27290a","firstName":"Voornaam","lastName":"Achternaam","birthDate":"2015-11-18","address":null,"tenantId":"4dde3a60-8ddd-11e5-a92c-7b365fa7719e"},{"id":"fe2aed10-8e2c-11e5-8c28-9b397d77dd16","firstName":"Voornaam","lastName":"Achternaam","birthDate":"2015-11-18","address":{"street":"Straat","number":"666A","zipCode":6666,"city":"City"},"tenantId":"4dde3a60-8ddd-11e5-a92c-7b365fa7719e"},{"id":"b7257d70-8e38-11e5-a47a-db2a954255f7","firstName":"Voornaam","lastName":"Achternaam","birthDate":null,"address":{"street":"Straat","number":"666A","zipCode":7777,"city":"Weireldstad"},"tenantId":"76d211e8-8e38-11e5-a47a-f7ee3b59f6c0"},{"id":"be7179c6-8e38-11e5-a47a-8b7b7421eb4a","firstName":"Voornaam","lastName":"Achternaam","birthDate":null,"address":{"street":"Straat","number":"666A","zipCode":7777,"city":"Weireldstad"},"tenantId":"76d211e8-8e38-11e5-a47a-f7ee3b59f6c0"},{"id":"c356a718-8e38-11e5-a47a-474c13fc3e4a","firstName":"Voornaam","lastName":"Achternaam","birthDate":null,"address":{"street":"Straat","number":"666A","zipCode":7777,"city":"Weireldstad"},"tenantId":"76d211e8-8e38-11e5-a47a-f7ee3b59f6c0"}]
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

TODO

## Updating a child

TODO

## Deleting a child

TODO

