# Child to activity

This API allows to control the relationship between an activity and a child. The relationship between them is "many to many", meaning a child can be present at multiple activities, and an activity can have multiple attending children.

## Getting the activities for a child

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/child/id/64be9e5a-abfb-11e5-89f7-af4275162cd3/activities
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sat, 26 Dec 2015 18:35:23 GMT
Content-Length: 693

[{"activity":{"id":"78f5ff42-aa59-11e5-81ea-c39ec00c1b07","place":"speelplein","activityTypeId":"0c5ac2be-aa59-11e5-81ea-e75c58858255","date":"2015-12-25","tenantCanonicalName":"platform"},"relationship":{"childId":"64be9e5a-abfb-11e5-89f7-af4275162cd3","activityId":"78f5ff42-aa59-11e5-81ea-c39ec00c1b07"}},{"activity":{"id":"c014a482-aa59-11e5-81ea-a341a8bca3b3","place":"speelplein","activityTypeId":"0c5ac2be-aa59-11e5-81ea-e75c58858255","date":"2015-12-25","startTime":"2015-12-24T17:16:46.526","tenantCanonicalName":"platform"},"relationship":{"childId":"64be9e5a-abfb-11e5-89f7-af4275162cd3","activityId":"c014a482-aa59-11e5-81ea-a341a8bca3b3","checkInTime":"2015-12-26T19:11:36.521"}}]
```

### Request

Send a `GET` request to `/api/v0/child/id/$CHILD_ID/activities` where `$CHILD_ID` is the UUID identifying the child.

### Response

A JSON list, following the following structure:

```
[
  {
    "relationship": {
      "childId": UUID,
      "activityId": UUID,
      "checkInTime": "timestamp",
      "checkOutTime": "timestamp"
    },
    "activity": { Activity object }
  },
  ...
]
```

Field name      | Type                        | Nullable | Description
--------------- | --------------------------- | -------- | -----------
`relationship`  | ChildToActivityRelationship | No       | The relationship of the child to the activity. The structure is described below.
`activity`      | Activity                    | No       | The activity object. For the JSON structure, see the activity API docs.


Field name      | Type      | Nullable | Description
--------------- | --------- | -------- | -----------
`childId`       | UUID      | No       | The UUID identifying the child
`activityId`    | UUID      | No       | The UUID identifying the activity
`checkInTime`   | Timestamp | Yes      | The time when the child checked in a the activity
`checkOutTime`  | Timestamp | Yes      | The time when the child checked out at the activity

No errors are returned when you request a UUID that does not identify a child, you just get back an empty list.

## Getting the children for an activity

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/activity/id/78f5ff42-aa59-11e5-81ea-c39ec00c1b07/children
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sat, 26 Dec 2015 22:04:45 GMT
Content-Length: 152

[{"id":"64be9e5a-abfb-11e5-89f7-af4275162cd3","firstName":"Voornaam","lastName":"Achternaam","birthDate":"2001-09-26","tenantCanonicalName":"platform"}]
```

### Request

Send a `GET` request to `/api/v0/activity/id/$ACTIVITY_ID/children` where `$ACTIVITY_ID` is the UUID identifying the activity.

### Response

A JSON list, following the following structure:

```
[
  { Child object },
  ...
]
```

For the structure of the child object, see the child API docs. No errors are returned when you request a UUID that does not identify a child, you just get back an empty list.

## Adding a relationship between a child and a activity

```
$ curl -i -X POST -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"activityId": "c014a482-aa59-11e5-81ea-a341a8bca3b3", "checkInTime": "2015-12-26T18:11:36.521Z"}'  http://localhost:9000/api/v0/child/id/64be9e5a-abfb-11e5-89f7-af4275162cd3/activities
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sat, 26 Dec 2015 18:17:16 GMT
Content-Length: 56

{"message":"Successfully registered","status":"success"}
```

```
$ curl -i -X POST -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"activityId": "78f5ff42-aa59-11e5-81ea-c39ec00c1b07"}'  http://localhost:9000/api/v0/child/id/64be9e5a-abfb-11e5-89f7-af4275162cd3/activities
HTTP/1.1 409 Conflict
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sat, 26 Dec 2015 18:06:53 GMT
Content-Length: 82

{"message":"This activity is already registered for this child.","status":"error"}
```

```
curl -i -X POST -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"activityId": "78f5ff42-aa59-11e5-81ea-c39ec00c1b07"}'  http://localhost:9000/api/v0/child/id/8a870782-9f56-11e5-940b-d381b14746db/activities
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sat, 26 Dec 2015 18:05:48 GMT
Content-Length: 118

{"message":"Non-existant child or activity, or child/activity does not belong to the current tenant","status":"error"}
```


## Removing the relationship between a child and an activity

```
$ curl -i -X DELETE -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/child/id/64be9e5a-abfb-11e5-89f7-af4275162cd3/activities/78f5ff42-aa59-11e5-81ea-c39ec00c1b07
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sun, 27 Dec 2015 09:56:27 GMT
Content-Length: 47

{"message":"Deleted 1 rows","status":"success"}
```

```
$ curl -i -X DELETE -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/child/id/64be9e5a-abfb-11e5-89f7-af4275162cd3/activities/78f5ff42-aa59-11e5-81ea-c39ec00c1b07
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Sun, 27 Dec 2015 09:58:44 GMT
Content-Length: 118

{"message":"Non-existant child or activity, or child/activity does not belong to the current tenant","status":"error"}
```

