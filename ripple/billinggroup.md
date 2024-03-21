# BillingGroup

請求グループのAPIリファレンスは以下の通りです。

## Create

請求グループの作成

**Role actions**

* `ModifyBillingGroup`&#x20;

**Request**

```http
POST /billinggroup HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
    "billinggroup_id":"Billing1",
    "billinggroup_name":"Billing1",
    "company_name":"Billing1 company",
    "phone":"03‐1234‐5678",
    "postal":"12345",
    "address":"123 street",
    "billing_title":"billing title",
    "personal":"personal name",
    "remarks":"test automation",
    "inv_aggregate":false,
    "language":"ja",
    "invoices": {
        "aws": {
            "calc_type":"account",
            "currency":"jpy",
            "discount_calc_logic":"usageamount",
            "discount_rate":0,
            "discount_target_usage":"cloudpaywithfee",
            "substitution_fee":"percent",
            "substitution_fee_calc_target":"nondiscount",
            "substitution_fee_calc_type":"allsum",
            "substitution_fee_target_usage":"cloudpaywithfee",
            "substitution_fix":0,
            "substitution_rate":0,
            "support_amount_target":"allusage",
            "support_fee":"fix",
            "support_fee_calc_target":"nondiscount",
            "support_fix":0,
            "support_rate":0,
            "tax_rate":0
        }
  }
}
```

**{request body} description**

| Field                 | Type      | Required | Validation       | Description                             |
| --------------------- | --------- | -------- | ---------------- | --------------------------------------- |
| billinggroup\_id      | _string_  | Yes      | -                | Billing group ID                        |
| billinggroup\_name    | _string_  | Yes      | -                | Billimg group name                      |
| company\_name         | _string_  | Yes      | -                | Company name                            |
| phone                 | _string_  | No       | -                | Tel                                     |
| postal                | _string_  | No       | -                | Postal                                  |
| address               | _string_  | No       | -                | Address                                 |
| billing\_title        | _string_  | No       | -                | Invoice title                           |
| personal              | _string_  | No       | -                | Personal name                           |
| remarks               | _string_  | No       | -                | Memo                                    |
| inv\_aggregate        | _boolean_ | Yes      | -                | Displaying invoice in bulk or by vendor |
| project\_id           | _string_  | No       | -                | Project id                              |
| invoice\_template\_id | _string_  | No       | -                | Invoice template id                     |
| invoices              | \[object] | No       | -                | Invoice setting                         |
| language              | _string_  | No       | サポート: `ja`, `en` | Display invoice language setting        |

**invoices object description**

| Field                            | Type     | Required | Validation                                                                 | Description              |
| -------------------------------- | -------- | -------- | -------------------------------------------------------------------------- | ------------------------ |
| calc\_type                       | _string_ | Yes      | - account   - tag                                                          | Invoice calculation type |
| currency                         | _string_ | Yes      | - jpy   - usd                                                              | Currency                 |
| discount\_calc\_logic            | _string_ | Yes      | - usageamount                                                              | -                        |
| discount\_rate                   | _double_ | Yes      | 0.00 \~ 1.00                                                               | -                        |
| discount\_target\_usage          | _string_ | Yes      | - cloudpaywithfee   - cloudpayonly                                         | -                        |
| substitution\_fee                | _string_ | Yes      | - percent   - fix   - automatic   - usagetable                             | -                        |
| substitution\_fee\_calc\_target  | _string_ | Yes      | - nondiscount   - discounted                                               | -                        |
| substitution\_fee\_calc\_type    | _string_ | Yes      | - allsum   - account                                                       | -                        |
| substitution\_fee\_target\_usage | _string_ | Yes      | - cloudpaywithfee   - cloudpayonly                                         | -                        |
| substitution\_fix                | _double_ | Yes      | 00 \~ 1000000                                                              | -                        |
| substitution\_rate               | _double_ | Yes      | 0.00 \~ 1.00                                                               | -                        |
| support\_amount\_target          | _string_ | Yes      | - allusage                                                                 | -                        |
| support\_fee                     | _string_ | Yes      | - fix   - percent   - aws\_developer   - aws\_business   - aws\_enterprise | -                        |
| support\_fee\_calc\_target       | _string_ | Yes      | - nondiscount   - discounted                                               | -                        |
| support\_fix                     | _double_ | Yes      | 0.00 \~ 1000000                                                            | -                        |
| support\_rate                    | _double_ | Yes      | 0.00 \~ 1.00                                                               | -                        |
| tax\_rate                        | _double_ | Yes      | 0.00 \~ 0.10                                                               | Tax                      |

