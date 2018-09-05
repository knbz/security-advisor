---

copyright:
  years: 2018
lastupdated: "2018-09-05"

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


There are times that you might already have have a tool that you use internally. You can integrate those tools such as, Aqua or Twistlock, with {{site.data.keyword.security-advisor_short}}.
{:shortdesc}


**Before you begin**

* Prior to integrating custom tools, you must sign an agreement with the {{site.data.keyword.security-advisor_short}} offering team.
* You must have an {{site.data.keyword.Bluemix_notm}} account.
* You must have a Partner Service account.
* You must share your metamodel with the {{site.data.keyword.security-advisor_short}} team.

**Note**: {{site.data.keyword.security-advisor_short}} does not persist any credentials that are related to the partner service. Enterprise users must authenticate by using SAML to both {{site.data.keyword.Bluemix_notm}} and to the business partner.

1. Log into the partner service and get a unique URL.
2. Log into {{site.data.keyword.Bluemix_notm}}.
3. Click **Business Partner Add On**. A screen displays. Fill out the information including the URL that you obtained in step 1. When you finish, {{site.data.keyword.security-advisor_short}} creates the artifacts that are required for integration such as the service ID, API key, account ID, and metadata. The `writer` role is assigned.
4. The {{site.data.keyword.security-advisor_short}} team calls you to deliver the information that was generated.
5. You configure the customer account in the **Integration tab** of the business partner service.
6. With the integration in place, you can start posting occurrences to {{site.data.keyword.security-advisor_short}} and view the findings in the service dashboard.
