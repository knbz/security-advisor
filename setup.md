---

copyright:
  years: 2014, 2018
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

# Taking advantage of pre-integrated services
{: #setup}

{{site.data.keyword.security-advisor_short}} comes with several pre-populated cards that can help you to monitor for security risks and threats.
{: shortdesc}

The following services {{site.data.keyword.security-advisor_short}} automatically creates a card for:

* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloudcerts_long_notm}}

Although you do not have to do anything to create the connection between {{site.data.keyword.security-advisor_short}} and the services, you must have instances of the services configured.


## Monitoring vulnerabilities in container images
{: #setup_images}

With {{site.data.keyword.registryshort_notm}}, you have access to Vulnerability Advisor, which continuously scans the images in your {{site.data.keyword.registryshort_notm}} instance for potential security issues. If issues are found, you are alerted and can view a comprehensive report in your {{site.data.keyword.security-advisor_short}} dashboard.
{:shortdesc}

Learn more about [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html#index).


**Before you begin**

Before you can get started with registry, be sure that you have the following CLIs and plugins installed:
* [The {{site.data.keyword.Bluemix_notm}} CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://clis.ng.bluemix.net/ui/home.html)
* The container-registry plug-in.

  ```
  ibmcloud plugin install container-registry -r Bluemix
  ```
  {: pre}

</br>
**Creating a namespace**

1. Log in to your account by using the CLI.

   ```
   ibmcloud login --sso
   ```
   {: pre}

2. Log in to {{site.data.keyword.registryshort_notm}}.

   ```
   ibmcloud cr login
   ```
   {: pre}

3. Optional: Create a namespace. You can always use an existing one.

   ```
   ibmcloud cr namespace-add
   ```
   {: pre}

3. Tag an image.

   ```
   docker tag <image>:<tag> registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}

5. Push the image.

   ```
   docker push registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}


After you push images to your {{site.data.keyword.registryshort_notm}} namespace, information about any vulnerabilities found is shown in the **Images with Vulnerabilities** card in the service dashboard. You can also drill down into specific images to find out more information, such as a description of all of the identified vulnerabilities and configuration issues.

</br>

## Monitoring certificates
{: #setup_certificates}

Did you know that {{site.data.keyword.cloudcerts_long_notm}} can help to monitor and manage your SSL/TLS certificates? By integrating {{site.data.keyword.cloudcerts_short}} and {{site.data.keyword.security-advisor_short}}, you can get alerts and reminders about when you might need to update your certificates and other information, which can help prevent future vulnerabilities.
{:shortdesc}

You can learn more about [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager/index.html#gettingstarted).

1. In the {{site.data.keyword.Bluemix_notm}} catalog, search for "{{site.data.keyword.cloudcerts_short}}".
2. Give your service instance a name, or use the preset name.
3. Click **Create**.
4. To import your organization's certificates into {{site.data.keyword.cloudcerts_short}}, click **Import Certificate**.

Now that your certificates are imported, information such as expiration times and expired certificates, is shown on the **Certificates** card in the {{site.data.keyword.security-advisor_short}} dashboard. By clicking the card, you can get more specific information about the certificates, such as which service instance that the certificates belong to. You can also see any steps that you can take to fix the security vulnerabilities.
