<staging>---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-17"

---

{:new_window: target="blank"}
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
## Adding a package to COS
{: #adding}

A rule package is a JSON file that contains a list of rules that you want to monitor. You can download rule packages or create your own. The Security Advisor engine validates that each rule follows the correct syntax.
{: shortdesc}

1. Download the package
2. Add a rule to the package then wait
3. When a finding is triggered add a finding with the same finding id to see it in the dashboard

Account admins may install a rule package into {{site.data.keyword.security-advisor_short}} COS bucket under "IBM.rules/activities" [add how to find the SA bucket name]


1. Add another rule to a list of rules in the JSON file.
	SHAWNA: INPUT EXAMPLE
2. To view the [finding](link) in your service dashboard, add a finding with the same finding ID.
	SHAWNA: INPUT EXAMPLE


Setting up Network Insights



Intro

Follow the steps below to install an agent to collect audit flow logs from your Kubernetes cluster. These audit flow logs will be stored in your Cloud Object Storage (COS) instance. You can then enable Security Advisor Activity Insights to analyze your activity flow logs to detect and alert you to suspicious activity.
Prerequisites

    For Windows 10, Before starting with steps mentioned above, activate WSL(windows subsystem for linux) and install ubuntu shell
    yq CLI
        For MacOS, Windows 10: Install yq CLI
        For CentOS, Red Hat and Ubuntu : Install yq CLI version 1.15 using below steps:
        wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64
        mv yq_linux_amd64 yq
        chmod +x yq
        mv yq /usr/local/bin/
        yq -V
    curl binary
        For CentOS and Red Hat, update curl binary using yum update -y nss curl libcurl
    Install IBM CLI

Steps to run

    Download the tar file.
    Unzip using tar -xvf security-advisor-activity-insights.tar
    cd security-advisor-activity-insights
    Run ./activity-insight-install.sh <cos_region> <cos_api_key> <at_region> <account_api_key> <account_spaces> <us_south_region_token>
        <cos_region> value is either us-south or eu-gb – the region where your COS is deployed
        <cos_api_key> is the api key you created to access your COS instance and bucket should have a Writer Role
        <at_region> value is either us-south or eu-gb - the region of the Activity Tracker instance
        <account_api_key> is the platform api key of your IBM Cloud account
        <account_spaces> comma seperate list of IBM account space GUID
        <us_south_region_token> optional parameter, if the cluster is in eu-gb region then create a registry token in the us-south region and pass it over here

Note:

    If you create your COS instance and bucket manually (not via Security Advisor UI), make sure to use the following naming convention for the bucket: sa.<account_id>.telemetric.<cos_region>. Also set up service-to-service authorization in IBM Cloud IAM for Security Advisor to read data from your COS instance. Set the source service to Security Advisor and the target service to your cloud object storage instance with a Reader IAM role.
    Take the rule packages and upload to your cos bucket. Default set of rule packages can be found here.

Deleting the setup

    helm del --purge activity-insights
    kubectl delete ns security-advisor-insights

Troubleshooting

    if you get an error something like Error: incompatible versions client and server, run helm init --upgrade
