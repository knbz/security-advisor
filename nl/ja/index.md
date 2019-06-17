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


# 入門チュートリアル
{: #getting-started}

{{site.data.keyword.security-advisor_long}} を使用すると、単一の一元化されたダッシュボードで、{{site.data.keyword.cloud_notm}} のセキュリティー状況を瞬時に表示できます。
{:shortdesc}

デフォルトでは、{{site.data.keyword.security-advisor_short}} はすべての {{site.data.keyword.cloud_notm}} アカウントで有効になっています。 したがって、このサービスのインスタンスをプロビジョンする必要はありません。
{: tip}

このサービスは、さまざまなソースからセキュリティー情報を受け取り、対処が必要なセキュリティー・アラートや脆弱性をサービス・ダッシュボードに表示します。 ダッシュボードには、すぐに使用可能な定義済みのカードが複数あります。 これらは {{site.data.keyword.cloud_notm}} 内のセキュリティー・サービスからの検出事項ですが、同一の場所からすべてのセキュリティー・ツールにアクセスできるように、カードやカスタム・パートナー・ソリューションを追加することもできます。

事前統合済みの検出事項により、以下をモニターできます。

- {{site.data.keyword.cloudcerts_long_notm}} で管理する証明書。
- {{site.data.keyword.registrylong_notm}} に保管されているコンテナー・イメージの脆弱性。



## サービス・ダッシュボードへのアクセス
{: #start-dashboard}

さあ始めましょう。 以下のいずれかの方法でサービスのダッシュボードにアクセスできます。

<ul>
  <li>タイルを使用する場合は、以下のようにします。
    <ol>
      <li><a href="https://cloud.ibm.com/login" target="_blank">{{site.data.keyword.cloud_notm}}<img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> にログインします。</li>
      <li>**「カタログ」**にナビゲートし、**「セキュリティーおよび ID」**をクリックします。</li>
      <li>{{site.data.keyword.security-advisor_short}} のタイルを選択します。 ダッシュボードが開き、事前構成されている統合ツール (脆弱性アドバイザーや証明書マネージャーなど) のセキュリティー情報が表示されます。</li>
    </ol>
  </li>
  <li>メニューを使用する場合は、以下のようにします。
    <ol>
      <li><a href="https://cloud.ibm.com/login" target="_blank">{{site.data.keyword.cloud_notm}}<img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> にログインします。</li>
      <li>ダッシュボードで、ハンバーガー・メニューをクリックして、オプションを展開します。</li>
      <li>**「セキュリティー」**をクリックします。 セキュリティー・ダッシュボードの概要が開きます。</li>
      <li>ナビゲーションの**「始めに (Getting Started)」**をクリックすると、サービスに関する一般的な概要情報が表示されます。作動中のサービスを表示して学習したい場合は、**「ダッシュボード」**をクリックしてください。</li>
    </ol>
  </li>
</ul>

事前統合済みの検出事項で、情報が表示されませんか? {{site.data.keyword.security-advisor_short}} がモニターする証明書かイメージがない可能性があります。 ダッシュボード・カードに情報を取り込むために {{site.data.keyword.security-advisor_short}} で必要なことについて詳しくは、[事前統合済みのサービスの活用](/docs/services/security-advisor?topic=security-advisor-setup-services)を参照してください。


## 次のステップ
{: #start-next}

ダッシュボードの動作を確認したので、{{site.data.keyword.security-advisor_short}} の利用方法を[さらに詳しく理解](/docs/services/security-advisor?topic=security-advisor-about)してください。 [dW Answers](https://developer.ibm.com){: external} でユーザー・フィードバックを送信して、このサービスの開発に役立つアイデアを提供することもできます。


## 可用性
{: #start-availability}

現在、{{site.data.keyword.security-advisor_short}} を利用できる地域は、ダラス (us-south) とロンドン (eu-gb) のみです。
