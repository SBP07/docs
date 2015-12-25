# Activity endpoint

This JSON REST endpoint allows managing activities

## Authentication

This endpoint requires a JSON Web Token. See the authentication documentation for details.

## Object structure

```
{
  "id": "b8fa6e38-aa5f-11e5-a277-530ee81b131e",
  "place": "speelplein",
  "activityTypeId": "0c5ac2be-aa59-11e5-81ea-e75c58858255",
  "date": "2015-12-25",
  "startTime": "2015-12-24T17:16:46.526",
  "endTime": "2015-12-24T20:16:46.526",
  "tenantCanonicalName": "platform"
}
```

Field name            | Type      | Nullable | Description | Example | Remarks
--------------------- | --------- | -------- | ----------- | ------- | -------
`id`                  | UUID      | No       | The UUID identifying the activity | b8fa6e38-aa5f-11e5-a277-530ee81b131e | Need not be present when posting, since this will be generated server-side. Otherwise non-nullable.
`place`               | String    | Yes      | The place where this activity takes place | `Speelplein De Speelberg`, `Theater Waregem`
`activityTypeId`      | UUID      | No       | The UUID identifying the type of this activity | `0c5ac2be-aa59-11e5-81ea-e75c58858255` |
`date`                | Date      | No       | The date on which this activity happens | `2015-12-25` | Format: `dd-MM-yyyy`
`startTime`           | Timestamp | Yes      | The time on which this activity starts | `2015-12-24T17:16:46.526` | Format: ISO date string, `.toISOString()` on a JS `Date` object
`endTime`             | Timestamp | Yes      | The time on which this activity ends | `2015-12-24T20:16:46.526` | Format: ISO date string, `.toISOString()` on a JS `Date` object
`tenantCanonicalName` | String    | No       | The canonical name identifying the tenant this contact person belongs to | `despeelberg` |


## Requesting an activity

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/activity/id/b8fa6e38-aa5f-11e5-a277-530ee81b131e
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Thu, 24 Dec 2015 17:02:24 GMT
Content-Length: 249

{"id":"b8fa6e38-aa5f-11e5-a277-530ee81b131e","place":"speelplein","activityTypeId":"0c5ac2be-aa59-11e5-81ea-e75c58858255","date":"2015-12-25","startTime":"2015-12-24T17:16:46.526","endTime":"2015-12-24T20:16:46.526","tenantCanonicalName":"platform"}
```

### Request

Send a `GET` request to `/api/v0/activity/id/$ID`, where `$ID` is the UUID identifying the activity.

### Response

A JSON object, following the following structure described above.

## Requesting all activities

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/activity                                       
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Thu, 24 Dec 2015 17:13:41 GMT
Content-Length: 796

[{"id":"44ba00d4-aa59-11e5-81ea-9bbc6e6c8a81","activityTypeId":"0c5ac2be-aa59-11e5-81ea-e75c58858255","date":"2015-12-25","tenantCanonicalName":"platform"},{"id":"78f5ff42-aa59-11e5-81ea-c39ec00c1b07","place":"speelplein","activityTypeId":"0c5ac2be-aa59-11e5-81ea-e75c58858255","date":"2015-12-25","tenantCanonicalName":"platform"},{"id":"c014a482-aa59-11e5-81ea-a341a8bca3b3","place":"speelplein","activityTypeId":"0c5ac2be-aa59-11e5-81ea-e75c58858255","date":"2015-12-25","startTime":"2015-12-24T17:16:46.526","tenantCanonicalName":"platform"},{"id":"b8fa6e38-aa5f-11e5-a277-530ee81b131e","place":"speelplein","activityTypeId":"0c5ac2be-aa59-11e5-81ea-e75c58858255","date":"2015-12-25","startTime":"2015-12-24T17:16:46.526","endTime":"2015-12-24T20:16:46.526","tenantCanonicalName":"platform"}]
```
### Request

