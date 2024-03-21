# Wave for Reseller

リセラーのAPIリファレンスは以下の通りです。

## Create reseller account

リセラーアカウントの発行

**Role actions**

* `ModifyReseller`

**Request**

```http
POST /reseller HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "email":"alphaus-cloud@alphaus.cloud",
  "company_id":"company1",
  "input_type":"Auto",
  "notification":true,
  "password":null
}
```

**request body description**

| Field        | Type      | Required | Validation      | Description                             |
| ------------ | --------- | -------- | --------------- | --------------------------------------- |
| email        | _string_  | Yes      | -               | Eメールアドレス                                |
| company\_id  | _string_  | Yes      | -               | 請求グループ内部ID                              |
| input\_type  | _string_  | Yes      | - Auto / Custom | Auto: パスワード自動生成 Custom: passwordを入力     |
| notification | _boolean_ | Yes      | -               | 作成時に通知をする/しない                           |
| password     | _string_  | No       | -               | パスワード                                   |
| meta         | \[object] | Yes      | -               | Wave機能表示設定。[metaについて](reseller.md#meta) |

**Response**

```ruby
HTTP 200

{
  "status":"success"
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
            url="https://api.alphaus.cloud/m/ripple/reseller",
            headers={
                "Content-Type": "application/json;",
                "Authorization": auth
            },
            data=json.dumps({
                "email": "reseller@waveresellersample.cloud",
                "notification": True,
                "meta": {
                    "usage_report_download": True,
                    "usage_account_menu_fees_fee": False,
                    "ri_utilization": False,
                    "usage_account_menu_account_edit": False,
                    "usage_account": True,
                    "usage_account_graph": True,
                    "usage_tag_graph": True,
                    "usage_account_menu_fees_refund": False,
                    "invoice_download_csv_merged": False,
                    "invoice_download_csv_discount": False,
                    "usage_account_menu_fees_other_fees": False,
                    "usage_account_menu_fees_credit": False,
                    "ri_purchased": False,
                    "open_api": False,
                    "dashboard_graph": True,
                    "usage_group": True,
                    "report_filters": False,
                    "usage_tag": True,
                    "ri_recommendation": False,
                    "invoice": False,
                    "usage_crosstag_graph": True,
                    "users_management": False,
                    "usage_account_menu_budget": False,
                    "usage_account_menu_budget_edit": False,
                    "usage_group_graph": True,
                    "usage_crosstag": True,
                    "aq_coverage_ratio": False,
                    "aq_sp_management": False,
                    "aq_right_sizing": False,
                    "aq_ri_sp_instances": False,
                    "aq_ri_management": False,
                    "sp_purchased": False,
                    "aq_scheduling": False,
                    "aqua_link": False
                },
                "company_id": "{company_id}",
                "input_type": "Auto"
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

## Get reseller account list

リセラーアカウントの取得

**Role actions**

* `ReadReseller`
* `ModifyReseller`

**Request**

```http
POST /reseller HTTP1.1
Authorization: Bearer {token}
```

**Response**

```ruby
HTTP 200

[
  {
    "user_id":"userid1",
    "billinggroup_id":"billing1",
    "billinggroup_name":"billingname1",
    "email":"alphaus-cloud@alphaus.cloud",
    "company_id":"company1",
    "update_time":null,
    "password_update_time":null,
    "wave_registered":"2020-01-01T10:00:00+09:00",
    "meta": {
      "aq_coverage_ratio":false
      "aq_ri_management":false
      "aq_ri_sp_instances":false
      "aq_right_sizing":false
      "aq_scheduling":false
      "aq_sp_management":false
      "dashboard_graph":true
      "usage_account":true
      "usage_account_graph":true
      "usage_account_menu_account_edit":false
      "usage_account_menu_budget":false
      "usage_account_menu_budget_edit":false
      "usage_account_menu_fees_fee":false
      "usage_account_menu_fees_credit":false
      "usage_account_menu_fees_refund":false
      "usage_account_menu_fees_other_fees":false
      "usage_report_download":true
      "usage_group":true
      "usage_group_graph":true
      "usage_tag":true
      "usage_tag_graph":true
      "usage_crosstag":true
      "usage_crosstag_graph":true
      "ri_purchased":true
      "ri_utilization":false
      "ri_recommendation":false
      "sp_purchased":false
      "invoice":false
      "invoice_download_csv_discount":false
      "invoice_download_csv_merged":false
      "open_api":false
      "users_management":false
      "report_filters":false
    }
  },
  ...
]
```

## Delete reseller account

リセラーアカウントの削除

**Role actions**

* `ModifyReseller`

**Request**

```http
DELETE /reseller/{user_id} HTTP1.1
Authorization: Bearer {token}
```

**{user\_id}**

リセラーアカウントのidを指定する

**Response**

```ruby
HTTP 200

{
  "status":"success"
}
```

## Update password for reseller account

リセラーアカウントパスワードの変更

**Role actions**

* `ModifyReseller`

**Request**

```http
PUT /reseller/{user_id}/password HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

**{user\_id}**

リセラーアカウントのidを指定する

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "input_type":"Auto",
  "notification":true,
  "password":null
}
```

**request body description**

| Field        | Type      | Required | Validation      | Description                         |
| ------------ | --------- | -------- | --------------- | ----------------------------------- |
| input\_type  | _string_  | Yes      | - Auto / Custom | Auto: パスワード自動生成 Custom: passwordを入力 |
| notification | _boolean_ | Yes      | -               | 変更時に通知をする/しない                       |
| password     | _string_  | No       | -               | パスワード                               |

**Response**

```ruby
HTTP 200

