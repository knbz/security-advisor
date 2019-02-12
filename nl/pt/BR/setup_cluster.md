---

copyright:
  years: 2018
lastupdated: "2018-12-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:note: .note}

# Configurando o Network Analytics (beta)
{: #cluster_install}

É possível experimentar o recurso Network Analytics do serviço instalando um {{site.data.keyword.security-advisor_short}} em um cluster do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

O Network Analytics é um recurso de visualização que está disponível apenas na região Sul dos EUA.
{: note}

Ao configurar o {{site.data.keyword.security-advisor_short}} em um cluster, as ações a seguir ocorrem:

* Os metadados do cluster são coletados e usados para concluir a instalação dos endereços IP do nó do trabalhador, das sub-redes, da URL e do endereço IP de ingresso, do CRN do cluster, do terminal do Log Analysis, do espaço e do token.
* Um ID de serviço e uma chave de API do Identity and Access Management (IAM) são gerados na conta do {{site.data.keyword.Bluemix_notm}} para que seu cluster possa ser identificado pelo {{site.data.keyword.security-advisor_short}}.
* Os componentes do {{site.data.keyword.security-advisor_short}} são implementados no namespace de **monitoramento** em seu cluster.

Se você tiver mais de um cluster, certifique-se de executar a instalação de cada cluster para gerar IDs de serviço e chaves de API para cada cluster.


Deseja saber mais sobre como o Network Analytics funciona no {{site.data.keyword.security-advisor_short}}? [Verifique os docs](network-analytics.html).
{: tip}

</br>

## Pré-requisitos
{: #cluster_prereqs}

Antes de iniciar, certifique-se de ter os pré-requisitos a seguir em vigor.
{: shortdesc}

O proprietário da conta deve concluir as etapas de instalação. Se não for você, verifique quem é o proprietário de sua conta do {{site.data.keyword.Bluemix_notm}} e peça que ajude com a instalação.
{: tip}

Em seguida, certifique-se de ter os pré-requisitos a seguir:

* Uma estação de trabalho do desenvolvedor Mac, Linux ou [Windows 10](https://win10faq.com/install-run-ubuntu-bash-windows-10/).
* [Node.js](https://nodejs.org/en/) 6 ou mais recente
* [jQ](https://stedolan.github.io/jq/download/)
* As CLIs e os plug-ins requeridos na versão mínima necessária:
  * [CLI do {{site.data.keyword.Bluemix_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 ou superior
  * [CLI do Kubernetes (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 ou mais recente</br> O instalador foi testado com a versão estável padrão do {{site.data.keyword.containerlong_notm}} `v1.10.11`. O instalador não foi testado com a `v1.11.5` ou com a versão mais recente `v1.12.3`.
  * [Plug-in do {{site.data.keyword.containerlong_notm}}](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
  * [Helm do Kubernetes (gerenciador de pacote)](https://docs.helm.sh/using_helm/#from-script)
* [Um cluster de Kubernetes livre ou padrão](https://console.bluemix.net/containers-kubernetes/catalog/cluster) na região **Sul dos EUA** do {{site.data.keyword.Bluemix_notm}}

Precisa de ajuda para criar e configurar um cluster? Tente executar por meio deste [tutorial](/docs/containers/cs_tutorials.html#cs_cluster_tutorial).
{: tip}

</br>

## Configurando o Helm
{: #login}

1.  Efetue login no {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login -a https://api.ng.bluemix.net -- sso
  ```
  {: pre}

2.  Liste todos os clusters na conta para obter o nome do cluster com o qual você deseja trabalhar.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3.  Direcione sua CLI para o cluster.

  1. Execute o comando a seguir para configurar o cluster. Você usará a saída na próxima etapa.

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

  2. Use a saída do comando anterior para configurar o caminho para o arquivo de configuração local do Kubernetes como uma variável de ambiente.

  Exemplo:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: screen}

4.  Instale o Tiller em seu cluster para que você possa trabalhar com os gráficos de Helm.

  ```
  helm init
  ```
  {: pre}

Certifique-se de manter essa janela da linha de comandos aberta conforme você continua.
{: tip}

</br>

## Instalando componentes do {{site.data.keyword.security-advisor_short}}
{: #cluster_components}

Depois que seu cluster está configurado para trabalhar com o Helm, é possível instalar os componentes do {{site.data.keyword.security-advisor_short}}.
{: shortdesc}


Para instalar os componentes, continue na mesma janela da CLI e conclua as etapas a seguir:

1. Faça download e extraia o [pacote de instalação](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true).
2. Continuando na mesma janela da CLI da seção anterior, navegue para o local de archive extraído e instale o pacote.

  ```
  ./install.sh
  ```
  {: pre}

3.  Verifique se os componentes estão instalados corretamente, verificando o [painel do {{site.data.keyword.security-advisor_short}}](https://console.bluemix.net/security-advisor/#/dashboard).

</br>

## Removendo os componentes do {{site.data.keyword.security-advisor_short}}
{: #cluster_uninstall}

Remova os componentes do {{site.data.keyword.security-advisor_short}} do cluster e o ID do serviço e a chave de API da conta do {{site.data.keyword.Bluemix_notm}}.

1. Efetue login no {{site.data.keyword.Bluemix_notm}}.

  ```
  ibmcloud login -a https://api.ng.bluemix.net -- sso
  ```
  {: pre}

2. Liste todos os clusters na conta para obter o nome do cluster.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3. Direcione sua CLI para o cluster.

  ```
  ibmcloud ks cluster-config <cluster-name-or-id>
  ```
  {: pre}

4. Configure o caminho para o arquivo local de configuração do Kubernetes como uma variável de ambiente. Exemplo:

  ```
  export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
  ```
  {: pre}

5. Navegue para a localização do archive extraído e execute o script desinstalador.

  ```
  ./uninstall.sh
  ```
  {: pre}

6. Opcional: desinstale do cluster o componente do servidor do Helm.

  ```
  helm reset
  ```
  {: pre}
