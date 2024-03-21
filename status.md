# Status

The following endpoint is the base url for the APIs below.

```
https://service.mobingi.com/m/status/
```

## List invoice calculation status

Get the current status of the invoice calculations.

**Request**

```http
GET calculations/status[?params] HTTP1.1
Authorization: Bearer {token}
```

Details for `params`.

| Key      | Value                                                                                        |
| -------- | -------------------------------------------------------------------------------------------- |
| `vendor` | Optional. Supported vendor is only `aws`  at the moment.                                     |
| `from`   | Optional. If not provided, default value is 2 months before current month. Format: `yyyymm`. |
| `to`     | Optional. If not provided, default value is current month. Format: `yyyymm`.                 |

**Response**

```ruby
HTTP/1.1 200 OK

[
  {
    "billing_month": "2020-04",
    "end_time": "",
    "finished": 3090,
    "id": "MSP-123456",
    "invoice_type": "account",
    "msp": "MSP-123456",
    "name": "Alphaus, Inc.",
    "run_id": "MSP-123456/76f20b9a-b7fa-4599-bc38-0691dbbd4ea3",
    "start_time": "2020-05-22T04:01:38Z",
    "status": "checking",
    "status_message": "checking",
    "total": 3090,
    "type": "msp",
    "vendor": "aws"
  },
  ...
]
```

Examples:

```bash
# Simple request. Get status for the past 2 months:
GET calculations/status

# If you want range from Oct 2019 - Jan 2020:
GET calculations/status?from=201910&to=202001
```
