# Authorization (RBAC)

{% hint style="info" %}
New API available on [https://alphauslabs.github.io/blueapidocs/#/Iam](https://alphauslabs.github.io/blueapidocs/#/Iam).
{% endhint %}

For general information about RBAC, check out this [link](https://docs.mobingi.com/v/ur-en/#rbac).

The following endpoint is the base url for the APIs below.

```
https://service.alphaus.cloud/m/auth/rbac/
```

## List permissions

List all permissions supported by RBAC in all namespaces. For reference, supported permissions can be found [here](https://github.com/mobingi/rbac-permissions).

**Request**

```http
GET /permissions HTTP1.1
authorization: Bearer {token}
```

**Response**

```ruby
HTTP/1.1 200 OK

[
  {
    "namespace":"wave",
    "permissions":[
      "Admin",
      "ModifySettings",
      "..."
    ]
  },
  {
    "namespace":"ripple",
    "permissions":[
      "Admin"
    ]
  }
]
```

## Create role

During role creation, if your `permissions` list contains an `Admin` entry, all other entries will be discarded except `Admin`.

Roles are root user-level. That means all roles created by the root user, or any subuser that has permissions to create roles, are available to all subusers.

**Request**

```http
POST /roles HTTP1.1
authorization: Bearer {token}
content-type: application/json

{
  "name":"testrole",
  "namespace":"wave",
  "permissions":[
    "ModifySettings",
    "ViewSettings",
    ...
  ]
}
```

Role names should have at least 6 characters in length and 32 characters maximum. It should also be alphanumeric. Hyphens and underscores are allowed in between. The regular expression used for validation is below:

```
^[A-Za-z0-9][A-Za-z0-9_-]*[A-Za-z0-9]$
```

**Response**

```ruby
HTTP/1.1 200 OK

{
  "name":"testrole",
  "namespace":"wave",
  "permissions":[
    "ModifySettings",
    "ViewSettings",
    ...
  ]
}
```

## List roles

**Request**

```http
GET /roles?namespace={namespace} HTTP1.1
authorization: Bearer {token}
```

The `{namespace}` parameter is optional. If not provided, all roles will be returned.

**Response**

```ruby
HTTP/1.1 200 OK

[
  {
    "name": "testrole",
    "namespace": "wave",
    "permissions": [
      "ModifySettings",
      "ViewSettings",
      "ModifyAccountSettings"
    ]
  },
  {
    "name": "waveAdmin",
    "namespace": "wave",
    "permissions": [
      "Admin"
    ]
  },
  ...
]
```

## Update role

Update role. If role name is different, rename mapped role name.

**Request**

```http
PATCH /roles/{namespace}/{rolename} HTTP1.1
authorization: Bearer {token}
content-type: application/json

{
  "namespace":"wave",
  "permissions":[
    "ModifySettings",
    "ViewSettings",
    ...
  ]
}
```

**Response**

```ruby
HTTP/1.1 200 OK

{
  "name": "testrole",
  "namespace":"wave",
  "permissions":[
    "ModifySettings",
    "ViewSettings",
    ...
  ]
}
```

## Delete role

Delete role. Deleting a role will also remove all mappings.

**Request**

```http
DELETE /roles/{namespace}/{rolename} HTTP1.1
authorization: Bearer {token}
```

## Map roles to user

You can only map (or attach) up to 5 roles to a user per namespace. There is no limit for filtering rules per user.

Valid values for `type` for filtering rules:

| Namespace | Value                       |
| --------- | --------------------------- |
| `wave`    | `linkAcct`, `group`, `tags` |
| `ripple`  | `billingGroup`              |

**Request**

```http
POST /userroles HTTP1.1
authorization: Bearer {token}
content-type: application/json

{
  "user_id":"subuser1",
  "roles":[
    {
      "namespace":"wave",
      "role": "somerole",
    },
    ...
    ]
}
```

**Response**

```ruby
HTTP/1.1 200 OK

{
  "success":[
    "somerole"
  ],
  "failed":[],
  "filters":[]
}
```

## List user role mappings

**Request**

For this endpoint, the returned role mappings are those attached to the caller.

```http
GET /userroles HTTP1.1
authorization: Bearer {token}
```

For listing role mappings of other subusers, use this endpoint.

```http
GET /{subuser}/userroles HTTP1.1
Authorization: Bearer {token}
```

`{subuser}` is the subuser name.

**Response**

```ruby
HTTP/1.1 200 OK

[
  {
    "root_user":"58c2297d25645",
    "sub_user":"subuser01",
    "namespace":"wave",
    "role":"testrole1"
  },
  {
    "root_user":"58c2297d25645",
    "sub_user":"subuser02",
    "namespace":"wave",
    "filter":"billingGroup:2222"
  },
  ...
]
```

## List user permissions

Retrieve all permissions to all roles attached to the `{subuser}`.

**Request**

```http
GET /{subuser}/permissions HTTP1.1
authorization: Bearer {token}
```

**Response**

```ruby
HTTP/1.1 200 OK

[
  {
    "namespace":"wave",
    "permissions":[
      "Admin",
      "ModifySettings",
      "..."
    ]
  },
  {
    "namespace":"ripple",
    "permissions":[
      "Admin"
    ]
  }
]
```

## Update map roles to user

You can only update map (or attach) up to 5 roles to a user per namespace. There is no limit for filtering rules per user.

Valid values for `type` for filtering rules:

| Namespace | Value                       |
| --------- | --------------------------- |
| `wave`    | `linkAcct`, `group`, `tags` |
| `ripple`  | `billingGroup`              |

This method replaces subuser's all roles to information in the request body.

**Request**

```http
PATCH /userroles HTTP1.1
authorization: Bearer {token}
content-type: application/json

{
  "roles":[
    {
      "namespace":"wave",
      "role": "somerole",
    },
    ...
    ]
}
```

```http
PATCH /{subuser}/userroles HTTP1.1
authorization: Bearer {token}
content-type: application/json

{
  "roles":[
    {
      "namespace":"wave",
      "role": "somerole",
    },
    ...
    ]
}
```

`{subuser}` is the subuser id.

**Response**

```ruby
HTTP/1.1 200 OK

{
  "success":[
    "somerole"
  ],
  "failed":[],
  "filters":[]
}
```
