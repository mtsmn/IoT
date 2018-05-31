---

copyright:
years: 2018
lastupdated: "2018-05-31"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# App ID Authentication and Authorization for Watson IoT Platform (Beta)
{: #app_id}

{{site.data.keyword.iot_full}} APIs support the authentication and authorization of users by using App ID.

**Important:** The App ID Authentication and Authorization for {{site.data.keyword.iot_short_notm}} feature is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

With App ID, users can obtain an App ID OAuth token and use it to authenticate API calls. A user can use the App ID endpoint to log in and retrieve their App ID token.

For example:

```
curl -X POST --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username={user-email}&password={password}' 'https://mobileclientaccess.mybluemix.net/oauth/v3/{tenantId}/token'
```

You must obtain the oAuth server URL and `tenantId` values from the App ID service credentials.

The token endpoint response contains the access and ID tokens, an optional refresh token, and expiration information.

For example:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

When a user calls a {{site.data.keyword.iot_short_notm}} API, they can provide the authorization header `Authorization: Bearer <Id_token>` on their API calls.

## Configuring App ID
{: app_id_config}

You must configure App ID before using the App Id token for authorization.

To create App Id configuration, use the following API:

`POST /api/v0002/authentication/providers/`

Request Body:

```
{
        "appIdConfigName": "<app_id_config_name>",
        "tenantId": <tenant_Id>,
        "issuer": <issuer>
}
```

You must obtain the `tenant_Id` and `issuer` values from the App ID service credentials.

Request Response: 200

```
{
        "appIdConfigName": "<app_id_config_name>",
        "tenantId": <tenant_Id>,
        "issuer": <issuer>
}
```

## Adding users
{: app_id_users}

You must add users before you can give them authorization based on their roles. To create user, use the following API:

`POST /api/v0002/authorization/users`

Request Body:

```
{
    "uniqueSecurityName": "{tenantId}:{amr}:user-email",
    "issuer": "{issuer}",
    "realmName": "someRealm",
    "email": "user-email",
    "owner": true,
    "displayName": "user-displayname",
    "status": 1,
    "roles": [
    "PD_ADMIN_USER"
  ]
}
```

The `uniqueSecurityName` and `issuer` values are populated with details from the App ID service credentials.

You must obtain the `tenantId`, `amr`, and `issuer` values from the App ID service credentials.


Request Response: 200

```
{
    "uniqueSecurityName": "{tenantId}:{amr}:user-email",
    "issuer": "idaas-sim.fra02-1.test.internetofthings.ibmcloud.com",
    "realmName": "someRealm",
    "email": " user-email ",
    "owner": true,
    "displayName": " user-displayname ",
    "status": 1,
    "roles": [
        "PD_ADMIN_USER"
    ]
}
```

## Generating an APP ID token
{: app_id_token}

To generate an App Id token, use the following API by authenticating to IBM Cloud with the service credentials:

`POST https://{issuer}/oauth/v3/{tenantId}/token`

You must obtain the `issuer` and `tenantId` values from the App ID service credentials.

Request Header:

```
Username:{clientId}
Password:{secret}
```

You must obtain the `clientId` and `secret` values from the App ID service credentials.

Request Body:

```
{
grant_type: password / authorization_code
code: OAuth2 grant code, on grant_type authorization_code flow this parameter is required
username: The resource owner username, on grant_type password flow this parameter is required
password: The resource owner password, on grant_type password flow this parameter is required
}
```

Request Response:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

## Using the App ID token for API calls
{: #app_id_use_token}

You can find the App ID token in the `id_token` field of your API ouput. The following example shows how to use the App ID token when you call APIs:

```
GET https://org.domain/api/v0002/bulk/devices
Input parameters	Values
Headers	Content-Type: application/json
Authorization: bearer <id_token>
```
