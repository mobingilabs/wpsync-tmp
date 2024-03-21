# Recalculation

再計算請求データのAPIリファレンスは以下の通りです。

## Get recalculation list

再計算請求データの取得

**Role actions**

* `ReadBillingGroup`
* `ModifyBillingGroup`

**Request**

```http
GET /billinggroup/recalculation/{month}?vendor={vendor} HTTP1.1
Authorization: Bearer {token}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01 \
リクエストパラーメータの`{vendor}`のサポートベンダー: `aws`,`azure`

**Response**

```ruby
HTTP 200

[
  {
    "customer_id":"012345678912",
    "customer_name":"customer1",
    "company_id":"company1",
    "billinggroup_id":"billing1",
    "billinggroup_name":"billing1",
    "project_code":null,
    "id": "recalculationid1",
    "calc_type":"Fee",
    "mobingi_type":"REGISTRAR",
    "description":"[OP-n4GB] Renewal",
    "product_name":"Amazon Registrar",
    "account_id":"012345678912",
    "currency_code":"USD",
    "product_code":"AmazonRegistrar",
    "unblended_cost":"90.0000000000",
    "time_interval":"2020-05-27T15:00:00Z\/2021-05-26T15:00:01Z",
    "usage_start":"2020-05-27T15:00:00Z",
    "apply":false,
    "exchange_rate":null,
    "tax_free":false,
    "vendor":"aws"
  },
  ...
]
```

## Apply recalculation

再計算請求データの適用・未適用の割当

**Role actions**

* `ModifyBillingGroup`

**Request**

```http
POST /billinggroup/recalculation HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
    "data": [
        "recalculationid1",
        "recalculationid2",
        "recalculationid3"
    ],
    "month": "2021-01",
    "exchange_rate":104.02,
    "tax_free":true,
    "apply":true,
    "vendor":"aws"
}
```

**{request body} description**

| Field          | Type      | Required | Validation        | Description   |
| -------------- | --------- | -------- | ----------------- | ------------- |
| data           | _array_   | Yes      | \[id1,id2,id3...] | 再計算請求データIDの一覧 |
| month          | _string_  | Yes      | -                 | 適用・未適用する対象月   |
| exchange\_rate | _double_  | Yes      | -                 | 適用・未適用する為替レート |
| tax\_free      | _boolean_ | Yes      | -                 | 免税設定          |
| apply          | _boolean_ | Yes      | -                 | 適用・未適用設定      |
| vendor         | _string_  | Yes      | -                 | ベンダー          |

**Response**

```ruby
HTTP 200

{"status":"success"}
```
