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


# Setting up monitoring of suspicious clients and server IPs for a Kubernetes cluster
{: #cluster_install}

To try out the Security Advisor network analysis feature, install the Security Advisor components in a Kubernetes cluster that is deployed to {{site.data.keyword.containerlong_notm}}. Then, you can see network insights and alerts in the Security Advisor dashboard.
{:shortdesc}

> **Note:** This feature of Security Advisor is a technology preview.

Learn how to use Security Advisor in your cluster:
* [Prerequisites](#cluster_prereqs)
* [Logging in to your cluster](#login)
* [Installing Security Advisor components](#cluster_components)
* [Removing Security Advisor components from your cluster](#cluster_uninstall)

## Prerequisites
{: #cluster_prereqs}

Before you begin:

* Mac, Linux or Windows 10 developer workstation
  * Windows 10: [enable the Linux Subsystem feature](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
* [Node.js](https://nodejs.org/en/) 6 or above
* [jQ](https://stedolan.github.io/jq/download/)
* [{{site.data.keyword.Bluemix_notm}} Command Line Interface](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 or above
* [{{site.data.keyword.containerlong_notm}} CLI plug-in](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
* [Kubernetes CLI (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 or above
* [Kubernetes Helm (package manager)](https://docs.helm.sh/using_helm/#from-script)
* [A free or standard Kubernetes cluster](https://console.bluemix.net/containers-kubernetes/catalog/cluster) in the **US South** region of {{site.data.keyword.Bluemix_notm}}
* Identify the {{site.data.keyword.Bluemix_notm}} account owner to complete the [installation steps](#cluster_components)

## Logging in to your cluster
{: #login}

1.  Log in to {{site.data.keyword.Bluemix_notm}}.

    ```
    bx login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  List all of the clusters in the account to get the name of the cluster.

    ```
    bx cs clusters
    ```
    {: pre}

3.  Target your CLI to the cluster.

    ```
    bx cs cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  Set the path to the local Kubernetes configuration file as an environment variable. Example:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  Set up Helm in the cluster.

    ```
    helm init
    ```
    {: pre}

> **Note:** Keep this command line window open and continue.

## Installing Security Advisor components
{: #cluster_components}

> **Note:** The installer must be executed by the {{site.data.keyword.Bluemix_notm}} account owner.

When you set up Security Advisor, the following actions take place:
1. Cluster metadata is collected and used to complete the installation of worker node IP addresses, subnets, Ingress URL and IP address, cluster CRN, Log Analysis endpoint, space, and token.
2. An IAM service ID and API key are generated in your {{site.data.keyword.Bluemix_notm}} account so that your cluster can be identified by Security Advisor. If you have more than one cluster, run the installation for each cluster to generate service IDs and API keys for each cluster.
3. The Security Advisor components are deployed in the **monitoring** namespace in your cluster.

<br/>
To install the Security Advisor components:

1.  Download and extract the [installation package](https://github.com/Bluemix-Docs/security-advisor/blob/staging/installation.tar.gz?raw=true).
2.  From the same command line window as the previous section, navigate to the extracted archive location and install the package.

    ```
    ./install.sh
    ```
    {: pre}

3.  Verify that the components are installed properly by checking the [Security Advisor dashboard](https://console.bluemix.net/security/advisor/#!/dashboard).

## Removing Security Advisor components from your Kubernetes cluster
{: #cluster_uninstall}

Remove the Security Advisor components from your cluster and the service ID and API key from your {{site.data.keyword.Bluemix_notm}} account.

1.  Log in to {{site.data.keyword.Bluemix_notm}}.

    ```
    bx login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  List all of the clusters in the account to get the name of the cluster.

    ```
    bx cs clusters
    ```
    {: pre}

3.  Target your CLI to the cluster.

    ```
    bx cs cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  Set the path to the local Kubernetes configuration file as an environment variable. Example:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  Navigate to the extracted archive location and run the uninstaller script.

    ```
    ./uninstall.sh
    ```
    {: pre}

6.  Optional: Uninstall the Helm server component from the cluster.

    ```
    helm reset
    ```
    {: pre}
