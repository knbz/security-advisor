---

copyright:
  years: 2018
lastupdated: "2018-12-03"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

## Integrating your security tools
{: #setup_custom}


There are times that you might already have have a tool that you use. You can integrate those tools such as, Neuvector, with {{site.data.keyword.security-advisor_short}}.
{: shortdesc}


**Before you begin**

* You must have an account with the partner that you want to integrate.

{{site.data.keyword.security-advisor_short}} does not persist any credentials that are related to the partner service. Enterprise users must authenticate by using SAML to both {{site.data.keyword.Bluemix_notm}} and to the business partner.
{: note}

1. Log into your security tool and get your unique URL.
2. Log into {{site.data.keyword.Bluemix_notm}}.
3. Click **Custom Integrations** and then click the **Add Custom Solution** card. A screen displays.
  1. Give your solution a name. You can use only alpha-numeric characters, white spaces, and dashes (-) are allowed.
  2. Enter the URL for the solution in the format: `www.<website>.<domain>`.

    {{site.data.keyword.security-advisor_short}} creates the artifacts that are required for integration such as the service ID, API key, account ID, and metadata. The `writer` role is assigned.
    
4. The {{site.data.keyword.security-advisor_short}} team calls you to deliver the information that was generated.
5. You configure the customer account in the **Integration tab** of the business partner service.
6. With the integration in place, you can start posting occurrences to {{site.data.keyword.security-advisor_short}} and view the findings in the service dashboard.
