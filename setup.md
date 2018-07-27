---

copyright:
  years: 2014, 2018
lastupdated: "2018-07-27"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Setting up {{site.data.keyword.security-advisor_short}}
{: #setup}

With {{site.data.keyword.security-advisor_long}}, you can continuously monitor your apps for suspicious activity. You can also detect future vulnerabilities such as expired certificates. When vulnerabilities are found, you are alerted via the service dashboard.
{:shortdesc}

## Monitoring vulnerabilities in container images
{: #setup_images}

A Docker image is the base of every container that you create. The image might contain your app, its configuration, and any dependencies that are required. An image is typically stored in a registry. By using {{site.data.keyword.registryshort_notm}}, you have access to Vulnerability Advisor which continuously scans your images for potential security issues. If issues are found, you are alerted and can view a comprehensive report in your {{site.data.keyword.security-advisor_short}} dashboard.
{:shortdesc}

Learn more about [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html#index).


**Before you begin**

Before you can get started with registry, be sure that you have the following CLIs and plugins installed:
- [The {{site.data.keyword.Bluemix_notm}} CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://clis.ng.bluemix.net/ui/home.html)
- The container-registry plug-in.

    ```
    ibmcloud plugin install container-registry -r Bluemix
    ```
    {: pre}

</br>
**Creating a namespace**

1. Log in to your account by using the CLI.

   ```
   bx login --sso
   ```
   {: pre}

2. Log in to {{site.data.keyword.registryshort_notm}}.

   ```
   bx cr login
   ```
   {: pre}

3. Optional: Create a namespace. You can always use an existing one.

   ```
   bx cr namespace-add
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


After you push images to your {{site.data.keyword.registryshort_notm}} namespace, information about the images, such as the severity of the identified vulnerabilities and configuration issues, is then shown in the **Compute** card. You can also drill down into specific images to find out more information, such as description of all the known vulnerabilities and configuration issues that were identified.

</br>

## Monitoring certificates
{: #setup_certificates}

Did you know that {{site.data.keyword.cloudcerts_long_notm}} can help to monitor and manage your SSL/TLS certificates? By integrating {{site.data.keyword.cloudcerts_short}} and {{site.data.keyword.security-advisor_short}}, you can get alerts and reminders about when you might need to update your certificates and other information. This can help prevent future vulnerabilities.
{:shortdesc}

You can learn more about [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager/index.html#gettingstarted).

1. In the {{site.data.keyword.Bluemix_notm}} catalog, search for "{{site.data.keyword.cloudcerts_short}}".
2. Give your service instance a name, or use the preset name.
3. Click **Create**.
4. To import your organization's certificates into {{site.data.keyword.cloudcerts_short}}, click **Import Certificate**.

Now that you've imported your certificates, information such as expiration times and expired certificates, is shown on the **Certificates** card in the {{site.data.keyword.security-advisor_short}} dashboard. By clicking on the card, you can get more specific information about the certificates, such as which service instance that the certificates belong to. You can also see any steps that you can take to fix the security vulnerabilities.



