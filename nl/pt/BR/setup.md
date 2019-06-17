---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

---

{:new_window: target="_blank"}
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

# Aproveitando os serviços pré-integrados
{: #setup-services}

O {{site.data.keyword.security-advisor_short}} é fornecido com vários cartões previamente preenchidos que podem ajudá-lo a monitorar riscos e ameaças de segurança.
{: shortdesc}

Os serviços a seguir do {{site.data.keyword.security-advisor_short}} criam automaticamente um cartão para:

* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloudcerts_long_notm}}

Embora não seja necessário fazer nada para criar a conexão entre o {{site.data.keyword.security-advisor_short}} e os serviços, deve-se ter instâncias dos serviços configurados com informações.


## Monitorando as vulnerabilidades em imagens de contêiner
{: #setup-images}

Com o {{site.data.keyword.registryshort_notm}}, você tem acesso ao Vulnerability Advisor, que varre continuamente as imagens na instância do {{site.data.keyword.registryshort_notm}} para possíveis problemas de segurança. Se problemas forem localizados, você será alertado e poderá visualizar um relatório abrangente no painel do {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

Saiba mais sobre o [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started).


### Antes de começar
{: #setup-before}

Antes de iniciar com o registro, certifique-se de que tenha as CLIs e plug-ins a seguir instalados:
* [ A  {{site.data.keyword.cloud_notm}}  CLI) ](/docs/cli?topic=cloud-cli-ibmcloud-cli)
* O plug-in Registro do Contêiner.

  ```
  ibmcloud plugin install container-registry
  ```
  {: codeblock}


### Criando um Namespace
{: #setup-create-namespace}

1. Efetue login na conta usando a CLI.

  ```
  ibmcloud login --sso
  ```
  {: codeblock}

2. Efetue login no {{site.data.keyword.registryshort_notm}}.

  ```
  ibmcloud cr login
  ```
  {: codeblock}

3. Opcional: crie um namespace. Sempre é possível usar um existente.

  ```
  ibmcloud cr namespace-add
  ```
  {: codeblock}

3. Identifique uma imagem.

  ```
  docker tag <image>:<tag> <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}

5. Envie por push a imagem.

  ```
  docker push <region>.icr.io/<namespace>/<image>:<tag>
  ```
  {: codeblock}


Depois do envio por push das imagens para o namespace do {{site.data.keyword.registryshort_notm}}, as informações sobre quaisquer vulnerabilidades localizadas são mostradas no cartão **Imagens com vulnerabilidades** no painel de serviço. Também é possível realizar drill down em imagens específicas para descobrir mais informações, como uma descrição de todas as vulnerabilidades identificadas e os problemas de configuração.


## Monitorando os certificados
{: #setup-certificates}

Você sabia que o {{site.data.keyword.cloudcerts_long_notm}} pode ajudar a monitorar e gerenciar os certificados SSL/TLS? Ao integrar o {{site.data.keyword.cloudcerts_short}} e o {{site.data.keyword.security-advisor_short}}, é possível obter alertas antecipadamente sobre a expiração de certificados que pode ajudar a evitar uma indisponibilidade de serviço ou do aplicativo.
{:shortdesc}

Dependendo dos dados de expiração do certificado do qual você faz upload para o {{site.data.keyword.cloudcerts_short}}, as descobertas aparecem no painel do {{site.data.keyword.security-advisor_short}} 90, 60, 10 e um dia antes da expiração do certificado. Além disso, você recebe notificações diárias sobre os certificados expirados. As notificações diárias iniciam o dia após a expiração do certificado.

Para acionar uma atualização manual, é possível tentar fazer upload de um certificado que expira em um dia para sua instância do {{site.data.keyword.cloudcerts_short}}. Quando a importação é bem-sucedida, é possível ver que o Principal indicador de risco (KRI) e as descobertas estão visíveis no painel do {{site.data.keyword.security-advisor_short}}.

É possível saber mais sobre os [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager?topic=certificate-manager-getting-started) lendo os docs.
{: tip}

### Criando um certificado
{: #setup-create-cert}

Para criar um certificado autoassinado que expira em um dia, é possível executar o comando OpenSSL a seguir em seu terminal.

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -subj "/CN=myservice.com" -out server.pem -days 1 -nodes
```
{: codeblock}


### Fazendo upload de um certificado
{: #setup-upload-cert}

1. No catálogo do {{site.data.keyword.cloud_notm}}, procure "{{site.data.keyword.cloudcerts_short}}".
2. Dê à sua instância de serviço um nome ou use o nome predefinido.
3. Clique em **Criar**.
4. Para importar certificados de sua organização em {{site.data.keyword.cloudcerts_short}}, clique em
**Importar certificado**.

Agora que os certificados foram importados, informações como os prazos de expiração e os certificados expirados, são mostradas no cartão **Certificados** no painel do {{site.data.keyword.security-advisor_short}}. Clicando no cartão, é possível obter informações mais específicas sobre os certificados, como a instância de serviço à qual os certificados pertencem. Também é possível ver quaisquer etapas que podem ser realizadas para corrigir as vulnerabilidades de segurança.

Para parar as notificações, deve-se renovar seu certificado, fazer upload do certificado para o {{site.data.keyword.cloudcerts_short}} e excluir o certificado expirado.
{: tip}