{
  "status":"success"
}
```

## Update meta for reseller account

リセラーアカウントメタ情報の変更

**Role actions**

* `ModifyReseller`

**Request**

```http
PUT /reseller/{user_id}/meta HTTP1.1
Authorization: Bearer {token}
Content-Type: application/json

{request body}
```

**{user\_id}**

リセラーアカウントのidを指定する

以下に`{request body}`のリクエストペイロードの例を示します。

**{request body}**

```ruby
{
  "meta": {
      "aq_coverage_ratio":false
      "aq_ri_management":false
      "aq_ri_sp_instances":false
      "aq_right_sizing":false
      "aq_scheduling":false
      "aq_sp_management":false
      "dashboard_graph":true
      "usage_account":true
      "usage_account_graph":true
      "usage_account_menu_account_edit":false
      "usage_account_menu_budget":false
      "usage_account_menu_budget_edit":false
      "usage_account_menu_fees_fee":false
      "usage_account_menu_fees_credit":false
      "usage_account_menu_fees_refund":false
      "usage_account_menu_fees_other_fees":false
      "usage_report_download":true
      "usage_group":true
      "usage_group_graph":true
      "usage_tag":true
      "usage_tag_graph":true
      "usage_crosstag":true
      "usage_crosstag_graph":true
      "ri_purchased":true
      "ri_utilization":false
      "ri_recommendation":false
      "sp_purchased":false
      "invoice":false
      "invoice_download_csv_discount":false
      "invoice_download_csv_merged":false
      "open_api":false
      "users_management":false
      "report_filters":false
    }
}
```

**request body description**

| Field | Type      | Required | Validation | Description                             |
| ----- | --------- | -------- | ---------- | --------------------------------------- |
| meta  | \[object] | Yes      | -          | Wave機能表示設定。[metaについて](reseller.md#meta) |

**Response**

```ruby
HTTP 200

