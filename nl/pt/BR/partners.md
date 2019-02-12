---

copyright:
  years: 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# Soluções de parceiros
{: #setup-partner}

As soluções de parceiros são uma maneira de ampliar sua segurança criando um link entre o {{site.data.keyword.security-advisor_long}} e outras ferramentas de segurança.
{: shortdesc}

## Assistente de integração
{: #wizard}

Como um administrador com direitos administrativos nas contas do IBM Cloud e do Parceiro, é possível usar o assistente de integração para funcionar rapidamente. O assistente tem quatro etapas básicas:

* Estabelecer confiança e associar suas contas do IBM Cloud e do Parceiro
* Copiar as informações necessárias, tais como credenciais e URLs, entre as contas
* Instalar os metadados de descoberta do parceiro no {{site.data.keyword.security-advisor_short}}
* Validar o emparelhamento postando uma descoberta do Parceiro no {{site.data.keyword.security-advisor_short}}

</br>

## Integrando o NeuVector
{: #neuvector}

**Antes de começar**

* Assegure-se de que tenha uma conta com o parceiro que deseja integrar.
* Assegure-se de que tenha as permissões administrativas necessárias para gerar a URL de integração do serviço de Parceiro.
* Assegure-se de que tenha acesso administrativo do IAM ao IBM Cloud e acesso do gerenciador ao {{site.data.keyword.security-advisor_short}}.

**Integrando**

1. Conecte suas contas.
  1. Forneça um nome para sua conexão.
  2. Forneça a URL do painel do NeuVector.
  3. Forneça a URL de configuração do NeuVector.
    1. Efetue login no NeuVector e navegue para as configurações.
    2. Clique em **Gerar URL**.
    3. Copie a URL e cole-a no assistente de integração do {{site.data.keyword.security-advisor_short}}.
  4. Clique em **Avançar**.
3. Certifique-se de dar permissão para que o serviço gere um ID de serviço e uma chave de API e crie a conexão com o serviço clicando em **Avançar**.
4. Clique em **Pronto**.
5. Acesse seu painel de serviço para revisar uma descoberta de teste que foi enviada para o {{site.data.keyword.containershort_notm}} pelo NeuVector.
