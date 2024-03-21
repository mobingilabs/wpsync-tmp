# Deployments

The following is the API reference for working with Ocean deployments.

## Create a deployment

Deploy an Ocean template.

**Request**

```http
POST /v0/deployments HTTP1.1
Authorization: Bearer {token}

{template body}
```

**Response**

```ruby
HTTP 202
```

Check out [https://github.com/mobingi/ocean-template-examples](https://github.com/mobingi/ocean-template-examples) for examples about `{template body}`. For details on how to write Ocean templates, check out this [reference](https://docs.mobingi.com/v/ocean-en/template-2018-07-02).

## List deployments

{% hint style="warning" %}
This page is still a work in progress.
{% endhint %}

Get a list of available deployments.

```http
REQUEST
GET /v0/deployments HTTP1.1
Authorization: Bearer {token}

---
RESPONSE
HTTP 200
tbd
```

## Describe a deployment

Describe a specific deployment based on name.

**Request**

```http
GET /v0/deployments/{name} HTTP1.1
Authorization: Bearer {token}
```

`{name}` is the template (or deployment) name.

**Response**

```ruby
HTTP 200

{
  "deployment":"template-name",
  "stacks":[
    {
      "name":"stack-name",
      "items":[
        {
          "name":"eksmaster",
          "resources":[
            {
              "key": "AWS::EC2::InternetGateway",
              "value": "igw-040e7443b67d8cda2"
            },
            {
              "key": "AWS::EC2::Route",
              "value": "aws-5-Route-1PR9K4JNZTQRY"
            }
          ],
          "status": "creating|updating|completed|failed"
        },
        {
          "name":"cfnextra",
          "resources":[
            {
              "key": "AWS::SNS::SNSTopic",
              "value": "arn:aws:sns:ap-northeast-1:...cfnextra-sample-snstopic"
            },
          ],
          "status": "creating|updating|completed|failed"
        }
      ],
      "region": "ap-northeast-1"
    }     
  ]
}
```

## Delete a deployment

Delete a deployment and all associated applications and resources.

**Request**

```http
DELETE /v0/deployments/{name}[?force=true] HTTP1.1
Authorization: Bearer {token}
```

`{name}` is the template (or deployment) name. For templates that have dependency to other templates, API will return error stating the dependency. If the parameter `force=true` is specified, the template resources will be deleted including all dependencies.

**Response**

For successful responses, server will return `HTTP 202`. Errors will return `HTTP 422`.
