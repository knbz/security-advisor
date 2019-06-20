---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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


# Conexões diretas
{: #setup-custom-gui}

Com o {{site.data.keyword.security-advisor_short}}, é possível integrar as suas ferramentas de segurança customizadas existentes, quer sejam serviços de software livre, desenvolvidos customizados ou de terceiro. É possível criar uma conexão direta do {{site.data.keyword.security-advisor_short}} com a outra ferramenta, incluindo a URL em sua lista de integrações. Criando o link, é possível monitorar facilmente todas as ferramentas que você usa.
{: shortdesc}


## Antes de começar
{: #custom-before-gui}

Antes de poder incluir a integração, deve-se primeiro ter uma conta com o parceiro que você deseja integrar.

O {{site.data.keyword.security-advisor_short}} não persiste nenhuma credencial que esteja relacionada ao serviço parceiro. Os usuários corporativos devem se autenticar usando SAML para o {{site.data.keyword.cloud_notm}} e para o parceiro de negócios.
{: note}

## Configurando a Conexão
{: #custom-configure-connection}

1. Efetue login em sua ferramenta de segurança e obtenha sua URL exclusiva.

2. Efetue login no {{site.data.keyword.cloud_notm}} usando o console.

3. Clique em  ** Integrações customizadas > Conexão direta **. Uma tela é exibida.

  1. Dê um nome à sua solução. É possível usar somente caracteres alfanuméricos, espaços em branco e traços (-) no nome.

  2. Insira a URL para a solução no formato: `www.<website>.<domain>`.

  3. Faça upload de um ícone ou de uma imagem para representar a ferramenta.

  4. Clique em  ** Conectar **  para concluir a configuração. O {{site.data.keyword.security-advisor_short}} cria os artefatos que são necessários para a integração, como o ID do serviço, a chave de API, o ID da conta e os metadados. A função `writer` é designada.
