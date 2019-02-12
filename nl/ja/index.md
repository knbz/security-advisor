---

copyright:
  years: 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# 入門チュートリアル
{: #index}

{{site.data.keyword.security-advisor_long}} を使用すると、単一の一元化されたダッシュボードで、{{site.data.keyword.Bluemix_notm}} のセキュリティー状況を瞬時に表示できます。
{:shortdesc}

{{site.data.keyword.security-advisor_short}} は、デフォルトですべての {{site.data.keyword.Bluemix_notm}} アカウントについて有効になっているクラウド・サービスです。したがって、このサービスのインスタンスをプロビジョンする必要はありません。
{: tip}

このサービスは、さまざまなソースからセキュリティー情報を受け取り、対処が必要なセキュリティー・アラートや脆弱性をサービス・ダッシュボードに表示します。ダッシュボードには、すぐに使用可能な定義済みのカードが複数あります。これらのカードは IBM Cloud 内のセキュリティー・サービスからの検出事項ですが、同一の場所からすべてのセキュリティー・ツールにアクセスできるように、カードやカスタム・パートナー・ソリューションを追加することもできます。

事前統合済みの検出事項により、以下をモニターできます。

- {{site.data.keyword.cloudcerts_long_notm}} で管理する証明書。
- {{site.data.keyword.registrylong_notm}} に保管されているコンテナー・イメージの脆弱性。

IBM Cloud Kubernetes サービス・クラスター上で実行されている、不審なクライアントや危険にさらされている可能性があるコンテナーに関する知見を得ることもできます。この機能を有効にして構成すると、着信と発信の両方のクラスター・ネットワーク通信が収集されて、継続的にモニターされ、脅威インテリジェンスに基づいて分析されます。詳しくは、[ネットワーク分析](network-analytics.html)を参照してください。

</br>

## サービス・ダッシュボードへのアクセス
{: #dashboard}

さあ始めましょう。 以下のいずれかの方法でサービスのダッシュボードにアクセスできます。

<ul>
  <li>タイルを使用する場合は、以下のようにします。
    <ol>
      <li><a href="https://console.cloud.ibm.com/catalog/" target="_blank">{{site.data.keyword.Bluemix_notm}}<img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> にログインします。</li>
      <li>**「カタログ」**にナビゲートし、**「セキュリティーおよび ID」**をクリックします。</li>
      <li>{{site.data.keyword.security-advisor_short}} のタイルを選択します。 ダッシュボードが開き、事前構成されている統合ツール (脆弱性アドバイザーや証明書マネージャーなど) のセキュリティー情報が表示されます。</li>
    </ol>
  </li>
  <li>メニューを使用する場合は、以下のようにします。
    <ol>
      <li><a href="https://console.cloud.ibm.com" target="_blank">{{site.data.keyword.Bluemix_notm}}<img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> にログインします。</li>
      <li>ダッシュボードで、ハンバーガー・メニューをクリックして、オプションを展開します。</li>
      <li>**「セキュリティー」**をクリックします。 セキュリティー・ダッシュボードの概要が開きます。</li>
      <li>ナビゲーションの**「始めに (Getting Started)」**をクリックすると、サービスに関する一般的な概要情報が表示されます。作動中のサービスを表示して学習したい場合は、**「ダッシュボード」**をクリックしてください。</li>
    </ol>
  </li>
</ul>

事前統合済みの検出事項で、情報が表示されませんか? {{site.data.keyword.security-advisor_short}} がモニターする証明書かイメージがない可能性があります。{{site.data.keyword.security-advisor_short}} がダッシュボード・カードに取り込む必要があるものについて詳しくは、[事前統合済みのサービスの活用](setup.html)を参照してください。

</br>

## 次のステップ
{: #next}

ダッシュボードの動作を確認したので、{{site.data.keyword.security-advisor_short}} の利用方法を[さらに詳しく理解](about.html)してください。[developerWorks](ts_index.html) でユーザー・フィードバックを送信して、サービスの開発中にアイデアを提供することもできます。

</br>

## 可用性
{: #availability}

現在、{{site.data.keyword.security-advisor_short}} を活用できるのは、ダラスとロンドンの地域のみです。
