# Invoice

請求関連のAPIリファレンスは以下の通りです。

## Get Account total cost list

アカウント合計一覧の取得

**Role actions**

* `ReadInvoice`
* `ModifyInvoice`

**Request**

```http
GET /invoice/{month}/details HTTP1.1
Authorization: Bearer {token}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01

**Response**

```ruby
HTTP 200

{
  "accounts": [
    {
      "customer_id": "012345678987",
      "customer_name": "customer 1",
      "total": 431,
      "total_exchanged": 43100,
      "adjustment_entries": [
        {
          "name": "upfront - Sign up charge for subscription: 000000000, planId: 000000000",
          "amount": 2,
          "amount_exchanged": 200
        }
      ]
    },
    {
      "customer_id": "123456789875",
      "customer_name": "customer 2",
      "total": 6,
      "total_exchanged": 600,
      "adjustment_entries": [
        {
          "name": "upfront - one-time fee for 1 year All Upfront ap-southeast-1 EC2 Savings Plan ID:0000000000 ",
          "amount": 1,
          "amount_exchanged": 100
        }
      ]
    }
  ],
  "billing_groups": [
    {
      "billing_group_id": "bgid1",
      "billing_group_name": "bg1",
      "vendor": "aws",
      "tax_excluded_amount": 0,
      "tax_excluded_amount_exchanged": 0,
      "tax": 0,
      "total_amount_exchanged": 0
    },
    {
      "billing_group_id": "bgid2",
      "billing_group_name": "bg2",
      "vendor": "aws",
      "tax_excluded_amount": 437,
      "tax_excluded_amount_exchanged": 43700,
      "tax": 4370,
      "total_amount_exchanged": 48070
    }
  ]
}
```

accountsの詳細

| Field               | Type     | Description        |
| ------------------- | -------- | ------------------ |
| customer\_id        | _string_ | 顧客ID               |
| customer\_name      | _string_ | 顧客名                |
| total               | _double_ | $金額(税抜)            |
| total\_exchanged    | _double_ | 換算後金額(税抜)          |
| adjustment\_entries | _list_   | 請求書に含んだ再計算請求データの一覧 |

billing\_groupsの詳細

| Field                            | Type     | Description  |
| -------------------------------- | -------- | ------------ |
| billing\_group\_id               | _string_ | 請求グループID     |
| billing\_group\_name             | _string_ | 請求グループ名      |
| vendor                           | _string_ | ベンダー         |
| tax\_excluded\_amount            | _double_ | $合計金額(税抜)    |
| tax\_excluded\_amount\_exchanged | _double_ | $換算後合計金額(税抜) |
| tax                              | _double_ | 消費税金額        |
| total\_amount\_exchanged         | _double_ | 合計請求金額       |

## Get Invoice list

請求書一覧の取得

**Role actions**

* `ReadInvoice`
* `ModifyInvoice`

**Request**

```http
GET /invoices/{month} HTTP1.1
Authorization: Bearer {token}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01

**Response**

