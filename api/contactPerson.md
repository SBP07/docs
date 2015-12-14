# Contact person API

## Object structure

```
{
  "id": "UUID",
  "firstName": "string",
  "lastName": "string",
  "address": {
    "street": "string",
    "zipCode": integer,
    "city": "string",
    "country": "string"
  },
  "landline": "string",
  "mobilePhone": "string",
  "tenantCanonicalName": "string"
}
```

Field name            | Type    | Nullable | Description | Example | Remarks
--------------------- | ------- | -------- | ----------- | ------- | -------
`id`                  | UUID    | No       | The UUID identifying the contact person | a9466912-04c0-4ae8-b6d9-6eeeea5b2c72 | Need not be present when posting, since this will be generated server-side. Otherwise non-nullable.
`firstName`           | String  | No       | The first name of the contact person | Piet |
`lastName`            | String  | No       | The last name of the contact person | Peters |
`address`             | Address | Yes      | The address of the contact | |
`landline`            | String  | Yes      | The landline phone number of the contact person | 056 56 56 56 |
`mobilePhone`         | String  | Yes      | The mobile phone number of the contact person | +324 32 32 32 |
`tenantCanonicalName` | String  | No       | The canonical name identifying the tenant this contact person belongs to | despeelberg |

### Address structure 

While the `address` key of the contact person is nullable, the individual keys of the address object are not. In other words, if you do specify the `address` key, it needs to be complete.

Field name  | Type    | Nullable | Description
----------- | ------- | -------- | -----------
`street`    | String  | No       | The street and number, e.g. "Baker Street 55A"
`zipCode`   | Integer | No       | The city's zip code, e.g. 9000
`city`      | String  | No       | The name of the city, e.g. "Gent"
`country`   | String  | No       | The country, e.g. "Belgium"

## Create

```
$ curl -i -X POST -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"firstName": "Voornaam", "lastName": "Achternaam", "landline": "056 56 56 56", "mobilePhone": "+324 32 32 32", "address": {"city": "Gent", "zipCode": 9000, "street": "Straatlaan 55A", "country": "Belgium"}, "tenantCanonicalName": "platform"}' http://localhost:9000/api/v0/contactPerson 
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Mon, 14 Dec 2015 19:38:38 GMT
Content-Length: 268

{"id":"45cb5232-a29a-11e5-81bb-4f1a53a8de04","firstName":"Voornaam","lastName":"Achternaam","address":{"street":"Straatlaan 55A","zipCode":9000,"city":"Gent","country":"Belgium"},"landline":"056 56 56 56","mobilePhone":"+324 32 32 32","tenantCanonicalName":"platform"}
```

### Request

Send a `POST` request to `/api/v0/contactPerson`. The `Content-Type` header should be set to `application/json`. The body of the request should follow the contact person JSON representation described above.

### Reponse

#### The contact person was successfully created

Status code: 201

The body of the request will be the newly created contact person (with the `id` field set).

## Update a contact person

```
$ curl -i -X PUT -H "X-Auth-Token: $TOKEN" -H "Content-Type: application/json" -d '{"id": "45cb5232-a29a-11e5-81bb-4f1a53a8de04", "firstName": "Voornaam", "lastName": "Achternaam", "landline": "056 56 56 56", "mobilePhone": "+324 32 32 32", "address": {"city": "Gent", "zipCode": 9000, "street": "Straatlaan 55A", "country": "Belgium"}, "tenantCanonicalName": "platform"}' http://localhost:9000/api/v0/contactPerson
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Mon, 14 Dec 2015 19:42:57 GMT
Content-Length: 268

{"id":"45cb5232-a29a-11e5-81bb-4f1a53a8de04","firstName":"Voornaam","lastName":"Achternaam","address":{"street":"Straatlaan 55A","zipCode":9000,"city":"Gent","country":"Belgium"},"landline":"056 56 56 56","mobilePhone":"+324 32 32 32","tenantCanonicalName":"platform"}
```

### Request

Send a `PUT` request to `/api/v0/contactPerson`. The `Content-Type` header should be `application/json`. The body of your request should follow the contact person JSON representation explained above. Notably, the `id` field will be used to determine what child to update.

### Response

#### On successful update

Status code: 200

The body of the request is the contact person after the update.

#### When the contact person is not found

Status code: 404

The body will contain a JSON object with two keys: `status`, set to `error`, and `message`, containing an informative message.

## Get all

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/contactPerson                                              
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Mon, 14 Dec 2015 19:08:29 GMT
Content-Length: 502

[{"id":"407422f0-9f89-11e5-b6fd-b71df7a39c3f","firstName":"Papa","lastName":"Test","tenantCanonicalName":"platform"},{"id":"47b21234-9f89-11e5-b6fd-0bcb9802e835","firstName":"Mama","lastName":"Test","tenantCanonicalName":"platform"},{"id":"9fed79b6-a295-11e5-82ca-ab08f78db2b7","firstName":"Voornaam","lastName":"Achternaam","address":{"street":"Straatlaan 55A","zipCode":9000,"city":"Gent","country":"Belgium"},"landline":"056 56 56 56","mobilePhone":"+324 32 32 32","tenantCanonicalName":"platform"}]
```

### Request

Send a `GET` request to `/api/v0/contactPerson`.

### Response

Status code: 200

The body has the following structure:

```
[
  { ContactPerson },
  ...
]
```

Where the `ContactPerson` object has the structure specified above.

## Get by id

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/contactPerson/id/9fed79b6-a295-11e5-82ca-ab08f78db2b7
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Mon, 14 Dec 2015 19:11:19 GMT
Content-Length: 268

{"id":"9fed79b6-a295-11e5-82ca-ab08f78db2b7","firstName":"Voornaam","lastName":"Achternaam","address":{"street":"Straatlaan 55A","zipCode":9000,"city":"Gent","country":"Belgium"},"landline":"056 56 56 56","mobilePhone":"+324 32 32 32","tenantCanonicalName":"platform"}
```

```
$ curl -i -X GET -H "X-Auth-Token: $TOKEN" http://localhost:9000/api/v0/contactPerson/id/9fed79b6-a295-11e5-82ca-ab08f78db2b8
HTTP/1.1 404 Not Found
Content-Type: application/json; charset=utf-8
X-Auth-Token: SOME_NEW_TOKEN
Date: Mon, 14 Dec 2015 19:35:57 GMT
Content-Length: 40

{"message":"Not found","status":"error"}
```

### Request

Send a `GET` request to `/api/v0/contactPerson/id/$ID`, where `$ID` is the UUID identifying the contact person.

### Response

#### The contact person exists

Status code: 200

The body of the request is the JSON representation of the contact person object. The structure is described above.

#### The contact person doesn't exist

Status code: 404

The body of the response has two keys: `status`, with a value of `error`, and `message`, which contains an informative message.

