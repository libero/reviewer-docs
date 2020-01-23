# Continuum Adaptor

{% hint style="info" %}
_**This service has not yet been implemented**_.

For now this will be an adaptor to`journal` as Libero is not expecting to implement this for a while yet.
{% endhint %}

## Summary

|   | Description / Link |
| :--- | :--- |
| **Responsibility** | To provide an interface to the legacy continuum APIs and systems |
| **Repository** | [https://github.com/libero/continuum-adaptor](https://github.com/libero/continuum-adaptor) |
| **Interfaces** |  |
| **Dependents** | Postgres |

## Design

The service provides the following:

* Token exchange from a `journal` to a `libero` authentication token.
* Provisions and stores user records linked to continuum `profile_id` identifier
* Gateway to profile information and role information from continuum APIs in a single REST response that can then be used by the consuming services and Reviewer Client.

### Authentication

The service accepts authentication via the `GET /authenticate/:token` route. The token is verified using the journal jwt secret \(`continuum_jwt_secret` in configuration\). The continuum `profile_id`  is used to retrieve a separate application user id \(UUID\). If a user is not found, then a new one will be provisioned and linked via a record in the `identity` table with type `elife` .

A new token is then signed with the `application_jwt_secret` configuration value and a redirect response to the `login_return_url` is made with the encoded token as part of the URL hash. The token format is:

```text
{
    "sub": "63bc7396-0a26-42d6-975d-06f881abe130",
    "issuer": "libero",
    "jti": "4612c811-2121-45f8-819e-a5644fe18f87"
}
    
```

The `sub` field corresponds to the user id. All fields follow the OpenID Connect standard.

### User Profile and Role Information

The service provides the route `GET /current-user` to combine information from the continuum profiles and people APIs. After verifying the `libero` token, the service retrieves the related `profile_id` from the database and proceeds to call the continuum APIs. The returned data structure is as follows:

```text
{
    "id": "63bc7396-0a26-42d6-975d-06f881abe130",
    "name": "Joe Bloggs",
    "role": "author"
}
```

The name is taken from the profile API under the `name.preferred` field, while the role comes from the people API under the `type.id` field.

## **Configuration**

```javascript
{
    "port": 3001,
    "rabbitmq_url": "rabbitmq",
    "login_url": "http://localhost:9000/submit",
    "login_return_url": "http://localhost:9000/login",
    "authentication_jwt_secret": "super_secret_jam",
    "continuum_jwt_secret": "some_secret_from_journal",
    "continuum_api_url": "http://reviewer-mocks:3003",
    "knex": {
        "client": "pg",
        "connection": {
            "host": "postgres",
            "database": "continuum-adaptor",
            "user": "postgres",
            "password": "postgres",
            "port": 5432
        }
    }
}
```

## Deploy & Test



## Examples

* Storage format
* Example Code see continuum-auth

```text
FROM xxxx
```

