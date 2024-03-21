# Account

アカウントのAPIリファレンスは以下の通りです。

## Create

アカウントの作成

**Role actions**

* `ModifyAccount`

**Request**

```http
POST /accts HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
    "vendor":"aws",
    "customer_id":"012345678912",
    "account_id":"919999618919"
    "company_id":"AJfdivbDjhvbpE",
    "name":"ripple customer1",
    "note":null
}
```

**{request body} description**

| Field        | Type     | Required | Validation                  | Description                              |
| ------------ | -------- | -------- | --------------------------- | ---------------------------------------- |
| customer\_id | _string_ | Yes      | - AWS 12桁   - Azure 16\~36桁 | - AWS AccountID   - Azure SubscriptionID |
| account\_id  | _string_ | Yes      | - AWS 12桁   - AZURE 7桁      | - AWS PayerAccountID   - Azure BillingID |
| company\_id  | _string_ | Yes      | -                           | 請求グループ内部ID                               |
| vendor       | _string_ | Yes      | - サポート: `aws`,`azure`       |                                          |
| name         | _string_ | Yes      | - 長さ: 3 \~ 100              | 登録する顧客名                                  |
| note         | _string_ | No       | -                           | 備考欄                                      |

**Response**

```ruby
HTTP 200

{"status":"success"}

HTTP 400 customer id が既に登録されている場合

{
  "code":"5005",
  "message":"account function exception",
  "description":"Customer id already exists."
}
```

**Pythonでのサンプル**

```
import requests
import json

def get_token():
    # Note: you can see details https://docs.alphaus.cloud/v/api-reference/authentication
    # Assign generated values for client_id and client_secret
    params={
        "grant_type": "client_credentials",
        "client_id": "{client_id}",
        "client_secret": "{client_secret}",
        "scope": "openid",
    }
    try:
        response = requests.post(
            url="https://login.alphaus.cloud/ripple/access_token",
            headers={
            },
            params=params,
            files=params,
        )
    except requests.exceptions.RequestException:
        print('HTTP Request failed')

    r = response.json()
    return r['access_token'], r["token_type"]

def send_request(type, token):
    # Authorization header
    auth = type + " " + token
    try:
        response = requests.post(
            url="https://api.alphaus.cloud/m/ripple/accts",
            headers={
                "Content-Type": "application/json;",
                "Authorization": auth
            },
            data=json.dumps({
                "account_id": "{account_id}",
                "vendor": "{vendor}",
                "customer_id": "{customer_id}",
                "note": None,
                "company_id": "{company_id}",
                "name": "customer_name"
            })
        )
        print('Response HTTP Status Code: {status_code}'.format(
            status_code=response.status_code))
        print('Response HTTP Response Body: {content}'.format(
            content=response.content))
    except requests.exceptions.RequestException:
        print('HTTP Request failed')

access_token, token_type = get_token()
send_request(token_type, access_token)
```

## List

アカウントリストの取得

**Role actions**

* `ReadAccount`&#x20;
* `ModifyAccount`

**Request**

```http
GET /accts?vendor={vendor} HTTP1.1
Authorization: Bearer {token}
```

以下に`{vendor}`のパラメータの例を示します。

**{vendor}**

* aws
* azure

**Response**

```ruby
HTTP 200

[
  {
    "billinggroup_id":"Billing1",
    "billinggroup_name":"Billing1",
    "company_id":"AJfdivbDjhvbpE",
    "customer_id":"012345678912",
    "customer_name":"ripple customer1",
    "account_id":"919999618919",
    "vendor":"aws",
    "note":null,
    "payer":false,
    "service_discount":null,
    "project_id":null,
    "azure_customer_id":null
  },
  ...
]
```

## Update

アカウントの更新

**Role actions**

* `ModifyAccount`

**Request**

```http
PUT /accts/{customer_id}/edit HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{customer_id}`のパラメータの例を示します。

**{customer\_id}**

AWS AccountID or Azure SubscriptionID

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
    "vendor":"aws",
    "account_id":"919999618919"
    "company_id":"AJfdivbDjhvbpE",
    "name":"ripple customer1",
    "note":null
}
```

**{request body} description**

| Field       | Type     | Required | Validation             | Description                              |
| ----------- | -------- | -------- | ---------------------- | ---------------------------------------- |
| account\_id | _string_ | Yes      | - AWS 12桁   - AZURE 7桁 | - AWS PayerAccountID   - Azure BillingID |
| company\_id | _string_ | Yes      | -                      | 請求グループ内部ID                               |
| vendor      | _string_ | Yes      | - サポート: `aws`,`azure`  |                                          |
| name        | _string_ | Yes      | - 長さ: 3 \~ 100         | 登録する顧客名                                  |
| note        | _string_ | No       | -                      | 備考欄                                      |

**Response**

```ruby
HTTP 200

{"status":"success"}

HTTP 400 customer id が登録されていない場合

{
  "code":"5005",
  "message":"account function exception",
  "description":"Customer id is not exists."
}
```

## Delete

アカウントの削除

**Role actions**

* `ModifyAccount`

**Request**

```http
DELETE /accts/{vendor}/{id} HTTP1.1
Authorization: Bearer {token}
```

以下に`{vendor}`のパラメータの例を示します。

**{vendor}**

* aws
* azure

以下に`{id}`フォーマットのパラメータの例を示します。

**{id}**

`{customer_id}|{account_id}`。 エンドポイント例: `/accts/aws/012345678912|919999618919`

**Response**

```ruby
HTTP 200

{"status":"success"}

HTTP 400 customer id が登録されていない場合

{
  "code":"5005",
  "message":"account function exception",
  "description":"Customer id is not exists."
}
```
