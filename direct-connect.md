---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}


# Direct connections
{: #setup-custom-gui}

{{site.data.keyword.security-advisor_short}} enables you to integrate your existing custom security tools, whether open source, custom developed, or 3rd party services. You can create a direct connection from {{site.data.keyword.security-advisor_short}} to the other tool by adding the URL to your list of integrations. By creating the link, you can easily keep track of all of the tools that you use.
{: shortdesc}


## Before you begin
{: #custom-before-gui}

Before you can add the integration, you must first have an account with the partner that you want to integrate.

{{site.data.keyword.security-advisor_short}} does not persist any credentials that are related to the partner service. Enterprise users must authenticate by using SAML to both {{site.data.keyword.cloud_notm}} and to the business partner.
{: note}

## Configuring the connection
{: #custom-configure-connection}

1. Log in to your security tool and get your unique URL.

2. Log in to {{site.data.keyword.cloud_notm}} by using the console.

3. Click **Custom Integrations > Direct Connection**. A screen displays.

  1. Give your solution a name. You can use only alpha-numeric characters, white spaces, and dashes (-) in the name.

  2. Enter the URL for the solution in the format: `www.<website>.<domain>`.

  3. Upload an icon or image to represent the tool.

  4. Click **Connect** to complete the configuration. {{site.data.keyword.security-advisor_short}} creates the artifacts that are required for integration such as the service ID, API key, account ID, and metadata. The `writer` role is assigned.