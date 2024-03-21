# API clients

{% hint style="info" %}
New API available on [https://alphauslabs.github.io/blueapidocs/#/Iam](https://alphauslabs.github.io/blueapidocs/#/Iam).
{% endhint %}

The following endpoint is the base url for the APIs below.

```
https://service.alphaus.cloud/m/u/users/
```

## Create API client

Create a new API client under a specific user.

**Request**

```http
POST /client/:user HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "test-apiclient-name",
}
```

`:user` is either root user or subuser.

Details for the POST `{body}`.

| Key    | Value                                 |
| ------ | ------------------------------------- |
| `name` | Required. The name of the API client. |

**Response**

```ruby
HTTP/1.1 200 OK

{
  "client_id": "ripple-abcdef123456",
  "client_secret": "critical",
  "grant_type": "client_credentials",
  "create_time": "2020-06-27T11:26:46.257375295Z",
  "user_id": "id0001",
  "username": "someusername",
  "name": "test-apiclient-name"
}
```

## List API clients

List all API clients under a specific user.

**Request**

```http
GET /clients/:user HTTP1.1
Authorization: Bearer {token}
```

`:user` is either root user or subuser.

**Response**

```ruby
HTTP/1.1 200 OK

[
  {
    "client_id": "ripple-abcdef123456",
    "client_secret": "",
    "grant_type": "client_credentials",
    "create_time": "2020-06-15T05:07:50.258779172Z",
    "user_id": "id001",
    "name": "test-apiclient-name"
  },
  ...
]
```

## Delete API client

Delete an existing API client under a specific user.

**Request**

```http
DELETE /client/:user/:clientid HTTP1.1
Authorization: Bearer {token}
```

`:user` is either root user or subuser. `:clientid` is the client id to delete.

**Response**

```ruby
HTTP/1.1 200 OK

{
  "status": "success"
}
```
