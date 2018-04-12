---

copyright:
  years: 2018
lastupdated: "2018-04-12"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Cloud IAM Authentication and Authorization for {{site.data.keyword.iot_short_notm}} (Beta)
{: #cloud_iam}

{{site.data.keyword.iot_full}} APIs support the authentication and authorization of users by using Identity and Access Management (IAM).

**Important:** The Cloud IAM Authentication and Authorization for {{site.data.keyword.iot_short_notm}} feature is available only as part of a limited beta program. Future updates might include changes that are incompatible with the current version of this feature. Try it out and [let us know what you think ![External link icon](../../../../icons/launch-glyph.svg)](https://developer.ibm.com/answers/smart-spaces/17/internet-of-things.html){: new_window}.

IAM enables users to obtain an IAM OAuth token and use it to authenticate API calls. For example, a user with the {{site.data.keyword.containershort_notm}} CLI installed might use the following command:

`bx iam oauth-tokens`

The command returns the logged-in user's IAM token in the following format:

`IAM Token: Bearer <some token>`

A user can also use the IAM token endpoint to log in and retrieve their IAM token, as shown in the following example:
`curl -s 'https://iam.bluemix.net/oidc/token?grant_type=urn:ibm:params:oauth:grant-type:apikey&response_type=cloud_iam&apikey=<apikey>' -H'Content-Type:x-www-form-urlencoded' -XPOST`

This API example uses an IAM API key to request the IAM token, and returns the `Bearer <token>`.

Then, when a user calls a {{site.data.keyword.iot_short_notm}} API, they can provide the authorization header `Authorization: Bearer <token>` on their API calls.

## Generating an IAM token
{: #iam_generate}

You can choose how to automate the creation of the IAM token, depending on how you authenticate with IBM Cloud and the type of IBM Cloud ID that you use.

If you use an unfederated ID, you can choose one of the following options for creating an IAM token:
 - **IBM Cloud user name and password:** You can create a token and to fully automate the creation of your IAM access token.
 - **Generate an IBM Cloud API key:** Use IBM Cloud API keys which are dependent on the IBM Cloud account that they are generated for. You cannot combine your IBM Cloud API key with a different account ID in the same IAM token. To access clusters that were created with an account other than the one your IBM Cloud API key is based on, you must log in to the account to generate a new API key.

If you use a federated ID, you can choose one of the following options for creating an IAM token:
 - **Generate an IBM Cloud API key:** IBM Cloud API keys are dependent on the IBM Cloud account they are generated for. You cannot combine your IBM Cloud API key with a different account ID in the same IAM token. To access clusters that were created with an account other than the one your IBM Cloud API key is based on, you must log in to the account to generate a new API key.
 - **Use a one-time passcode:** If you authenticate with IBM Cloud by using a one-time passcode, you cannot fully automate the creation of your IAM token because the retrieval of your one-time passcode requires a manual interaction with your web browser. To fully automate the creation of your IAM token, you must create an IBM Cloud API key instead.

When you create your access token, the body information that is included in your request varies based on the IBM Cloud authentication method that you use. Replace the values, as follows:
- `<my_username>` = Your IBM Cloud user name.
- `<my_password>` = Your IBM Cloud password.
- `<my_api_key>` = Your IBM Cloud API key.
- `<my_passcode>` = Your IBM Cloud one-time passcode. Run `bx login --sso` and follow the instructions in your CLI output to retrieve your one-time passcode by using your web browser.

Example:
`POST https://iam.<region>.bluemix.net/oidc/token`

To specify an IBM Cloud region, review the region abbreviations as they are used in the API endpoints. See [IBM Cloud region API endpoints [External link icon](../../icons/launch-glyph.svg)](https://console.bluemix.net/docs/containers/cs_regions.html#bluemix_regions) for more information.

Input Parameter	 | Values
---------------- | -----------
Header	| Content-Type:application/x-www-form-urlencoded<br>Authorization: Basic Yng6Yng=<br>Note: You are provided with Yng6Yng=, the URL-encoded authorization for the username bx and the password bx.
Body for IBM Cloud user name and password	|	grant_type: password<br>response_type: cloud_iam, uaa<br>username: `<my_username>`<br>password: `<my_password>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Note: Add the uaa_client_secret key with no value specified.
Body for IBM Cloud API keys	|	grant_type: urn:ibm:params:oauth:grant-type:apikey<br>response_type: cloud_iam<br>uaa<br>apikey: `<my_api_key>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Note: Add the uaa_client_secret key with no value specified.
Body for IBM Cloud one-time passcode	|	grant_type: urn:ibm:params:oauth:grant-type:passcode<br>response_type: cloud_iam, uaa<br>passcode: `<my_passcode>`<br>uaa_client_id: cf<br>uaa_client_secret:<br>Note: Add the uaa_client_secret key with no value specified.

Example API output:

```
{
"access_token": "<iam_token>",
"refresh_token": "<iam_refresh_token>",
"uaa_token": "<uaa_token>",
"uaa_refresh_token": "<uaa_refresh_token>",
"token_type": "Bearer",
"expires_in": 3600,
"expiration": 1493747503
}
```
You can find the IAM token in the **access_token** field of your API ouput.

The following example shows how to use IAM token while calling APIs.

```
GET https://org.domain/api/v0002/bulk/devices
```

Input parameters  |	Values
----------------- | -----------
Headers	|	Content-Type: application/json<br>Authorization: bearer `<iam_token>`