```ruby
HTTP 200

{
  "total": {
    "stock":108850,
    "sales":108850,
    "azure_stock":0,
    "azure_sales":0
  },
  "billinggroup": [
    {
      "company_id":"company1",
      "name":"company1",
      "billinggroup_id":"billinggroup1",
      "billinggroup_name":"billinggroup1",
      "project_id":null,
      "project_code":null,
      "project_label":null,
      "project_currency":null,
      "month":"2020-12",
      "invoice_no":"2020-12billing1",
      "created_data": {
        "aws": {
          "invoice_no":"2020-12billing1",
          "calc_type":"account",
          "currency":"jpy",
          "discount_rate":0.02,
          "discount_target_usage":"cloudpaywithfee",
          "discount_calc_logic":"usageamount",
          "tax_rate":0.10,
          "support_fee":"fix",
          "support_rate":0,
          "support_fee_calc_target":"nondiscount",
          "support_fix": 0,
          "substitution_fee":"percent",
          "substitution_rate":0,
          "substitution_fix":0,
          "substitution_fee_calc_target":"nondiscount",
          "substitution_fee_target_usage":"cloudpaywithfee",
          "substitution_fee_calc_type":"allsum",
          "exchange_rate":102.45,
          "memo":null,
          "additional_items": []
        },
        "azure":null
      },
      "saved_data": {
        "aws": {
          "invoice_no":null,
          "calc_type":"account",
          "currency":"jpy",
          "discount_rate":0.02,
          "discount_target_usage":"cloudpaywithfee",
          "discount_calc_logic":"usageamount",
          "tax_rate":0.10,
          "support_fee":"fix",
          "support_rate":0,
          "support_fee_calc_target":"nondiscount",
          "support_fix":0,
          "substitution_fee":"percent",
          "substitution_rate":0,
          "substitution_fix":0,
          "substitution_fee_calc_target":"nondiscount",
          "substitution_fee_target_usage":"cloudpaywithfee",
          "substitution_fee_calc_type":"allsum",
          "exchange_rate":102.45,
          "memo":null,
          "additional_items": []
        },
        "azure":null
      },
      "default_data": {
        "aws": {
          "invoice_no":null,
          "calc_type":"account",
          "currency":"jpy",
          "discount_rate":0.02,
          "discount_target_usage":"cloudpaywithfee",
          "discount_calc_logic":"usageamount",
          "tax_rate":0.10,
          "support_fee":"fix",
          "support_rate":0,
          "support_fee_calc_target":"nondiscount",
          "support_fix":0,
          "substitution_fee":"percent",
          "substitution_rate":0,
          "substitution_fix":0,
          "substitution_fee_calc_target":"nondiscount",
          "substitution_fee_target_usage":"cloudpaywithfee",
          "substitution_fee_calc_type":"allsum",
          "exchange_rate":null,
          "memo":null,
          "additional_items": []
        },
        "azure": {
          "invoice_no":null,
          "calc_type":"account",
          "currency":"jpy",
          "discount_rate":0,
          "discount_target_usage":"cloudpaywithfee",
          "discount_calc_logic":"usageamount",
          "tax_rate":0.10,
          "support_fee":"fix",
          "support_rate":0,
          "support_fee_calc_target":"nondiscount",
          "support_fix":0,
          "substitution_fee":"percent",
          "substitution_rate":0,
          "substitution_fix":0,
          "substitution_fee_calc_target":"nondiscount",
          "substitution_fee_target_usage":"cloudpaywithfee",
          "substitution_fee_calc_type":"allsum",
          "exchange_rate":null,
          "memo":null,
          "additional_items": []
        }
      },
      "accounts": [
        {
          "account_id":"012345678912",
          "customer_id":"012345678912",
          "customer_name":"customer1",
          "vendor":"aws",
          "service_discount":null
        },
        ...
      ],
      "create_time":"2020-12-21T11:26:55+09:00",
      "update_time":null,
      "total": {
        "aws":108850,
        "azure":0
      },
      "language":"ja"
    },
    ...
  ]
}
```

## 請求書設定の保存

**Role actions**

* `ModifyInvoice`

**Request**

```http
PUT /invoices/save/{month} HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "settings":[],
  "internal":true
}
```

| Field    | Type        | Required | Validation | Description                                             |
| -------- | ----------- | -------- | ---------- | ------------------------------------------------------- |
| settings | \_object\_  | Yes      | -          | 請求書設定                                                   |
| internal | \_boolean\_ | Yes      | -          | True: デフォルトの設定を請求書設定として一括で保存します。 False: settingsを参照します。 |

**Response**

```ruby
HTTP 200

{
  "status": "success"
}
```

## 請求書為替レートの保存

**Role actions**

* `ModifyInvoice`

```http
PUT /invoices/exchangerate/{month} HTTP1.1
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
  "billing_groups":["company id value1","company id value2"...],
  "exchange_rate":100
}

```

| Field           | Type       | Required | Validation           | Description              |
| --------------- | ---------- | -------- | -------------------- | ------------------------ |
| vendor          | \_string\_ | Yes      | サポート: `aws`, `azure` | ベンダー                     |
| billing\_groups | \_object\_ | Yes      | -                    | 請求グループ一覧。 company\_idを設定 |
| exchange\_rate  | \_double\_ | Yes      | -                    | 為替レート                    |

**Response**

```ruby
HTTP 200

{
  "status": "success"
}
```

## 請求書の作成

請求書の作成を行う前に以下のAPIで請求書の設定を行ってください。

1\. \[[`請求書設定の保存`](invoice.md#no)]

2\. \[[`請求書為替レートの保存`](invoice.md#rtono)]

**Role actions**

* `ModifyInvoice`

```http
POST /invoices/calculation/{month} HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm`例: 2020-01

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "vendor":"aws",
  "group":["company id value1","company id value2"...],
  "bulk":false
}
```

| Field  | Type        | Required | Validation                  | Description                                                             |
| ------ | ----------- | -------- | --------------------------- | ----------------------------------------------------------------------- |
| vendor | \_string\_  | Yes      | サポート: `aws`, `azure`, `gcp` | ベンダー                                                                    |
| group  | \_object\_  | Yes      | -                           | 請求グループ一覧。 company\_idを設定                                                |
| bulk   | \_boolean\_ | Yes      | -                           | <p>一括設定。<br>True:一括で請求書を作成。 </p><p>False: groupに設定された請求グループの請求書を作成。</p> |

**Response**

```ruby
HTTP 200

{
  "status": "success"
}
```
