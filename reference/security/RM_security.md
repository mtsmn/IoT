---

copyright:
  years: 2016, 2018
lastupdated: "2018-11-27"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}
{:important: .important}

# Risk and security management
{: #RM_security}

You can enhance security to enable creating, enforcing, and reporting on device connection security. With this enhanced security, certificates and transport layer security (TLS) authentication are used in addition to the user IDs and tokens that are used by {{site.data.keyword.iot_short_notm}} to determine how and where devices connect with the platform.


## Certificate Revocation Lists and client certificates
{: #CRLs}

Certificate Revocation Lists (CRLs) are supported for configurations that use client certificates to authenticate devices. The location that is specified as the CRL distribution point must support either HTTP or HTTPS access, and the site must be reachable. The client connection is rejected if the distribution point of the CRL cannot be reached, or if the CRL is not valid or is not found at the distribution point.

Client certificates must be signed by certificate authority (CA) certificates that have "Certificate Sign" specified in the "X509v3 Key Usage" section in "X509v3 extensions". To use CRLs to revoke client certificates, the CA certificate that signs a client certificate must also include "CRL Sign" in the "X509v3 Key Usage" section in "X509v3 extensions". This configuration allows the CA certificate to issue and sign CRL updates when required. Only the CRL that is specified in the client certificate distribution point is used to determine whether a certificate has been revoked.

Ensure that you test your certificates and CRLs, particularly if you generate your own certificates and CRLs instead of using certificates from a recognized certificate authority.


## Configuring certificates
{: #certificates}

When certificates are enabled during communication between devices and the server, any devices that do not have valid certificates  configured in the security settings are denied access, even if they use valid user IDs and passwords

To configure client certificates and server access for devices, the system operator imports the associated certificate authority (CA) certiﬁcates and messaging server certificates into {{site.data.keyword.iot_short_notm}}. The security analyst then configures the default security policies for connections between devices and the platform. The analyst can add different policies for different device types.

For information about how to configure certificates, see [Configuring certificates](set_up_certificates.html).


## Security settings
{: #connect_policy}

The security settings, including the use of connection policy settings, CA certificates, and messaging server certificates, enforce how devices connect to the platform. You can set up default connection policies for all device types and create custom settings for specific device types. The policy can be set to allow unencrypted connections, to enforce only transport layer security (TLS) connections, and to enable devices to authenticate with client-side certificates.

For information about how to configure connection security policies, see [Configuring security policies](set_up_policies.html).

Blacklist and whitelist policies provide the ability to control the locations from which devices are allowed to connect to the organization’s account. A blacklist identifies all of the IP addresses, CIDRs, or countries that are denied server access. A whitelist gives explicit access to specific IP addresses.

For information about how to configure blacklist and whitelist policies, see [Configuring blacklists and whitelists](set_up_policies.html#config_black_white).

## Organization plans and security policies

<p>The {{site.data.keyword.iot_short_notm}} pricing plans were updated on November 27, 2018.   
For more information, including upgrade information, see [{{site.data.keyword.iot_short_notm}} service plans](plans_overview.html). The contents of this [IBM Cloud documentation collection](https://console.bluemix.net/docs/services/IoT/) pertain to the {{site.data.keyword.iot_short_notm}} Lite plan, and to the previous Standard and Advanced Security plans. For documentation about the {{site.data.keyword.iot_short_notm}} Connection and Analytics Service plans, with their extended feature set, see the [{{site.data.keyword.iot_short_notm}} knowledge center ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSQP8H/iot/overview/overview.html).
</p>
{: important}

The enhanced security policies enable organizations to determine how they want devices to connect and be authenticated to the platform, by using connection policies and blacklist and whitelist policies. The security policy options that are available to an organization depend on the organization's plan type:

**Standard Plan:**
- System operators can configure connection policies with the following options:
    - TLS Optional
    - TLS with Token Authentication
    - TLS with Token and Client Certificate Authentication

**Advanced Security Plan (ASP) or Lite Plan:**
- System operators can configure connection policies with the following options:
    - TLS Optional
    - TLS with Token Authentication
    - TLS with Client Certificate Authentication
    - TLS with Token and Client Certificate Authentication
    - TLS with Client Certificate or Token
- System operators can configure blacklists or whitelists

## Risk and Security Management dashboard and reporting
{: #dashboard}

The system operator and the security analyst can use the Risk and Security Management dashboard to view the overall security status. Cards on the dashboard can provide a comprehensive compliance overview and the connection status of devices.

The following dashboard cards are available for analyzing risk and compliance:
 - **Connection Security Compliance**- Shows the level of compliance of devices that are connected to the server.
 - **Blacklist/Whitlelist Compliance** - Shows the level of compliance of devices based on the active blacklist or whitelist policy.
 - **Overall Policy Compliance** (Beta) - Shows the overall level of compliance based on all policies that are in place.
 - **Policy Violations** (Beta) - Shows the overall violations for each policy.

**Important**: The **Overall Policy Compliance** and **Policy Violations** cards are available only as part of a limited Beta program. To enable these dashboard cards, turn on the **Experimental Features** on the **Settings** page.

### Drill-down policy reporting (Beta)
{: #drill-down}

**Important**: The drill-down reporting feature for Risk Management policies is available only as part of a limited Beta program. To enable drill-down reporting, turn on the **Experimental Features** on the **Settings** page.

As part of the Beta feature, the system operator can drill down in each report to view the specific details of the devices that are compliant or in violation of policies.

You can access drill-down reports from the dashboard cards, from the **Policies** page where all of the security policies are listed, and from the details page for each policy. You can drill down to three levels of details in the reports:
1. The first level lists all of the policies that are contained in the selected policy type, for example, the default connection policy and any custom connection policies.
2. The second level shows the devices that are in violation of the policy and the cause of the violation.
3. The third level shows the specific details of an individual device that is in violation of the policy.
