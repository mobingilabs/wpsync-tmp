# ALM (v3)

{% hint style="warning" %}
This API is already deprecated.
{% endhint %}

## Endpoint

```
https://api.mobingi.com
```

## OAuth Authentication

In order to interact with the API, your application must authenticate. Mobingi API handles this through **OAuth**. An OAuth token functions as a complete authentication request. In effect, it acts as a substitute for a username and password pair.

_To get an OAuth token, make a POST request to_

POST `/v3/access_token`

| **Parameters** | **Type** | **Required** | **Detail**                                                                                                                                                                                                                                                                                                                     |
| -------------- | -------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| client\_id     | string   | Yes          |                                                                                                                                                                                                                                                                                                                                |
| client\_secret | string   | Yes          |                                                                                                                                                                                                                                                                                                                                |
| grant\_type    | string   | Yes          | This value is either `client_credentials` or `password`.  If you grant with password, you are interacting the same as working around Mobingi UI; If you grant with client\_credentials, you are acting as an Alm-Agent, and most resource related permissions are denied by [RBAC](https://learn.mobingi.com/enterprise/rbac). |

Example Request:

```bash
curl -X POST https://api.mobingi.com/v3/access_token \
-H "Content-Type: application/json" \
-d '{"grant_type":"client_credentials","client_id":"lg-5447820c870e1-xBV0OpTEN-tm","client_secret":"sFVYDoe07fxPjNgYvauYGOYCeXbOTE"}'
```

Response Body:

```javascript
{
  "token_type": "Bearer",
  "expires_in": 43200,
  "access_token": "eyJ0eXAiOiJQiLCJhbGciOMeXzQfME"
}
```

You can then start making API requests by passing the `access_token` value to the _Authorization_ Header

```
Authorization: Bearer eyJ0eXAiOiJQiLCJhbGciOMeXzQfME
```

## ALM Templates <a href="#alm-templates" id="alm-templates"></a>

### Apply Template <a href="#template-apply" id="template-apply"></a>

Applies the Mobingi Alm template and creates stack.

POST `/v3/alm/template`

| **Parameters**      | **Type** | **Required** | **Detail**                                      |
| ------------------- | -------- | ------------ | ----------------------------------------------- |
| { _template body_ } | string   | Yes          | Mobingi Alm template body in json string format |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Request body

```bash
{
  "version": "2017-03-03",
  "label": "template version label #1",
  "description": "This template creates a sample stack with EC2 instance on AWS",
  "vendor": {
    "aws": {
      "cred": "AKIAJ...DZLA",
      "region": "ap-northeast-1"
    }
  },
  "configurations": [
    {
      "role": "web",
      "flag": "Web01",
      "provision": {
        "image": "${computed}",
        "volume_type": "${computed}",
        "instance_type": "t2.micro",
        "instance_count": 1,
        "keypair": true
      }
    }
  ]
}
```

Response Body

```bash
HTTP/1.1 201 Created

{
  "status": "success",
  "stack_status": "CREATE_IN_PROGRESS",
  "stack_id": "mo-5447820c870e1-ZgNTSRM8K-tk",
  "version_id": "98O0jK6CQk8qLi14S2SLU8z3JIo3.JPx"
}
```

### Update Template <a href="#template-update" id="template-update"></a>

Updates the Mobingi Alm template and applies the changes to stack resources.

_Note:_ `vendor` section will be ignored when performing this API call. You can not change cloud vendors after the stack is created.

PUT `/v3/alm/template/{stack_id}`

| **Parameters**      | **Type** | **Required** | **Detail**                                      |
| ------------------- | -------- | ------------ | ----------------------------------------------- |
| { _template body_ } | string   | Yes          | Mobingi Alm template body in json string format |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Request body

```bash
{
  "version": "2017-03-03",
  "label": "template version label #2",
  "description": "This template creates a sample stack with EC2 instance on AWS",
  "vendor": {
    "aws": {
      "cred": "AKIAJ...DZLA",
      "region": "ap-northeast-1"
    }
  },
  "configurations": [
    {
      "role": "web",
      "flag": "Web01",
      "provision": {
        "image": "${computed}",
        "instance_type": "m3.medium",
        "instance_count": 2,
        "keypair": true
      }
    }
  ]
}
```

Response Body

```bash
HTTP/1.1 202 Accepted

{
  "status": "success",
  "stack_status": "UPDATE_IN_PROGRESS",
  "stack_id": "mo-5447820c870e1-ZgNTSRM8K-tk",
  "version_id": "gCn2JuPhndwxMZuidOER0yyxM8jZB6Vn"
}
```

### Compare Templates <a href="#template-compare" id="template-compare"></a>

Compares the resource changes between two Mobingi Alm templates.

POST `/v3/alm/template/compare`

| **Parameters** | **Type** | **Required** | **Detail**                                     |
| -------------- | -------- | ------------ | ---------------------------------------------- |
| id             | array    | conditional  | items contain stack id and version information |
| body           | array    | conditional  | items contain template body source             |

_Note: You can compare two templates by retrieving them from their version id and stack id. Or, you can compare a target template body (posted in json format to_ `body` _parameter) with a source template retrieved by its version id and stack id._

_The current API version only supports two templates comparison, and you always should retrieve the source template by its version id and stack id. That being said, you have to always pass at least one item in parameter_ `id` _array. Below are two examples:_

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Request body ( _Example 1_ )

```bash
{
    "id": [
        {
            "mo-5447826c870e7-ZgNTSRM8K-tk": {
                "version": "98O0jK5CQk8gLi14S2SLU8z3JIo3.JPx"
            }
        },
        {
            "mo-5447826c870e7-ZgNTSRM8K-tk": {
                "version": "gCn2JuPhndwxMZuodOER0yyxM8jZB6Vn"
            }
        }
    ]
}
```

Request body ( _Example 2_ )

```bash
{
    "id": [
        {
            "mo-5447826c870e7-9S3zWP7jM-tk": {
                "version": "dK7R9_PuclTqSysMniPTcmpE.5u58RVL"
            }
        }
    ],
    "body": [
        "{\n \"version\": \"2017-03-03\",\n \"label\": \"template version label #2\",\n \"description\": \"This template creates a sample stack with EC2 instance on AWS\",\n \"vendor\": {\n \"aws\": {\n \"cred\": \"AKIAJ...DZLA\",\n \"region\": \"ap-northeast-1\"\n }\n },\n \"configurations\": [\n {\n \"role\": \"web\",\n \"flag\": \"Web01\",\n \"provision\": {\n \"image\": \"${computed}\", \"instance_type\": \"m3.medium\",\n \"instance_count\": 2,\n \"keypair\": true,\n }\n }\n ]\n }"
    ]
}
```

Response Body

```bash
HTTP/1.1 202 Accepted

{
  "status": "success",
  "source":{
      "version": "2017-03-03",
      "label": "template version label #1",
      "description": "This template creates a sample stack with EC2 instance on AWS",
      "vendor": {
        "aws": {
          "cred": "AKIAJ...DZLA",
          "region": "ap-northeast-1"
        }
      },
      "configurations": [
        {
          "role": "web",
          "flag": "Web01",
          "provision": {
            "image": "${computed}",
            "instance_type": "t2.micro",
            "volume_type": "${computed}",
            "instance_count": 1,
            "keypair": true
          }
        }
      ]
  },
  "target": {
      "version": "2017-03-03",
      "label": "template version label #2",
      "description": "This template creates a sample stack with EC2 instance on AWS",
      "vendor": {
        "aws": {
          "cred": "AKIAJ...DZLA",
          "region": "ap-northeast-1"
        }
      },
      "configurations": [
        {
          "role": "web",
          "flag": "Web01",
          "provision": {
            "image": "${computed}",
            "instance_type": "m3.medium",
            "instance_count": 2,
            "keypair": true
          }
        }
      ]
  },
  "diff": {
    "new": [],
    "removed": {
      "configurations\/1\/provision\/volume_type": "${computed}"
    },
    "edited": {
      "label": {
        "oldvalue": "template version label #1",
        "newvalue": "template version label #2"
      },
      "configurations\/provision\/instance_type": {
        "oldvalue": "t2.micro",
        "newvalue": "m3.medium"
      },
      "configurations\/provision\/instance_count": {
        "oldvalue": 1,
        "newvalue": 2
      }
    }
  }
}
```

### Template Versions <a href="#template-list" id="template-list"></a>

List Mobingi Alm template versions

GET `/v3/alm/template`

| **Parameters** | **Type** | **Required** | **Detail**                                        |
| -------------- | -------- | ------------ | ------------------------------------------------- |
| stack\_id      | string   | Yes          | The unique id returned when applying the template |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```bash
HTTP/1.1 200 OK

[
  {
    "version_id": "1kk2HiGLxF1fThVLJvC0h6fd5z3QWOiM",
    "latest": true,
    "last_modified": "2017-08-25T10:40:29.000Z",
    "size": "2963"
  },
  {
    "version_id": "gCn2JuPhndwxMZuodOER0yyxM8jZB6Vn",
    "latest": false,
    "last_modified": "2017-08-25T10:20:38.000Z",
    "size": "211"
  },
  {
    "version_id": "98O0jK5CQk8gLi14S2SLU8z3JIo3.JPx",
    "latest": false,
    "last_modified": "2017-08-25T08:48:12.000Z",
    "size": "2940"
  }
]
```

### Describe Template <a href="#template-describe" id="template-describe"></a>

Describes the template body of a specific version.

GET `/v3/alm/template/{stack_id}`

| **Parameters** | **Type** | **Required** | **Detail**                                                                                                                             |
| -------------- | -------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| version\_id    | string   | No           | The id of the template version associated with the stack. If empty or "latest" provided, the most updated template version is returned |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```bash
HTTP/1.1 200 OK

{
  "version": "2017-03-03",
  "label": "template version label #2",
  "description": "This template creates a sample stack with EC2 instance on AWS",
  "vendor": {
    "aws": {
      "cred": "AKIAJ...DZLA",
      "region": "ap-northeast-1"
    }
  },
  "configurations": [
    {
      "role": "web",
      "flag": "Web01",
      "provision": {
        "image": "${computed}",
        "instance_type": "m3.medium",
        "instance_count": 2,
        "keypair": true
      }
    }
  ]
}
```

## Stacks <a href="#stacks" id="stacks"></a>

### List Stacks <a href="#stack-list" id="stack-list"></a>

List all stacks running under current organization account.

GET `/v3/alm/stack`

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```bash
HTTP/1.1 200 OK

[
    {
      "auth_token": "zQT8zJ37o9iZDIAFOVOoZzLCu0nR",
      "user_id": "5447820c870e1",
      "configuration": {
        "version": "2017-03-03",
        "label": "template version label #2",
        "description": "This template creates a sample stack with EC2 instance on AWS",
        "vendor": {
          "aws": {
            "cred": "AKIAJ...DZLA",
            "region": "ap-northeast-1"
          }
        },
        "configurations": [
          {
            "role": "web",
            "flag": "Web01",
            "provision": {
              "image": "${computed}",
              "instance_type": "m3.medium",
              "instance_count": 2,
              "keypair": true
            }
          }
        ]
      },
      "nickname": "clean sail demonstrate",
      "create_time": "2017-08-26T19:31:25+09:00",
      "username": "thompson",
      "stack_id": "mo-5447820c870e1-ZgNTSRM8K-tk",
      "stack_status": "CREATE_COMPLETE",
      "version_id": "1kk2HiGLxF1fThVLJvC0h6fd5z3QWOiM"
  },
  {
      ..
  }
]
```

### Describe Stack <a href="#stack-describe" id="stack-describe"></a>

Describes the stack detail information.

GET `/v3/alm/stack/{stack_id}`

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```bash
HTTP/1.1 200 OK

{
  "auth_token": "zQT8zJ37o9iZDIAFOVOoZzLCu0nR",
  "user_id": "5447820c870e1",
  "configuration": {
    "version": "2017-03-03",
    "label": "template version label #2",
    "description": "This template creates a sample stack with EC2 instance on AWS",
    "vendor": {
      "aws": {
        "cred": "AKIAJ...DZLA",
        "region": "ap-northeast-1"
      }
    },
    "configurations": [
      {
        "role": "web",
        "flag": "Web01",
        "provision": {
          "image": "${computed}",
          "instance_type": "m3.medium",
          "instance_count": 2,
          "keypair": true
        }
      }
    ]
  },
  "nickname": "clean sail demonstrate",
  "create_time": "2017-08-26T19:31:25+09:00",
  "username": "thompson",
  "stack_id": "mo-5447820c870e1-ZgNTSRM8K-tk",
  "stack_status": "CREATE_COMPLETE",
  "version_id": "1kk2HiGLxF1fThVLJvC0h6fd5z3QWOiM"
}
```

### Describe Container <a href="#describe-container" id="describe-container"></a>

Describes the stack container detail information.

GET `/v3/alm/container/{container_id}`

| **Parameters** | **Type** | **Required** | **Detail**                                         |
| -------------- | -------- | ------------ | -------------------------------------------------- |
| container\_id  | string   | Yes          | The id of the container associated with the stack. |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```javascript
HTTP/1.1 200 OK

{
    "container_id": "i-0c32760b85f60aca7",
    "agent_id": "e8b15d4f-f027-4c65-80f5-ded5858cd213",
    "update_time": "1510544332",
    "stack_id": "mo-5447820c870e1-ZgNTSRM8K-tk",
    "instance_id": "i-0c32760b85f60aca7",
    "status": "complete"
}
```

### List Containers <a href="#list-containers" id="list-containers"></a>

List the stack containers filtering by {stack\_id} or {instance\_id}

GET `/v3/alm/container`

| **Parameters** | **Type** | **Required** | **Detail**                                                                        |
| -------------- | -------- | ------------ | --------------------------------------------------------------------------------- |
| stack\_id      | string   | conditional  | If both {stack\_id} and {instance\_id} are presents, {stack\_id} will be ignored. |
| instance\_id   | string   | conditional  | The id of the instance.                                                           |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```javascript
HTTP/1.1 200 OK

[
    {
        "agent_id": "4e9a6b9a-2a1f-454e-be5e-847573b44f10",
        "container_id": "i-049a49d8881adf122",
        "update_time": "1510564361",
        "stack_id": "mo-5447820c870e1-ZgNTSRM8K-tk",
        "instance_id": "i-049a49d8881adf122",
        "status": "complete"
    },
    {
        "agent_id": "09afaa82-0a71-4680-a68a-91badaaaa134",
        "container_id": "i-0363778be4d1bc81f",
        "update_time": "1510564297",
        "stack_id": "mo-5447820c870e1-ZgNTSRM8K-tk",
        "instance_id": "i-0363778be4d1bc81f",
        "status": "complete"
    },
    {
        "agent_id": "9c075f08-81c9-4147-8974-f543640dccf6",
        "container_id": "i-02f27fcae984fc946",
        "update_time": "1510564348",
        "stack_id": "mo-5447820c870e1-ZgNTSRM8K-tk",
        "instance_id": "i-02f27fcae984fc946",
        "status": "complete"
    }
]
```

## RBAC <a href="#rbac" id="rbac"></a>

### Create Role <a href="#rbac-create-role" id="rbac-create-role"></a>

Creates a new role. (**Note: This endpoint can only be accessed by master account**)

POST `/v3/role`

| **Parameters** | **Type** | **Required** | **Detail**                         |
| -------------- | -------- | ------------ | ---------------------------------- |
| name           | string   | Yes          | Name of Mobingi Role               |
| scope          | string   | Yes          | Mobingi Role in json string format |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/x-www-form-urlencoded
```

Request body

```bash
{
  "name": "sample name",
  "scope": "{ _role scope body_ }"
```

Response Body

```bash
HTTP/1.1 200

{
  "status": "success",
  "role_id": "morole-544****0e1-ZgNTSRM8K-tk"
}
```

### Update Role <a href="#rbac-update-role" id="rbac-update-role"></a>

Updates an existing role.

**Note:** _This endpoint is denied to all users except master account, defined by_ [_default RBAC scope_](https://learn.mobingi.com/enterprise/rbac-reference#default-roles)_, and this scope cannot be overwritten._

PUT `/v3/role/{role_id}`

| **Parameters** | **Type** | **Required** | **Detail**                         |
| -------------- | -------- | ------------ | ---------------------------------- |
| name           | string   | Yes          | Name of Mobingi Role               |
| scope          | string   | Yes          | Mobingi Role in json string format |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/x-www-form-urlencoded
```

Request body

```bash
{
  "name": "sample name",
  "scope": "{ _role scope body_ }"
}
```

Response Body

```bash
HTTP/1.1 200

{
  "status": "success",
  "role_id": "morole-544****70e1-ZgNTSRM8K-tk"
}
```

### Delete Role <a href="#rbac-delete-role" id="rbac-delete-role"></a>

Deletes an existing Role.

**Note:** _This endpoint is denied to all users except master account, defined by_ [_default RBAC scope_](https://learn.mobingi.com/enterprise/rbac-reference#default-roles)_, and this scope cannot be overwritten._

DELETE `/v3/role/{role_id}`

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```bash
HTTP/1.1 200

{
  "status": "success",
  "role_id": "morole-544****0e1-ZgN****M8K-tk"
}
```

### List Roles <a href="#rbac-list-roles" id="rbac-list-roles"></a>

Lists all roles created under current account.

**Note:** _This endpoint is denied to all users except master account, defined by_ [_default RBAC scope_](https://learn.mobingi.com/enterprise/rbac-reference#default-roles)_, and this scope cannot be overwritten._

GET `/v3/role`

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```bash
HTTP/1.1 200
[
    {
        "role_id": "morole-544****0e1-ZgNT***M8K-tk",
        "user_id": "544****0e1",
        "name": "sample name",
        "scope": "{ _role scope body_ }",
        "create_time": "",
        "update_time": ""
    },
    {
        ....
    }
]
```

### Describe Roles <a href="#rbac-describe-roles" id="rbac-describe-roles"></a>

1.  **Describe roles attached to the user**

    Lists all roles attached to a user specified by username.

    **Note:** _This endpoint is denied to all users except master account, defined by_ [_default RBAC scope_](https://learn.mobingi.com/enterprise/rbac-reference#default-roles)_, and this scope cannot be overwritten._

    GET `/v3/user/{username}/role`

````
Request Header
```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
````

Response Body

```bash
HTTP/1.1 200
[
    {
        "role_id": "morole-544****0e1-ZgNT***M8K-tk",
        "user_id": "544****0e1",
        "name": "sample name",
        "scope": "{ _role scope body_ }",
        "create_time": "",
        "update_time": ""
    },
    {
        ....
    }

]
```

````
1. **Describe Current Logged In User Roles**

```text
Lists all roles attached to current user.

_This endpoint is requested by users instead of master account, and returns the roles that attached to them._

<div class="callout callout-info">
GET <code>/v3/user/role</code>
</div>



Request Header
```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
````

Response Body

```bash
HTTP/1.1 200
[
    {
        "user_role_id": "mour-5447****0e1-TEW****dsIE-tk",
        "role_id": "morole-5447****0e1-ZgN****RM8K-tk",
        "user": "{ user_id: 5447****0e1, username: tes***est }",
        "scope": "{ _role scope body_ }",
        "create_time": "",
        "update_time": ""
    },
    {
        ....
    }

]
```

````
### Attach Role to User {#rbac-attach-user-role}

Attach an existing role to a user.

**Note:** _This endpoint is denied to all users except master account, defined by_ [_default RBAC scope_](https://learn.mobingi.com/enterprise/rbac-reference#default-roles)_, and this scope cannot be overwritten._

 POST `/v3/user/role`

| **Parameters** | **Type** | **Required** | **Detail** |
| --- | :---: | ---: | :--- |
| username | string | Yes | target user |
| role\_id | string | Yes | Mobingi Role Id |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/x-www-form-urlencoded
````

Request body

```bash
{
  "username": "testtest",
  "role_id": "morole-544****0e1-ZgN****8K-tk"
}
```

Response Body

```bash
HTTP/1.1 200
{
  "status": "success",
  "user_role_id": "mour-544****0e1-ZgN****M8K-tk"
}
```

### Reattach Role to User <a href="#rbac-reattach-user-role" id="rbac-reattach-user-role"></a>

Reattach a role to user.

**Note:** _This endpoint is denied to all users except master account, defined by_ [_default RBAC scope_](https://learn.mobingi.com/enterprise/rbac-reference#default-roles)_, and this scope cannot be overwritten._

PUT `/v3/user/role/{role_id}`

| **Parameters** | **Type** | **Required** | **Detail**  |
| -------------- | -------- | ------------ | ----------- |
| username       | string   | Yes          | target user |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/x-www-form-urlencoded
```

Request body

```bash
{
  "username": "testtest"
}
```

Response Body

```bash
HTTP/1.1 200

{
  "status": "success",
  "role_id": "morole-5447****0e1-ZgN***M8K-tk"
}
```

### Detach Role from User <a href="#rbac-detach-user-role" id="rbac-detach-user-role"></a>

Deatch a role from user.

**Note:** _This endpoint is denied to all users except master account, defined by_ [_default RBAC scope_](https://learn.mobingi.com/enterprise/rbac-reference#default-roles)_, and this scope cannot be overwritten._

DELETE `/v3/user/role/{role_id}`

| **Parameters** | **Type** | **Required** | **Detail**  |
| -------------- | -------- | ------------ | ----------- |
| username       | string   | Yes          | target user |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Request body

```bash
{
  "username": "testtest"
}
```

Response Body

```bash
HTTP/1.1 200

{
  "status": "success",
  "role_id": "morole-5447****0e1-ZgN****M8K-tk"
}
```

### Describe Role Scope <a href="#rbac-describe-role-scope" id="rbac-describe-role-scope"></a>

Describes the role scope body.

GET `/v3/role/templates`

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```bash
HTTP/1.1 200
[
    {
        "id": "admin",
        "name": "Management Role",
        "scope": {
            "version": "2017-05-05",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": [
                        "*"
                    ],
                    "Resource": [
                        "*"
                    ]
                }
            ]
        }
    },
    {
        ....
    }
]
```

## Alm-Agent <a href="#alm-agent" id="alm-agent"></a>

_In this section, all endpoints are designated to work with Mobingi alm-agent in order to perform application lifecycle automation by Mobingi. Mobingi alm-agent is the Linux server side program that automatically installed during instance launch and initialization. If you are a contributor to the OSS repo_ [_github.com/mobingi/alm-agent_](https://github.com/mobingi/alm-agent)_, you're looking at the right reference here. If you are a developer working on integrating Mobingi ALM with your client applications or contributing to Mobingi API/UI only, you can ignore this API references section._

### Register Agent Status <a href="#alm-agent-register-agent-status" id="alm-agent-register-agent-status"></a>

This endpoint listens to the notifications sent by Mobingi alm-agent for self status registration.

For example, when an instance is launched and Mobingi alm agent is installed, the agent will first call this endpoint to register itself.

Another example, when an AWS spot instance is scheduled to be shutdown, the agent will send notice to this endpoint and allow Mobingi system to perform other necessary actions (_such as spot replacement)_.

POST `/v3/alm/agent/agent_status`

| **Parameters** | **Type** | **Required** | **Detail**                                                                         |
| -------------- | -------- | ------------ | ---------------------------------------------------------------------------------- |
| stack\_id      | string   | Yes          | The stack id which this server instance is belonged to                             |
| agent\_id      | string   | Yes          | The agent's unique identifier                                                      |
| status         | string   | Yes          | Sample values: _starting_, _installing_, _error_, _notice_, spot\_terminate, etc.. |
| instance\_id   | string   | No           | The server instance id where Mobingi alm-agent is installed on                     |
| message        | string   | No           | Optional, a description of the status message, e.g: _image/repository not found_   |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```bash
HTTP/1.1 202 Accepted
```

### Register Container Status <a href="#alm-agent-register-container-status" id="alm-agent-register-container-status"></a>

This endpoint listens to the notifications sent by Mobingi alm-agent with the status updates during a container's lifecycle. Possible status examples: _starting_, _updating_, _restarting_, _running_, _terminated_, etc.

POST `/v3/alm/agent/container_status`

| **Parameters** | **Type** | **Required** | **Detail**                                                                          |
| -------------- | -------- | ------------ | ----------------------------------------------------------------------------------- |
| stack\_id      | string   | Yes          | The stack id which this server instance is belonged to                              |
| agent\_id      | string   | Yes          | The agent's unique identifier                                                       |
| container\_id  | string   | Yes          | The container unique id. \_Sometimes, this value could be an instance's id.         |
| status         | string   | Yes          | sample values: _starting_, _updating_, _restarting_, _running_, _terminated_, etc.. |
| instance\_id   | string   | No           | The server instance id where Mobingi alm-agent is installed on                      |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```bash
HTTP/1.1 202 Accepted
```

### Describe Container Configuration <a href="#alm-agent-container-config" id="alm-agent-container-config"></a>

This endpoint is used by Mobingi alm-agent to describing `container` section of the layer configuration from Mobingi Alm Template, identified by `flag` name.

GET `/v3/alm/agent/config`

| **Parameters** | **Type** | **Required** | **Detail**                                       |
| -------------- | -------- | ------------ | ------------------------------------------------ |
| stack\_id      | string   | Yes          | the stack id where the instance is associated to |
| flag           | string   | Yes          | The flag identifier of the configuration layer   |

Request Header

```bash
Authorization: Bearer eyJ0eXAiOiJQiL...CJhbGciOMeXzQfME
Content-Type: application/json
```

Response Body

```bash
HTTP/1.1 200 OK

{
  "image": "registry.mobingi.com/mobingi/ubuntu-apache2-php5",
  "environment_variables": {
    "Stage": "_development",
    "DB_USERNAME": "root",
    "DB_PSSSWORD": "7zk3FBP37",
    "my_secret": "D3nz!lwA_h1ngt0n"
  },
  "gitReference": "master",
  "gitPrivateKey": "-----BEGIN PRIVATE ...\n-----END PRIVATE KEY-----\n",
  "gitRepo": "https://github.com/mobingilabs/default-site-php.git",
  "updated": 1492161755
}
```

**Note:** _If the response has an empty body, it could mean that wrong_ `flag` _name was provided, or it doesn't have any container config defined._
