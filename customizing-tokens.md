---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-15"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}


# Customizing Tokens
{: #customizing-tokens}

You can configure your {{site.data.keyword.appid_short_notm}} tokens to meet the specific needs of your application.
{: shortdesc}

**What kinds of tokens are there?**

App ID uses four different types of tokens to secure your applications.

* Access tokens: Enable communication with back-end resources that are protected by authorization filters.
* Identity tokens: Contain personal information and are used to authenticate a user.
* Refresh tokens: Contain both an access and an identity token, and can be used to extend the amount of time that a user can go without re-authenticating. When a user interacts with your application, a new set of tokens is created and the refresh token is updated.
* Anonymous tokens: Contain a set of access and identity tokens that are blank as these are issued to users that have not yet been authenticated. Because these tokens are not associated with a user, they have limited capabilities.


**What are my customization options?**

You can customize your tokens in several ways. The first being the length of time that the token is valid for. Another way that you can customize your tokens is by adding custom attributes or claims to the tokens themselves.

<table>
  <tr>
    <th>Token type</th>
    <th>Value type</th>
    <th>Default</th>
    <th>Options</th>
  </tr>
  <tr>
    <td>Access</td>
    <td>Minutes</td>
    <td>60</td>
    <td>Any value between 5 and 1440</td>
  </tr>
  <tr>
    <td>Identity</td>
    <td>Minutes</td>
    <td>60</td>
    <td>Any value between 5 and 1440</td>
  </tr>
  <tr>
    <td>Refresh</td>
    <td>Days</td>
    <td>30</td>
    <td>Any value between 1 and 9</td>
  </tr>
  <tr>
    <td>Anonymous</td>
    <td>Days</td>
    <td>30</td>
    <td>Any value between 1 and 9</td>
  </tr>
</table>

**Are there any security considerations?**

Yes. Because tokens are used to identify users and secure your resources, the lifespan of a token affects several different things. By customizing your token configuration you can ensure that your security and user experience needs are met. However, should a token ever become compromised, a malicious user has more time to affect your application.


Want to learn more about tokens? Check out [Understanding tokens](authorization.html#tokens).
{: tip}


## Mapping custom attributes
{: #claim-mapping}

You can map user attributes to your access and identity tokens. This means that you don't have to go to the /userinfo endpoint or pull custom attributes later, because they're already stored in the tokens!
{: shortdesc}


**Why would I add attributes to my tokens?**

Without having to make extra network calls, everything that your app may need to know about a user or what they can do is already in the token! Provided you don't have massive amounts of data, this makes you more efficient. You also have more control as you can specify how the token is signed.


**What kinds of attributes can I add?**

You can map _any_ type of attribute. You can map scopes or roles for specific users, or even create your own scopes!


### Understanding claims

The claims that are provided by App ID fall into several categories that are differentiated by their level of customization.

**Registered claims**

There are some fields in the access and identity tokens that are defined by App ID and cannot be overridden by custom mappings. If your claim is restricted, it is ignored by the service. Claims: `iss`, `aud`, `sub`, `iat`, `exp`, `amr`, and `tenant`.


**Restricted claims**

Depending on the token that the claims are mapped to, some claims have limited customization possibilities.

For an access token the `scope` field is the only restricted claim. It cannot be overridden by custom mappings, but it can be extended with your own scopes. When the scope field is mapped to an access token, the value must be a string and cannot be prefixed by `appid_` or it will be ignored.

In identity tokens, the claims `identities` and `oauth_clients` cannot be modified or overridden.


**Normalized claims**

Every identity token contains a set of claims that is recognized by App ID as normalized claims. When they are available, they are directly mapped from your identity provider to the token. These claims cannot be explicitly omitted but can be overridden by custom claim mappings. The fields include `name`, `email`, `picture`, `local`, and `gender`.


## Claim Mapping Object
{: #claim-mapping-object}

Define the value of claim mapping
what you can and can't do
How do you use it?


Each claim mapping is defined by its data source object and the key identifying the claim to retrieve from it.

Custom claims are set for each token type separately and are sequentially applied. You can register up to 100 claims for each token to a maximum payload of 100KB.
{:tip}

<table>
    <thead>
      <th colspan=3><img src="images/idea.png" alt="Idea icon"/> Understanding the claim mapping object</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>source</em></code></td>
        <td>Required</td>
        <td>This property defines where the claim is sourced from. This can refer to the identity provider's user info or the user's {{site.data.keyword.appid_short_notm}} custom attributes. </br> </br> Options include: `saml`, `cloud_directory`, `facebook`, `google`, `appid_custom`, `ibmid`, `attributes`.</td>
      </tr>
      <tr>
        <td><code><em>sourceClaim</em></code></td>
        <td>Required</td>
        <td>Defines the source data's claim to map to your token. </td>
      </tr>
    </tbody>
  </table>

You can reference nested fields in your claim mappings using the dot syntax. `e.g. nested.attribute`
{:tip}

This can be merged with the above -> how do you take information from one of your identity providers and use



## Configuring {{site.data.keyword.appid_short_notm}} tokens
{: #configuring-tokens}

### Configuring Token Lifetime from the UI
{: #configuring-tokens-by-ui}

1. Navigate to your service dashboard.

2. Click the Identity Providers dropdown to view all of your options and select the **Manage** page.

3. Select the `Settings` tab to modify your tokens.

4. Use the GUI to set your Access, Refresh, and Anonymous token expiration.

### Configuring Tokens using the Management APIs
{: #configuring-tokens-by-api}

**Before you begin**

You will need:

1. Your {{site.data.keyword.appid_short_notm}} instance's tenant ID

2. Your IAM Token

**Setting up your claim mappings**

1. Make a PUT request to the `/config/tokens` endpoint with your token configuration.

  Header:
  ```
  PUT {management-url}/management/v4/{tenantId}/tokens
       Host: <management-server-url>
       Authorization: 'Bearer <IAM_TOKEN>'
       Content-Type: application/json
  ```
  {: pre}

  Body:
  ```
   {
       "access": {
           "expires_in": 3600,
       },
       "refresh": {
           "expires_in": 2592000,
           "enabled": true
       },
       "anonymous": {
           "expires_in": 2592000,
           "enabled": true
       },
       "accessTokenClaims": [
           {
              "source": "saml",
              "sourceClaim": "name_id"
           }
       ],
       "idTokenClaims": [
           {
              "source": "saml",
              "sourceClaim": "attributes.uid"
           }
       ]
   }
  ```
  {: pre}

  <table>
    <thead>
      <th colspan=3><img src="images/idea.png" alt="Idea icon"/> Understanding the token configuration</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>access</em></code></td>
        <td>Optional</td>
        <td>Object containing expiration time, `expires_in`, in minutes of your access and identity tokens. <br/> <br/> Defaults to 60 minutes. </td>
      </tr>
      <tr>
          <td><code><em>refresh</em></code></td>
          <td>Optional</td>
          <td>Object containing expiration time in days,of your refresh token. <br/> <br/> Expiration defaults to 30 days. </td>
      </tr>
      <tr>
          <td><code><em>anonymousAccess</em></code></td>
          <td>Optional</td>
          <td>Object containing expiration time,  `expires_in`, in days of your anonymous access/identity tokens. <br/> <br/> Expiration defaults to 30 days.
      </tr>
      <tr>
          <td><code><em>accessTokenClaims</em></code></td>
          <td>Optional</td>
          <td>An array of claim mapping objects. See section on the [claim mapping object](#claim-mapping-object) for details.</td>
      </tr>
      <tr>
          <td><code><em>idTokenClaims</em></code></td>
          <td>Optional</td>
          <td>An array of claim mapping objects. See section on the [claim mapping object](#claim-mapping-object) for details.</td>
      </tr>
    </tbody>
  </table>

  Example request:
  ```
  $ curl -X PUT
    --header 'Content-Type: application/json'
    --header 'Accept: application/json'
    --header 'Authorization: <IAM Token'
    -d '{
          "accessTokenClaims": [
            {
              "source": "saml",
              "sourceClaim": "name_id"
            }
          ],
          "idTokenClaims": [
            {
              "source": "attributes",
              "sourceClaim": "theme"
            }
          ]
      }'
}' '{management-url}/management/v4/{Tenant ID}/config/tokens'
  ```
  {: screen}
