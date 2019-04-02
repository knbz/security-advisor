---

copyright:
  years: 2014, 2019
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

# 事前統合済みのサービスの活用
{: #setup-services}

{{site.data.keyword.security-advisor_short}} には複数の定義済みのカードが付属しており、セキュリティー・リスクや脅威をモニターするのに役立ちます。
{: shortdesc}

下記の {{site.data.keyword.security-advisor_short}} サービスは、以下に関するカードを自動的に作成します。

* {{site.data.keyword.registrylong_notm}}
* {{site.data.keyword.cloudcerts_long_notm}}

{{site.data.keyword.security-advisor_short}} とこのサービスの間の接続を作成するため行うことは何もありませんが、情報を使用して構成されたサービスのインスタンスがなければなりません。


## コンテナー・イメージの脆弱性のモニタリング
{: #setup-images}

{{site.data.keyword.registryshort_notm}} を使用すると、潜在的なセキュリティー問題がないか {{site.data.keyword.registryshort_notm}} インスタンス内のイメージを継続的にスキャンする脆弱性アドバイザーを利用できます。 問題が検出されると、アラートが通知され、{{site.data.keyword.security-advisor_short}} ダッシュボードで総合的なレポートを確認できます。
{:shortdesc}

詳しくは、[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-index#index) を参照してください。


**始めに**

レジストリーの使用を開始する前に、以下の CLI とプラグインがインストールされていることを確認してください。
* [{{site.data.keyword.cloud_notm}} CLI)](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
* コンテナー・レジストリー・プラグイン。

  ```
  ibmcloud plugin install container-registry -r Bluemix
  ```
  {: pre}


**名前空間の作成**

1. CLI を使用して、自分のアカウントにログインします。

  ```
  ibmcloud login --sso
  ```
  {: pre}

2. {{site.data.keyword.registryshort_notm}} にログインします。

  ```
  ibmcloud cr login
  ```
  {: pre}

3. オプション: 名前空間を作成します。 いつでも既存のものを使用できます。

  ```
  ibmcloud cr namespace-add
  ```
  {: pre}

3. イメージにタグを付けます。

  ```
  docker tag <image>:<tag> registry.ng.bluemix.net/<namespace>/<image>:<tag>
  ```
  {: pre}

5. イメージをプッシュします。

  ```
  docker push registry.ng.bluemix.net/<namespace>/<image>:<tag>
  ```
  {: pre}


イメージを {{site.data.keyword.registryshort_notm}} 名前空間にプッシュすると、検出された脆弱性に関する情報が、サービス・ダッシュボード内の**「脆弱性のあるイメージ (Images with Vulnerabilities)」**カードに表示されます。 特定のイメージにドリルダウンすると、すべての特定された脆弱性や構成の問題の説明など、詳しい情報が表示されます。


## 証明書のモニタリング
{: #setup-certificates}

{{site.data.keyword.cloudcerts_long_notm}} で SSL/TLS 証明書のモニターと管理が可能なことをご存じでしたか? {{site.data.keyword.cloudcerts_short}} と {{site.data.keyword.security-advisor_short}} を統合すると、証明書の有効期限に関するアラートを事前に受け取れるので、サービスやアプリケーションの停止の予防に役立ちます。
{:shortdesc}

{{site.data.keyword.cloudcerts_short}} にアップロードする証明書の有効期限日に応じて、証明書の期限が切れる 90 日、60 日、10 日、1 日前に {{site.data.keyword.security-advisor_short}} ダッシュボードに検出事項が表示されます。 さらに、期限切れの証明書に関する通知を毎日受け取ります。 毎日の通知は、証明書の有効期限が切れた次の日から始まります。

手動更新をトリガーするには、1 日で期限が切れる証明書の {{site.data.keyword.cloudcerts_short}} インスタンスへのアップロードを試行することもできます。 インポートが正常に実行されると、{{site.data.keyword.security-advisor_short}} ダッシュボードに重要リスク指標 (KRI) と検出項目が表示されます。

[{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager?topic=certificate-manager-gettingstarted#gettingstarted) について詳しくは、資料を参照してください。
{: tip}

**証明書の作成**

1 日で有効期限が切れる自己署名証明書を作成するには、端末で以下の OpenSSL コマンドを実行します。

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -subj "/CN=myservice.com" -out server.pem -days 1 -nodes
```
{: pre}


**証明書のアップロード**

1. {{site.data.keyword.Bluemix_notm}} カタログで、「{{site.data.keyword.cloudcerts_short}}」を検索します。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. **「作成」**をクリックします。
4. 組織の証明書を {{site.data.keyword.cloudcerts_short}} にインポートするには、**「証明書のインポート (Import Certificate)」**をクリックします。

これで、証明書がインポートされたため、有効期限や期限切れの証明書などの情報が {{site.data.keyword.security-advisor_short}} ダッシュボードの**「証明書」**カードに表示されました。 カードをクリックすると、証明書を所有するサービス・インスタンスなど、証明書に関する具体的な情報が表示されます。 セキュリティーの脆弱性を修正するための手順なども表示されます。

通知を停止するには、証明書を更新し、その証明書を {{site.data.keyword.cloudcerts_short}} にアップロードして、期限切れの証明書を削除しなければなりません。
{: tip}
