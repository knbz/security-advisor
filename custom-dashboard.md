---

copyright:
  years: 2018
lastupdated: "2018-08-28"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Customizing your information
{: #custom}

{{site.data.keyword.security-advisor_short}} pulls findings from other {{site.data.keyword.Bluemix}} APIs such as Vulnerability Advisor and {{site.data.keyword.cloudcerts_short}}. For example, {{site.data.keyword.security-advisor_short}} might display findings of a server that is infected with malware, a container with vulnerabilities, and related metadata.
{:shortdesc}

Why would you want to create customizations? Say you have an application that is running a {{site.data.keyword.containershort_notm}} cluster with the name `cloudkingdom`. One of the pods in the cluster is sending an abnormal amount of data to external servers. You want to capture this finding in your {{site.data.keyword.security-advisor_short}} dashboard.

You can use the API to:
  - Register new finding types
  - Map finding types by using metadata
  - Record occurrences of findings as KPIs and events

<br/>

## Customizing your information
{: #customizing}

You can use the {{site.data.keyword.security-advisor_short}} API to customize the Network and Compute information that is displayed by making a post request to the API: `https://console.bluemix.net/apidocs/security-advisor`
{: shortdesc}

**Before you begin**

*  Get your {{site.data.keyword.Bluemix_notm}} account ID.
  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: pre}
*  Retrieve your IAM token. The token is used in the `--header` of each API request.
  ```
  bx iam oauth-tokens
  ```
  {: pre}

**Customizing**

1.  Create a project by posting a `curl` request. Note the `id` in the response. You use it in the next step to target the project endpoint.

    Request:

    ```
    curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: <iam-token>'\
    -d '{ "id": "my-security-advisor", "name": "my-security-advisor", "shared": false }'\
    'https://console.bluemix.net/apidocs/security-advisor'
    ```
    {: codeblock}

    Example response:

    ```
    {
      "id": "my-security-advisor",
      "name": "projects/my-security-advisor",
      "shared": false
    }
    ```
    {: screen}

2.  Create a `note` and give it an `id` such as `notes-2`. You use the ID in the next step to capture occurrences.

    Request:

    ```
    curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' \
    --header 'Authorization: <iam-token>' \
     -d '{
       "kind": "FINDING",
       "id": "notes-2",
       "reported_by": {
           "id": "outlier",
           "title": "{{site.data.keyword.security-advisor_short}}"
       },
       "finding":
           {
               "severity":
                   "HIGH",
               "next_steps":
                   [
                       {
                           "title": "Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities"
                       }]
           }
       ,
       "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
       "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
    }' 'https://console.bluemix.net/apidocs/security-advisor/projects/my-security-advisor/notes'
    ```
    {: codeblock}

    Example response:

    ```
     {
      "create_time": "2018-03-14T12:09:31.442042Z",
      "create_timestamp": 1521029371.442042,
      "finding": {
        "next_steps": [
          {
            "title": "Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities"
          }
        ],
        "severity": "HIGH"
      },
      "id": "notes-2",
      "kind": "FINDING",
      "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod\u2019s normal behavior",
      "name": "projects/my-security-advisor/notes/notes-2",
      "project_doc_id": "2436eb67dafff1627d2fb3d92bda7fcd/projects/my-security-advisor",
      "project_id": "my-security-advisor",
      "reported_by": {
        "id": "outlier",
        "title": "{{site.data.keyword.security-advisor_short}}"
      },
      "shared": true,
      "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
      "update_time": "2018-03-14T12:09:31.442042Z",
      "update_timestamp": 1521029371.442042,
      "update_week_date": "2018-W11-3"
    }
    ```
    {: screen}



3.  Capture the `occurrences` of the finding. Replace `"<account-id>"` under `"context"` with your {{site.data.keyword.Bluemix_notm}} account ID.

    Request:

    ```
    curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: <iam-token>' \
     -d '{
       "kind": "FINDING",
       "id": "occurrence2",
       "note_name": "projects/my-security-advisor/notes/notes-2",

       "context": {
           "region": "dal10",
           "account_id": "<account-id>",
           "resource_name": "cloudkingdom-pod",
           "resource_type": "Pod",
           "service_crn": "crn:v1:staging:public:containers-kubernetes:dal10:a/2436eb67dafff1627d2fb3d92bda7fcd:39438df3496a49e8aa39eb556ab15b0e::",
           "service_name": "Kubernetes Cluster"
       },
       "finding": {
           "certainty": "HIGH",
           "network": {
               "direction": "Outbound",
               "protocol": "Ethernet/IPv4/TCP"
           },
           "data_transferred": {
               "client_bytes": 983412
           }
       }
    }' 'https://console.bluemix.net/apidocs/security-advisor/projects/my-security-advisor/occurrences'
    ```
    {: codeblock}

    Example response:

    ```
    {
      "context": {
        "account_id": "<account-id>",
        "region": "dal10",
        "resource_name": "cloudkingdom-pod",
        "resource_type": "Pod",
        "service_crn": "crn:v1:staging:public:containers-kubernetes:dal10:a/2436eb67dafff1627d2fb3d92bda7fcd:39438df3496a49e8aa39eb556ab15b0e::",
        "service_name": "Kubernetes Cluster"
      },
      "create_time": "2018-03-14T12:10:43.446982Z",
      "create_timestamp": 1521029443.446982,
      "finding": {
        "certainty": "high",
        "data_transferred": {
          "client_bytes": 983412
        },
        "network": {
          "direction": "Outbound",
          "protocol": "Ethernet/IPv4/TCP"
        },
        "next_steps": [
          {
            "title": "Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities"
          }
        ],
        "severity": "high"
      },
      "id": "occurrence2",
      "kind": "FINDING",
      "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod\u2019s normal behavior",
      "name": "projects/my-security-advisor/occurrences/occurrence2",
      "note_doc_id": "2436eb67dafff1627d2fb3d92bda7fcd/projects/my-security-advisor/notes/notes-2",
      "note_name": "projects/my-security-advisor/notes/notes-2",
      "project_doc_id": "2436eb67dafff1627d2fb3d92bda7fcd/projects/my-security-advisor",
      "project_id": "my-security-advisor",
      "reported_by": {
        "id": "outlier",
        "title": "{{site.data.keyword.security-advisor_short}}"
      },
      "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
      "update_time": "2018-03-14T12:10:43.446982Z",
      "update_timestamp": 1521029443.446982,
      "update_week_date": "2018-W11-3"
    }
    ```
    {: screen}


