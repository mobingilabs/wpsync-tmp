# Project

プロジェクトのAPIリファレンスは以下の通りです。

## Create project

プロジェクトの作成

**Role actions**

* `ModifyProject`

**Request**

```http
POST /project HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
    "project_code":"project 1",
    "project_label":"project 1 description",
    "project_currency":"jpy"
}
```

**{request body} description**

| Field             | Type     | Required | Validation             | Description |
| ----------------- | -------- | -------- | ---------------------- | ----------- |
| project\_code     | _string_ | Yes      | 1 \~ 40                | プロジェクトコード   |
| project\_label    | _string_ | Yes      | 1 \~ 40                | プロジェクトラベル   |
| project\_currency | _string_ | Yes      | support:\['jpy','usd'] | 表示通貨        |

**Response**

```ruby
HTTP 200

{"status":"success"}
```

## Edit project

プロジェクトの更新

**Role actions**

* `ModifyProject`

**Request**

```http
POST /project/{project_id} HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

```ruby
{
    "project_code":"project 1",
    "project_label":"project 1 description",
    "project_currency":"jpy"
}
```

**{request body} description**

| Field             | Type     | Required | Validation             | Description |
| ----------------- | -------- | -------- | ---------------------- | ----------- |
| project\_code     | _string_ | Yes      | 1 \~ 40                | プロジェクトコード   |
| project\_label    | _string_ | Yes      | 1 \~ 40                | プロジェクトラベル   |
| project\_currency | _string_ | Yes      | support:\['jpy','usd'] | 表示通貨        |

## Delete project

プロジェクトの削除

**Role actions**

* `ModifyProject`

**Request**

```http
DELETE /project/{project_id} HTTP1.1
Authorization: Bearer {token}
```

## Get project list

プロジェクト一覧の取得

**Role actions**

* `ReadProject`
* `ModifyProject`

**Request**

```http
GET /project HTTP1.1
Authorization: Bearer {token}
```

**Response**

```ruby
HTTP 200

[
  {
    "project_id": "prj-ad413e7b2006f3746r790a",
    "project_code": "プロジェクト1",
    "project_label": "プロジェクト1のラベル",
    "project_currency": "jpy"
  },
  {
    "project_id": "prj-fdf13fvf7vb61h0llr6519",
    "project_code": "プロジェクト2",
    "project_label": "プロジェクト2のラベル",
    "project_currency": "jpy"
  },...
]
```

## Get project list per month

月のプロジェクトデータの取得

**Role actions**

* `ReadProject`
* `ModifyProject`

**Request**

```http
GET /project/{month} HTTP1.1
Authorization: Bearer {token}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01

**Response**

```ruby
HTTP 200

[
  {
    "project_id": "prj-ad413e7b2006f3746r790a",
    "project_code": "プロジェクト1",
    "project_label": "プロジェクト1のラベル",
    "project_currency": "jpy",
    "amount": {
      "aws": {
        "profit": 0,
        "profit_exchanged": 0,
        "profit_exchanged_rate": 0,
        "profit_rate": 0,
        "sales": 0,
        "sales_exchanged": 0,
        "stock": 0,
        "stock_exchanged": 0
      },
      "total": {
        "profit": 0,
        "profit_exchanged": 0,
        "profit_exchanged_rate": 0,
        "profit_rate": 0,
        "sales": 0,
        "sales_exchanged": 0,
        "stock": 0,
        "stock_exchanged": 0
      }
    }
  },...
]
```

## Export project list per month

月のプロジェクトデータCSVの出力

**Role actions**

* `ReadProject`
* `ModifyProject`

**Request**

```http
GET /project/{month}/csv HTTP1.1
Authorization: Bearer {token}
```

リクエストパラーメータの`{month}`のフォーマット: `yyyy-mm` 例: 2020-01

**Response**

```ruby
HTTP 200

{
    "status":"success",
    "data":"csv url"
}
```
