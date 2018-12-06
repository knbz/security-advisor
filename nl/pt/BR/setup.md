---

copyright:
  years: 2014, 2018
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

# Configurando o {{site.data.keyword.security-advisor_short}}
{: #setup}

Com o {{site.data.keyword.security-advisor_long}}, é possível monitorar continuamente os aplicativos quanto aos riscos e ameaças de segurança. Quando as vulnerabilidades são localizadas, você é alertado por meio do painel de serviço.
{:shortdesc}

## Monitorando as vulnerabilidades em imagens de contêiner
{: #setup_images}

Uma imagem do Docker é a base de cada contêiner que você cria. A imagem pode conter o aplicativo, sua configuração e quaisquer dependências que sejam necessárias. Geralmente, uma imagem é armazenada em um registro. Ao usar o {{site.data.keyword.registryshort_notm}}, você tem acesso ao Vulnerability Advisor, que varre continuamente as imagens em busca de potenciais problemas de segurança. Se problemas forem localizados, você será alertado e poderá visualizar um relatório abrangente no painel do {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

Saiba mais sobre o [{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html#index).


**Antes de começar**

Antes de poder iniciar o registro, certifique-se de que tenha as seguintes CLIs e plug-ins instalados:
- [A CLI do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://clis.ng.bluemix.net/ui/home.html)
- O plug-in container-registry.

    ```
    ibmcloud plugin install container-registry -r Bluemix
    ```
    {: pre}

</br>
**Criando um namespace**

1. Efetue login na conta usando a CLI.

   ```
   bx login --sso
   ```
   {: pre}

2. Efetue login no {{site.data.keyword.registryshort_notm}}.

   ```
   bx cr login
   ```
   {: pre}

3. Opcional: crie um namespace. Sempre é possível usar um existente.

   ```
   bx cr namespace-add
   ```
   {: pre}

3. Identifique uma imagem.

   ```
   docker tag <image>:<tag> registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}

5. Envie por push a imagem.

   ```
   docker push registry.ng.bluemix.net/<namespace>/<image>:<tag>
   ```
   {: pre}


Depois do envio por push das imagens para o namespace do {{site.data.keyword.registryshort_notm}}, as informações sobre quaisquer vulnerabilidades localizadas são mostradas no cartão **Imagens com vulnerabilidades** no painel de serviço. Também é possível realizar drill down em imagens específicas para descobrir mais informações, como a descrição de todas as vulnerabilidades e os problemas de configuração conhecidos que foram identificados.

</br>

## Monitorando os certificados
{: #setup_certificates}

Você sabia que o {{site.data.keyword.cloudcerts_long_notm}} pode ajudar a monitorar e gerenciar os certificados SSL/TLS? Ao integrar o {{site.data.keyword.cloudcerts_short}} e o {{site.data.keyword.security-advisor_short}}, é possível obter alertas e lembretes sobre quando pode ser necessário atualizar os certificados e outras informações, o que pode ajudar a evitar vulnerabilidades futuras.
{:shortdesc}

É possível saber mais sobre o [{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager/index.html#gettingstarted).

1. No catálogo do {{site.data.keyword.Bluemix_notm}}, procure "{{site.data.keyword.cloudcerts_short}}".
2. Dê à sua instância de serviço um nome ou use o nome predefinido.
3. Clique em **Criar**.
4. Para importar certificados de sua organização em {{site.data.keyword.cloudcerts_short}}, clique em
**Importar certificado**.

Agora que os certificados foram importados, informações como os prazos de expiração e os certificados expirados, são mostradas no cartão **Certificados** no painel do {{site.data.keyword.security-advisor_short}}. Clicando no cartão, é possível obter informações mais específicas sobre os certificados, como a instância de serviço à qual os certificados pertencem. Também é possível ver quaisquer etapas que podem ser realizadas para corrigir as vulnerabilidades de segurança.
