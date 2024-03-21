# Report

**基本的なResponse Format**

```javascript
{
    vendor : [
        {
            id : string
            name : string
            date : [
                {
                    blended_cost : number
                    date : string
                    timestamp : number
                    true_unblended_cost : number
                    unblended_cost : number
                },...
            ]
        },...
    ]
}
```

| Response value        | type     | description                                         |
| --------------------- | -------- | --------------------------------------------------- |
| `vendor`              | _array_  | パラメーターで指定したvendor 例 : `aws`                         |
| `id`                  | _string_ | - `service` servicename    - `account` account id   |
| `name`                | _string_ | - `service` servicename    - `account` account name |
| `date`                | _array_  | 取得したデータ一覧                                           |
| `date`                | _string_ | - `monthly` : `YYYY-MM`   - `daily` : `YYYY-MM-DD`  |
| `timestamp`           | _number_ | `date` のUNIXタイムスタンプ                                 |
| `blended_cost`        | _number_ | AWS CURの `lineitem/blendedcost`                     |
| `unblended_cost`      | _number_ | AWS CURの `lineitem/unblendedcost`                   |
| `true_unblended_cost` | _number_ | mobingiで再計算したunblendedcost                          |

## レポートの取得

**Request**

```http
GET /v1/reports/{owner}/{resolution}?from={from}&to={to}&by={by}&vendor={vendor} HTTP1.1
Content-Type: application/json
```

`{Path Variables}` の例

| Path         | description                      |
| ------------ | -------------------------------- |
| `owner`      | 使用可能な値   - `company`             |
| `resolution` | 使用可能な値   - `monthly`   - `daily` |

`Request URL` の例

```ruby
GET /v1/reports/company/monthly?from=2019-01-01&to=2019-02-01&by=service&vendor=aws
```

| Params   | description                                                                                                       |
| -------- | ----------------------------------------------------------------------------------------------------------------- |
| `from`   | 型 : _string_   フォーマット : _YYYY-MM\_DD_   説明 :   Monthly は自動的に `YYYY-MM`へ変換されます。   Daily は自動的に `YYYY-MM-DD`へ変換されます。 |
| `to`     | 型 : _string_   フォーマット : _YYYY-MM\_DD_   説明 :   Monthly は自動的に `YYYY-MM`へ変換されます。   Daily は自動的に `YYYY-MM-DD`へ変換されます。 |
| `by`     | **使用可能な値**   型 : _string_   - `service`   - `account`                                                             |
| `vendor` | **使用可能な値**   型 : _string_   - `aws`                                                                               |
