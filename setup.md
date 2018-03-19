---

copyright:
  years: 2018
lastupdated: "2018-03-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Setting up IBM Cloud Security Advisor
{: #setup}
With Security Advisor, you can use a variety of detection capabilities to find and alert you about suspicious network activity.
{:shortdesc}

## Setting up monitoring of vulnerabilities in container images
{: #setup_images}

To get started with monitoring of container images, push Docker container images to an IBM Cloud Container Registry namespace created in your IBM Cloud account.

Learn more about [IBM Cloud Container Registry](/docs/services/Registry/index.html#index).
{:shortdesc}

1. Using the IBM Cloud CLI, login to your account:

   ```
   bx login --sso
   ```
   {: pre}

2. Login to the IBM Cloud Container Registry using the CR plug-in:

   ```
   bx cr login
   ```
   {: pre}

3. Optional: Create a namespace or use an existing one. To create a namespace:

   ```
   bx cr namespace-add
   ```
   {: pre}

3. Tag an image to be pushed to the namespace in the Container Registry, e.g. in US South:

   ```
   docker tag <image>:<tag> registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}

5. Push the image:

   ```
   docker push registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}



After you push images to your Container Registry namespace, information about the images, such as the severity of the identified vulnerabilities and configuration issues, is then shown in the **Compute** card.

You can also drill down into specific images to find out more information, such as description of all the known vulnerabilities and configuration issues that were identified.

## Setting up monitoring of certificates
{: #setup_certificates}

To get started with monitoring of certificates, create an instance of {{site.data.keyword.cloudcerts_long_notm}} and import your SSL certificates.

Learn more about [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager/index.html#gettingstarted).
{:shortdesc}

1. In the {{site.data.keyword.Bluemix_notm}} catalog, search for "{{site.data.keyword.cloudcerts_short}}".
2. Give your service instance a name, or use the preset name.
3. Click **Create**.
4. To import your organization's certificates into {{site.data.keyword.cloudcerts_short}}, click **Import Certificate**.



After you import certificates, information about certificates, such as expired certificates and certificates that are about to expired, is then shown in the **Certificates** card.

You can also drill down into specific certificates to find out more information, such as the instance that the certificate belongs to as well as remediation steps.


