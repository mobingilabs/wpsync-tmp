# RI management

RI管理のAPIリファレンスは以下の通りです。

## Get RI purchased list

RI管理データの取得

**Role actions**

* `ReadRi`
* `ModifyRi`

**Request**

```http
GET /ri/purchased?vendor={vendor} HTTP1.1
Authorization: Bearer {token}
```

リクエストパラーメータの`{vendor}`のサポートベンダー: `aws`,`azure`

**Response**

```ruby
HTTP 200

[
  {
    "billinggroup_id":"billinggroup1",
    "billinggroup_name":"billinggroup1",
    "customer_id":"012345678912",
    "customer_name":"customer1",
    "dest_customer_id":"",
    "end":"2021-12-03T00:00:00Z",
    "id":"ATEcVTAzVDA4pbnN6YW5jZXXFZbRmNGFlMTAtYWV",
    "arn":"arn:aws:ec2:ap-northeast-1:012345678912:reserved-instances\/adbcderf-cdef-xwcs-ecqx-5vfbk2767xxs",
    "instance_type":"t2.large",
    "modification_status":"Original",
    "normalization_factor":4,
    "number":1,
    "offer_class":"standard",
    "paid_by":"PaidByOwner",
    "payment_option":"All Upfront",
    "platform":"Linux\/UNIX",
    "region":"ap-northeast-1",
    "remove":false,
    "scope":"Region",
    "service":"AmazonEC2",
    "start":"2020-12-03T00:00:00Z",
    "tenancy":"Shared",
    "term_length":"1yr",
    "unblended_rate":0,
    "upfront_value":672,
    "usage_type":"APN1-HeavyUsage:t2.large",
    "vendor":"aws",
    "zone":"",
    "disabled":false
  },
  ...
]
```

## Move RI purchased

RI管理データの移動

**Role actions**

* `ModifyRi`

**Request**

```http
POST /ri/purchased/{ri_id} HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

リクエストパラーメータの`{ri_id}` `GET /ri/purchased?vendor={vendor}`から取得したid

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
    "customer_id":"123456789123",
    "number":1,
    "vendor":"aws"
}
```

**{request body} description**

| Field        | Type      | Required | Validation  | Description |
| ------------ | --------- | -------- | ----------- | ----------- |
| customer\_id | _string_  | Yes      | -           | 移動先の顧客ID    |
| number       | _integer_ | Yes      | -           | 移動する数       |
| vendor       | _string_  | Yes      | サポート: `aws` | ベンダー        |

**Response**

```ruby
HTTP 200

{"status":"success"}
```

## Remove RI purchased

移動先のRI管理データを元のデータへ戻す

**Role actions**

* `ModifyRi`

**Request**

```http
POST /ri/purchased/{ri_id}/remove?vendor={vendor} HTTP1.1
Authorization: Bearer {token}
```

リクエストパラーメータの`{ri_id}` `GET /ri/purchased?vendor={vendor}`から取得したid \
リクエストパラーメータの`{vendor}`のサポートベンダー: `aws`

**Response**

```ruby
HTTP 200

{"status":"success"}
```
