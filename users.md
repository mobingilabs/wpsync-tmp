# Users

{% hint style="info" %}
New API available on [https://alphauslabs.github.io/blueapidocs/#/Iam](https://alphauslabs.github.io/blueapidocs/#/Iam).
{% endhint %}

The following endpoint is the base url for the APIs below.

```
https://service.alphaus.cloud/m/u/
```

## Create subuser

Create new subuser.

**Request**

```http
POST /users HTTP1.1
authorization: Bearer {token}
content-type: application/json

{
  "username": "newsubuser",
  "password": "mysecretpassword",
  "email": "dev@mobingi.com",
  "notification": {
    "email": "false"
  }
}
```

Details for the POST `{body}`.

| Key                  | Value                                                                                                        |
| -------------------- | ------------------------------------------------------------------------------------------------------------ |
| `username`           | Required. Min: 4, max: 18, allowed characters: letters, numbers, \_ (underscore), . (period) and - (hyphen). |
| `password`           | Required. Min: 8, max: 18.                                                                                   |
| `notification.email` | Required. Enable or disable notifications. Valid values: `"true"`, `"false"`.                                |
| `email`              | Optional email address.                                                                                      |

**Response**

```ruby
HTTP/1.1 200 OK

[
  {
    "username":"mysubuser",
    "msp_user":"MSP-123456",
    "mobingi_user": "abcdef",
    "email":"mysubuser@domain.com",
    "nickname":"",
    ...
  }
]
```

## List subusers

List all subusers.

**Request**

```http
GET /users HTTP1.1
authorization: Bearer {token}
```

**Response**

```ruby
HTTP/1.1 200 OK

[
  {
    "username":"mysubuser",
    "msp_user":"MSP-123456",
    "mobingi_user": "abcdef",
    "email":"mysubuser@domain.com",
    "nickname":"",
    ...
  }
]
```
