---

copyright:
  years: 2014, 2018
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

# {{site.data.keyword.security-advisor_short}} のセットアップ
{: #setup}

{{site.data.keyword.security-advisor_long}} を使用すると、アプリケーションのセキュリティー・リスクと脅威を継続的にモニターできます。脆弱性が検出されると、サービス・ダッシュボードにアラートが表示されます。
{:shortdesc}

## コンテナー・イメージの脆弱性のモニタリング
{: #setup_images}

Docker イメージが、作成するすべてのコンテナーの基礎になります。一般には、イメージの中にアプリケーション、構成、必要なすべての依存関係が含まれています。イメージは、通常、レジストリー内に保管されます。{{site.data.keyword.registryshort_notm}} を使用すると、潜在的なセキュリティー問題がないかイメージを継続的にスキャンする脆弱性アドバイザーを利用できます。問題が検出されると、アラートが通知され、{{site.data.keyword.security-advisor_short}} ダッシュボードで総合的なレポートを確認できます。
{:shortdesc}

詳しくは、[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry/index.html#index) を参照してください。


**始める前に**

レジストリーの使用を開始する前に、以下の CLI とプラグインがインストールされていることを確認してください。
- [{{site.data.keyword.Bluemix_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://clis.ng.bluemix.net/ui/home.html)
- container-registry プラグイン。

    ```
    ibmcloud plugin install container-registry -r Bluemix
    ```
    {: pre}

</br>
**名前空間の作成**

1. CLI を使用して、自分のアカウントにログインします。

   ```
   bx login --sso
   ```
   {: pre}

2. {{site.data.keyword.registryshort_notm}} にログインします。

   ```
   bx cr login
   ```
   {: pre}

3. オプション: 名前空間を作成します。いつでも既存のものを使用できます。

   ```
   bx cr namespace-add
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


イメージを {{site.data.keyword.registryshort_notm}} 名前空間にプッシュすると、検出された脆弱性に関する情報が、サービス・ダッシュボード内の**「脆弱性のあるイメージ (Images with Vulnerabilities)」**カードに表示されます。特定のイメージにドリルダウンすると、すべての既知の脆弱性や検出された構成の問題など、詳しい情報が表示されます。

</br>

## 証明書のモニタリング
{: #setup_certificates}

{{site.data.keyword.cloudcerts_long_notm}} で SSL/TLS 証明書のモニターと管理が可能なことをご存じでしたか?{{site.data.keyword.cloudcerts_short}} と {{site.data.keyword.security-advisor_short}} を統合すると、証明書やその他の情報の更新が必要な時期を知らせるアラートやリマインダーを受け取れるので、脆弱性の予防に役立ちます。
{:shortdesc}

詳しくは、[{{site.data.keyword.cloudcerts_long_notm}}](/docs/services/certificate-manager/index.html#gettingstarted) を参照してください。

1. {{site.data.keyword.Bluemix_notm}} カタログで、「{{site.data.keyword.cloudcerts_short}}」を検索します。
2. サービス・インスタンスに名前を付けます。または、事前設定された名前を使用します。
3. **「作成」**をクリックします。
4. 組織の証明書を {{site.data.keyword.cloudcerts_short}} にインポートするには、**「証明書のインポート (Import Certificate)」**をクリックします。

証明書がインポートされ、有効期限や期限切れの証明書などの情報が {{site.data.keyword.security-advisor_short}} ダッシュボードの**「証明書」**カードに表示されました。カードをクリックすると、証明書を所有するサービス・インスタンスなど、証明書に関する具体的な情報が表示されます。セキュリティーの脆弱性を修正するための手順なども表示されます。
