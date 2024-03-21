# Pre-request

open apiを使用するためのtokenを取得する必要があります。

**Response Format**

```
{
    token_type : string
    expires_in : number
    access_token : string
}
```

| Response value | type     | description |
| -------------- | -------- | ----------- |
| `token_type`   | _string_ | 認証スキーム      |
| `expires_in`   | _number_ | 期限 43200秒   |
| `access_token` | _string_ | token値      |

## Token取得

openapiで使用するtokenを取得

**Request**

```http
POST /v1/access_token HTTP1.1
Content-Type: form-data

{request body}
```

`{request body}` の例

```ruby
{
  "grant_type":"client_credentials",
  "client_id":"test-client-id",
  "client_secret":"ABCDEFGHI"
}
```

| Body            | description |
| --------------- | ----------- |
| `grant_type`    | 固定値         |
| `client_id`     | 顧客ID        |
| `client_secret` | 顧客Secret    |
