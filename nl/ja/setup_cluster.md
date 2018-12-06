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

# Kubernetes クラスターで不審なクライアントおよびサーバー IP をモニタリングするためのセットアップ
{: #cluster_install}

ネットワーク分析機能を試すには、{{site.data.keyword.containerlong_notm}} にデプロイされているクラスターに {{site.data.keyword.security-advisor_short}} コンポーネントをインストールしてください。すると、ネットワークに関する知見やアラートが {{site.data.keyword.security-advisor_short}} ダッシュボードに表示されます。
{:shortdesc}

{{site.data.keyword.security-advisor_short}} のこの機能はテクノロジー・プレビューです。
{: tip}

{{site.data.keyword.security-advisor_short}} のネットワーク分析について詳しくは、[こちらの資料を参照してください](network-analytics.html)。


## 前提条件
{: #cluster_prereqs}

開始前に、以下のことを行います。

* Mac、Linux、または Windows 10 の開発者ワークステーション
  * Windows 10: [Linux サブシステム機能を有効にします](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
* [Node.js](https://nodejs.org/en/) 6 以上
* [jQ](https://stedolan.github.io/jq/download/)
* [{{site.data.keyword.Bluemix_notm}} コマンド・ライン・インターフェース](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 以上
* [{{site.data.keyword.containerlong_notm}} CLI プラグイン](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
* [Kubernetes CLI (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 以上
* [Kubernetes Helm (パッケージ・マネージャー)](https://docs.helm.sh/using_helm/#from-script)
* {{site.data.keyword.Bluemix_notm}} の**米国南部**地域の[無料または標準の Kubernetes クラスター](https://console.bluemix.net/containers-kubernetes/catalog/cluster)
* このインストール手順を実行できる {{site.data.keyword.Bluemix_notm}} アカウント所有者を確認します

## クラスターにログインする
{: #login}

1.  {{site.data.keyword.Bluemix_notm}} にログインします。

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  クラスターの名前を取得するために、アカウントに含まれているすべてのクラスターのリストを出力します。

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  CLI のターゲットをクラスターに設定します。

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  ローカル Kubernetes 構成ファイルのパスを環境変数として設定します。例:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  クラスター内に Helm をセットアップします。

    ```
    helm init
    ```
    {: pre}

このコマンド・ライン・ウィンドウを開いたまま先に進んでください。
{: tip}

## Security Advisor コンポーネントをインストールする
{: #cluster_components}

インストーラーは、{{site.data.keyword.Bluemix_notm}} アカウント所有者が実行する必要があります。
{: tip}

{{site.data.keyword.security-advisor_short}} をセットアップすると、以下のアクションが実行されます
1. インストールを実行するために、ワーカー・ノードの IP アドレス、サブネット、Ingress の URL と IP アドレス、クラスター CRN、ログ分析エンドポイント、スペース、およびトークンのクラスター・メタデータが収集され、使用されます。
2. Security Advisor がクラスターを識別できるように、IAM サービス ID と API 鍵が {{site.data.keyword.Bluemix_notm}} アカウントに生成されます。複数のクラスターがある場合は、クラスターごとにインストールを実行して、クラスターごとにサービス ID と API 鍵を生成します。
3. {{site.data.keyword.security-advisor_short}} コンポーネントは、クラスター内の **monitoring** 名前空間にデプロイされます。

<br/>
{{site.data.keyword.security-advisor_short}} コンポーネントをインストールするには、以下のようにします。

1.  [インストール・パッケージ](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true)をダウンロードして解凍します。
2.  前のセクションと同じコマンド・ライン・ウィンドウで、アーカイブを解凍した場所にナビゲートし、パッケージをインストールします。

    ```
    ./install.sh
    ```
    {: pre}

3.  [{{site.data.keyword.security-advisor_short}} ダッシュボード](https://console.bluemix.net/security-advisor/#/dashboard)を調べて、コンポーネントが正しくインストールされていることを確認します。

## {{site.data.keyword.security-advisor_short}} コンポーネントの削除
{: #cluster_uninstall}

クラスターから {{site.data.keyword.security-advisor_short}} コンポーネントを削除し、{{site.data.keyword.Bluemix_notm}} アカウントからサービス ID と API 鍵を削除します。

1.  {{site.data.keyword.Bluemix_notm}} にログインします。

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  クラスターの名前を取得するために、アカウントに含まれているすべてのクラスターのリストを出力します。

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  CLI のターゲットをクラスターに設定します。

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

4.  ローカル Kubernetes 構成ファイルのパスを環境変数として設定します。例:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: pre}

5.  アーカイブを解凍した場所にナビゲートし、アンインストーラー・スクリプトを実行します。

    ```
    ./uninstall.sh
    ```
    {: pre}

6.  オプション: クラスターから Helm サーバー・コンポーネントをアンインストールします。

    ```
    helm reset
    ```
    {: pre}
