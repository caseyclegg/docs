# Permissions

Permissions offer fine-grained control over your Application's access to different aspects of your token infrastructure.
We suggest limiting the scope of your Application to the least amount possible, and to not share them across your
internal services.

Permissions are associated with every Application and can be configured when
you [create an Application](#applications-create-application)
or [update an Application](#applications-update-application).

Every API endpoint will document the required permissions needed to perform the operation against the endpoint.

## Permission Object

| Attribute           | Type     | Description                                                                                 |
|---------------------|----------|---------------------------------------------------------------------------------------------|
| `type`              | *string* | Permission type referenced by Basis Theory API endpoints                                    |
| `description`       | *string* | Description of the permission                                                               |
| `application_types` | *array*  | List of [application types](#applications-application-types) that can assign the permission |

## Permission Types

### Management Permissions

| Permission                 | Description                                     | Application Types |
|----------------------------|-------------------------------------------------|-------------------|
| `tenant:read`              | Read Tenants                                    | `management`      |
| `tenant:update`            | Update Tenants                                  | `management`      |
| `tenant:delete`            | Delete Tenants                                  | `management`      |
| `application:read`         | Read Applications                               | `management`      |
| `application:create`       | Create Applications                             | `management`      |
| `application:update`       | Update and regenerate API keys for Applications | `management`      |
| `application:delete`       | Delete Applications                             | `management`      |
| `log:read`                 | Read audit logs                                 | `management`      |
| `reactor:read`             | Read Reactor Formulas and Reactors              | `management`      |
| `reactor:create`           | Create Reactors Formulas and Reactors           | `management`      |
| `reactor:update`           | Update Reactors Formulas and Reactors           | `management`      |
| `reactor:delete`           | Delete Reactors Formulas and Reactors           | `management`      |
| `proxy:read`               | Read Proxies                                    | `management`      |
| `proxy:create`             | Create Proxies                                  | `management`      |
| `proxy:update`             | Update Proxies                                  | `management`      |
| `proxy:delete`             | Delete Proxies                                  | `management`      |
| `tenant:member:read`       | Read Tenant Members                             | `management`      |
| `tenant:member:update`     | Update Tenant Members                           | `management`      |
| `tenant:member:delete`     | Delete Tenant Members                           | `management`      |
| `tenant:invitation:create` | Create Tenant Invitations                       | `management`      |
| `tenant:invitation:read`   | Read Tenant Invitations                         | `management`      |
| `tenant:invitation:update` | Update Tenant Invitations                       | `management`      |
| `tenant:invitation:delete` | Delete Tenant Invitations                       | `management`      |
| `report:read`              | Read reports                                    | `management`      |

### Token Permissions

All Token permissions follow the form of `token:<classification>:<operation>:<scope?>`.

| Permission                             | Description                                                                                                                                                                                                                                                                                                        | Application Types                        |
|----------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|
| `token:<classification>:create`        | Create Tokens in [\<classification\>](#tokens-token-classifications) (e.g. `token:general:create`)                                                                                                                                                                                                                 | `public`, `private` |
| `token:<classification>:read:low`      | Read plaintext token data in [\<classification\>](#tokens-token-classifications) with `low` [\<impact_level\>](#tokens-token-impact-levels). Tokens in `<classification>` with higher impact level will be restricted based on the token's [restriction policy](#tokens-token-restriction-policies)                | `private`                       |
| `token:<classification>:read:moderate` | Read plaintext token data in [\<classification\>](#tokens-token-classifications) with `moderate` [\<impact_level\>](#tokens-token-impact-levels) and lower. Tokens in `<classification>` with higher impact level will be restricted based on the token's [restriction policy](#tokens-token-restriction-policies) | `private`                       |
| `token:<classification>:read:high`     | Read plaintext token data in [\<classification\>](#tokens-token-classifications) with `high` [\<impact_level\>](#tokens-token-impact-levels) and lower (i.e. `low` and `moderate`).                                                                                                                                | `private`                       |
| `token:<classification>:update`        | Update Tokens in [\<classification\>](#tokens-token-classifications) (e.g. `token:general:update`)                                                                                                                                                                                                                 | `private`                       |
| `token:<classification>:delete`        | Delete Tokens in [\<classification\>](#tokens-token-classifications) (e.g. `token:general:delete`)                                                                                                                                                                                                                 | `private`                       |
| `token:<classification>:use:proxy`     | Use Tokens in [\<classification\>](#tokens-token-classifications) via [Proxy](#proxy) (e.g. `token:general:use:proxy`)                                                                                                                                                                                             | `private`                       |
| `token:<classification>:use:reactor`   | Use Tokens in [\<classification\>](#tokens-token-classifications) via [Reactor](#reactors) (e.g. `token:general:use:reactor`)                                                                                                                                                                                      | `private`                       |

## List Permissions

> Request

```shell
curl "https://api.basistheory.com/permissions" \
  -H "BT-API-KEY: key_N88mVGsp3sCXkykyN2EFED"
```

```javascript
import {BasisTheory} from '@basis-theory/basis-theory-js';

const bt = await new BasisTheory().init('key_N88mVGsp3sCXkykyN2EFED');

const permissions = await bt.permissions.list();
```

```csharp
using BasisTheory.net.Permissions;

var client = new PermissionClient("key_N88mVGsp3sCXkykyN2EFED");

var permissions = await client.GetAsync();
```

```python
import basistheory
from basistheory.api import permissions_api

with basistheory.ApiClient(configuration=basistheory.Configuration(api_key="key_N88mVGsp3sCXkykyN2EFED")) as api_client:
    permissions_client = permissions_api.PermissionsApi(api_client)

    permissions = permissions_client.get()
```

```go
package main

import (
  "context"
  "github.com/Basis-Theory/basistheory-go/v3"
)

func main() {
  configuration := basistheory.NewConfiguration()
  apiClient := basistheory.NewAPIClient(configuration)
  contextWithAPIKey := context.WithValue(context.Background(), basistheory.ContextAPIKeys, map[string]basistheory.APIKey{
    "ApiKey": {Key: "key_N88mVGsp3sCXkykyN2EFED"},
  })

  permissions, httpResponse, err := apiClient.PermissionsApi.Get(contextWithAPIKey).Execute()
}
```

> Response

```json
[
  {
    "type": "token:pci:read:low",
    "description": "Read tokens with PCI classification of low impact level",
    "application_types": [
      "private"
    ]
  },
  {...},
  {...}
]
```

<span class="http-method get">
  <span class="box-method">GET</span>
  `https://api.basistheory.com/permissions`
</span>

Gets the list of all supported permissions.

### Query Parameters

| Parameter          | Required | Type     | Default | Description                                                                  |
|--------------------|----------|----------|---------|------------------------------------------------------------------------------|
| `application_type` | false    | *string* | `null`  | [Application type](#applications-application-types) to filter permissions by |

### Response

Returns an array of [permission objects](#permissions-permission-object). Returns [an error](#errors) if permissions
could not be retrieved.
