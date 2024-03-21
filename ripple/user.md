# User

ユーザーのAPIリファレンスは以下の通りです。

## Get user

ユーザー情報の取得

**Role actions**

* `ModifySettings`

**Request**

```http
GET /user HTTP1.1
Authorization: Bearer {token}
```

**Response**

```ruby
HTTP 200

{
  "msp_start": "2020-01",
  "language": "ja",
  "invoices_info": {},
  "invoice_meta": {},
  "meta": {},
  "services": [],
  "username": null,
  "support_fee": [],
  "msp_id": "MSP-5aa311904d5d6",
  "invoice_layout": {},
  "service_discount": {},
  "months": [],
  "export_digit": "up",
  "exchange_rate": [],
  "pay_accounts": [],
  "service_fee": null,
  "company_name": "Company Name",
  "reseller_lng": null,
  "email": "info@alphaus.cloud"
}
```

**exchange\_rate object例**

| Field | Type     | Description         |
| ----- | -------- | ------------------- |
| month | _string_ | 月 format: `yyyy-mm` |
| rate  | _double_ | 為替レート               |

```ruby
"exchange_rate": [
    {
      "month": "2020-04",
      "rate": 105.00
    }
]
```

**pay\_accounts object例**

vendor : aws

| Field               | Type     | Description          |
| ------------------- | -------- | -------------------- |
| vendor              | _string_ | ベンダー                 |
| id                  | _string_ | 支払いアカウント             |
| name                | _string_ | 支払いアカウント名            |
| bucket\_name        | _string_ | aws s3 bucket名       |
| prefix              | _string_ | aws s3 bucket prefix |
| report\_name        | _string_ | aws s3 report名       |
| role\_arn           | _string_ | aws iam role arn     |
| report\_last\_saved | _string_ | CUR更新日時              |

```ruby
"pay_accounts": [
    {
      "vendor": "aws",
      "id": "123456789109",
      "name": "Payer Account",
      "bucket_name": null,
      "prefix": null,
      "report_name": null,
      "role_arn": null,
      "report_last_saved": []
    }
]
```

vendor : azure

| Field             | Type     | Description            |
| ----------------- | -------- | ---------------------- |
| vendor            | _string_ | ベンダー                   |
| id                | _string_ | 支払いアカウント               |
| name              | _string_ | 支払いアカウント名              |
| application\_name | _string_ | azure application name |
| application\_id   | _string_ | azure application id   |
| commerce\_id      | _string_ | azure commerce id      |

```ruby
"pay_accounts": [
    {
      "vendor": "azure",
      "id": "1234567",
      "name": "Payer Account",
      "application_name": null,
      "application_id": null,
      "commerce_id": null
    }
]
```
