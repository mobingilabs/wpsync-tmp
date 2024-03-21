# ExchangeRate

為替レートのAPIリファレンスは以下の通りです。

## Set exchange rate per month

月ごとの為替レートの更新

**Role actions**

* `ModifySettings`

**Request**

```http
POST /user/exchange HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
    "exchange_rate":
        {
            "rate":110,
            "month":"2020-01"
        }
}
```

**exchange\_rate object description**

| Field | Type     | Required | Validation | Description |
| ----- | -------- | -------- | ---------- | ----------- |
| rate  | _double_ | Yes      | -          | 為替レート       |
| month | _string_ | Yes      | -          | 対象月         |

**Response**

```ruby
HTTP 200

{
  "status": "success"
}
```

## List exchange rate

月ごとの為替レートの取得。ドキュメント[`get:/user`](https://docs.mobingi.com/v/api-reference/ripple/user) レスポンス`{exchange_rate}`から確認できます。

## List invoice id exchange rate per month

月ごとのInvoiceID為替レートの取得

**Role actions**

* `ModifySettings`
* `ReadSettings`

**Request**

```http
GET /invoiceid/exchangerate/{vendor}/{month} HTTP1.1
Authorization: Bearer {token}

{request body}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01 \
リクエストパラーメータの`{vendor}`のサポートベンダー: `aws`,`azure`

**Response**

```ruby
HTTP 200

[
  {
    "invoice_id":"738676530",
    "account_id":"128347567789",
    "exchange_rate":107.302
  },
  {
    "invoice_id":"123656789",
    "account_id":"987655467321",
    "exchange_rate": null
  }
]
```

## Set invoice id exchange rate per month

月ごとのInvoiceID為替レートの設定

**Role actions**

* `ModifySettings`

**Request**

```http
POST /invoiceid/exchangerate/{vendor}/{month} HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01 \
リクエストパラーメータの`{vendor}`のサポートベンダー: `aws`,`azure`

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "settings": [
    {
      "account_id":"987655467321",
      "invoice_id":"123656789",
      "exchange_rate":101.07
    }
  ]
}
```

| Field          | Type     | Required | Validation | Description        |
| -------------- | -------- | -------- | ---------- | ------------------ |
| account\_id    | _string_ | Yes      | -          | 支払いアカウント           |
| invoice\_id    | _string_ | Yes      | -          | 支払いアカウントのInvoiceID |
| exchange\_rate | _double_ | Yes      | -          | 為替レート              |

**Response**

```ruby
HTTP 200

{
  "status":"success",
  "error_data": []
}
```

## List payer account id exchange rate per month

月ごとのPayerAccountID為替レートの取得

**Role actions**

* `ModifySettings`
* `ReadSettings`

**Request**

```http
GET /payer/exchange_rate/{month} HTTP1.1
Authorization: Bearer {token}

{request body}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01

**Response**

```ruby
HTTP 200

[
  {
    "id":"128347567789",
    "vendor":"aws",
    "name":"Payer Account",
    "exchange_rate":109.154
  },
  {
    "id":"111345678911",
    "vendor":"aws",
    "name":"Payer Account2",
    "exchange_rate":108.02
  }
]
```

## Set payer account id exchange rate per month

月ごとのPayerAccountID為替レートの設定

**Role actions**

* `ModifySettings`

**Request**

```http
POST /payer/exchange_rate/{month} HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "settings": [
    {
      "vendor":"aws",
      "account_id":"128347567789",
      "exchange_rate":109.154
    }
  ]
}
```

| Field          | Type     | Required | Validation    | Description |
| -------------- | -------- | -------- | ------------- | ----------- |
| vendor         | _string_ | Yes      | `aws`,`azure` | ベンダー        |
| account\_id    | _string_ | Yes      | -             | 支払いアカウント    |
| exchange\_rate | _double_ | Yes      | -             | 為替レート       |

**Response**

```ruby
HTTP 200

{
  "status":"success"
}
```

## Set invoice exchange rate per month

月ごとの請求書設定の為替レートの更新

**Role actions**

* `ModifyInvoice`

**Request**

```http
POST /invoices/exchangerate/{month} HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "vendor":"aws",
  "billing_groups": [
    "abcdfeg",
    "hijklmn"
  ],
  "exchange_rate":105.076
}
```

| Field           | Type     | Required | Validation           | Description |
| --------------- | -------- | -------- | -------------------- | ----------- |
| vendor          | _string_ | Yes      | サポート: `aws`, `azure` | ベンダー        |
| billing\_groups | _object_ | Yes      | -                    | 請求グループ一覧    |
| exchange\_rate  | _double_ | Yes      | -                    | 為替レート       |

**Response**

```ruby
HTTP 200

{
  "status":"success"
}
```