**Response**

```ruby
HTTP 200

{
    "status":"success",
    "company_id":"RomwoEjdjhws",
    "billinggroup_id":"Billing1"
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
            url="https://api.alphaus.cloud/m/ripple/billinggroup",
            headers={
                "Content-Type": "application/json;",
                "Authorization": auth
            },
            data=json.dumps({
                "display_cost": "true_unblended_cost",
                "phone": None,
                "billinggroup_id": "BG-SAMPLE-01",
                "billinggroup_name": "BG-SAMPLE-01",
                "inv_aggregate": True,
                "personal": None,
                "exchange_rate_type": None,
                "company_name": "BG-SAMPLE-01",
                "postal": None,
                "address": None,
                "billing_title": None,
                "remarks": None
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

請求グループリストの取得

**Role actions**

* `ReadBillingGroup`&#x20;
* `ModifyBillingGroup`&#x20;

**Request**

```http
GET /billinggroup HTTP1.1
Authorization: Bearer {token}
```

**Response**

```ruby
HTTP 200

[
    {
    "company_id":"RomwoEjdjhws",
    "billinggroup_id":"Billing1",
    "billinggroup_name":"Billing1",
    "name":"Billing1 Company",
    "invoices":{
        "aws": {
            "calc_type":"account",
            "currency":"jpy",
            "discount_calc_logic":"usageamount",
            "discount_rate":0,
            "discount_target_usage":"cloudpaywithfee",
            "substitution_fee":"percent",
            "substitution_fee_calc_target":"nondiscount",
            "substitution_fee_calc_type":"allsum",
            "substitution_fee_target_usage":"cloudpaywithfee",
            "substitution_fix":0,
            "substitution_rate":0,
            "support_amount_target":"allusage",
            "support_fee":"fix",
            "support_fee_calc_target":"nondiscount",
            "support_fix":0,
            "support_rate":0,
            "tax_rate":0
        }
        "azure": {
            "calc_type":"account",
            "currency":"jpy",
            "discount_calc_logic":"usageamount",
            "discount_rate":0,
            "discount_target_usage":"cloudpaywithfee",
            "substitution_fee":"percent",
            "substitution_fee_calc_target":"nondiscount",
            "substitution_fee_calc_type":"allsum",
            "substitution_fee_target_usage":"cloudpaywithfee",
            "substitution_fix":0,
            "substitution_rate":0,
            "support_amount_target":"allusage",
            "support_fee":"fix",
            "support_fee_calc_target":"nondiscount",
            "support_fix":0,
            "support_rate":0,
            "tax_rate":0
        }
    },
    "contact":"personal name",
    "address":"123 street",
    "postal":"12345",
    "phone":"03‐1234‐5678",
    "title":null,
    "req_generate":null,
    "remarks":null,
    "inv_aggregate":null,
    "project_id":null,
    "project_code":null,
    "project_label":null,
    "project_currency":null,
    "language":"ja",
    "qrcode":false,
    "invoice_template_id":null,
    "custom_fields":null,
    "untagged_groups":null,
    "account":[],
    "tag":[]
    },
    ...
]
```

## List details

請求グループ詳細の取得

**Role actions**

* `ReadBillingGroup`&#x20;
* `ModifyBillingGroup`&#x20;

**Request**

```http
GET /billinggroup/{id}/resource HTTP1.1
Authorization: Bearer {token}
```

リクエストパラーメータの`{id}`は請求グループ内部ID`{company_id}`です

**Response**

```ruby
HTTP 200

