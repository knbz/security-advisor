---

copyright:
  years: 2019
lastupdated: "2019-02-07"

---

{:new_window: target="blank"}
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

# Partners
{: #setup-partner}

Partner integrations are a way to bring critical alerts and findings from 3rd party providers into the {{site.data.keyword.security-advisor_long}} dashboard. These partners have worked with IBM to help shape, simplify, and guide the integration experience for you.
{: shortdesc}

## Before you begin
{: #partner-before}

Before you start integrating partners, be sure that you have the following prerequisites.

* Ensure that you have an account with the partner that you want to integrate.
* Ensure that you have the required administrative permissions to generate the integration URL for the Partner service.
* Ensure that you have IAM administrative access to IBM Cloud and manager access to {{site.data.keyword.security-advisor_short}}.

## Integration wizard
{: #wizard}

As an administrator with administrative rights in both the {{site.data.keyword.cloud_short}} and Partner accounts, you can use the integration wizard to quickly get up and running. The wizard has four basic steps:

* Establish trust and associate your {{site.data.keyword.cloud_short}} and partner accounts
* Copy the required information such as credentials and URLs between the accounts
* Install partner finding metadata into {{site.data.keyword.security-advisor_short}}
* Validate the pairing by posting a findings from the partner into {{site.data.keyword.security-advisor_short}}


## Integrating NeuVector
{: #setup-neuvector}

With NeuVector, you can detect network threats and violations to prevent attacks of your container based applications. Through monitoring, you can prevent exploits and breakouts by detecting root privilege escalations, port scans, reverse shells, and suspicious file system activity in your containers and hosts.
{: shortdesc}

To integrate NeuVector with {{site.data.keyword.security-advisor_short}}, you can use the following steps:

1. Navigate to **Integrations > Partner Integrations** and select **NeuVector** from the options provided.
2. Connect your accounts.
  1. Provide a name for your connection.
  2. Provide the NeuVector dashboard URL.
  3. Provide the NeuVector configuration URL.
    1. Login to NeuVector and navigate to settings.
    2. Click **Generate URL**.
    3. Copy the URL and paste it in the {{site.data.keyword.security-advisor_short}} integration wizard.
  4. Click **Next**.
3. Verify that you give permission for the service to generate a service ID and API key and create the connection with the service by clicking **Next**.
4. Click **Done**.
5. Go to your service dashboard to review a test finding that was sent to {{site.data.keyword.containershort_notm}} by NeuVector.


