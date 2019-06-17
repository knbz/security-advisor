---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

keywords: Centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

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


# Descobertas Customizadas
{: #setup_custom}

Com o {{site.data.keyword.security-advisor_short}}, é possível integrar as suas ferramentas de segurança customizadas existentes, quer sejam serviços de software livre, desenvolvidos customizados ou de terceiro. Integrando descobertas de terceiro, você pode trazer todas as suas ferramentas de segurança e descobertas em um painel no qual é possível monitorar eventos de segurança críticos.
{: shortdesc}


## Antes de começar
{: #custom-before-api}

Antes de integrar descobertas por meio de sua ferramenta de terceiro, certifique-se de ter os pré-requisitos a seguir.

1. Certifique-se de que o ID do usuário ou do serviço que esteja usando esteja designado à [função do IAM](https://cloud.ibm.com/iam#/users) de **Gerente**.

2. Efetue login no {{site.data.keyword.cloud_notm}}.

  ```
  ibmcloud login
  ```
  {: codeblock}

3. Obtenha o ID da conta. Para obter mais informações sobre funções de serviço, confira as [políticas de acesso do {{site.data.keyword.security-advisor_short}}](/docs/iam?topic=iam-iammanidaccser#iammanidaccser).

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

4. Obtenha o token do Identity and Access Management (IAM). O token é usado no `-- header` de cada solicitação de API.

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  Os tokens IAM expiram a cada 60 minutos. Para aprender como [obter um novo token automaticamente](/docs/iam?topic=iam-iamtoken_from_apikey) usando uma chave de API.
  {: tip}



## Importando Descobertas e KRIs
{: #custom-adding}

As APIs seguem Grafeas como especificações de metadados de artefato para armazenar, consultar e recuperar os metadados críticos para as descobertas que são relatadas por suas ferramentas de segurança e serviços. A integração pode ser feita usando as APIs para configurar os principais indicadores de risco (KRIs).
{: shortdesc}


### Etapa 1: Registrando um novo tipo de descoberta
{: #custom-register-finding}

Para registrar um novo tipo de descobertas, é possível criar uma nota. Para criar a nota, é possível usar a [API de descobertas](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}. Certifique-se de que você escolha um ID de provedor exclusivo para identificar sua ferramenta customizada. Se você estiver automatizando o processo usando o seu ID de serviço como o seu ID do provedor.

Solicitação de exemplo:

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{  \"kind\": \"FINDING\",  \"short_description\": \"My security tool finding\",  \"long_description\": \"Longer description of what the security tool found.\",  \"provder_id\": \"my-custom-tool\",  \"id\": \"my-custom-tool-findings-type\",  \"reported_by\": {    \"id\": \"my-custom-tool\",    \"title\": \"My custom security tool\"  } ,  \"finding\": {    \"severity\": \"MEDIUM\",    \"next_steps\": [      {        \"title\": \"Explain why it's reported as a risk.\"      }    ]  }}"
```
{: codeblock}

| Gerais | Descrição | 
|:-----------------|:-----------------|
| `note_name` | `<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type`  |
| `kind` | `FINDING` |
| `remediation` | As etapas que precisam ser executadas para resolver o problema. |
| `provider_id` | Sua ferramenta de segurança customizada. |
| `id` | Um ID para o tipo de descoberta que sua ferramenta de segurança localizou. |
{: class="simple-tab-table"}
{: caption="Tabela 1. Entendendo os componentes gerais do comando" caption-side="top"}
{: #registerfindingtable1}
{: tab-title="General"}
{: tab-group="register"}

| Contexto | Descrição | 
|:-----------------|:-----------------|
| `region` | O local no qual a descoberta ocorreu. |
| `resource_id` | O ID para o recurso. |
| `resource_name` | O nome do recurso. |
| `resource_type` | O tipo do recurso. |
| `service_name` | O nome do serviço. |
{: class="simple-tab-table"}
{: caption="Tabela 2. Entendendo os componentes de contexto do comando" caption-side="top"}
{: #registerfinding2}
{: tab-title="Context"}
{: tab-group="register"}

| Descoberta | Descrição | 
|:-----------------|:-----------------|
| `severity` | O nível de urgência que a descoberta apresenta.  |
| `next_steps` | As etapas que podem ser realizadas para corrigir o problema. |
| `url` | Uma URL na qual os detalhes da descoberta podem ser localizados. |
{: class="simple-tab-table"}
{: caption="Tabela 3. Entendendo os componentes de descoberta do comando" caption-side="top"}
{: #registerfinding3}
{: tab-title="Finding"}
{: tab-group="register"}

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
{: screen}

Certifique-se de lembrar o nome da nota que é retornada como parte da resposta. Nesse exemplo, o valor é `/providers/my-custom-tool/notes/my-custom-tool-findings-type`. Esse valor é usado na próxima etapa.
{: tip}


### Etapa 2: Postando descobertas
{: #custom-post-findings}

Crie uma [ocorrência](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence){: external} para postar descobertas como KRIs ou eventos em seu painel do consultor de segurança.

Para cada cartão, é possível definir dois KRIs.
{: note}

Solicitação de exemplo: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/occurrences"  -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"note_name\": \"<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\",    \"kind\": \"FINDING\",    \"remediation\": \"How to resolve Data leakage threat\",    \"provider_id\": \"my-custom-tool\",    \"id\": \"my-custom-tool-finding-2\",    \"context\": {        \"region\": \"location\",        \"resource_id\": \"cluster crn\",        \"resource_name\": \"cloudkingdom\",        \"resource_type\": \"container\",        \"service_name\": \"kubernetes service\"    },    \"finding\": {        \"severity\": \"HIGH\",        \"next_steps\": [{            \"title\": \"Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities\",                        \"url\":\"https://cloud.ibm.com/containers-kubernetes/clusters\"        }],                \"short_description\": \"One of the pods in your cluster appears to be leaking an excessive amount of data\",                \"long_description\": \"One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod's normal behavior\"    }}"
```
{: codeblock}

| Gerais | Descrição | 
|:-----------------|:-----------------|
| `kind` | `FINDING` |
| `short_description` | Uma descrição simples que resume a descoberta; não mais do que um par de palavras. |
| `long_description` | Uma descrição mais longa que contém mais detalhes sobre a descoberta. |
| `provider_id` | Sua ferramenta de segurança customizada. |
| `id` | Um ID para o tipo de descoberta que sua ferramenta de segurança localizou. |
{: class="simple-tab-table"}
{: caption="Tabela 2. Entendendo os componentes gerais do comando" caption-side="top"}
{: #postfinding1}
{: tab-title="General"}
{: tab-group="post"}

| Relatado por | Descrição | 
|:-----------------|:-----------------|
| `id` | O ID da ferramenta de segurança que relatou a descoberta.  |
| `title` | O título da ferramenta de segurança que relatou a descoberta. |
{: class="simple-tab-table"}
{: caption="Tabela 2. Entendendo o comando relatado por componentes" caption-side="top"}
{: #postfinding2}
{: tab-title="Reported by"}
{: tab-group="post"}

| Descoberta | Descrição | 
|:-----------------|:-----------------|
| `severity` | O nível de urgência que a descoberta apresenta.  |
| `next_steps` | As etapas que podem ser realizadas para corrigir o problema. |
| `title` | O título da descoberta. |
{: class="simple-tab-table"}
{: caption="Tabela 3. Entendendo os componentes de descoberta do comando" caption-side="top"}
{: #postfinding3}
{: tab-title="Finding"}
{: tab-group="post"}


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
{: screen}

### Etapa 3: Definindo o cartão para exibir
{: #custom-define-card}

Defina como você deseja que o seu cartão exiba as suas descobertas em seu painel criando uma [nota](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}.

Solicitação de exemplo: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"kind\": \"CARD\",        \"provider_id\": \"my-custom-tool\",    \"id\": \"custom-tool-card\",    \"short_description\": \"Security risk found by my custom tool\",        \"long_description\": \"More detailed description about why this security risk needs to be fixed\",    \"reported_by\": {      \"id\": \"my-custom-tool\",      \"title\": \"My security tool\"    },        \"card\": {      \"section\": \"My security tools\",      \"order\": 1 ,     \"title\": \"My security tool findings\",      \"subtitle\": \"My security tool\",      \"finding_note_names\": [        \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"      ],      \"elements\": [        {          \"kind\": \"NUMERIC\",          \"text\": \"Count of findings reported by my security tool\",          \"default_time_range\": \"1d\",          \"value_type\": {            \"kind\": \"FINDING_COUNT\",            \"finding_note_names\": [              \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"            ]          }        }      ]    }  }"
```
{: codeblock}

| Gerais | Descrição | 
|:-----------------|:-----------------|
| `provider_id` | O ID de sua ferramenta de segurança. |
| `id` | Um ID para o tipo de descoberta que sua ferramenta de segurança localizou. |
| `short_description` | Uma descrição simples que resume a descoberta; não mais do que um par de palavras. |
| `long_description` | Uma descrição mais longa que contém mais detalhes sobre a descoberta. |
{: class="simple-tab-table"}
{: caption="Tabela 1. Entendendo os componentes gerais do comando" caption-side="top"}
{: #definecard1}
{: tab-title="General"}
{: tab-group="card"}

| Relatado por | Descrição | 
|:-----------------|:-----------------|
| `id` | O ID da ferramenta de segurança que relatou a descoberta.  |
| `title` | O título da ferramenta de segurança que relatou a descoberta. |
{: class="simple-tab-table"}
{: caption="Tabela 2. Entendendo o comando relatado por componentes" caption-side="top"}
{: #definecard2}
{: tab-title="Reported by"}
{: tab-group="card"}

| Card | Descrição | 
|:-----------------|:-----------------|
| `section` | A seção na qual você deseja que o cartão seja exibido. É possível ter até três seções customizadas com seis placas em cada seção. Caracteres máximos: 30  |
| Opcional: `order` | A ordem na qual o cartão é exibido dentro da seção especificada. O pedido é especificado no intervalo de 1 a 6. Se você escolher um número que já está aplicado a outro cartão, a criação falhará. Você recebe uma mensagem de erro que indica que "O pedido fornecido já está tomado por outro cartão na seção." Se o pedido fornecido for maior que o número atual de cartões mais 1, a criação do cartão falhará. Por exemplo, se você tiver atualmente dois cartões e estiver criando outro, não será possível especificar 5 no pedido do cartão porque todos juntos, você tem um total de três cartões. Se o pedido para os cartões não for especificado, eles serão organizados alfabeticamente na seção designada. |
| `title` | O título que você deseja que o seu cartão tenha. Caracteres máximos: 28 |
| `subtitle` | O subtítulo que você deseja que o seu cartão tenha. Caracteres máximos: 30  |
| `finding_note_names` | `providers//notes/my-custom-tool-findings-type` |
{: class="simple-tab-table"}
{: caption="Tabela 3. Entendendo os componentes de cartão do comando" caption-side="top"}
{: #definecard3}
{: tab-title="Card"}
{: tab-group="card"}

| Elementos: tipo de valor | Descrição | 
|:-----------------|:-----------------|
| `kind` | As opções incluem: `NUMERIC`, `TIME_SERIES` e `BREAKDOWN`. |
| `text` | O texto que você deseja exibir. Se o tipo for `NUMERIC`, o número máximo de caracteres será 60. Se o tipo for `TIME_SERIES` ou `BREAKDOWN`, o número máximo de caracteres será 65. |
| `default_time_range` | A quantidade de tempo que você deseja verificar. Os valores são configurados em dias. As opções atuais incluem: `1d`, `2d`, `3d` e `4d`. |
| `value_type` | O tipo de elemento. Se o tipo for `NUMERIC`, o campo será `value_type` e você poderá ter até quatro elementos por cartão. Se o tipo for `TIME_SERIES` ou `BREAKDOWN`, o campo será `value_types`. O número máximo de ambos `TIME_SERIES` ou `BREAKDOWN` é 1. Se você tiver entradas numéricas apenas, poderá ter até quatro elementos por cartão. Se você desejar usar uma combinação, poderá ter até duas entradas numéricas e uma de série temporal ou detalhamento. Não é possível ter ambos a série temporal e o detalhamento no mesmo cartão. Se você definir os seus tipos de valor como uma matriz para série temporal, poderá ter até três entradas. |
| `value_type`: `kind` | O tipo de valor. As opções incluem: `KRI` e `FINDING_COUNT`. |
| `value_type`: `finding_note_names` | Se `kind` for `FINDING_COUNT`, o nome das descobertas que você deseja ver em seu cartão, que é especificado como uma matriz. |
| `value_type`: `kri_note_names` | Se `kind` for `FINDING_COUNT`, o nome das descobertas que você deseja ver em seu cartão, que é especificado como uma matriz. |
| `value_type`: `text` | O texto do tipo de elemento. O número máximo de caracteres é 22. |
{: class="simple-tab-table"}
{: caption="Tabela 4. Entendendo os componentes de elemento do comando" caption-side="top"}
{: #definecard4}
{: tab-title="Elements"}
{: tab-group="card"}

Resposta de exemplo:

```
{
"author": {
  "account_id": "account id",
      "email": "email id ",
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
  "order": "1",
  "title": "My Security Tool Findings",
  "subtitle": "My Security Tool",
},
"context": {
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
{: screen}


## Uso de Exemplo
{: #custom-example}

Diga que você tem um aplicativo que é executado em um cluster do {{site.data.keyword.containershort_notm}} com o nome `cloudkingdom`. Dependendo do tamanho de seu aplicativo, será possível ter vários pods dentro de seu cluster que você precisa para monitorar todos ao mesmo tempo. E se você tiver várias ferramentas customizadas que monitoram e detectam seu cluster para diferentes ameaças? Se um de seus pods no cluster começar a enviar uma quantidade anormal de dados para servidores externos, você gostaria de saber o mais rápido possível. A ferramenta customizada que monitora a transferência de dados pode detectar a descoberta e enviá-la para o {{site.data.keyword.security-advisor_short}}. Se você tivesse outra integração customizada que detectasse um problema, ela também enviaria a descoberta para o {{site.data.keyword.security-advisor_short}}. Em seguida, o {{site.data.keyword.security-advisor_short}} exibe as descobertas de todas as suas ferramentas de monitoramento em um único painel. Lá, é possível ter rapidamente uma visão geral de quaisquer alertas, investigar um problema e aprender sobre como tomar as etapas de correção.

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
		"service_name": "kubernetes service"
	},
	"finding": {
		"severity": "HIGH",
		"next_steps": [{
			"title": "Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities",
                        "url":"https://cloud.ibm.com/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