{
  "status":"success"
}
```

### meta

metaのリストを示します。

`Default`はリセラーアカウントを発行する際に設定されるデフォルトの設定です。

| aq\_coverage\_ratio                     | _boolean_ | false | Aqua インスタン適用率                | インスタン適用率ページの表示                        |
| --------------------------------------- | --------- | ----- | ---------------------------- | ------------------------------------- |
| aq\_ri\_management                      | _boolean_ | false | Aqua RI管理                    | RI管理ページの表示                            |
| aq\_ri\_sp\_instances                   | _boolean_ | false | Aqua RI/SP                   | RI/SPレコメンデーションページの表示                  |
| aq\_right\_sizing                       | _boolean_ | false | Aqua ライトサイジング                | ライトサイジングページの表示                        |
| aq\_scheduling                          | _boolean_ | false |  Aqua スケジューリング               | スケジューリングページの表示                        |
| aq\_sp\_management                      | _boolean_ | false | Aqua SP管理                    | SP管理ページの表示                            |
| dashboard\_graph                        | _boolean_ | true  | ダッシュボード                      | ダッシュボードグラフの表示                         |
| usage\_account                          | _boolean_ | true  | アカウントレポート                    | アカウント利用明細の表示 \[Account]               |
| usage\_account\_graph                   | _boolean_ | true  | グラフの表示 \[アカウント]              | アカウント利用明細グラフの表示 \[Account]            |
| usage\_account\_menu\_account\_edit     | _boolean_ | false | アカウント名の編集                    | アカウント名の編集 \[Account]                  |
| usage\_account\_menu\_budget            | _boolean_ | false | バジェットの表示 \[アカウント]            | バジェット設定の表示 \[Account]                 |
| usage\_account\_menu\_budget\_edit      | _boolean_ | false | バジェットの編集 \[アカウント]            | バジェット設定の編集 \[Account]                 |
| usage\_account\_menu\_fees\_fee         | _boolean_ | false | Feeの表示 \[アカウント > その他明細情報]    | Feeの表示 \[Account]                     |
| usage\_account\_menu\_fees\_credit      | _boolean_ | false | Creditの表示 \[アカウント > その他明細情報] | Creditの表示 \[Account]                  |
| usage\_account\_menu\_fees\_refund      | _boolean_ | false | Refundの表示 \[アカウント > その他明細情報] | Refundの表示 \[Account]                  |
| usage\_account\_menu\_fees\_other\_fees | _boolean_ | false | その他Feeの表示 \[アカウント > その他明細情報] | その他Feeの表示 \[Account]                  |
| usage\_report\_download                 | _boolean_ | true  | レポートのダウンロード \[アカウント]         | 利用明細レポートのダウンロード表示 \[Account]          |
| usage\_group                            | _boolean_ | true  | グループレポート                     | 利用明細の表示 \[Group]                      |
| usage\_group\_graph                     | _boolean_ | true  | グラフの表示 \[グループ]               | 利用明細グラフの表示 \[Group]                   |
| usage\_tag                              | _boolean_ | true  | タグレポート                       | 利用明細の表示 \[Tag]                        |
| usage\_tag\_graph                       | _boolean_ | true  | グラフの表示 \[タグ]                 | 利用明細グラフの表示 \[Tag]                     |
| usage\_crosstag                         | _boolean_ | true  | クロスタグレポート                    | 利用明細の表示 \[Cross Tag]                  |
| usage\_crosstag\_graph                  | _boolean_ | true  | グラフの表示 \[クロスタグ]              | 利用明細グラフの表示 \[Cross Tag]               |
| ri\_purchased                           | _boolean_ | true  | 購入済みRIの表示                    | 購入済みRIの表示                             |
| ri\_utilization                         | _boolean_ | false | RI適用率の表示                     | RI適用率の表示                              |
| ri\_recommendation                      | _boolean_ | false | レコメンデーションの表示                 | RIレコメンデーションの表示                        |
| sp\_purchased                           | _boolean_ | false | 購入済みSavingsPlansの表示          | 購入済みSavingsPlansの表示                   |
| invoice                                 | _boolean_ | false | 請求書の表示                       | ご利用明細の表示                              |
| invoice\_download\_csv\_discount        | _boolean_ | false | 割引詳細CSVのダウンロード               | 割引詳細CSVのダウンロード \[Usage details]       |
| invoice\_download\_csv\_merged          | _boolean_ | false | 請求書（統合版）CSVのダウンロード           | 請求書（統合版）CSVのダウンロード \[Usage details]   |
| open\_api                               | _boolean_ | false | API アクセストークン                 | API アクセストークン \[Settings]              |
| users\_management                       | _boolean_ | false | サブユーザー管理                     | サブユーザー管理 \[Settings]                  |
| report\_filters                         | _boolean_ | false | レポートフィルター                    | レポートフィルター                             |
| budgetalerts                            | _boolean_ | false | 予算超過通知                       | 予算に関するアラート \[Notification]\[Settings] |

