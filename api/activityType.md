# Activity type endpoint

This JSON REST endpoint allows managing activity types

Activity types are categories, kinds of activities. One playground might have, for example, "Afternoon", "Childcare before start" and "External activity" as activity types.

## Authentication

This endpoint requires a JSON Web Token. See the authentication documentation for details.

## Object structure

```
{
  "id": "0c5ac2be-aa59-11e5-81ea-e75c58858255",
  "mnemonic": "VM",
  "description": "Voormiddag",
  "tenantCanonicalName": "platform"
}
```

Field name            | Type      | Nullable | Description | Example | Remarks
--------------------- | --------- | -------- | ----------- | ------- | -------
`id`                  | UUID      | No       | The UUID identifying the activity type | 0c5ac2be-aa59-11e5-81ea-e75c58858255 | Need not be present when posting, since this will be generated server-side. Otherwise non-nullable.
`mnemonic`            | String    | No       | A mnemonic, used in reports to conserve space, to identify this activity type | `VM`, `NM`, `EXT`
`description`         | String    | No       | A description of the activity type | `Voormiddag`, `Namiddag`, `Externe activiteit` |
`tenantCanonicalName` | String    | No       | The canonical name identifying the tenant this contact person belongs to | `despeelberg` |


## Requesting an activity type

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/activity/type/id/0c5ac2be-aa59-11e5-81ea-e75c58858255
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Fri, 25 Dec 2015 19:59:39 GMT
Content-Length: 121

{"id":"0c5ac2be-aa59-11e5-81ea-e75c58858255","mnemonic":"VM","description":"Voormiddag","tenantCanonicalName":"platform"}
```

### Request

Send a `GET` request to `/api/v0/activity/type/id/$ID`, where `$ID` is the UUID identifying the activity type.

### Response

A JSON object, following the following structure described above.

## Requesting all activity types

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/activity/type
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Fri, 25 Dec 2015 20:01:35 GMT
Content-Length: 243

[{"id":"0c5ac2be-aa59-11e5-81ea-e75c58858255","mnemonic":"VM","description":"Voormiddag","tenantCanonicalName":"platform"},{"id":"477c2146-ab42-11e5-b313-4f6abf5ad371","mnemonic":"NM","description":"Namiddag","tenantCanonicalName":"platform"}]
```
### Request

Send a GET request to `/api/v0/actvity/type`

### Resonse

Status code: 200

The body has the following structure:

```
[
  {...},
  {...}
]
```

Where each object is an activity type. The JSON structure for an activity type is described above.

## Creating a new activity type

```
$ curl -i -X POST -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"mnemonic":"EXT","description":"Externe activiteit","tenantCanonicalName":"platform"}' http://localhost:9000/api/v0/activity/type
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Fri, 25 Dec 2015 20:04:22 GMT
Content-Length: 130

{"id":"b0e21032-ab42-11e5-b313-8bbd3a520785","mnemonic":"EXT","description":"Externe activiteit","tenantCanonicalName":"platform"}
```

### Request

Send a `POST` request to `/api/v0/activity/type`

The body of your request should be JSON, with the structure specified above. The `id` field should not be set, since it's generated serverside. The header `Content-Type` should be equal to `application/json`.

### Response

#### On successfully created

Response code: 201

The body of the request is the created activity type.

## Updating an activity type

```
$ curl -i -X PUT -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"id":"b0e21032-ab42-11e5-b313-8bbd3a520785","mnemonic":"EXT","description":"Externe activiteit","tenantCanonicalName":"platform"}' http://localhost:9000/api/v0/activity/type

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Fri, 25 Dec 2015 20:06:38 GMT
Content-Length: 130

{"id":"b0e21032-ab42-11e5-b313-8bbd3a520785","mnemonic":"EXT","description":"Externe activiteit","tenantCanonicalName":"platform"}
```

### Request

Send a PUT request to `/api/v0/activity/type`. The `Content-Type` of your request should be `application/json`. The body of your request should be JSON, in the format specified above. The `id` is required.

### Response

#### On a successful update

Status code: 200

The body of the request is the updated activity type.

## Deleting an activity type

TODO

