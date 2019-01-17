<staging>---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-17"

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


## Enabling Network Insights
{: #setup}

With {{site.data.keyword.security-advisor_long}}, you can gain insights based on threat intelligence, behavioral patterns, and machine learning for potentially compromised containers that run on your {{site.data.keyword.containerlong_notm}} clusters.
{: shortdesc}

As of January 20th, 2019, Network Insights replaces the beta Network Analytics feature. Any analytics cards in your service dashboard and the findings that they contain have been deleted to make room for the new offering. Don't hesitate to reach out to our team if you encounter any issues when creating your new configurations.
{: important}


## Before you begin
{: #prereq}

Before you can get started with Network Insights, you must install the following pre-requistes:

- For Windows 10, activate Windows Subsystem for Linux and install an [Ubuntu shell](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
- Install the yq CLI
  - For MacOS, Windows 10: Install [yq CLI](http://mikefarah.github.io/yq/)
  - For CentOS, Red Hat and Ubuntu : Install yq CLI version 1.15 by running the following commands:
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- Install cURL binary and ensure that it's updated.
  - For CentOS and Red Hat, you can update by running: `yum update -y nss curl libcurl`
- Install the [IBM Cloud CLI and required plugins](/docs/cli/index.html#overview)
- Install the [Kubernetes CLI (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.10.11 or higher
- Install the [Kubernetes Helm (package manager)](/docs/containers/cs_integrations.html#helm) v2.9.0 or higher.
- A Kubernetes cluster version 1.7 or higher

</br>

## Uninstalling the beta
{: #uninstall}

If you used the beta version of Network Analytics, you must uninstall the old {{site.data.keyword.security-advisor_short}} components before you can install the new ones. Be sure to repeat this process for each cluster that contains any service components.
{: shortdesc}

1. Log in to {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
  ```
  {: pre}

2. List all of the clusters in the account to get the name of the cluster.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3. Target your CLI to the cluster.

  ```
  ibmcloud ks cluster-config <cluster-name-or-id>
  ```
  {: pre}

4. Set the path to the local Kubernetes configuration file as an environment variable. Example:

  ```
  export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
  ```
  {: pre}

5. Navigate to the extracted archive location and run the uninstaller script.

  ```
  ./uninstall.sh
  ```
  {: pre}

6. Optional: Uninstall the Helm server component from the cluster.

  ```
  helm reset
  ```
  {: pre}

</br>

## Creating a COS bucket
{: #setup-guide}

By using the Security Advisor GUI, you can create a new COS instance and bucket.

1. Navigate to **Partners and Add-ons > Add-ons** and click **Create new COS instance and bucket**.

2. SHAWNA: Need to see design

</br>

## Installing {{site.data.keyword.security-advisor_short}} components
{: #install-components}

You can install an agent to collect network flow logs from your Kubernetes cluster. The logs are stored in your Cloud Object Storage instance. You can then enable Network Insights to analyze your the logs and detect and alert you to suspicious network activity.
{: shortdesc}

Be sure to repeat the installation for each cluster that you want to monitor.
{: note}


1. Clone the following repository to your local system.

  ```
  https://github.ibm.com/security-services/security-advisor-network-analytics-installer
  ```
  {: codeblock}

2. Change into the *security-advisor-network-analytics-installer* folder.

3. Unzip the `.tar` file by running the following command.

  ```
  tar -xvf security-advisor-network-analytics.tar
  ```
  {: codeblock}

4. Change into *security-advisor-network-analytics > network-insights-chart** folder.

5. Edit the cluster-config.yaml file to point to your cluster. To find the information you can use the following steps.
  1. Log in to IBM Cloud.

    ```
    ibmcloud login
    ```
    {: codeblock}

  2. Get the information from your cluster.

    ```
    ibmcloud ks <cluster>
    ```
    {: codeblock}

6. Run the following command to install the Insights. The command validates your bucket naming convention, creates Kubernetes secrets, updates the values with your cluster GUID, and deploys Network Insights. If you encounter an error, you can try running `helm init --upgrade`.

  ```
  ./network-insight-install.sh <cos_region> <cos_api_key> <us_south_region_token>
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
      <td>The API key that you created to access your COS instance and bucket. The key should have the platform role `writer`.</td>
    </tr>
    <tr>
      <td><code>us_south_region_token</code></td>
      <td>Optional: SHAWNA: what is this?. If the cluster is located in the <code>eu-gb</code> region, then create a Container Registry token in the <code>us-south</code> region and pass it to the other region.</td>
    </tr>
  </table>

  If you created your COS instance and bucket manually, be sure that it the correct naming convention is followed: `sa.<account_id>.telemetric.<cos_region>`.
  {: tip}

7. To allow Security Advisor to read data that is stored in your COS instance, set up service-to-service authorization by using IBM Cloud IAM. Set `source` to `Security Advisor` and `target` to your COS instance. Assign the `Reader` IAM role.

8. On the **Partners and Add-ons > Add-ons** toggle **Network Insights** to **Enabled** in the Security Advisor GUI.

</br>

## Deleting the components
{: #delete}

If you no longer have a need to use Network Insights with your Cluster, you can delete the service components.
{: shortdesc}

1. Run the following Helm command to delete the components.

```
helm del --purge network-insights
```
{: codeblock}

2. Delete the Kubernetes secrets.

```
kubectl delete ns security-advisor-insights
```
{: codeblock}

Be sure to delete the process for each cluster that you want to remove the agents from.
{: tip}

</br>
</br>