{
    "company_id":"RomwoEjdjhws",
    "billinggroup_id":"Billing1",
    "billinggroup_name":"Billing1",
    "name":"Billing1 Company",
    "invoices":{
        "aws": {
            "calc_type":"account",
            "currency":"jpy",
            "discount_calc_logic":"usageamount",
            "discount_rate":0,
            "discount_target_usage":"cloudpaywithfee",
            "substitution_fee":"percent",
            "substitution_fee_calc_target":"nondiscount",
            "substitution_fee_calc_type":"allsum",
            "substitution_fee_target_usage":"cloudpaywithfee",
            "substitution_fix":0,
            "substitution_rate":0,
            "support_amount_target":"allusage",
            "support_fee":"fix",
            "support_fee_calc_target":"nondiscount",
            "support_fix":0,
            "support_rate":0,
            "tax_rate":0
        }
        "azure": {
            "calc_type":"account",
            "currency":"jpy",
            "discount_calc_logic":"usageamount",
            "discount_rate":0,
            "discount_target_usage":"cloudpaywithfee",
            "substitution_fee":"percent",
            "substitution_fee_calc_target":"nondiscount",
            "substitution_fee_calc_type":"allsum",
            "substitution_fee_target_usage":"cloudpaywithfee",
            "substitution_fix":0,
            "substitution_rate":0,
            "support_amount_target":"allusage",
            "support_fee":"fix",
            "support_fee_calc_target":"nondiscount",
            "support_fix":0,
            "support_rate":0,
            "tax_rate":0
        }
    },
    "contact":"personal name",
    "address":"123 street",
    "postal":"12345",
    "phone":"03‐1234‐5678",
    "title":null,
    "req_generate":null,
    "remarks":null,
    "inv_aggregate":null,
    "project_id":null,
    "project_code":null,
    "project_label":null,
    "project_currency":null,
    "language":"ja",
    "qrcode":false,
    "invoice_template_id":null,
    "custom_fields":null,
    "untagged_groups":null,
    "account":[],
    "tag":[]
}
```

## Update

請求グループ情報の更新

**Role actions**

* `ModifyBillingGroup`&#x20;

**Request**

```http
POST /billinggroup/{id} HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
    "billinggroup_id":"Billing1",
    "billinggroup_name":"Billing1",
    "company_name":"Billing1 Company",
    "phone":"03-123-4567",
    "postal":"1243",
    "address":"updateed address",
    "billing_title":null,
    "personal":"Personal name",
    "remarks":"Some remarks data",
    "inv_aggregate":false,
    "project_id":"{created_project_id}",
    "language": "ja"
}
```

| Field              | Type      | Required | Validation       | Description                             |
| ------------------ | --------- | -------- | ---------------- | --------------------------------------- |
| billinggroup\_id   | _string_  | Yes      | -                | Billing group ID                        |
| billinggroup\_name | _string_  | Yes      | 長さ 1 \~ 100      | Billing group Name                      |
| company\_name      | _string_  | Yes      | 長さ 1 \~ 100      | Company name                            |
| phone              | _string_  | No       | 長さ 12 \~ 16      | Tel                                     |
| postal             | _string_  | No       | 長さ 4 \~ 10       | Postal                                  |
| address            | _string_  | No       | 長さ 1 \~ 100      | Address                                 |
| billing\_title     | _string_  | No       | 長さ 1 \~ 100      | Invoice title                           |
| personal           | _string_  | No       | 長さ 1 \~ 100      | Personal name                           |
| remarks            | _string_  | No       | 長さ 1 \~ 100      | Memo                                    |
| inv\_aggregate     | _boolean_ | No       |                  | Displaying invoice in bulk or by vendor |
| project\_id        | _string_  | No       |                  | Project id                              |
| language           | _string_  | No       | サポート: `ja`, `en` | Display invoice language setting        |

**Response**

```ruby
HTTP 200

