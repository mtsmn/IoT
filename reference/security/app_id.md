---

copyright:
years: 2018
lastupdated: "2018-10-25"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# App ID Authentication for Watson IoT Platform (Beta)
{: #app_id}

App ID is used for authenticating users who need access to applications that are hosted in the IBM Cloud. These users access the service through mobile or web applications provided by the service.
{: shortdesc}

**Important:** The App ID Authentication and Authorization for {{site.data.keyword.iot_short_notm}} feature is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.
 
{{site.data.keyword.iot_short_notm}} also supports authenticating users through Cloud IAM. Cloud IAM is built in to the IBM Cloud and is used for authenticating and authorizing administrative and developer users who need to configure and manage their IBM services. For more information about Cloud IAM, see [Cloud IAM Authentication and Authorization for Watson IoT Platform](cloud_iam.html#cloud_iam).

App ID users do not typically perform administrative or development activities on a cloud service, and they cannot log in to the {{site.data.keyword.iot_short_notm}} web dashboard. Only Cloud IAM users can log in to the dashboard.

The {{site.data.keyword.iot_short_notm}} APIs support the authentication of users from the IBM Cloud App ID service.  You can configure your {{site.data.keyword.iot_short_notm}} organization to use an instance of the App ID service, and then add your App ID users to your organization. Your application authenticates these users with App ID which can then be used to invoke the {{site.data.keyword.iot_short_notm}} APIs. For more information about the IBM Cloud App ID service, see [IBM Cloud App ID Getting Started Tutorial ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/appid/index.html){: new_window}.

The current App ID support is for non-messaging REST APIs, including APIs for configuring and managing devices, users, and other {{site.data.keyword.iot_short_notm}} capabilities. App ID cannot be used to publish and subscribe to messages.

Typically, you use App ID to authenticate REST APIs that are called on behalf of non-administrative users. A developer can create an application that uses an App ID token to invoke a REST API on behalf of a registered user, such as a maintenance technician, who does not have administrative access. The use of App ID allows the non-administrative users to be authenticated individually rather than by using a shared authentication token in {{site.data.keyword.iot_short_notm}}. App ID is also useful for authenticating non-administrative users who need to use the {{site.data.keyword.iot_short_notm}} command-line interface (CLI).

**Important:** The invocation of {{site.data.keyword.iot_short_notm}} REST APIs should be done by using a user application and not directly from a browser or mobile apps. The user application can provide an additional level of access control to determine which device functions a particular user is allowed to access, as well as validating the MQTT messages that are sent to devices. The following image shows this invocation flow:

![REST API invocation by using a user application](images/app_id_user_app.PNG)

App ID users must be registered in the {{site.data.keyword.iot_short_notm}} by using the same process that is used by Cloud IAM users, and they both appear in the dashboard interface.

Since App ID users are typically not administrators, it is important to consider what resources and commands they should be allowed to access, and then set the {{site.data.keyword.iot_short_notm}} resource-level access control settings appropriately. In most cases, App ID users are assigned a custom role that is more restrictive than the defined default roles of a {{site.data.keyword.iot_short_notm}} organization. For more information about roles, see [User, application, and gateway roles](../../roles_index.html#user-application-and-gateway-roles).

By default, App ID users can access all devices. If an App ID user should only be able to manage a restricted list of devices, assign resource groups the App ID users.

**Note:** {{site.data.keyword.iot_short_notm}} limits the total number of registered users to 1,000.

## Setting up an App ID service
{: #set_up_app_id}

Before you set up an App ID service, you must add an instance of the IBM Cloud App ID service and configure identity providers. If you do not already have an instance of App ID you can provision one from the [IBM Cloud Services Catalog ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog){: new_window}.

After you have set up the App ID service, you must obtain service credentials to configure App ID as a provider for your {{site.data.keyword.iot_short_notm}} organization. You can obtain or create service credentials in the App ID service UI.

1. In the IBM Cloud dashboard, select your App ID service and click **Service Credentials**.
2. Click **View Credentials** to locate the `tenantId`, `clientId` and `secret` in the credentials. These values are required for configuring App ID in later steps. The issuer will be the hostname of `oauthServerUrl`, for example `appid-oauth.ng.bluemix.net`.

The following image shows an example of how to retrieve the credentials:

![Retrieving App ID Credentials](images/app_id_credentials.PNG "Example of retrieving App ID credentials")


## Configuring App ID in {{site.data.keyword.iot_short_notm}}
{: #config_app_id}

You must configure App ID before you can use the App ID token for authorization. To create App ID configuration, use the `POST /api/v0002/authentication/providers` API where `tenant_Id` and `issuer` are obtained from step 2 in [Setting up an App ID service](#set_up_app_id):

Request Body:

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

Request Response: 200

```
{
	"appIdConfigName": “TestAppIdConfigName",
    	"tenantId": “8c807245-a35d-4027-bde9-ab12cd710cef”,
	"issuer": “appid-oauth.ng.bluemix.net”
}
```

## Adding Users to {{site.data.keyword.iot_short_notm}}
{: #users_app_id}

You must add users and give them authorizations based on their roles. To create users, use the `POST /api/v0002/authorization/users` API, with the following details:

 - `issuer` is obtained from step 2 in [Setting up an App ID service](#set_up_app_id).
 - `uniqueSecurityName` is filled with the details of `appIdConfigName`, `amr`, and the user’s email that are configured in the identity providers of the App ID service.
 - `appIdConfigName` is the name that is used in [Configuring App ID](##config_app_id), for example “TestAppIdConfigName”.
 - `amr` is the authentication method reference, which is the identity provider that is used in the App ID service. App ID supports the following Identity providers:

	 - Cloud Directory
	 - SAML 2.0 Federation
	 - Facebook
	 - Google

	 Based on the identity provider that is used in the App ID service, `amr` would be `cloud_directory`, `saml`, `facebook` or `google`.

In the following example, Cloud Directory is being used:

Request Body:

```
{
    "uniqueSecurityName": "TestAppIdConfigName:cloud_directory:test_user@gmail.com",
    "issuer": “appid-oauth.ng.bluemix.net”,
    "realmName": "cloud_directory",
    "email": “test_user@gmail.com”,
    "owner": true,
    "displayName": “test_user”,
    "status": 1,
    "roles": [
    "PD_ADMIN_USER"
  ]
}
```

Request Response: 200

```
{
    "uniqueSecurityName": “TestAppIdConfigName:cloud_directory:test_user@gmail.com",
    "issuer": “appid-oauth.ng.bluemix.net”,
    "realmName": "cloud_directory",
    "email": “test_user@gmail.com”,
    "owner": true,
    "displayName": “test_user”,
    "status": 1,
    "roles": [
        "PD_ADMIN_USER"
    ]
}
```

## Generating an APP ID token
{: #token_app_id}

After you have configured App ID and added users, your application can start generating App ID tokens for your users, and use them to call platform APIs. For more information about getting started with App ID, see the following video: [IBM Bluemix App ID ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://www.youtube.com/watch?v=HYomAFlNxqw){: new_window}.

A user can use the App ID endpoint to log in and retrieve their App ID token by posting the following examples.

### HTTP example of generating an App ID token

To generate App ID token, use the following API by authenticating to IBM Cloud with the service credentials:

`POST https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token`

Where `tenantId`, `issuer`, `clientId`, and `secret` are obtained from step 2 in [Setting up an App ID service](#set_up_app_id).

```
HTTP Basic auth headers :
Username: 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31
Password: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl
```

Request Body for grant_type=password:

```
Content-Type: application/x-www-form-urlencoded
grant_type=password&
  username=<user_name>&
  password=<password>&
```

Request Body for grant_type= authorization_code:

```
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code&
code=<code>&
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

### Curl example of generating an App ID token

The following snippet shows a CURL example of generating an App ID token:

```
curl -X POST -u 8f6e579d-2a5d-4b51-bacd-6cccabcbdc31: Y2ZlNTZkYTYtMjMwMi00ZiFkLWFgNTgtZTExNzQyMDQzYjJl --header 'Content-Type: application/x-www-form-urlencoded' --header 'Accept: application/json' -d 'grant_type=password&username=<user-name>&password=<password>' 'https://appid-oauth.ng.bluemix.net/oauth/v3/8c807245-a35d-4027-bde9-ab12cd710cef/token
```

Where `username` and `password` are defined for the user in App ID.

Request Response:

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiO…",
  "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc… ",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Swagger example of generating an App ID token

To use Swagger to generate an App ID token, use the [Swagger example ![External link icon](../../../../icons/launch-glyph.svg "External link icon")](https://mobileclientaccess.mybluemix.net/swagger-ui/#!/Authorization_Server_V3/token){: new_window}.

Complete all of the details in the Swagger parameters, and click **Try it out** at the end of the method `/oauth/v3/{tenantId}/token`.

The token endpoint response contains the access and ID tokens, the optional refresh token, and expiration information.

## Using the App ID Token
{: #use_app_id}

After the App ID token is created, your application can start using it to call platform APIs. The App ID token is the `id_token` property in the response where it was generated in [Generating and App ID Token](#token_app_id). When the App ID token expires, your application must generate a new token to continue calling platform APIs.

The following examples show how to use the App ID token while calling APIs.

###	HTTP example of using an App ID token

`GET https://org.domain/api/v0002/bulk/devices`

Input parameters  |	Values
----------------- | -----------
Headers	|	Content-Type: application/json<br>Authorization: bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…

### Curl example of using an App ID token

`curl -X GET -H ‘Authorization Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJhc…’
https://org.domain/api/v0002/bulk/devices`
