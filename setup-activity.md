---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-26"

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

# Activity Insights
{: #setup-activity}

With {{site.data.keyword.security-advisor_long}}, you can continuously monitor your {{site.data.keyword.cloud_notm}} Activity Tracker logs to identify unauthorized or suspicious behavior and changes in your resources. You can use rule packages that are based on best practices that are provided by the service or create your own custom rules.
{: shortdesc}


## Before you begin
{: #activity-prereq}

To get started with Activity Insights, be sure that you have the following prerequisites.

- If you are working on Windows 10, activate Windows Subsystem for Linux and install an [Ubuntu shell](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
- Install the yq CLI:
  * For [macOS and Windows 10](http://mikefarah.github.io/yq/).
  * For CentOS, Red Hat, and Ubuntu run the following commands to install version 1.15.
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- Updated cURL binary: For CentOS and Red Hat, you can update by running `yum update -y nss curl libcurl`.
- The [{{site.data.keyword.cloud_notm}} CLI and required plug-ins](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
- The [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.10.11 or higher
- The [Kubernetes Helm (package manager)](/docs/containers?topic=containers-integrations#helm) v2.9.0 or higher.
- A standard Kubernetes cluster version v1.10.11 or higher


## Creating a COS bucket
{: #activity-setup-cos}

By using the {{site.data.keyword.security-advisor_short}} GUI, you can create a new COS instance and bucket.

1. Navigate to the **Integrations** tab of the service dashboard.

2. Toggle **Analysis Disabled** in the Activity Insights box to **Analysis Enabled**.

3. Click **Go to set up**.

4. In the prerequisites section, click **Create COS instance and bucket**. Your COS instance and bucket are automatically created for you with the proper naming convention and IAM permissions. The bucket information is displayed.

If you have an existing instance of COS and bucket, be sure that it uses the naming convention `sa.<account_id>.telemetric.<cos_region>`. To allow the service to read the data that is stored in your COS instance, set up [service-to-service authorization](/docs/iam?topic=iam-serviceauth#serviceauth) by using {{site.data.keyword.cloud_notm}} IAM. Set `source` to `{{site.data.keyword.security-advisor_short}}` and `target` to your COS instance. Assign the `Reader` IAM role.


## Installing {{site.data.keyword.security-advisor_short}} components
{: #activity-install-components}

You can install an agent to collect audit flow logs from your {{site.data.keyword.cloud_notm}} account. The logs are stored in your Cloud Object Storage instance where you can enable Activity Insights to analyze your logs for suspicious behavior.
{: shortdesc}

1. Clone the following repository to your local system.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Change into the `security-advisor-activity-insights` folder.

3. Extract the `.tar` file by running the following command.

  ```
  tar -xvf security-advisor-activity-insights.tar
  ```
  {: codeblock}

4. Change into `security-advisor-activity-insights` folder.
5. Log in to the {{site.data.keyword.cloud_notm}} CLI. Follow the prompts in the CLI to finish logging in.

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Region</th>
      <th>Endpoint</th>
    </tr>
    <tr>
      <td>United Kingdom</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>US South</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

6. Set the context for your cluster.

  1. Get the command to set the environment variable and download the Kubernetes configuration files.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copy the output beginning with `export` and paste it into your terminal to set the `KUBECONFIG` environment variable.

7. Install Helm by using the [Kubernetes Service integration docs](/docs/containers?topic=containers-integrations#helm).

8. Optional: [Enable TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md). If you're using your workstation to handle the installation of analytics components in multiple clusters and TLS is enabled, be sure that the TLS configurations are current and match the current cluster where you plan to install the components.

9. Run the following command to install the Insights. The command validates the naming convention of your bucket, creates Kubernetes secrets, updates the values with your cluster GUID, and deploys Activity Insights.

  ```
  ./activity-insight-install.sh <cos_region> <cos_api_key> <at_region> <account_api_key> <account_spaces>
  ```
  {: codeblock}

  If you encounter an error, try running `helm init --upgrade`.
  {: tip}

  <table>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>cos_region</code></td>
      <td>The region where your COS instance is deployed. Options include: <code>us-south</code> and <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>cos_api_key</code></td>
      <td>The [API key](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials#service-credentials) that you created to access your COS instance and bucket. The key must have the platform role `writer`.</td>
    </tr>
    <tr>
      <td><code>at_region</code></td>
      <td>The region in which you created your COS instance and bucket. Options include: <code>us-south</code> and <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>account_api_key</code></td>
      <td>The platform API key for your {{site.data.keyword.cloud_notm}} account.</td>
    </tr>
    <tr>
      <td><code>account_spaces</code></td>
      <td>A comma-separated list of the space GUIDs for your {{site.data.keyword.cloud_notm}} account.</td>
    </tr>
  </table>


## Adding rule packages to COS
{: #activity-adding-rules}

A rule package is a JSON file that contains a list of rules that you want to monitor. You can download rule packages or [create your own](/docs/services/security-advisor?topic=security-advisor-activity#activity-packages). The {{site.data.keyword.security-advisor_short}} engine validates that each rule follows the correct syntax.
{: shortdesc}

1. Clone the following repository to get several preset rule packages. A folder is created on your local system with the name `security-advisor-activity-insights`.

  ```
  https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Locally, create a folder with the name `IBM.rules/activities`.

3. Copy the JSON files from `security-advisor-activity-insights/security-advisor-ata-rule-packages` to `IBM.rules/activities`.

4. Navigate to your {{site.data.keyword.cloud_notm}} Dashboard and select the COS service instance that is associated with Activity Insights.

5. On the **Buckets** tab of the service dashboard, select the bucket that is associated with Activity Insights.

5. On the COS instance home page, click **Upload** and select **Folders**.

6. If prompted, click **Install Aspera Connect client**. If you do not see a prompt, you already have the client installed. If you needed to install the client, repeat step 5 when the installation is finished.

7. Select the *IBM.rules/activities* folder.

8. Click **Upload**.

Want to use your own packages? Use one of the JSON files as a guide and create rules that fit your organizations needs. After you create the file, add it to the *IBM.rules/activities* folder in your COS instance. For more information about the types of rules and formatting, check out [Understanding rule packages](/docs/services/security-advisor?topic=security-advisor-activity#activity).
{: tip}


## Deleting the components
{: #activity-delete}

If you no longer need to use Activity Insights, you can delete the service components from your cluster.
{: shortdesc}

1. Delete the service components by using Helm. Be sure to use the `-tls` flag if you have TLS enabled.

  ```
  helm del --purge activity-insights [--tls]
  ```
  {: codeblock}

2. Delete the Kubernetes secrets.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}
