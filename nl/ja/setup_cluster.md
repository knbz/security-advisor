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

# ネットワーク分析のセットアップ (ベータ版)
{: #cluster_install}

{{site.data.keyword.security-advisor_short}} を {{site.data.keyword.containerlong_notm}} クラスター内にインストールして、このサービスのネットワーク分析機能をお試しいただけます。
{: shortdesc}

ネットワーク分析は、米国南部地域のみで使用可能なプレビュー機能です。
{: note}

クラスター内で {{site.data.keyword.security-advisor_short}} をセットアップすると、以下のアクションが実行されます。

* クラスター・メタデータが収集され、次のインストールを完了するために使用されます。ワーカー・ノードの IP アドレス、サブネット、Ingress の URL と IP アドレス、
クラスター CRN、ログ分析エンドポイント、スペース、およびトークン。
* {{site.data.keyword.security-advisor_short}} がクラスターを識別できるように、ID およびアクセス管理 (IAM) サービス ID と API 鍵が {{site.data.keyword.Bluemix_notm}} アカウントに生成されます。
* {{site.data.keyword.security-advisor_short}} コンポーネントは、クラスター内の **monitoring** 名前空間にデプロイされます。

複数のクラスターがある場合は、クラスターごとにインストールを実行して、クラスターごとにサービス ID と API 鍵を生成するようにしてください。


{{site.data.keyword.security-advisor_short}} のネットワーク分析について詳しくは、[こちらの資料を確認してください](network-analytics.html)。
{: tip}

</br>

## 前提条件
{: #cluster_prereqs}

始めに、以下の前提条件が整っていることを確認してください。
{: shortdesc}

アカウント所有者がインストール手順を完了しなければなりません。アカウント所有者でない場合は、{{site.data.keyword.Bluemix_notm}} アカウントの所有者を確認してインストールを依頼してください。
{: tip}

次に、以下の前提条件が整っていることを確認してください。

* Mac、Linux、または [Windows 10](https://win10faq.com/install-run-ubuntu-bash-windows-10/) の開発者ワークステーション。
* [Node.js](https://nodejs.org/en/) 6 以上
* [jQ](https://stedolan.github.io/jq/download/)
* 最小限必要なバージョンの必須の CLI とプラグイン。
  * [{{site.data.keyword.Bluemix_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) v0.6.5 以上
  * [Kubernetes CLI (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.7 以上</br> インストーラーは、{{site.data.keyword.containerlong_notm}} のデフォルトの安定バージョンの `v1.10.11` を使用してテストされています。インストーラーは、`v1.11.5` や最新バージョンの `v1.12.3` を使用してテストされていません。
  * [{{site.data.keyword.containerlong_notm}} プラグイン](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
  * [Kubernetes Helm (パッケージ・マネージャー)](https://docs.helm.sh/using_helm/#from-script)
* {{site.data.keyword.Bluemix_notm}} の**米国南部**地域の[無料または標準の Kubernetes クラスター](https://console.bluemix.net/containers-kubernetes/catalog/cluster)

クラスターの作成と構成に支援が必要ですか? この[チュートリアル](/docs/containers/cs_tutorials.html#cs_cluster_tutorial)を最後まで実行してみてください。
{: tip}

</br>

## Helm のセットアップ
{: #login}

1.  {{site.data.keyword.Bluemix_notm}} にログインします。

  ```
  ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2.  処理しようとしているクラスターの名前を取得するために、アカウントに含まれているすべてのクラスターのリストを出力します。

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3.  CLI のターゲットをクラスターに設定します。

  1. 以下のコマンドを実行して、クラスターを構成します。出力を次のステップで使用します。

    ```
    ibmcloud ks cluster-config <cluster-name-or-id>
    ```
    {: pre}

  2. 前のコマンドの出力を使用して、ローカル Kubernetes 構成ファイルのパスを環境変数として設定します。

  例:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
    ```
    {: screen}

4.  Helm チャートを処理できるように、tiller をクラスターにインストールします。

  ```
  helm init
  ```
  {: pre}

このコマンド・ライン・ウィンドウを開いたまま先に進むようにしてください。
{: tip}

</br>

## {{site.data.keyword.security-advisor_short}} コンポーネントのインストール
{: #cluster_components}

Helm を処理するようにクラスターを構成し終えたら、{{site.data.keyword.security-advisor_short}} コンポーネントをインストールできます。
{: shortdesc}


コンポーネントをインストールするには、同じ CLI ウィンドウで続行し、以下の手順を完了してください。

1. [インストール・パッケージ](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true)をダウンロードして解凍します。
2. 前のセクションと同じ CLI ウィンドウで続行し、アーカイブを解凍した場所にナビゲートし、パッケージをインストールします。

  ```
  ./install.sh
  ```
  {: pre}

3.  [{{site.data.keyword.security-advisor_short}} ダッシュボード](https://console.bluemix.net/security-advisor/#/dashboard)を調べて、コンポーネントが正しくインストールされていることを確認します。

</br>

## {{site.data.keyword.security-advisor_short}} コンポーネントの削除
{: #cluster_uninstall}

クラスターから {{site.data.keyword.security-advisor_short}} コンポーネントを削除し、{{site.data.keyword.Bluemix_notm}} アカウントからサービス ID と API 鍵を削除します。

1. {{site.data.keyword.Bluemix_notm}} にログインします。

  ```
  ibmcloud login -a https://api.ng.bluemix.net --sso
  ```
  {: pre}

2. クラスターの名前を取得するために、アカウントに含まれているすべてのクラスターのリストを出力します。

  ```
  ibmcloud ks clusters
  ```
  {: pre}

3. CLI のターゲットをクラスターに設定します。

  ```
  ibmcloud ks cluster-config <cluster-name-or-id>
  ```
  {: pre}

4. ローカル Kubernetes 構成ファイルのパスを環境変数として設定します。 例:

  ```
  export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-prod-dal10-<cluster_name>.yml
  ```
  {: pre}

5. アーカイブを解凍した場所にナビゲートし、アンインストーラー・スクリプトを実行します。

  ```
  ./uninstall.sh
  ```
  {: pre}

6. オプション: クラスターから Helm サーバー・コンポーネントをアンインストールします。

  ```
  helm reset
  ```
  {: pre}
