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

{{site.data.keyword.security-advisor_long}} を使用すると、{{site.data.keyword.cloud_notm}} Activity Tracker ログを継続的にモニターして、不正または不審な動作やリソースの変更を特定できます。 ベスト・プラクティスに基づいた、サービスが提供するルール・パッケージを使用することも、独自のカスタム・ルールを作成することもできます。
{: shortdesc}


## 始めに
{: #activity-prereq}

Activity Insights を使い始めるにあたり、以下の前提条件を確認してください。

- Windows 10 を使用している場合は、Windows Subsystem for Linux をアクティブにして [Ubuntu シェル](https://win10faq.com/install-run-ubuntu-bash-windows-10/){: external}をインストールする。
- yq CLI をインストールする。
  * [macOS および Windows 10](http://mikefarah.github.io/yq/){: external} の場合。
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
- [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external} v1.10.11 以降
- [Kubernetes Helm (パッケージ・マネージャー)](/docs/containers?topic=containers-helm) v2.9.0 以降。
- 標準 Kubernetes クラスター v1.10.11 以降


## COS バケットの作成
{: #activity-setup-cos}

{{site.data.keyword.security-advisor_short}} GUI を使用することで、新しい COS インスタンスとバケットを作成できます。

1. サービス・ダッシュボードの**「統合」**タブにナビゲートします。

2. Activity Insights ボックスで**「Analysis Disabled (分析無効)」**を**「Analysis Enabled (分析有効)」**に切り替えます。

3. **「セットアップに移動 (Go to set up)」**をクリックします。

4. 前提条件セクションで、**「COS インスタンスとバスケットの作成 (Create COS instance and bucket)」**をクリックします。 適切な命名規則と IAM 許可により COS インスタンスとバケットが自動的に作成されます。 バケットの情報が表示されます。

既存の COS のインスタンスとバケットがある場合、
`sa.<account_id>.telemetric.<cos_region>` という命名規則が使用されていることを確認してください。 COS インスタンスに格納されているデータをサービスで読み取れるようにするために、{{site.data.keyword.cloud_notm}} IAM を使用して
[サービス間の許可](/docs/iam?topic=iam-serviceauth)をセットアップします。 `source` を `{{site.data.keyword.security-advisor_short}}`、`target` をご使用の COS インスタンスに設定してください。 `「リーダー」`の IAM 役割を割り当ててください。


## {{site.data.keyword.security-advisor_short}} コンポーネントのインストール
{: #activity-install-components}

エージェントをインストールして、{{site.data.keyword.cloud_notm}} アカウントから監査フローのログを収集することができます。 ログは Cloud Object Storage インスタンスに保管されます。Activity Insights がここにあるログを分析して疑わしい動作の有無を調べられるようにすることができます。
{: shortdesc}

1. 以下のリポジトリーをローカル・システムに複製します。

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. `security-advisor-activity-insights` フォルダーに変更します。

3. 以下のコマンドを実行し、`.tar` ファイルを解凍します。

  ```
  tar -xvf security-advisor-activity-insights.tar
  ```
  {: codeblock}

4. `security-advisor-activity-insights` フォルダーに変更します。
5. {{site.data.keyword.cloud_notm}} CLI にログインします。 CLI のプロンプトに従って、ログインを完了します。

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
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

7. [Kubernetes サービス統合文書](/docs/containers?topic=containers-helm)を使用して Helm をインストールします。

8. オプション: [TLS を有効にします](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}。 ワークステーションを使用して複数のクラスターへの分析コンポーネントのインストールを処理しており、TLS が有効になっている場合は、TLS 構成が最新であり、コンポーネントをインストールする予定の現行クラスターと一致していることを確認してください。

9. 以下のコマンドを実行して Insights をインストールします。 このコマンドは、バケットの命名規則を検証し、Kubernetes 秘密を作成し、クラスターの GUID で値を更新し、Activity Insights をデプロイします。

  ```
  ./activity-insight-install.sh <cos_region> <cos_api_key> <at_region> <account_api_key> <account_spaces>
  ```
  {: codeblock}

  エラーが発生する場合は、`helm init --upgrade` を実行してみてください。
  {: tip}

  <table>
    <tr>
      <th>変数</th>
      <th>説明</th>
    </tr>
    <tr>
      <td><code>cos_region</code></td>
      <td>COS インスタンスがデプロイされる地域。 オプションには、<code>us-south</code> と <code>eu-gb</code> があります。</td>
    </tr>
    <tr>
      <td><code>cos_api_key</code></td>
      <td>COS インスタンスとバケットにアクセスするために作成した [API キー](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)。 キーにはプラットフォーム役割`「ライター」`が必要です。</td>
    </tr>
    <tr>
      <td><code>at_region</code></td>
      <td>COS インスタンスとバケットを作成した地域。 オプションには、<code>us-south</code> と <code>eu-gb</code> があります。</td>
    </tr>
    <tr>
      <td><code>account_api_key</code></td>
      <td>{{site.data.keyword.cloud_notm}} アカウントのプラットフォーム API キー。</td>
    </tr>
    <tr>
      <td><code>account_spaces</code></td>
      <td>{{site.data.keyword.cloud_notm}} アカウントのスペース GUID のコンマ区切りリスト。</td>
    </tr>
  </table>


## COS へのルール・パッケージの追加
{: #activity-adding-rules}

ルール・パッケージは、モニターするルールのリストを含む JSON ファイルです。 ルール・パッケージはダウンロードすることも、[独自のものを作成する](/docs/services/security-advisor?topic=security-advisor-activity#activity-packages)こともできます。 {{site.data.keyword.security-advisor_short}} エンジンは、それぞれのルールが正しい構文に従っているか検証します。
{: shortdesc}

1. 以下のリポジトリーを複製して、事前設定された複数のルール・パッケージを取得します。 `security-advisor-activity-insights` という名前のフォルダーがローカル・システム上に作成されます。

  ```
  https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. `IBM.rules/activities` という名前のフォルダーをローカルに作成します。

3. JSON ファイルを `security-advisor-activity-insights/security-advisor-ata-rule-packages` から `IBM.rules/activities` にコピーします。

4. {{site.data.keyword.cloud_notm}} ダッシュボードにナビゲートし、Activity Insights と関連付けられている COS サービス・インスタンスを選択します。

5. サービス・ダッシュボードの**「バケット」**タブで、Activity Insights と関連付けられているバケットを選択します。

5. COS インスタンスのホーム・ページで、**「アップロード」**をクリックし、**「フォルダー」**を選択します。

6. プロンプトが表示されたら、**「Aspera Connect クライアントのインストール (Install Aspera Connect client)」**をクリックします。 プロンプトが表示されなかった場合は、クライアントが既にインストールされていることを意味します。 クライアントをインストールする必要があった場合は、インストール終了時にステップ 5 を繰り返します。

7. *IBM.rules/activities* フォルダーを選択します。

8. **「アップロード」**をクリックします。

独自のパッケージを使用しますか? JSON ファイルの 1 つを手本にして、組織の必要に合ったルールを作成します。 ファイルを作成した後、COS インスタンス内の *IBM.rules/activities* フォルダーに追加します。 ルールの種類と書式設定について詳しくは、[ルール・パッケージについて](/docs/services/security-advisor?topic=security-advisor-activity)を確認してください。
{: tip}


## コンポーネントの削除
{: #activity-delete}

Activity Insights を使用する必要がなくなった場合は、このサービス・コンポーネントをクラスターから削除することができます。
{: shortdesc}

1. Helm を使用して、サービス・コンポーネントを削除します。 TLS が有効になっている場合は必ず、`-tls` フラグを使用してください。

  ```
  helm del --purge activity-insights [--tls]
  ```
  {: codeblock}

2. Kubernetes 秘密を削除します。

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}
