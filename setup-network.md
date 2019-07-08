---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-08"

---

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

# Network Insights
{: #setup-network}

With {{site.data.keyword.security-advisor_long}}, you can monitor behavior by using machine learning, learned patterns, and threat intelligence to detect potentially compromised containers that run on your {{site.data.keyword.containerlong_notm}} clusters.
{: shortdesc}

As of 20 January 2019, Network Insights (beta) replaces the Network Analytics feature. Any analytics cards in your service dashboard are deleted, but the findings remain in the findings database.
{: important}

## Before you begin
{: #network-prereq}

If you are currently using the Network Analytics feature, you must [delete the service components](/docs/services/security-advisor?topic=security-advisor-setup-network#uninstall-analytics) before you can install Network Insights. To get started with Network Insights, be sure that you have the following prerequisites.

- If you are working on Windows 10, activate Windows Subsystem for Linux and install an [Ubuntu shell](https://win10faq.com/install-run-ubuntu-bash-windows-10/){: external}.
- Install the yq CLI:
  * For [macOS and Windows 10](http://mikefarah.github.io/yq/){: external}.
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
- The [{{site.data.keyword.cloud_notm}} CLI and required plug-ins](/docs/cli/reference/ibmcloud?topic=cloud-cli-install-ibmcloud-cli)
- The [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external} v1.10.11 or higher
- The [Kubernetes Helm (package manager)](/docs/containers?topic=containers-helm) v2.9.0 or higher.
- A standard Kubernetes cluster version v1.10.11 or higher

## Creating a COS bucket
{: #network-setup-cos}

By using the {{site.data.keyword.security-advisor_short}} GUI, you can create a new COS instance and bucket.

1. On the **Integrations** tab of the service dashboard, toggle **Analysis Disabled** in the Network Insights box to **Analysis Enabled**.

2. Click **Go to set up**.

3. In the prerequisites section, click **Create COS instance and bucket**. Your COS instance and bucket are automatically created for you with the proper naming convention and IAM permissions.

If you have an existing instance of COS and bucket, be sure that it uses the naming convention: `sa.<account_id>.telemetric.<cos_region>`. To allow the service to read the data that is stored in your COS instance, set up [service-to-service authorization](/docs/iam?topic=iam-serviceauth) by using {{site.data.keyword.cloud_notm}} IAM. Set `source` to `{{site.data.keyword.security-advisor_short}}` and `target` to your COS instance. Assign the `Reader` IAM role.


## Installing {{site.data.keyword.security-advisor_short}} components
{: #network-install-components}

You can install an agent to collect network flow logs from your Kubernetes cluster. The logs are stored in your Cloud Object Storage instance. You can then enable Network Insights to analyze your logs and detect and alert you to suspicious network activity.
{: shortdesc}

Be sure to repeat the installation for each cluster that you want to monitor.
{: note}

1. Log in to the {{site.data.keyword.cloud_notm}} CLI. Follow the prompts in the CLI to complete finish logging in.

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

2. Set the context for your cluster.

  1. Get the command to set the environment variable and download the Kubernetes configuration files.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copy the output beginning with `export` and paste it into your terminal to set the `KUBECONFIG` environment variable.

3. Get the version of your Kubernetes cluster.

  ```
  kube_version=$(kubectl version --output json) echo $(echo $kube_version | yq r - serverVersion.major).$(echo $kube_version | yq r - serverVersion.minor)
  ```
  {: codeblock}

3. If you're using Kubernetes Service v1.12.x, install Helm by using the following commands. If not, checkout the Kubernetes documentation to see which installation steps that you should take to [set up Helm in a cluster with public access](/docs/containers?topic=containers-helm#public_helm_install).

  1. Delete any existing deployments.

    ```
    kubectl delete deployment tiller-deploy -n kube-system
    ```
    {: codeblock}

  2. Apply the Tiller RBAC policies to your deployment.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: codeblock}
  
  3. Initialize Helm in your cluster and install Tiller in your cluster.

    ```
    helm init --service-account tiller
    ```
    {: codeblock}

  4. Check if your installation is successful by verifying that the status of the `tiller-deploy` pod is `running`.

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: codeblock}

4. Ensure that your Kubernetes Service is version 1.11 or later and then clone the following repository to your local system.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-network-insights.git
  ```
  {: codeblock}

  Kubernetes Service version 1.10 is deprecated and is not supported. If you already have v1.10+ installed, fix the vulnerabilities in the existing image by restarting the analyzer pods by running the following Helm command: `helm upgrade --recreate-pods network-insights`.
  {: deprecated}

5. Change into the `security-advisor-network-insights` folder.

6. Change into the `v1.10+` directory.

7. Extract the `.tar` file by running the following command.

  ```
  tar -xvf security-advisor-network-insights.tar
  ```
  {: codeblock}

8. Change into the `security-advisor-network-insights` folder.

9. Install Helm by using the [Kubernetes Service integration docs](/docs/containers?topic=containers-helm).

10. Optional: [Enable TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. If you're using your workstation to handle the installation of analytics components in multiple clusters and TLS is enabled, be sure that the TLS configurations are current and match the current cluster where you plan to install the components.

11. Run the following command to install the Helm chart and its dependencies. The command validates that your bucket uses the correct naming convention, creates Kubernetes secrets, updates the values with your cluster GUID, and deploys the Network Insights Helm chart. If you encounter an error, try running `helm init --upgrade`.

  ```
  ./network-insight-install.sh <cos_region> <cos_api_key>
  ```
  {: codeblock}

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
      <td>The [API key](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) that you created to access your COS instance and bucket. The key must have the platform role `writer`.</td>
    </tr>
  </table>

You successfully configured your cluster to work with {{site.data.keyword.security-advisor_short}} Network Insights!



## Deleting the components
{: #network-delete}

If you no longer have a need to use Network Insights, you can delete the service components from your cluster.
{: shortdesc}

1. Log in to the {{site.data.keyword.cloud_notm}} CLI. Follow the prompts in the CLI to complete finish logging in.

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

2. Set the context for your cluster.

  1. Get the command to set the environment variable and download the Kubernetes configuration files.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copy the output beginning with `export` and paste it into your terminal to set the `KUBECONFIG` environment variable.

3. Delete the components by using Helm.

  ```
  helm del --purge network-insights [--tls]
  ```
  {: codeblock}

4. Delete the Kubernetes secrets.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}

Be sure to delete the process for each cluster that you want to remove the agents from.
{: tip}

## Uninstalling Network Analytics
{: #uninstall-analytics}

If you used the beta version of Network Analytics, you must uninstall the old {{site.data.keyword.security-advisor_short}} components before you can install the new ones. Be sure to repeat this process for each cluster that contains any service components.
{: shortdesc}

1. Log in to {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
  ```
  {: codeblock}

2. List all of the clusters in your account to get the name of the cluster.

  ```
  ibmcloud ks clusters
  ```
  {: codeblock}

3. Set the context for your cluster.

  1. Get the command to set the environment variable and download the Kubernetes configuration files.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copy the output beginning with `export` and paste it into your terminal to set the `KUBECONFIG` environment variable.

4. Navigate to the extracted archive location and run the uninstaller script.

  ```
  ./uninstall.sh
  ```
  {: codeblock}

5. Optional: Uninstall the Helm server component from the cluster.

  ```
  helm reset
  ```
  {: codeblock}
