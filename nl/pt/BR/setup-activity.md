---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

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

# Activity Insights
{: #setup-activity}

Com o {{site.data.keyword.security-advisor_long}}, é possível monitorar continuamente seus logs do {{site.data.keyword.cloud_notm}} Activity Tracker para identificar o comportamento desautorizado ou suspeito e as mudanças em seus recursos. É possível usar pacotes de regra que sejam baseados nas melhores práticas fornecidas pelo serviço ou criar suas próprias regras customizadas.
{: shortdesc}


## Antes de começar
{: #activity-prereq}

Para iniciar o Activity Insights, certifique-se de que você tenha os pré-requisitos a seguir.

- Se você estiver trabalhando no Windows 10, ative o Windows Subsystem for Linux e instale um [shell do Ubuntu](https://win10faq.com/install-run-ubuntu-bash-windows-10/){: external}.
- Instale a CLI yq:
  * Para  [ macOS e Windows 10 ](http://mikefarah.github.io/yq/){: external}.
  * Para o CentOS, o Red Hat e o Ubuntu execute os comandos a seguir para instalar a versão 1.15.
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod + x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- Binário cURL atualizado: para o CentOS e o Red Hat, é possível atualizar executando `yum update -y nss curl libcurl`.
- A CLI do  [ {{site.data.keyword.cloud_notm}}  e plug-ins necessários ](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
- A  [ CLI do Kubernetes ](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external}  v1.10.11 ou superior
- O [Helm do Kubernetes (gerenciador de pacote)](/docs/containers?topic=containers-helm) v2.9.0 ou superior.
- Um cluster Kubernetes versão v1.10.11 ou superior


## Criando um bucket COS
{: #activity-setup-cos}

Usando a GUI do {{site.data.keyword.security-advisor_short}}, é possível criar uma nova instância e depósito do COS.

1. Navegue para a guia **Integrações** do painel de serviço.

2. Alterne **Análise desativada** na caixa do Activity Insights para **Análise ativada**.

3. Clique em  ** Ir para a configuração **.

4. Na seção de pré-requisitos, clique em **Criar instância e depósito do COS**. A sua instância e o depósito do COS são criados automaticamente para você com a convenção de nomenclatura adequada e as permissões do IAM. As informações do depósito são exibidas.

Se você tiver uma instância existente do COS e do depósito, certifique-se de que ela use a convenção de nomenclatura `sa.<account_id>.telemetric.<cos_region>`. Para permitir que o serviço leia os dados que estão armazenados em sua instância do COS, configure [autorização de serviço a serviço](/docs/iam?topic=iam-serviceauth) usando o {{site.data.keyword.cloud_notm}} IAM. Configure `source` como `{{site.data.keyword.security-advisor_short}}` e `target` como a sua instância do COS. Designe a função  ` Leitor `  do IAM.


## Instalando componentes do {{site.data.keyword.security-advisor_short}}
{: #activity-install-components}

É possível instalar um agente para coletar logs de fluxo de auditoria de sua conta do {{site.data.keyword.cloud_notm}}. Os logs são armazenados em sua instância do Cloud Object Storage, na qual é possível ativar o Activity Insights para analisar seus logs por comportamento suspeito.
{: shortdesc}

1. Clone o repositório a seguir para o seu sistema local.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Mude para a pasta  ` security-advisor-activity-insights ` .

3. Extraia o arquivo `.tar` executando o comando a seguir.

  ```
  tar -xvf security-advisor-activity-insights.tar
  ```
  {: codeblock}

4. Mude para a pasta  ` security-advisor-activity-insights ` .
5. Efetue login na CLI do  {{site.data.keyword.cloud_notm}} . Siga os prompts na CLI para concluir a criação de log.

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Região</th>
      <th>Nó de Extremidade</th>
    </tr>
    <tr>
      <td>Reino Unido</td>
      <td><code> eu-gb </code></td>
    </tr>
    <tr>
      <td>Sul dos EUA</td>
      <td><code> us-south </code></td>
    </tr>
  </table>

6. Configure o contexto para seu cluster.

  1. Obtenha o comando para configurar a variável de ambiente e fazer download dos arquivos de configuração do Kubernetes.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. Copie a saída que começa com `export` e cole-a em seu terminal para configurar a variável de ambiente `KUBECONFIG`.

7. Instale o Helm usando os [docs de integração de Serviço do Kubernetes](/docs/containers?topic=containers-helm).

8. Opcional:  [ Ativar TLS ](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. Se você estiver usando a sua estação de trabalho para manipular a instalação de componentes de analítica em múltiplos clusters e o TLS estiver ativado, certifique-se de que as configurações do TLS sejam atuais e correspondam ao cluster atual no qual você planeja instalar os componentes.

9. Execute o comando a seguir para instalar o Insights. O comando valida a convenção de nomenclatura de seu depósito, cria segredos do Kubernetes, atualiza os valores com o GUID do cluster e implementa o Activity Insights.

  ```
  ./activity-insight-install.sh <cos_region> <cos_api_key> <at_region> <account_api_key> <account_spaces>
  ```
  {: codeblock}

  Se você encontrar um erro, tente executar `helm init --upgrade`.
  {: tip}

  <table>
    <tr>
      <th>Variável</th>
      <th>Descrição</th>
    </tr>
    <tr>
      <td><code> cos_region </code></td>
      <td>A região em que a sua instância do COS está implementada. As opções incluem:  <code> us-south </code>  e  <code> eu-gb </code>.</td>
    </tr>
    <tr>
      <td><code> cos_api_key </code></td>
      <td>A [chave de API](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) que você criou para acessar a sua instância e o depósito do COS. A chave deve ter a função de plataforma `writer`.</td>
    </tr>
    <tr>
      <td><code> at_region </code></td>
      <td>A região na qual você criou sua instância e depósito COS. As opções incluem:  <code> us-south </code>  e  <code> eu-gb </code>.</td>
    </tr>
    <tr>
      <td><code> account_api_key </code></td>
      <td>A chave de API da plataforma para sua conta do {{site.data.keyword.cloud_notm}}.</td>
    </tr>
    <tr>
      <td><code> account_spaces </code></td>
      <td>Uma lista separada por vírgulas dos GUIDs de espaço para sua conta do {{site.data.keyword.cloud_notm}}.</td>
    </tr>
  </table>


## Incluindo pacotes de regra no COS
{: #activity-adding-rules}

Um pacote de regra é um arquivo JSON que contém uma lista de regras que você deseja monitorar. É possível fazer download de pacotes de regras ou [criar o seu próprio](/docs/services/security-advisor?topic=security-advisor-activity#activity-packages). O mecanismo do {{site.data.keyword.security-advisor_short}} valida que cada regra segue a sintaxe correta.
{: shortdesc}

1. Clone o repositório a seguir para obter vários pacotes de regras pré-configurados. Uma pasta é criada em seu sistema local com o nome `security-advisor-activity-insights`.

  ```
  https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Localmente, crie uma pasta com o nome `IBM.rules/activities`.

3. Copie os arquivos de JSON de `security-advisor-activity-insights/security-advisor-ata-rule-packages` para `IBM.rules/activities`.

4. Navegue até o Painel do {{site.data.keyword.cloud_notm}} e selecione a instância de serviço do COS que está associada ao Activity Insights.

5. Na guia **Depósitos** do painel de serviço, selecione o depósito que está associado ao Activity Insights.

5. Na página inicial da instância do COS, clique em **Fazer upload** e selecione **Pastas**.

6. Se solicitado, clique em **Instalar o cliente Aspera Connect**. Quando você não vê um prompt, o cliente já está instalado. Se você precisou instalar o cliente, repita a etapa 5 quando a instalação for concluída.

7. Selecione a pasta  * IBM.rules/activities * .

8. Clique em  ** Upload **.

Deseja usar seus próprios pacotes? Use um dos arquivos JSON como um guia e crie regras que se adéquem às necessidades da sua organização. Depois de criar o arquivo, inclua-o na pasta *IBM.rules/activities* em sua instância do COS. Para obter mais informações sobre os tipos de regras e formatação, confira [Entendendo pacotes de regras](/docs/services/security-advisor?topic=security-advisor-activity).
{: tip}


## Excluindo os componentes
{: #activity-delete}

Se você não precisar mais usar o Activity Insights, será possível excluir os componentes de serviço de seu cluster.
{: shortdesc}

1. Exclua os componentes de serviço usando o Helm. Certifique-se de usar o sinalizador `-tls` se você tiver o TLS ativado.

  ```
  helm del -- purge activity-insights [ -- tls ]
  ```
  {: codeblock}

2. Exclua os segredos do Kubernetes.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}
