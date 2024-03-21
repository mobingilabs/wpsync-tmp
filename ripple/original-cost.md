# OriginalCost

原価データのAPIリファレンスは以下の通りです。

## Set exchange rate per InvoiceID

InvoiceIDの為替レート設定

**Role actions**

* `ModifyOriginalCost`

**Request**

```http
POST /invoiceid/:vendor/:month HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01 \
リクエストパラーメータの`{vendor}`のサポートベンダー: `aws`

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "data":[
      {
        "invoice_id":"123456789",
        "exchange_rate":101.051
      },
      {
        "invoice_id":"468215697",
        "exchange_rate":100.02
      }
  ]
}
```

**request body description**

| Field          | Type     | Required | Validation | Description |
| -------------- | -------- | -------- | ---------- | ----------- |
| invoice\_id    | _string_ | Yes      | -          | InvoiceID   |
| exchange\_rate | _double_ | Yes      | -          | 登録する為替レート   |

**Response**

```ruby
HTTP 200

{
  "status":"success"
}
```

## Get InvoiceID list

InvoiceIDの取得

**Role actions**

* `ReadOriginalCost`
* `ModifyOriginalCost`

**Request**

```http
GET /invoiceid/:vendor/:month HTTP1.1
Authorization: Bearer {token}

{request body}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01 \
リクエストパラーメータの`{vendor}`のサポートベンダー: `aws`

**Response**

```ruby
HTTP 200

[
  {
    "invoice_id":"123456789",
    "exchange_rate":101.051
  },
  {
    "invoice_id":"468215697",
    "exchange_rate":100.02
  }
]
```

## Export InvoiceID CSV

InvoiceID CSVの出力

**Role actions**

* `ModifyOriginalCost`

**Request**

```http
POST /export/invoiceidcsv/:month HTTP1.1
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
  "exchange":true,
  "digit":"up"
}
```

**request body description**

| Field    | Type      | Required | Validation                   | Description          |
| -------- | --------- | -------- | ---------------------------- | -------------------- |
| vendor   | _string_  | Yes      | サポート: `aws`                  | ベンダー                 |
| exchange | _boolean_ | Yes      | -                            | USDで出力または為替後(JPY)を出力 |
| digit    | _double_  | Yes      | サポート: `up`,`down`,`rounding` | 小数点丸め設定              |

**Response**

```ruby
HTTP 200

{
  "status":"success",
  "url":"csv link"
}
```
