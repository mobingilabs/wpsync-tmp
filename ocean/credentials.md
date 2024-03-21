# Credentials

Before you can do any Ocean deployments, you need to register your cloud credentials to Ocean. These credentials will be used by Ocean to access your cloud account and deploy any resources you need in your Ocean deployments.

## Create a credential

Register a cloud credential to Ocean.

**Request**

```http
POST /v0/credentials HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

The following are some example request payloads for `{request body}` per provider.

Alibaba:

```ruby
{
  "vendor":"alicloud",
  "name":"testalicloudcreds",
  "key":"ABCDEF",
  "secret":"somesecret"
}
```

AWS:

```ruby
{
  "vendor":"aws",
  "name":"testawscreds",
  "key":"ABCDEF",
  "secret":"somesecret"
}
```

Azure:

```ruby
{
  "vendor":"azure",
  "name":"testazurecreds",
  "key":"ABCDEF",
  "secret":"somesecret",
  "application":"997613dc-032a-440f-bead-2bb5b19ad002",
  "subscription":"c825c2bf-cdc5-4b96-9644-a1dd5144ae0f",
  "directory":"292cd594-02c7-4a56-86e7-e30615557e83"
}
```

GCP (for GCP, `secret` is your service account's JSON file):

```ruby
{
  "vendor":"gcp",
  "name":"testgcpcreds",
  "project_id":"gcp-project-id",
  "secret":"aGVsbG93b3JsZA==",
  "isbase64":true
}
```

**Response**

```ruby
HTTP 201

{
  "credential_id":"cred-aws-58c2297d25645-fa7vzi6E1l",
  "user_id":"1234567890",
  "username":"testsubuser",
  "name":"testawscreds",
  "key":"ABCDEF",
  "secret":"***",
  "create_time":"2019-04-04T04:16:02Z",
  "update_time":"2019-04-04T04:16:02Z",
  "vendor":"aws"
}
```

When creating [Ocean templates](https://docs.mobingi.com/v/ocean-en/reference-2018-07-02), you will use the credential's `name` part as the name for the template's credential entry. This name should be unique per account.

## List credentials

Return a list of registered credentials.

**Request**

```http
GET /v0/credentials[?vendor={vendor-name}] HTTP1.1
Authorization: Bearer {token}
```

**Response**

Example:

```ruby
HTTP 200

{
  "alicloud":[
    {
      "credential_id":"cred-alicloud-xx",
      "user_id":"1234",
      "username":"subuser",
      "name":"testalicloudcreds",
      "key":"ABCDEF",
      "secret":"***",
      "create_time":"2019-01-31T06:47:08Z",
      "update_time":"2019-01-31T06:47:08Z",
      "vendor":"alicloud"
    }
  ],
  "aws":[
    {
      "credential_id":"cred-aws-xx",
      "user_id":"1234",
      "username":"subuser",
      "name":"testawscreds",
      "key":"ADCDEF",
      "secret":"***",
      "create_time":"2019-04-04T04:16:02Z",
      "update_time":"2019-04-04T04:16:02Z",
      "vendor":"aws"
    }
  ],
  "gcp":[
    {
      "credential_id":"cred-gcp-xx",
      "user_id":"1234",
      "username":"subuser",
      "name":"testgcpcreds",
      "key":"testgcpcreds",
      "secret":"***",
      "project_id":"gcp-project-id",
      "create_time":"2018-05-21T17:15:36+09:00",
      "update_time":"2018-05-21T17:15:36+09:00",
      "vendor":"gcp"
    }
  ],
  "azure":[
    {
      "credential_id":"cred-azure-xx",
      "user_id":"1234",
      "username":"subuser",
      "name":"testazurecreds",
      "key":"ABCDEF",
      "secret":"***",
      "application_id":"997613dc-032a-440f-bead-2bb5b19ad002",
      "subscription_id":"c825c2bf-cdc5-4b96-9644-a1dd5144ae0f",
      "directory_id":"292cd594-02c7-4a56-86e7-e30615557e83",
      "create_time":"2018-05-21T17:15:36+09:00",
      "update_time":"2018-05-21T17:15:36+09:00",
      "vendor":"azure"
    }
  ]
}
```

## Describe a credential

Describe a specific credential specified by id.

**Request**

```http
GET /v0/credentials/{id} HTTP1.1
Authorization: Bearer {token}
```

**Response**

```ruby
HTTP 200

{
  "credential_id":"cred-gcp-xx",
  "user_id":"1234",
  "username":"subuser",
  "name":"testgcpcreds",
  "key":"testgcpcreds",
  "secret":"***",
  "project_id":"gcp-project-id",
  "create_time":"2018-05-21T17:15:36+09:00",
  "update_time":"2018-05-21T17:15:36+09:00",
  "vendor":"gcp"
}
```

## Delete a credential

Remove a registered credential from Ocean.

**Request**

```http
DELETE /v0/credentials/{id} HTTP1.1
Authorization: Bearer {token}
```

**Response**

```ruby
HTTP 200
```
