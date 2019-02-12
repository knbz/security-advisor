---

copyright:
  years: 2018
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Integrando as suas próprias ferramentas de segurança
{: #custom}

O {{site.data.keyword.security-advisor_long}} fornece uma API de descobertas que pode ser usada para unificar e melhorar o gerenciamento de segurança no IBM Cloud. É possível usar as APIs de descobertas para registrar novos tipos de descoberta para os serviços do parceiro e as ferramentas de segurança customizadas. Após o registro dos metadados, as ferramentas podem enviar quaisquer ocorrências de descobertas, como KPIs e eventos, usando as APIs. As descobertas são exibidas como um cartão separado no painel do {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

As APIs do {{site.data.keyword.security-advisor_short}} seguem a especificação de API de metadados de artefato semelhante à [Grafeas](https://grafeas.io/) para armazenar, consultar e recuperar metadados críticos para as descobertas que são relatadas por todas as ferramentas e serviços de segurança.

## Integrando as suas próprias ferramentas
{: #integrate}

**Antes de começar**

1. Efetue login no {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

2. Obtenha o ID da conta. Assegure-se de que a [função do IAM](https://console.bluemix.net/iam/#/users) **Gerenciador** esteja designada ao seu ID. Para obter mais informações sobre as funções de serviço, consulte as políticas de acesso do [{{site.data.keyword.security-advisor_short}}](/docs/services/security-advisor/iam.html).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. Obtenha o token do IAM. O token é usado no `-- header` de cada solicitação de API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

</br>

**Incluindo e monitorando as descobertas**

1. Registre um novo tipo de descoberta criando uma nota. Para criar a nota, use a [API de descobertas](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote).

  Solicitação de exemplo:

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Content-Type: application/json" -d "{ \"kind\": \"FINDING\", \"short_description\": \"My security tool finding\", \"long_description\": \"See what my custom security tool found\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-findings-type\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Custom Security Tool\" }, \"finding\": { \"severity\": \"MEDIUM\", \"next_steps\": [ { \"title\": \"Learn why this is reported as a risk\" } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Ícone Mais informações"/> Entendendo esses componentes de comandos </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Uma descrição simples que resume a descoberta com poucas palavras.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Uma descrição mais longa que contém mais detalhes sobre a descoberta.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Sua ferramenta de segurança customizada.</td>
      </tr>
      <tr>
        <td><code>id </code></td>
        <td>Um ID para o tipo de descoberta que sua ferramenta de segurança localizou.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id </code></li><li><code>title </code></li></ul></td>
        <td></br><ul><li>O ID da ferramenta de segurança que relatou a descoberta.</li><li>O título da ferramenta de segurança que relatou a descoberta.</li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severidade</code></li> <li><code>next_steps</code></li> <li><code>title </code></li></ul></td>
        <td></br><ul><li>O nível de urgência que a descoberta apresenta.</li> <li>As etapas que podem ser realizadas para corrigir o problema.</li> <li>O título da descoberta.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Resposta de exemplo:

  ```
    {
    "author": {
      "account_id": "account id",
      "email": "email id",
      "id": "IBM ID",
      "kind": "user"
    }, "context": {
      "account_id": "account id"
    },
    "create_time": "2018-09-04T10:57:56.913871Z",
    "create_timestamp": 1536058676914,
    "finding": {
      "next_steps": [ {
          "title": "Learn why this is reported as a risk"
        }
      ],
      "severity": "MEDIUM"
    },
    "id": "my-custom-tool-findings-type",
    "kind": "FINDING",
    "long_description": "See what my custom security tool found",
    "name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "reported_by": {
      "id": "my-custom-tool", "title": "Minha ferramenta de segurança customizada"
    },
    "shared": true,
    "short_description": "My security tool finding",
    "update_time": "2018-09-04T10:57:56.913890Z",
    "update_timestamp": 1536058676914,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: codeblock}

2. Crie uma descoberta postando uma [ocorrência](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence).

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account-id>/providers/my-custom-tool/occurrences" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Replace-If-Exists: true" -H "Content-Type: application/json" -d "{ \"note_name\": \"<account-id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\", \"kind\": \"FINDING\", \"remediation\": \"how to resolve this\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-finding-1\", \"context\": { \"region\": \"location\", \"resource_id\": \"pluginId\", \"resource_name\": \"www.myapp.com\", \"resource_type\": \"worker\", \"service_name\": \"application\" }, \"finding\": { \"severity\": \"HIGH\", \"next_steps\": [{ \"url\": \"Details URL\" }] }}"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Ícone Mais informações"/> Entendendo esses componentes de comandos </th>
    </thead>
    <tbody>
      <tr>
        <td><code>note_name</code></td>
        <td><code>&lt;account id&gt;/providers/my-custom-tool/notes/my-custom-tool-findings-type</code></td>
      </tr>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>remediation</code></td>
        <td>As etapas que precisam ser executadas para resolver o problema.</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Sua ferramenta de segurança customizada.</td>
      </tr>
      <tr>
        <td><code>id </code></td>
        <td>Um ID para o tipo de descoberta que sua ferramenta de segurança localizou.</td>
      </tr>
      <tr>
        <td><code>context</code><ul><li><code>região</code></li><li><code>resource_id</code></li> <li><code>resource_name</code></li> <li><code>resource_type</code></li> <li><code>service_name</code></li></ul></td>
        <td></br><ul><li><code>A localização na qual a descoberta ocorreu</code></li><li><code>O ID do recurso específico.</code></li> <li><code>O nome do recurso.</code></li> <li><code>O tipo de recurso.</code></li> <li><code>O nome do serviço. </code></li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severidade</code></li> <li><code>next_steps</code></li> <li><code>url</code></li></ul></td>
        <td></br><ul><li>O nível de urgência que a descoberta apresenta.</li> <li>As etapas que podem ser realizadas para corrigir o problema.</li> <li>Uma URL na qual os detalhes da descoberta podem ser localizados.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Resposta de exemplo:

  ```
    {
    "author": {
      "account_id": "account id",
      "email": "email id ",
      "id": "user id",
      "kind": "user"
    }, "context": {
      "account_id": "account id",
      "region": "location",
      "resource_id": "pluginId",
      "resource_name": "www.myapp.com",
      "resource_type": "worker",
      "service_name": "application"
    },
    "create_time": "2018-09-04T11:32:14.564809Z",
    "create_timestamp": 1536060734565,
    "finding": {
      "next_steps": [ {
          "title": "Learn why this is reported as a risk",
          "url": "Details URL"
        }
      ],
      "severity": "HIGH"
    },
    "id": "my-custom-tool-finding-1",
    "kind": "FINDING",
    "long_description": "See what my custom security tool found",
    "name": "<account id>/providers/my-custom-tool/occurrences/my-custom-tool-finding-1",
    "note_name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "remediation": "how to resolve this",
    "reported_by": {
      "id": "my-custom-tool", "title": "Minha ferramenta de segurança customizada"
    },
    "short_description": "My security tool finding",
    "update_time": "2018-09-04T11:32:14.564828Z",
    "update_timestamp": 1536060734565,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: codeblock}

3. Defina o cartão no painel para exibir a descoberta criando uma [nota](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote).

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam token>" -H "Content-Type: application/json" -d "{ \"kind\": \"CARD\", \"provider_id\": \"my-custom-tool\", \"id\": \"custom-tool-card\", \"short_description\": \"security risk found by my custom tool\", \"long_description\": \"Details about why this is security risk to be fixed\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Security Tool\" }, \"card\": { \"section\": \"My Security Tools\", \"title\": \"My Security Tool Findings\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ], \"elements\": [ { \"kind\": \"NUMERIC\", \"text\": \"Count of findings reported by my security tool\", \"default_time_range\": \"1d\", \"value_type\": { \"kind\": \"FINDING_COUNT\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ] } } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Ícone Mais informações"/> Entendendo esses componentes de comandos </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>CARD</code></td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>Sua ferramenta de segurança customizada.</td>
      </tr>
      <tr>
        <td><code>id </code></td>
        <td>Um ID para o tipo de descoberta que sua ferramenta de segurança localizou.</td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>Uma descrição que resume a descoberta com poucas palavras.</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>Uma descrição mais longa que contém mais detalhes sobre a descoberta.</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id </code></li><li><code>title </code></li></ul></td>
        <td></br><ul><li>O ID da ferramenta de segurança que relatou a descoberta.</li><li>O título da ferramenta de segurança que relatou a descoberta.</li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title </code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>A seção na qual o cartão se encaixa.</li> <li>O título do cartão</li> <li><code>providers/<provider_id>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elementos</code> <ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>O tipo de elemento.</li> <li>O texto que você deseja exibir</li> <li>A quantidade de tempo que você deseja verificar.</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code> <ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>O tipo de valor</li> <li>O nome das descobertas que você deseja ver em seu cartão.</li></ul></td>
      </tr>
    </tbody>
  </table>

  Resposta de exemplo:
  ```
    {
    "author": {
      "account_id": "<account id",
      "email": "email id",
      "id": "user id",
      "kind": "user"
    },
    "card": {
      "elements": [
        {
          "default_time_range": "1d",
          "kind": "NUMERIC",
          "text": "Count of findings reported by my security tool",
          "value_type": {
            "finding_note_names": [
              "providers/my-custom-tool/notes/my-custom-tool-findings-type"
            ],
            "kind": "FINDING_COUNT"
          }
        }
      ],
      "finding_note_names": [
        "providers/my-custom-tool/notes/my-custom-tool-findings-type"
      ],
      "section": "My Security Tools",
      "title": "My Security Tool Findings"
    }, "context": {
      "account_id": "<account id>"
    },
    "create_time": "2018-09-04T11:49:36.056047Z",
    "create_timestamp": 1536061776056,
    "id": "custom-tool-card",
    "kind": "CARD",
    "long_description": "Details about why this is security risk to be fixed",
    "name": "<account id>/providers/my-custom-tool/notes/custom-tool-card",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "reported_by": {
      "id": "my-custom-tool",
      "title": "My Security Tool"
    },
    "shared": true,
    "short_description": "security risk found by my custom tool",
    "update_time": "2018-09-04T11:49:36.056066Z",
    "update_timestamp": 1536061776056,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: codeblock}

4. Navegue para o painel de serviço para ver o cartão que você criou.

## Cenário de exemplo
{: #example}

Por que você desejaria criar customizações? Digamos que você tenha um aplicativo que está executando um cluster do {{site.data.keyword.containershort_notm}} com o nome `cloudkingdom`. Um dos pods no cluster está enviando uma quantidade anormal de dados para os servidores externos. Você deseja capturar essa descoberta no painel do {{site.data.keyword.security-advisor_short}}.

Se a ferramenta customizada monitorar e detectar a quantidade anormal de dados que está sendo transferida, a ferramenta enviará a descoberta para o {{site.data.keyword.security-advisor_short}}.

Carga útil de exemplo:

```
{
	"note_name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
	"kind": "FINDING",
	"remediation": "How to resolve Data leakage threat",
	"provider_id": "my-custom-tool",
	"id": "my-custom-tool-finding-2",
	"context": {
		"region": "location",
		"resource_id": "cluster crn",
		"resource_name": "cloudkingdom",
		"resource_type": "container",
		"service_name": "kubernetess service"
	},
	"finding": {
		"severity": "HIGH",
		"next_steps": [{
			"title": "Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities",
                        "url":"https://console.bluemix.net/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}

}
```
{: codeblock}