Send a GET request to `/api/v0/actvity`

### Resonse

Status code: 200

The body has the following structure:

```
[
  {...},
  {...}
]
```

Where each object is an activity. The JSON structure for an activity is described above.

## Creating a new child

```
$ curl -i -X POST -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"activityTypeId": "0c5ac2be-aa59-11e5-81ea-e75c58858255", "tenantCanonicalName": "platform", "date": "2015-12-25", "place": "speelplein", "startTime": "2015-12-24T16:16:46.526Z", "endTime": "2015-12-24T19:16:46.526Z"}'  http://localhost:9000/api/v0/activity
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Thu, 24 Dec 2015 17:16:13 GMT
Content-Length: 249

{"id":"08812936-aa62-11e5-a277-972ef7193e0e","place":"speelplein","activityTypeId":"0c5ac2be-aa59-11e5-81ea-e75c58858255","date":"2015-12-25","startTime":"2015-12-24T17:16:46.526","endTime":"2015-12-24T20:16:46.526","tenantCanonicalName":"platform"}
```

### Request

Send a `POST` request to `/api/v0/activity`

The body of your request should be JSON, with the structure specified above. The `id` field should not be set, since it's generated serverside. The header `Content-Type` should be equal to `application/json`.

### Response

#### On successfully created

Response code: 201

The body of the request is the created child.

## Updating an activity

```
$ curl -i -X PUT -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"id": "44ba00d4-aa59-11e5-81ea-9bbc6e6c8a81", "activityTypeId":"0c5ac2be-aa59-11e5-81ea-e75c58858255","date":"2015-06-06","tenantCanonicalName":"platform"}' http://localhost:9000/api/v0/activity
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIyLUVOZGVxUCtsRXQyVVN3aWg2YXRVeWRGdjJWVENoWkN0THprN3RmXC9mbE83TXBLUWtFXC9WV0V1a2R3MWZTbFFyK3lyT3FidWJoQ0xhOEFjVU9qQjRcLzA3bEluK0RQaWlKWGhzcTdtVzhlIiwiaXNzIjoicGxheS1zaWxob3VldHRlIiwiZXhwIjoxNDUzNjYyNTIyLCJpYXQiOjE0NTEwNzA3MjYsImp0aSI6IjM0YTE4NjQyZTJlNDA1MGQ3MmQ5OTdiODkzYjRjZDdiNWYyMzNjMGE3MjdmZjkwYjFmZmEzYmVhNWQ4NWJiOWIyMzY4ZjdjYjU5NzQxNGI3Nzg2MDQwNDY5ZWNlMGQ3M2YxY2U3MjMyZTVhMWE2MjA3MmZiZTIxZDk5MmYwOWE4Y2YyNmE0N2M5MzdmNjMzMjQ0MDg3ZGM2ZDFjNmQ3MzhhMzQ0NGNiYjNmMjFhZGViMzcxY2M0MjI1YTc2MGRlMTc2NmY0ZDAzYTQ4ZDJkYjhmNjhkNmRhMzg2YzIxN2E5ZDBmMzI3NjhiYTg1OWI2NTNiNzJjMzBjNTgwZTg1ZWYifQ.JOxsJVnh9ivj5WbvVEOvbpq5eHM8t555qlS0SKFgnHg
Date: Fri, 25 Dec 2015 19:12:06 GMT
Content-Length: 154

{"id":"44ba00d4-aa59-11e5-81ea-9bbc6e6c8a81","activityTypeId":"0c5ac2be-aa59-11e5-81ea-e75c58858255","date":"2015-06-06","tenantCanonicalName":"platform"}
```

### Request

Send a PUT request to `/api/v0/activity`. The `Content-Type` of your request should be `application/json`. The body of your request should be JSON, in the format specified above. The `id` is required.

### Response

#### On a successful update

Status code: 200

The body of the request is the updated activity.

## Deleting an activity

TODO

