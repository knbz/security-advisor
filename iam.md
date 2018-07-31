---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-31"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Managing service access
{: #service-access}

As an account owner, you can manage access to instances of {{site.data.keyword.security-advisor_long}}, by using {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). By setting policies within your account that create different levels of access for different users, you can ensure that each instance of {{site.data.keyword.security-advisor_short}} is secure.
{: shortdesc}

For more information about IAM, see [IAM Access](/docs/iam/users_roles.html).

## {{site.data.keyword.security-advisor_short}} access policies
{: #access}

Every user that accesses an instance of the {{site.data.keyword.security-advisor_short}} service in your account must be assigned an access policy with an IAM user role defined. The policy determines which actions that a user can perform within the context of that specific service instance.
{: shortdesc}

To view the {{site.data.keyword.security-advisor_short}} dashboard, you must be assigned either the `Administrator` or `Editor` platform role.
{: tip}

The actions are customized and defined by the {{site.data.keyword.Bluemix_notm}} service as operations that are allowed to be performed in the service. The actions are then mapped to IAM service user roles. In the following table, the actions and required permissions for {{site.data.keyword.security-advisor_short}} are mapped.

<table>
<caption>Roles required for actions</caption>
<col width="35%">
<col width="35%">
<col width="10%">
<col width="10%">
<col width="10%">
  <tr>
    <th>Action</th>
    <th>Explanation</th>
    <th>Reader</th>
    <th>Writer</th>
    <th>Manager</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>View security report findings.</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Generate security report findings.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Delete security report findings.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Edit security report findings.</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>View metadata.</td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Delete metadata.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Generate metadata.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Update metadata.</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="Feature available" style="width:32px;" /></td>
  </tr>
</table>

## API mappings
{: #api-map}

How do those roles correspond to the API? Check out the following table to see how the service actions are mapped to the API.


| Method | Endpoint                                                                |  Service action                  |
|--------|-------------------------------------------------------------------------|----------------------------------|
| POST   | /v1/{account_id}/graph                                                  | security-advisor.findings.read   |
| POST   | /v1/{account_id}/projects/{project_id}/notes                            | security-advisor.metadata.write  |
| GET    | /v1/{account_id}/projects/{project_id}/notes                            | security-advisor.metadata.read   |
| GET    | /v1/{account_id}/projects/{project_id}/notes/{note_id}                  | security-advisor.metadata.read   |
| PUT    | /v1/{account_id}/projects/{project_id}/notes/{note_id}                  | security-advisor.metadata.update |
| DELETE | v1/{account_id}/projects/{project_id}/notes/{note_id}                   | security-advisor.metadata.delete |
| GET    | /v1/{account_id}/projects/{project_id}/occurrences/{occurrence_id}/note | security-advisor.findings.read   |
| POST   | /v1/{account_id}/projects/{project_id}/occurrences                      | security-advisor.findings.write  |
| GET    | /v1/{account_id}/projects/{project_id}/occurrences                      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/projects/{project_id}/notes/{note_id}/occurrences      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/projects/{project_id}/occurrences/{occurrence_id}      | security-advisor.findings.read   |
| PUT    | /v1/{account_id}/projects/{project_id}/occurrences/{occurrence_id}      | security-advisor.findings.update |
| DELETE | /v1/{account_id}/projects/{project_id}/occurrences/{occurrence_id}      | security-advisor.findings.delete |




## How it works
{: #how-it-works}



**How does the service connect to IAM?**
{{site.data.keyword.security-advisor_short}} communicates to Cloud IAM by using unique service ID's and API keys that are generated by Resource Management Console. An Administrator ID is used to define the roles and actions for the service. An `Operator` is used to validate the roles for a service ID or user.

**How are custom links handled?**
Custom links to business partner solutions are managed as a metadata. A business partner is a service that is integrated into {{site.data.keyword.security-advisor_short}}.

**Scenario 1 - Viewing findings**
As an Administrator, you can grant a user a `Reader` service access role to those users that require access to the security report findings. When the user logs into the dashboard, their IAM token is passed to {{site.data.keyword.security-advisor_short}}. The service validates the token and provides access.

**Scenario 2 - Using a service ID to post findings**
As an Administrator for a {{site.data.keyword.security-advisor_short}} instance, you can create a service ID that can be assigned as the posting entity. To have the service ID be the entity, you can grant the service access role, `Writer` to the ID. When the ID is used to make a `POST` request, Grafeas verifies the role with a token.

**Scenario 3 - Posting findings through a business partner**
As an `Administrator`, you can create a service ID for a business partner and then set an access policy for that partner. The API validates the token to ensure that it is assigned the right level of access. If the ID has the right permissions, then the partner is able to post to the dashboard.
