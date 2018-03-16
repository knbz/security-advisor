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

# Customizing the dashboard
{: #custom_dashboard}

With the custom dashboard experimental feature in IBM Cloud Security Advisor, you can customize the visualization of Network and Compute related security findings.
{:shortdesc}

Security Advisor pulls findings from other {{site.data.keyword.Bluemix}} APIs such as Vulnerability Advisor, and {{site.data.keyword.cloudcerts_short}}. For example, Security Advisor might display findings of a server that is infected with malware, a container with vulnerabilities, and related metadata.

You can use the findings API to:
  - Register new finding types
  - Map finding types to UI cards by using meta data
  - Record occurrences of findings as KPIs and events
  - Organize data under logical tiles

<br/>

## Using the custom dashboard
{: #using}

You can customize two dashboard sections: Network and Compute. To add security data to the sections, post the relevant API payloads to the Findings API endpoint: `http://grafeas.ng.bluemix.net/v1alpha1/`.

For access to the Findings API, [post in dW Answers with the `securityadvisor` tag](https://developer.ibm.com/answers/search.html?f=&type=question&q=securityadvisor).

For more information about the Findings API, see the [swagger documentation](http://grafeas.ng.bluemix.net/ui/).

## Example of customizing the dashboard
{: #example}

In this example, you have an application that is running {{site.data.keyword.containershort_notm}} Kubernetes cluster with the name `cloudkingdom`. One of the pods in this cluster is sending an abnormal amount of data to external servers. You want to capture this finding in your Security Advisor custom dashboard, so you use the Findings API.

Before you begin:
*  Get the {{site.data.keyword.Bluemix_notm}} account ID by using the `bx account list` [command](/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_account_list).
*  Fetch the IAM token by using the `bx iam oauth-tokens` [command](/docs/cli/reference/bluemix_cli/bx_cli.html#bluemix_iam_oauth_tokens). Use this in the `--header` of each API request.
* Request access to Findings API by [posting in dW Answers with the `securityadvisor` tag](https://developer.ibm.com/answers/search.html?f=&type=question&q=securityadvisor).

Steps:

1.  Create a project by posting a `curl` request to the Findings API. Note the `id` in the response, which you use in the next step to target the project in the Findings API endpoint.

    **Request:**

    ```
    curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: <iam-token>'\
    -d '{ "id": "my-security-advisor", "name": "my-security-advisor", "shared": false }'\
    'https://grafeas.ng.bluemix.net/v1alpha1/projects'
    ```
    {: codeblock}

    **Response:**

    ```
    {
      "id": "my-security-advisor",
      "name": "projects/my-security-advisor",
      "shared": false
    }
    ```
    {: screen}

2.  Create a `note` and give it an `id` such as `notes-2` that you use in next step to capture occurences.

    **Request:**

    ```
    curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' \
    --header 'Authorization: <iam-token>' \
     -d '{
       "kind": "FINDING",
       "id": "notes-2",
       "reported_by": {
           "id": "outlier",
           "title": "Security Advisor"
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
    }' 'https://grafeas.ng.bluemix.net/v1alpha1/projects/my-security-advisor/notes'
    ```
    {: codeblock}

    **Response:**

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
        "title": "Security Advisor"
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

    **Request:**

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
    }' 'https://grafeas.ng.bluemix.net/v1alpha1/projects/my-security-advisor/occurrences'
    ```
    {: codeblock}

    **Response:**

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
        "title": "Security Advisor"
      },
      "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
      "update_time": "2018-03-14T12:10:43.446982Z",
      "update_timestamp": 1521029443.446982,
      "update_week_date": "2018-W11-3"
    }
    ```
    {: screen}

4.  Create a card to show the findings that are captured for `notes-2` occurrences.

    **Request:**

    ```
    curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: <iam-token>' \
     -d '{
       "kind": "CARD",
       "id": "data-leakage-card",
       "short_description": "Data Leakage",
       "long_description": "Data Leakage card",
       "shared": false,

       "card": {
           "section": "Network",
           "title": "Data Leakage",
           "finding_note_names": [
               "projects/my-security-advisor/notes/notes-2"
           ],
           "elements": [
               {
                   "kind": "NUMERIC",
                   "text": "Excessive data",
                   "value_type": {
                       "kind": "FINDING_COUNT",
                       "finding_note_names": [
                           "projects/my-security-advisor/notes/notes-2"
                       ]
                   }
               },
               {
                   "kind": "TIME_SERIES",
                   "text": "Dala Leakage History",
                   "default_interval": "1d",
                   "default_time_range": "1w",
                   "value_types": [
                       {
                           "kind": "FINDING_COUNT",
                           "finding_note_names": [
                               "projects/my-security-advisor/notes/notes-2"
                           ],
                           "text": "Excessive"
                       }
                   ]
               }
           ]
       }
     }' 'https://grafeas.ng.bluemix.net/v1alpha1/projects/my-security-advisor/notes'
    ```
    {: codeblock}

    **Response:**

    ```
    {
      "card": {
        "elements": [
          {
            "kind": "NUMERIC",
            "text": "Excessive data",
            "value_type": {
              "finding_note_names": [
                "projects/my-security-advisor/notes/notes-2"
              ],
              "kind": "FINDING_COUNT"
            }
          },
          {
            "default_interval": "1d",
            "default_time_range": "1w",
            "kind": "TIME_SERIES",
            "text": "Dala Leakage History",
            "value_types": [
              {
                "finding_note_names": [
                  "projects/my-security-advisor/notes/notes-2"
                ],
                "kind": "FINDING_COUNT",
                "text": "Excessive"
              }
            ]
          }
        ],
        "finding_note_names": [
          "projects/my-security-advisor/notes/notes-2"
        ],
        "section": "Network",
        "title": "Data Leakage"
      },
      "create_time": "2018-03-14T12:09:04.366685Z",
      "create_timestamp": 1521029344.366685,
      "id": "data-leakage-card",
      "kind": "CARD",
      "long_description": "Data Leakage card",
      "name": "projects/my-security-advisor/notes/data-leakage-card",
      "project_doc_id": "2436eb67dafff1627d2fb3d92bda7fcd/projects/my-security-advisor",
      "project_id": "my-security-advisor",
      "shared": false,
      "short_description": "Data Leakage",
      "update_time": "2018-03-14T12:09:04.366685Z",
      "update_timestamp": 1521029344.366685,
      "update_week_date": "2018-W11-3"
    }
    ```
    {: screen}

5.  Verify that your card is in the [Security Advisor Custom Dashboard](https://console.bluemix.net/security/advisor/#!/custom-dashboard).
