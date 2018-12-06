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

# Configurando o monitoramento de clientes e IPs do servidor suspeitos para um cluster do Kubernetes
{: #cluster_install}

Para experimentar o recurso Network Analytics, instale os componentes do {{site.data.keyword.security-advisor_short}} em um cluster que é implementado para o {{site.data.keyword.containerlong_notm}}. Em seguida, é possível ver os insights e os alertas de rede no painel do {{site.data.keyword.security-advisor_short}}.
{:shortdesc}

Esse recurso do {{site.data.keyword.security-advisor_short}} é uma visualização de tecnologia.
{: tip}

Deseja saber mais sobre o Network Analytics no {{site.data.keyword.security-advisor_short}}? [Verifique os docs](network-analytics.html).


## Pré-requisitos
{: #cluster_prereqs}

Antes de iniciar:

* Estação de trabalho do desenvolvedor do Mac, Linux ou Windows 10
  * Windows 10: [ativar o recurso do subsistema Linux](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
* [Node.js](https://nodejs.org/en/) 6 ou mais recente
* [jQ](https://stedolan.github.io/jq/download/)
* [Interface da linha de comandos do {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 ou mais recente
* [Plug-in da CLI do {{site.data.keyword.containerlong_notm}}](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
* [CLI do Kubernetes (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 ou mais recente
* [Helm do Kubernetes (gerenciador de pacote)](https://docs.helm.sh/using_helm/#from-script)
* [Um cluster do Kubernetes grátis ou padrão](https://console.bluemix.net/containers-kubernetes/catalog/cluster) na região **Sul dos EUA** do {{site.data.keyword.Bluemix_notm}}
* Identifique o proprietário da conta do {{site.data.keyword.Bluemix_notm}} para concluir as etapas de instalação

## Efetuando login no cluster
{: #login}

1.  Efetue login no {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud login -a https://api.ng.bluemix.net -- sso
    ```
    {: pre}

2.  Liste todos os clusters na conta para obter o nome do cluster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  Direcione sua CLI para o cluster.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  Configure o caminho para o arquivo local de configuração do Kubernetes como uma variável de ambiente. Exemplo:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  Configure o Helm no cluster.

    ```
    helm init
    ```
    {: pre}

Mantenha essa janela da linha de comandos aberta e continue.
{: tip}

## Instalando os componentes do Security Advisor
{: #cluster_components}

O instalador deve ser executado pelo proprietário da conta do {{site.data.keyword.Bluemix_notm}}.
{: tip}

Ao configurar o {{site.data.keyword.security-advisor_short}}, as seguintes ações ocorrem:
1. Os metadados do cluster são coletados e usados para concluir a instalação dos endereços IP do nó do trabalhador, das sub-redes, da URL e do endereço IP de ingresso, do CRN do cluster, do terminal do Log Analysis, do espaço e do token.
2. Um ID de serviço e uma chave de API do IAM são gerados na conta do {{site.data.keyword.Bluemix_notm}} para que o cluster possa ser identificado pelo Security Advisor. Se você tiver mais de um cluster, execute a instalação para cada cluster para gerar os IDs de serviço e as chaves de API para cada cluster.
3. Os componentes do {{site.data.keyword.security-advisor_short}} são implementados no namespace de **monitoramento** no cluster.

<br/>
Para instalar os componentes do {{site.data.keyword.security-advisor_short}}:

1.  Faça download e extraia o [pacote de instalação](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true).
2.  Na mesma janela da linha de comandos da seção anterior, navegue para a localização do archive extraído e instale o pacote.

    ```
    ./install.sh
    ```
    {: pre}

3.  Verifique se os componentes estão instalados adequadamente consultando o painel do [{{site.data.keyword.security-advisor_short}}](https://console.bluemix.net/security-advisor/#/dashboard).

## Removendo os componentes do {{site.data.keyword.security-advisor_short}}
{: #cluster_uninstall}

Remova os componentes do {{site.data.keyword.security-advisor_short}} do cluster e o ID do serviço e a chave de API da conta do {{site.data.keyword.Bluemix_notm}}.

1.  Efetue login no {{site.data.keyword.Bluemix_notm}}.

    ```
    ibmcloud login -a https://api.ng.bluemix.net -- sso
    ```
    {: pre}

2.  Liste todos os clusters na conta para obter o nome do cluster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  Direcione sua CLI para o cluster.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  Configure o caminho para o arquivo local de configuração do Kubernetes como uma variável de ambiente. Exemplo:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  Navegue para a localização do archive extraído e execute o script desinstalador.

    ```
    ./uninstall.sh
    ```
    {: pre}

6.  Opcional: desinstale do cluster o componente do servidor do Helm.

    ```
    helm reset
    ```
    {: pre}