{"status":"success"}
```

## Update invoice setting

請求グループ請求書設定の更新

**Role actions**

* `ModifyBillingGroup`&#x20;

**Request**

```http
POST /billinggroup/{id}/invoices HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "invoices": {
    "calc_type":"account",
    "currency":"jpy",
    "discount_calc_logic":"usageamount",
    "discount_rate":0,
    "discount_target_usage":"cloudpaywithfee",
    "substitution_fee":"percent",
    "substitution_fee_calc_target":"nondiscount",
    "substitution_fee_calc_type":"allsum",
    "substitution_fee_target_usage":"cloudpaywithfee",
    "substitution_fix":0,
    "substitution_rate":0,
    "support_amount_target":"allusage",
    "support_fee":"fix",
    "support_fee_calc_target":"nondiscount",
    "support_fix":0,
    "support_rate":0,
    "tax_rate":0.10
  },
  "vendor":"{vendor}"
}
```

| Field                            | Type     | Required | Validation                                                                                | Description  |
| -------------------------------- | -------- | -------- | ----------------------------------------------------------------------------------------- | ------------ |
| calc\_type                       | _string_ | Yes      | account,tag                                                                               | 計算タイプ        |
| currency                         | _string_ | Yes      | jpy,usd                                                                                   | 通貨           |
| discount\_calc\_logic            | _string_ | Yes      | usageamount,allamount                                                                     | 値引き対象        |
| discount\_rate                   | _double_ | Yes      | 0 \~ 1                                                                                    | 値引率          |
| discount\_target\_usage          | _string_ | Yes      | cloudpayonly ,cloudpaywithfee                                                             | 値引き計算方法      |
| substitution\_fee                | _string_ | Yes      | percent, fix, automatic, usagetable                                                       | 代行手数料請求方法    |
| substitution\_fee\_calc\_target  | _string_ | Yes      | cloudpayonly, cloudpaywithfee                                                             | 代行手数料計算対象    |
| substitution\_fee\_calc\_type    | _string_ | Yes      | allsum, account                                                                           | 請求代行サービス計算方法 |
| substitution\_fee\_target\_usage | _string_ | Yes      | nondiscount, discounted                                                                   | 請求代行手数料対象    |
| substitution\_fix                | _double_ | Yes      | 0 \~ 1,000,000                                                                            | 代行手数料 固定     |
| substitution\_rate               | _double_ | Yes      | 0 \~ 1                                                                                    | 代行手数料 (%)    |
| support\_amount\_target          | _string_ | Yes      | allusage, cloudpayonlywithfee                                                             | 表示なし         |
| support\_fee                     | _string_ | Yes      | - aws percent, aws\_developer, aws\_business, aws\_enterprise, fix   - azure percent, fix | サポート料請求方法    |
| support\_fee\_calc\_target       | _string_ | Yes      | cloudpayonly, cloudpaywithfee                                                             | サポート料計算対象    |
| support\_fix                     | _double_ | Yes      | 0 \~ 1,000,000                                                                            | サポート料 固定     |
| support\_rate                    | _double_ | Yes      | 0 \~ 1                                                                                    | サポート料 %      |
| tax\_rate                        | _double_ | Yes      | 0 \~ 0.08                                                                                 | 消費税率 %       |

**Response**

```ruby
HTTP 200

{"status":"success"}
```

## Update free format

請求グループその他費用の追加・更新

**Role actions**

* `ModifyBillingGroup`&#x20;

**Request**

```http
POST /billinggroup/{id}/freeformat/{vendor} HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

リクエストパラーメータの`{id}`は請求グループ内部ID`{company_id}`です。

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
    "additional_items":[
    {
        "enabled":true,
        "label":"testlabel",
        "unit_cost":1,
        "quantity":10000,
        "total":10000
    }
]
}
```

**additional\_items object description**

| Field      | Type      | Required | Validation | Description |
| ---------- | --------- | -------- | ---------- | ----------- |
| enabled    | _boolean_ | Yes      | -          | 有効、無効       |
| label      | _string_  | Yes      | 長さ 1 \~ 60 | タイトル        |
| unit\_cost | _double_  | Yes      | -          | 単価          |
| quantity   | _double_  | Yes      | -          | 数量          |
| total      | _double_  | Yes      | -          | 金額          |

**Response**

```ruby
HTTP 200

{"status":"success"}
```

## Delete free format

請求グループその他費用の削除

**Role actions**

* `ModifyBillingGroup`&#x20;

**Request**

```http
DELETE /billinggroup/{id}/freeformat/{vendor} HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json
```

リクエストパラーメータの`{id}`は請求グループ内部ID`{company_id}`です。

請求グループに追加されているその他費用を全て削除します。

**Response**

```ruby
HTTP 200

{"status":"success"}
```

## Update Invoice Template

請求グループ請求テンプレートの更新

**Role actions**

* `ModifyBillingGroup`&#x20;

**Request**

```http
POST /billinggroup/{id}/invoicetemplate HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

リクエストパラーメータの`{id}`は請求グループ内部ID`{company_id}`です

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
    "invoice_template_id": "abcdefg"
}
```

**Response**

```ruby
HTTP 200

{"status":"success"}
```

## Delete

請求グループの削除

**Role actions**

* `ModifyBillingGroup`&#x20;

**Request**

```http
DELETE /billinggroup/{id} HTTP1.1
Authorization: Bearer {token}
```

**Response**

```ruby
HTTP 200

{"status":"success"}
```
