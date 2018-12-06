---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-11"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# {{site.data.keyword.cloudaccesstrailshort}} イベント
{: #at_events}

{{site.data.keyword.cloudaccesstrailshort}} サービスを使用すると、{{site.data.keyword.security-advisor_long}} サービス・インスタンスで実行されたユーザー開始のアクティビティーを表示、管理、および監査できます。
{: shortdesc}

このサービスの仕組みについて詳しくは、[{{site.data.keyword.cloudaccesstrailshort}} の資料](/docs/services/cloud-activity-tracker/index.html)を参照してください。


## イベントを確認できる場所
{: #monitor}

イベントは、イベントが生成された {{site.data.keyword.Bluemix_notm}} 領域の {{site.data.keyword.cloudaccesstrailshort}} の**アカウント・ドメイン**で確認できます。

1. {{site.data.keyword.Bluemix_notm}} アカウントにログインします。
2. カタログから、{{site.data.keyword.security-advisor_long}} のインスタンスと同じアカウントに、{{site.data.keyword.cloudaccesstrailshort}} サービスのインスタンスをプロビジョンします。
3. {{site.data.keyword.cloudaccesstrailshort}} ダッシュボードの **「管理」** タブで、**「Kibana で表示」**をクリックします。
4. ログを表示する時間フレームを設定します。 デフォルトは 15 分です。
5. **「使用可能なフィールド」**リストで、**「タイプ」**をクリックします。 **アクティビティー・トラッカー**の虫眼鏡のアイコンをクリックして、サービスによって追跡されるもののみにログを制限します。
6. その他の使用可能なフィールドを使用して、検索を絞り込むことができます。

アカウント所有者以外のユーザーがログを表示するには、プレミアム・プランを使用している必要があります。他のユーザーがイベントを表示できるようにするには、[アカウント・イベントを表示する許可の付与](/docs/services/cloud-activity-tracker/how-to/grant_permissions.html#grant_permissions)を参照してください。
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
</table>
