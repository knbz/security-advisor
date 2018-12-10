---

copyright:
  years: 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Partner solutions
{: #setup-partner}

Partner solutions are a way to extend your security by creating a link between {{site.data.keyword.security-advisor_long}} and other security tools.
{: shortdesc}

## Integration wizard
{: #wizard}

As an administrator with administrative rights in both the IBM Cloud and Partner accounts, you can use the integration wizard to quickly get up and running. The wizard has four basic steps:

* Establish trust and associate your IBM Cloud and Partner accounts
* Copy the required information such as credentials and URLs between the accounts
* Install partner finding metadata into {{site.data.keyword.security-advisor_short}}
* Validate the pairing by posting a findings from the Partner into {{site.data.keyword.security-advisor_short}}

</br>

## Integrating NeuVector
{: #neuvector}

**Before you begin**

* Ensure that you have an account with the partner that you want to integrate.
* Ensure that you have the required administrative permissions to generate the integration URL for the Partner service.
* Ensure that you have IAM administrative access to IBM Cloud and manager access to {{site.data.keyword.security-advisor_short}}.

**Integrating**

1. Connect your accounts.
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
