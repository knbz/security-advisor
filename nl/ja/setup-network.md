---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

---

{:new_window: target="_blank"}
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

# Network Insights
{: #setup-network}

{{site.data.keyword.security-advisor_long}} では、機械学習、学習パターン、および脅威インテリジェンスを使用して、{{site.data.keyword.containerlong_notm}}・クラスター上で実行される、危険にさらされている可能性のあるコンテナーを検出することによって、動作をモニターすることができます。
{: shortdesc}

2019 年 1 月 20 日より、Network Analytics 機能は Network Insights (ベータ版) に置き換えられます。サービス・ダッシュボード内の分析カードはすべて削除されますが、検出事項は検出事項データベースに残ります。
{: important}

## 始めに
{: #network-prereq}

現在 Network Analytics 機能を使用している場合は、Network Insights をインストールする前に、[サービス・コンポーネントを削除する](/docs/services/security-advisor?topic=security-advisor-setup-network#uninstall-analytics)必要があります。Network Insights を使い始めるにあたり、以下の前提条件を確認してください。

- Windows 10 を使用している場合は、Linux 用の Windows サブシステムをアクティブにして [Ubuntu shell](https://win10faq.com/install-run-ubuntu-bash-windows-10/) をインストールする
- yq CLI をインストールする。
  * [macOS および Windows 10](http://mikefarah.github.io/yq/) の場合。
  * CentOS、Red Hat、および Ubuntu の場合は、以下のコマンドを実行してバージョン 1.15 をインストールする。
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- 更新された cURL バイナリー: CentOS および Red Hat の場合、`yum update -y nss curl libcurl` を実行することによって更新できます。
- [{{site.data.keyword.cloud_notm}} CLI および必要なプラグイン](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
- [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.10.11 以降
- [Kubernetes Helm (パッケージ・マネージャー)](/docs/containers?topic=containers-integrations#helm) v2.9.0 以降。
- 標準 Kubernetes クラスター v1.10.11 以降

## COS バケットの作成
{: #network-setup-cos}

{{site.data.keyword.security-advisor_short}} GUI を使用することで、新しい COS インスタンスとバケットを作成できます。

1. サービス・ダッシュボードの**「統合」**タブで、Network Insights ボックスの**「Analysis Disabled (分析無効)」**を**「Analysis Enabled (分析有効)」**に切り替えます。

2. **「セットアップに移動 (Go to set up)」**をクリックします。

3. 前提条件セクションで、**「COS インスタンスとバスケットの作成 (Create COS instance and bucket)」**をクリックします。適切な命名規則と IAM 許可により COS インスタンスとバケットが自動的に作成されます。

既存の COS のインスタンスとバケットがある場合、`sa.<account_id>.telemetric.<cos_region> という命名規則が使用されていることを確認してください。`. COS インスタンスに格納されているデータをサービスで読み取れるようにするために、{{site.data.keyword.cloud_notm}} IAM を使用して
[サービス間の許可](/docs/iam?topic=iam-serviceauth#serviceauth)をセットアップします。
`source` を `{{site.data.keyword.security-advisor_short}}`、`target` をご使用の COS インスタンスに設定してください。`「リーダー」`の IAM 役割を割り当ててください。

## {{site.data.keyword.security-advisor_short}} コンポーネントのインストール
{: #network-install-components}

エージェントをインストールして、Kubernetes クラスターからネットワーク・フローのログを収集することができます。ログは Cloud Object Storage インスタンスに保管されます。したがって、Activity Insights がここにあるログを分析して疑わしいネットワーク・アクティビティーを検出し、それについての警告を出すことができるようにすることができます。
{: shortdesc}

モニターするクラスターごとにインストール手順を繰り返してください。
{: note}

1. 以下のリポジトリーをローカル・システムに複製します。

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-network-insights.git
  ```
  {: codeblock}

2. `security-advisor-network-analytics` フォルダーに変更します。

3. 以下のコマンドを実行し、`.tar` ファイルを解凍します。

  ```
  tar -xvf security-advisor-network-insights.tar
  ```
  {: codeblock}

4. `security-advisor-network-insights` フォルダーに変更します。

5. {{site.data.keyword.cloud_notm}} CLI にログインします。 CLI のプロンプトに従って、ログインを完了します。

  ```
  ibmcloud login -a https://api.<region>.bluemix.net
  ```
  {: codeblock}

  <table>
    <tr>
      <th>地域</th>
      <th>エンドポイント</th>
    </tr>
    <tr>
      <td>英国</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>米国南部</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

6. クラスターのコンテキストを設定します。

  1. 環境変数を設定して Kubernetes 構成ファイルをダウンロードするためのコマンドを取得します。

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. `export` で始まる出力をコピーし、端末に貼り付け、`KUBECONFIG` 環境変数を設定します。

7. [Kubernetes サービス統合文書](/docs/containers?topic=containers-integrations#helm)を使用して Helm をインストールします。

8. オプション: [TLS を有効にします](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md)。ワークステーションを使用して複数のクラスターへの分析コンポーネントのインストールを処理しており、TLS が有効になっている場合は、TLS 構成が最新であり、コンポーネントをインストールする予定の現行クラスターと一致していることを確認してください。

9. 以下のコマンドを実行して Helm チャートとその依存関係をインストールします。このコマンドは、バケットで正しい命名規則を使用しているかどうか検証し、Kubernetes 秘密を作成し、クラスターの GUID で値を更新し、Network Insights Helm チャートをデプロイします。エラーが発生する場合は、`helm init --upgrade` を実行してみてください。

  ```
  ./network-insight-install.sh <cos_region> <cos_api_key>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>変数</th>
      <th>説明</th>
    </tr>
    <tr>
      <td><code>cos_region</code></td>
      <td>COS インスタンスがデプロイされる地域。オプションには、<code>us-south</code> と <code>eu-gb</code> があります。</td>
    </tr>
    <tr>
      <td><code>cos_api_key</code></td>
      <td>COS インスタンスとバケットにアクセスするために作成した [API キー](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials#service-credentials)。キーにはプラットフォーム役割`「ライター」`が必要です。</td>
    </tr>
  </table>

クラスターは、
{{site.data.keyword.security-advisor_short}} Network Insights と連動するよう正常に構成されました。



## コンポーネントの削除
{: #network-delete}

Network Insights を使用する必要がなくなった場合は、このサービス・コンポーネントをクラスターから削除することができます。
{: shortdesc}

1. {{site.data.keyword.cloud_notm}} CLI にログインします。 CLI のプロンプトに従って、ログインを完了します。

  ```
  ibmcloud login -a https://api.<region>.bluemix.net
  ```
  {: codeblock}

  <table>
    <tr>
      <th>地域</th>
      <th>エンドポイント</th>
    </tr>
    <tr>
      <td>英国</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>米国南部</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

2. クラスターのコンテキストを設定します。

  1. 環境変数を設定して Kubernetes 構成ファイルをダウンロードするためのコマンドを取得します。

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. `export` で始まる出力をコピーし、端末に貼り付け、`KUBECONFIG` 環境変数を設定します。

3. Helm を使用して、コンポーネントを削除します。

  ```
  helm del --purge network-insights [--tls]
  ```
  {: codeblock}

4. Kubernetes 秘密を削除します。

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}

必ず、エージェントを削除するクラスターごとにプロセスを削除してください。
{: tip}

## Network Analytics のアンインストール
{: #uninstall-analytics}

Network Analytics のベータ版を使用した場合は、新しい{{site.data.keyword.security-advisor_short}} コンポーネントをインストールする前に古いコンポーネントをアンインストールする必要があります。サービス・コンポーネントが含まれているクラスターごとにこの処理を繰り返してください。
{: shortdesc}

1. {{site.data.keyword.Bluemix_notm}} にログインします。

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
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
