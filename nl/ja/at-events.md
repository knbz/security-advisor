---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-14"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

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



# {{site.data.keyword.cloudaccesstrailshort}} イベント
{: #at_events}

{{site.data.keyword.cloudaccesstrailshort}} サービスを使用すると、{{site.data.keyword.security-advisor_long}} サービス・インスタンスで実行されたユーザー開始のアクティビティーを表示、管理、および監査できます。
{: shortdesc}

このサービスの仕組みについて詳しくは、[{{site.data.keyword.cloudaccesstrailshort}} の資料](/docs/services/cloud-activity-tracker/reference?topic=cloud-activity-tracker-getting-started-with-cla#getting-started-with-cla)を参照してください。


## イベントを確認できる場所
{: #monitor}

イベントは、イベントが生成された {{site.data.keyword.Bluemix_notm}} 領域の {{site.data.keyword.cloudaccesstrailshort}} の**アカウント・ドメイン**で確認できます。

1. {{site.data.keyword.Bluemix_notm}} アカウントにログインします。
2. カタログから、{{site.data.keyword.security-advisor_short}} のインスタンスと同じアカウントに、{{site.data.keyword.cloudaccesstrailshort}} サービスのインスタンスをプロビジョンします。
3. {{site.data.keyword.cloudaccesstrailshort}} ダッシュボードの **「管理」** タブで、**「Kibana で表示」**をクリックします。
4. ログを表示する時間フレームを設定します。 デフォルトは 15 分です。
5. **「使用可能なフィールド」**リストで、**「タイプ」**をクリックします。 **アクティビティー・トラッカー**の虫眼鏡のアイコンをクリックして、サービスによって追跡されるもののみにログを制限します。
6. その他の使用可能なフィールドを使用して、検索を絞り込むことができます。

アカウント所有者以外のユーザーがログを表示するには、プレミアム・プランを使用している必要があります。 他のユーザーがイベントを表示できるようにするには、[アカウント・イベントを表示する許可の付与](/docs/services/cloud-activity-tracker/how-to?topic=cloud-activity-tracker-grant_permissions#grant_permissions)を参照してください。
{: tip}

## イベント・リスト
{: #events}

{{site.data.keyword.cloudaccesstrailshort}} に送信されるイベントのリストについては、以下の表を確認してください。
{: shortdesc}

<table>
  <tr>
    <th>アクション</th>
    <th>説明</th>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>過去に作成された注記を表示。</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>注記を作成。</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>注記を削除。</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>注記を更新。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>1 つ以上のオカレンスを表示。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>オカレンスを作成。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>オカレンスを削除。</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>オカレンスを更新。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>サービスに追加されたカスタム・ソリューションを読み取る。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>サービスにカスタム・ソリューションを追加する。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>サービス内の既存のカスタム・ソリューションを更新する。</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>サービスからカスタム・ソリューションを削除します。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>サービス・インスタンスに追加したパートナー・ソリューションを表示する。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>サービス・インスタンスにパートナー・ソリューションを追加する。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>サービス・インスタンス内のパートナー・ソリューションを更新する。</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>サービス・インスタンスからパートナー・ソリューションを削除する。</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>{{site.data.keyword.security-advisor_short}} によって提供される Network Insights フィーチャーを有効にする。</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>{{site.data.keyword.security-advisor_short}} によって提供される Network Insights フィーチャーを無効にする。</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>{{site.data.keyword.security-advisor_short}} によって提供される Activity Insights フィーチャーを有効にする。</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>{{site.data.keyword.security-advisor_short}} によって提供される Activity Insights フィーチャーを無効にする。</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>Network Insights と Activity Insights のために {{site.data.keyword.security-advisor_short}} によって Cloud Object Storage インスタンスを作成する。</td>
  </tr>
</table>
