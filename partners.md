---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

{:new_window: target="_blank"}
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

IBM Business Partner integrations are a way to bring critical alerts and findings from third party providers into the {{site.data.keyword.security-advisor_long}} dashboard. These partners are working with IBM to help shape, simplify, and guide the integration experience for you.
{: shortdesc}

## Before you begin
{: #partner-before}

Before you start integrating partners, be sure that you have the following prerequisites.

* Ensure that you have an account with the partner that you want to integrate.
* Ensure that you have the required administrative permissions to generate the integration URL for the partner service.
* Ensure that you have IAM administrative access to {{site.data.keyword.cloud_notm}} and manager access to {{site.data.keyword.security-advisor_short}}.

## Integration wizard
{: #wizard}

As an administrator with administrative permissions in both the {{site.data.keyword.cloud_notm}} and Partner accounts, you can use the integration wizard to quickly get up and running. The wizard has four basic steps:

* Establish trust and associate your {{site.data.keyword.cloud_notm}} and partner accounts
* Copy the required information such as credentials and URLs between the accounts
* Install the partner's finding metadata into {{site.data.keyword.security-advisor_short}}
* Validate the pairing by posting a finding from the partner into {{site.data.keyword.security-advisor_short}}


## Integrating NeuVector
{: #setup-neuvector}

With NeuVector, you can detect network threats and violations to prevent attacks of your container-based applications. Through monitoring, you can prevent exploits and breakouts by detecting root privilege escalations, port scans, reverse shells, and suspicious file system activity in your containers and hosts.
{: shortdesc}

To integrate NeuVector with {{site.data.keyword.security-advisor_short}}, you can use the following steps:

1. Navigate to **Integrations > Partner Integrations** and select **NeuVector** from the options provided.
2. Connect your accounts.
  1. Provide a name for your connection.
  2. Provide the NeuVector dashboard URL.
  3. Provide the NeuVector configuration URL.
    1. Log in to NeuVector and navigate to settings.
    2. Click **Generate URL**.
    3. Copy the URL and paste it in the {{site.data.keyword.security-advisor_short}} integration wizard.
  4. Click **Next**.
3. Verify that you give permission for the service to generate a service ID and API key and create the connection with the service by clicking **Next**.
4. Click **Done**.
5. Go to your service dashboard to review a test finding that was sent to {{site.data.keyword.containershort_notm}} by NeuVector.



## Integrating Twistlock
{: #setup-twistlock}

You can prevent risk by stopping the deployment of vulnerable images in your environment. With automated and custom policy enforcement, Twistlock offers complete control at every stage of the application lifecycle.
{: shortdesc}

When you configure the partner connection, two cards are created in your dashboard that summarize the findings from Twistlock.

**Twistlock Runtime** introduces two Key Risk Indicators (KRIs):

* Total incidents and audits: Findings that are related to incidents or audits on your cloud-native workloads.
* Total firewall audits: Findings that are related to issues with your firewall.

**Twistlock vulnerabilities** introduce one KRI:

* Total images with new vulnerabilities: Findings that are related to vulnerabilities found in your container images.

You can learn more about the company in the Twistlock docs.

### Configuring Twistlock
{: #configure-twistlock}

1. In the {{site.data.keyword.security-advisor_short}} dashboard, navigate to **Integrations > Partner Integrations** and select **Twistlock** from the options provided.

2. Click **Yes, connect my account to {{site.data.keyword.security-advisor_short}}**.

  If you don't already have an account, click **No, help me create an account > Create an account**. You can fill out your information and the Twistlock sales team will contact you to get started.
  {: note}

3. Give your connection a name. Be sure that the name is unique to your account and that you use only alpha-numeric characters, space, or a hyphen.

4. Optional: If you don't already have a configuration URL, click **Generate registration URL** to go to the Twistlock documentation and learn how to create the URL. Be sure that you have the Twistlock token that comes with your license to gain access to the docs.

5. In the Twistlock dashboard, navigate to the **Manage > Alerts** tab and click **Add profile**.

6. Select **{{site.data.keyword.security-advisor_short}}** from the **Provider** drop down.

7. Click **Copy** to use the provided configuration URL.

8. Back in the {{site.data.keyword.security-advisor_short}} dashboard, paste the URL in the **Enter the Twistlock configuration URL** box.

9. Click **Connect Partner**.

### Verifying the connection
{: #twistlock-verify}

1. In the {{site.data.keyword.security-advisor_short}} dashboard, check to see whether the Twistlock cards are displaying as expected.

2. In the Twistlock dashboard, refresh the **Alerts** tab. The {{site.data.keyword.security-advisor_short}} connection shows. Open the connection.

3. Verify that the alert types that you want to be notified of are  checked and then click **Verify** to ensure that the connection is complete.
