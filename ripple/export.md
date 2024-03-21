# Export

CSV出力に関するAPIリファレンスは以下の通りです。

## Get invoice csv(account)

請求書CSV(account)の出力

**Role actions**

* `ReadInvoice`
* `ModifyInvoice`

**Request**

```http
POST /export/csv/invoice_account/{month} HTTP1.1
Authorization: Bearer {token}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01

**Response**

```ruby
HTTP 200

{
  "status":"success",
  "url":"csv link"
}
```

CSVの内容

| Field                    | Description |
| ------------------------ | ----------- |
| BillingGroupID           | 請求グループID    |
| BillingGroupName         | 請求グループ名     |
| CompanyName              | 会社名         |
| Vendor                   | ベンダー        |
| CustomerID               | 顧客ID        |
| CustomerName             | 顧客名         |
| ServiceType              | サービスタイプ     |
| ServiceName              | サービス名       |
| Charge                   | 金額          |
| DiscountCharge           | 割引後額        |
| DiscountCharge - TaxFree | 割引後額 - 免税金額 |
| FreeFormat               | フリーフォーマット金額 |
| CustomService            | 請求サービス金額    |
| Tax                      | 消費税         |
| Total                    | 振込金額        |

## Get invoice csv(tag)

請求書CSV(tag)の出力

**Role actions**

* `ReadInvoice`
* `ModifyInvoice`

**Request**

```http
POST /export/csv/invoice_tag/{month} HTTP1.1
Authorization: Bearer {token}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01

**Response**

```ruby
HTTP 200

{
  "status":"success",
  "url":"csv link"
}
```

CSVの内容

| Field                    | Description |
| ------------------------ | ----------- |
| BillingGroupID           | 請求グループID    |
| BillingGroupName         | 請求グループ名     |
| CompanyName              | 会社名         |
| Vendor                   | ベンダー        |
| Tag                      | タグ詳細        |
| ServiceType              | サービスタイプ     |
| ServiceName              | サービス名       |
| Charge                   | 金額          |
| DiscountCharge           | 割引後額        |
| DiscountCharge - TaxFree | 割引後額 - 免税金額 |
| FreeFormat               | フリーフォーマット金額 |
| CustomService            | 請求サービス金額    |
| Tax                      | 消費税         |
| Total                    | 振込金額        |

## Get billing group csv

請求グループCSVの出力

**Role actions**

* `ReadBillingGroup`
* `ModifyBillingGroup`

**Request**

```http
POST /exportcsv/billing-group HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "redo":false
}
```

| Field | Type      | Required | Validation | Description                                               |
| ----- | --------- | -------- | ---------- | --------------------------------------------------------- |
| redo  | _boolean_ | Yes      | -          | true:作成済みののCSVを出力。CSVがない場合はCSVを生成する。 false:新しいCSVを生成して出力。 |

**Response**

```ruby
HTTP 200

{
  "status":"success",
  "path":"csv link"
}
```

CSVの内容

| Field     | Description |
| --------- | ----------- |
| 請求グループID  | -           |
| 請求グループ名   | -           |
| 企業名       | -           |
| 郵便番号      | -           |
| 住所        | -           |
| 電話番号      | -           |
| 宛名        | -           |
| 請求書タイトル   | -           |
| コスト       | -           |
| カスタムフィールド | -           |

## Get billing group setting csv

請求グループ設定CSVの出力

**Role actions**

* `ReadBillingGroup`
* `ModifyBillingGroup`

**Request**

```http
POST /exportcsv/billing-group-setting HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "redo":false
}
```

| Field | Type      | Required | Validation | Description                                               |
| ----- | --------- | -------- | ---------- | --------------------------------------------------------- |
| redo  | _boolean_ | Yes      | -          | true:作成済みののCSVを出力。CSVがない場合はCSVを生成する。 false:新しいCSVを生成して出力。 |

**Response**

```ruby
HTTP 200

{
  "status":"success",
  "path":"csv link"
}
```

CSVの内容

| Field           | Description |
| --------------- | ----------- |
| 請求グループID        | -           |
| 請求グループ名         | -           |
| ベンダー            | -           |
| 値引率             | -           |
| 消費税             | -           |
| AWSサポート請求方法     | -           |
| AWSサポート（一律%の場合） | -           |
| AWSサポート（固定）     | -           |
| 代行手数料請求方法       | -           |
| 代行手数料（％）        | -           |
| 代行手数料（固定）       | -           |
| 集計タイプ           | -           |
| 値引き対象           | -           |
| 請求代行サービス計算対象    | -           |
