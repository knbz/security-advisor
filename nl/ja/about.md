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

# {{site.data.keyword.security-advisor_short}} の概要
{: #about}

{{site.data.keyword.security-advisor_long}} を使用すれば、セキュリティー管理者は、クラウドのアプリケーションとワークロードのセキュリティー問題を検出し、優先順位を付けて対応することができます。
{:shortdesc}

{{site.data.keyword.security-advisor_short}} を使用すると、{{site.data.keyword.Bluemix_notm}} の脆弱性アドバイザーや {{site.data.keyword.cloudcerts_short}} などの洞察サービスを一元管理できます。セキュリティー・イベントを管理し、分析を適用して検出事項を作成できます。これにより、{{site.data.keyword.Bluemix_notm}} のセキュリティー管理プロセスを統合して向上させることができます。

{{site.data.keyword.security-advisor_short}} には、{{site.data.keyword.containerlong_notm}} クラスターに対する試験的なネットワーク分析機能もあり、ネット・フロー・データを収集して不審なクライアントやサーバー IP との通信を検出できます。

## 主要な概念
{: #concepts}

{{site.data.keyword.security-advisor_short}} を使用する場合に理解しておいたほうが良いさまざまな概念について説明します。
{: shortdesc}

<dl>
  <dt>検出事項</dt>
    <dd>検出事項とは、ロー・イベントを処理すると作成される優先度の高いセキュリティー上の問題です。検出事項は、問題の「誰が、何を、いつ、どこで」を明確にするために欠かせない重要な情報で構成されます。セキュリティー管理者は、{{site.data.keyword.security-advisor_short}} の検出事項を基に、検出された状態に優先順位を付けて対処することができます。</dd>
  <dt>重要パフォーマンス指標 (KPI)</dt>
    <dd>サービスやワークロードの特定のセキュリティー制御について、検出事項の値がパフォーマンスの許容範囲外である場合に、重要パフォーマンス指標がトリガーされます。</dd>
  <dt>注記</dt>
    <dd>注記を作成して、分析中に出てくる検出事項を分類できます。さまざまなプロバイダーの間で同じ注記が何度も出現する場合があります。</dd>
  <dt>オカレンス</dt>
    <dd>オカレンスは、プロバイダー固有の詳しい注記です。オカレンスには、脆弱性の詳細、対処手順、その他の一般情報が含まれます。</dd>
  <dt>サービス CRN</dt>
    <dd>サービス CRN は、検出事項に関係する {{site.data.keyword.Bluemix_notm}} サービスを示すものです。</br>
      <table>
        <tr>
          <th>検出事項のタイプ</th>
          <th>CRN</th>
        </tr>
        <tr>
          <td>{{site.data.keyword.cloudcerts_short}}</td>
          <td>サービス・インスタンスの CRN。</td>
        </tr>
        <tr>
          <td>ネットワーク分析</td>
          <td>Kubernetes クラスターの CRN。</td>
        </tr>
        <tr>
          <td>脆弱性アドバイザー</td>
          <td>コンテナーの CRN。</td>
        </tr>
      </table></dd>
    <dt>リソース CRN</dt>
      <dd>リソース CRN は、検出事項に関係する具体的なリソースを示すものです。</dd>
</dl>
