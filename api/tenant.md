# Tenant

## Object structure

```
{
    "canonicalName": "platform",
    "id": "11111111-1111-1111-1111-111111111111",
    "name": "Platformadministratie"
}
```

Field name      | Type   | Nullable | Description | Example | Remarks
--------------- | ------ | -------- | ----------- | ------- | -------
`id`            | UUID   | No       | The UUID identifying the tenant| a9466912-04c0-4ae8-b6d9-6eeeea5b2c72 | Need not be present when posting, since this will be generated server-side. Otherwise non-nullable.
`name`          | String | No       | The name of the tenant | Speelplein De Speelberg | 
`canonicalName` | String | No       | The canonical name of the tenant | despeelberg | Lowercase, alphanumeric representation of the name. Example usage: in a URL of as a subdomain.


## Requesting a tenant

```
curl -i -H "X-Auth-Token: $TOKEN" -X GET http://localhost:9000/api/v0/tenant/id/11111111-1111-1111-1111-111111111111 
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN 
Date: Wed, 09 Dec 2015 19:06:58 GMT
Content-Length: 103

{"id":"11111111-1111-1111-1111-111111111111","canonicalName":"platform","name":"Platformadministratie"}
```

### Request

Send a `GET` request to `/api/v0/tenant/id/$ID`, where `$ID` is the UUID identifying the tenant.

### Response

A JSON object, following the structure described above.

## Requesting all tenants

```
curl -i -H "X-Auth-Token: $TOKEN" -X GET http://localhost:9000/api/v0/tenant
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Wed, 09 Dec 2015 16:29:51 GMT
Content-Length: 105

[{"id":"11111111-1111-1111-1111-111111111111","canonicalName":"platform","name":"Platformadministratie"}]
```

### Request

Send a `GET` request to `/api/v0/tenant`.

### Resonse

Status code: 200

The body has the following structure:

```
[
  {...},
  {...}
]
```

Where each object is a tenant. The JSON structure for a tenant is described above.

## Creating a tenant

```
curl -i -H "Content-Type: application/json" -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"name": "Speelplein De Speelberg", "canonicalName": "despeelberg"}' -X POST http://localhost:9000/api/v0/tenant 
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Wed, 09 Dec 2015 19:54:18 GMT
Content-Length: 108

{"id":"a1cfeefc-9eae-11e5-98fa-c70f76bf40ca","canonicalName":"despeelberg","name":"Speelplein De Speelberg"}
```

### Request

Send a `POST` request to `/api/v0/tenant`, with a body containing the keys `name` and `canonicalName`. These keys are explained above. The `id` should not be set.

### Response

#### The tenant was successfully created

Status code: 201

The body contains the created tenant, with the generated id.

#### The `canonicalName` is not unique (already exists in the database)

Status code: 409 (Conflict)

The following body:

```
{"message":"Unique key violation: unique key already exists in the database.","status":"error"}
```

