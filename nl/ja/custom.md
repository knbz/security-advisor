---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

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


# カスタム
{: #setup_custom}

{{site.data.keyword.security-advisor_short}} では、既存のカスタム・セキュリティー・ツールがオープン・ソースのサービス、カスタム開発したサービス、またはサード・パーティーのサービスのいずれであっても、そのツールを統合することができます。{{site.data.keyword.security-advisor_short}} から他のツールへの直接接続を作成するには、URL を統合リストに追加します。また、API および重要リスク指標 (KRI) を構成するための API を使用することで、サード・パーティーによる検出事項を統合して、重要なセキュリティー・イベントをダッシュボード内の新規カードに直接取り込むこともできます。
{: shortdesc}


## 直接接続の追加
{: #setup-custom-gui}

{{site.data.keyword.security-advisor_short}} ダッシュボードを使用して、セキュリティー・ツールを容易に追跡できます。
{: shortdesc}

### 始めに
{: #custom-before-gui}

統合を追加するには、その前に、統合しようとしているパートナーのアカウントをまず用意する必要があります。

{{site.data.keyword.security-advisor_short}} は、パートナー・サービスに関連した資格情報を永続化しません。 エンタープライズ・ユーザーが、{{site.data.keyword.Bluemix_notm}} とビジネス・パートナーの両方に対して SAML を使用して認証しなければなりません。
{: note}

### 接続の構成
{: #custom-configure-connection}

1. セキュリティー・ツールにログインし、固有の URL を取得します。

2. コンソールを使用して {{site.data.keyword.cloud_notm}} にログインします。

3. **「カスタム統合 (Custom Integrations)」>「直接接続 (Direct Connection)」**をクリックします。画面が表示されます。

  1. ソリューションに名前を付けます。 英数字、空白文字、ダッシュ (-) のみを名前で使用できます。

  2. `www.<website>.<domain>` の形式で、ソリューションの URL を入力します。

  3. ツールを表わすアイコンまたはイメージをアップロードします。

  4. **「接続」**をクリックして、構成を完了します。{{site.data.keyword.security-advisor_short}} は、サービス ID、API 鍵、アカウント ID、メタデータなどの、統合に必要な成果物を作成します。 `writer` 役割が割り当てられます。



## サード・パーティーによる検出事項の統合

API は、Grafeas に似た成果物メタデータ仕様に従って、セキュリティー・ツールやサービスから報告された検出事項の重要なメタデータを保管、照会、および取得します。
{: #setup-custom-api}

### 始めに
{: #custom-before-api}

サード・パーティー・ツールからの検出事項を統合する前に、以下の前提条件を必ず満たしてください。

1. 使用するユーザー ID またはサービス ID に、**管理者**の [IAM 役割](https://cloud.ibm.com/iam#/users)が割り当てられていることを確認します。

1. {{site.data.keyword.cloud_notm}} にログインします。

  ```
  ibmcloud login
  ```
  {: codeblock}

2. アカウント ID を取得します。 サービスの役割について詳しくは、[{{site.data.keyword.security-advisor_short}} のアクセス・ポリシー](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)を参照してください。

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

3. Identity and Access Management (IAM) トークンを取得します。このトークンを、各 API 要求の `--header` で使用します。

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM トークンの有効期限は 60 分です。詳しくは、API キーを使用して[新規トークンを自動的に取得する](/docs/iam?topic=iam-iamtoken_from_apikey#iamtoken_from_apikey)方法を参照してください。
  {: tip}



### 検出事項と KRI のインポート
{: #custom-adding}

1. 注記を作成して、新しいタイプの検出事項を登録します。 注記を作成するには、[Findings API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote) を使用します。カスタム・ツールを識別するためには、必ず、固有のプロバイダー ID を選択してください。サービス ID を使用してプロセスを自動化する場合は、そのサービス ID をプロバイダー ID として使用することができます。

  要求の例:

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Content-Type: application/json" -d "{ \"kind\": \"FINDING\", \"short_description\": \"My security tool finding\", \"long_description\": \"See what my custom security tool found\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-findings-type\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Custom Security Tool\" }, \"finding\": { \"severity\": \"MEDIUM\", \"next_steps\": [ { \"title\": \"Learn why this is reported as a risk\" } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="詳細アイコン"/> このコマンドの構成要素について </th>
    </thead>
    <tbody>
      <tr>
        <td><code>note_name</code></td>
        <td><code>&lt;account id&gt;/providers/my-custom-tool/notes/my-custom-tool-findings-type</code></td>
      </tr>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>remediation</code></td>
        <td>問題を解決するために取るべき手順。</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>カスタム・セキュリティー・ツール。</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>セキュリティー・ツールで検出されたこのタイプの検出事項の ID。</td>
      </tr>
      <tr>
        <td><code>context</code><ul><li><code>region</code></li><li><code>resource_id</code></li> <li><code>resource_name</code></li> <li><code>resource_type</code></li> <li><code>service_name</code></li></ul></td>
        <td></br><ul><li><code>検出事項が検出された場所。</code></li><li><code>具体的なリソースの ID。</code></li> <li><code>リソースの名前。</code></li> <li><code>リソースのタイプ。</code></li> <li><code>サービスの名前。</code></li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>url</code></li></ul></td>
        <td></br><ul><li>この検出事項が表す緊急性のレベル。</li> <li>問題に対処するために取れる手順。</li> <li>検出事項の詳細がわかる URL。</li></ul></td>
      </tr>
    </tbody>
  </table>

  応答の例:

  ```
    {
    "author": {
      "account_id": "account id",
      "email": "email id",
      "id": "IBM ID",
      "kind": "user"
    },
    "context": {
      "account_id": "account id"
    },
    "create_time": "2018-09-04T10:57:56.913871Z",
    "create_timestamp": 1536058676914,
    "finding": {
      "next_steps": [
        {
          "title": "Learn why this is reported as a risk"
        }
      ],
      "severity": "MEDIUM"
    },
    "id": "my-custom-tool-findings-type",
    "kind": "FINDING",
    "long_description": "See what my custom security tool found",
    "name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "reported_by": {
      "id": "my-custom-tool",
      "title": "My Custom Security Tool"
    },
    "shared": true,
    "short_description": "My security tool finding",
    "update_time": "2018-09-04T10:57:56.913890Z",
    "update_timestamp": 1536058676914,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: screen}

  応答の一部として戻された注記の名前を覚えておいてください。この例では、その値は `/providers/my-custom-tool/notes/my-custom-tool-findings-type` です。次のステップでこの値を使用します。
  {: tip}

2. 検出事項を KRI またはイベント ([オカレンス](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence)と呼ばれることもあります) として通知します。

  カードごとに 2 つの KRI を定義することができます。
  {: note}

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account-id>/providers/my-custom-tool/occurrences" -H "accept: application/json" -H "Authorization: <iam-token>" -H "Replace-If-Exists: true" -H "Content-Type: application/json" -d "{ \"note_name\": \"<account-id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\", \"kind\": \"FINDING\", \"remediation\": \"how to resolve this\", \"provider_id\": \"my-custom-tool\", \"id\": \"my-custom-tool-finding-1\", \"context\": { \"region\": \"location\", \"resource_id\": \"pluginId\", \"resource_name\": \"www.myapp.com\", \"resource_type\": \"worker\", \"service_name\": \"application\" }, \"finding\": { \"severity\": \"HIGH\", \"next_steps\": [{ \"url\": \"Details URL\" }] }}"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="詳細アイコン"/> このコマンドの構成要素について </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>FINDING</code></td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>検出事項について要約した簡単な説明 (数ワード)。</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>検出事項について詳しく述べた長い説明。</td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>カスタム・セキュリティー・ツール。</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>セキュリティー・ツールで検出されたこのタイプの検出事項の ID。</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>検出事項を報告したセキュリティー・ツールの ID。</li><li>検出事項を報告したセキュリティー・ツールのタイトル。</li></ul></td>
      </tr>
      <tr>
        <td><code>finding</code> <ul><li><code>severity</code></li> <li><code>next_steps</code></li> <li><code>title</code></li></ul></td>
        <td></br><ul><li>この検出事項が表す緊急性のレベル。</li> <li>問題に対処するために取れる手順。</li> <li>検出事項のタイトル。</li></ul></td>
      </tr>
    </tbody>
  </table>

  応答の例:

  ```
    {
    "author": {
      "account_id": "account id",
      "email": "email id ",
      "id": "user id",
      "kind": "user"
    },
    "context": {
      "account_id": "account id",
      "region": "location",
      "resource_id": "pluginId",
      "resource_name": "www.myapp.com",
      "resource_type": "worker",
      "service_name": "application"
    },
    "create_time": "2018-09-04T11:32:14.564809Z",
    "create_timestamp": 1536060734565,
    "finding": {
      "next_steps": [
        {
          "title": "Learn why this is reported as a risk",
          "url": "Details URL"
        }
      ],
      "severity": "HIGH"
    },
    "id": "my-custom-tool-finding-1",
    "kind": "FINDING",
    "long_description": "See what my custom security tool found",
    "name": "<account id>/providers/my-custom-tool/occurrences/my-custom-tool-finding-1",
    "note_name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "remediation": "how to resolve this",
    "reported_by": {
      "id": "my-custom-tool",
      "title": "My Custom Security Tool"
    },
    "short_description": "My security tool finding",
    "update_time": "2018-09-04T11:32:14.564828Z",
    "update_timestamp": 1536060734565,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: screen}

3. [注記](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote)を作成して、検出事項を表示するダッシュボードのカードを定義します。

  ```
  curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account id>/providers/my-custom-tool/notes" -H "accept: application/json" -H "Authorization: <iam token>" -H "Content-Type: application/json" -d "{ \"kind\": \"CARD\", \"provider_id\": \"my-custom-tool\", \"id\": \"custom-tool-card\", \"short_description\": \"security risk found by my custom tool\", \"long_description\": \"Details about why this is security risk to be fixed\", \"reported_by\": { \"id\": \"my-custom-tool\", \"title\": \"My Security Tool\" }, \"card\": { \"section\": \"My Security Tools\", \"title\": \"My Security Tool Findings\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ], \"elements\": [ { \"kind\": \"NUMERIC\", \"text\": \"Count of findings reported by my security tool\", \"default_time_range\": \"1d\", \"value_type\": { \"kind\": \"FINDING_COUNT\", \"finding_note_names\": [ \"providers/my-custom-tool/notes/my-custom-tool-findings-type\" ] } } ] } }"
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="詳細アイコン"/> このコマンドの構成要素について </th>
    </thead>
    <tbody>
      <tr>
        <td><code>kind</code></td>
        <td><code>CARD</code></td>
      </tr>
      <tr>
        <td><code>provider_id</code></td>
        <td>カスタム・セキュリティー・ツール。</td>
      </tr>
      <tr>
        <td><code>id</code></td>
        <td>セキュリティー・ツールで検出されたこのタイプの検出事項の ID。</td>
      </tr>
      <tr>
        <td><code>short_description</code></td>
        <td>検出事項を要約した数ワードの説明。</td>
      </tr>
      <tr>
        <td><code>long_description</code></td>
        <td>検出事項について詳しく述べた長い説明。</td>
      </tr>
      <tr>
        <td><code>reported_by</code><ul><li><code>id</code></li><li><code>title</code></li></ul></td>
        <td></br><ul><li>検出事項を報告したセキュリティー・ツールの ID。</li><li>検出事項を報告したセキュリティー・ツールのタイトル。</li></ul></td>
      </tr>
      <tr>
        <td><code>card</code> <ul><li><code>section</code></li> <li><code>title</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>このカードを表示するセクション。</li> <li>カードのタイトル</li> <li><code>providers/<provider_id>/notes/my-custom-tool-findings-type</code></li></ul></td>
      </tr>
      <tr>
        <td><code>elements</code> <ul><li><code>kind</code></li> <li><code>text</code></li> <li><code>default_time_range</code></li></ul></td>
        <td></br><ul><li>エレメントのタイプ。</li> <li>表示するテキスト</li> <li>検査する時間。</li></ul></td>
      </tr>
      <tr>
        <td><code>value_type</code> <ul><li><code>kind</code></li> <li><code>finding_note_names</code></li></ul></td>
        <td></br><ul><li>値のタイプ</li> <li>カードに表示する検出事項の名前。</li></ul></td>
      </tr>
    </tbody>
  </table>

  応答の例:
  ```
    {
    "author": {
      "account_id": "<account id",
      "email": "email id",
      "id": "user id",
      "kind": "user"
    },
    "card": {
      "elements": [
        {
          "default_time_range": "1d",
          "kind": "NUMERIC",
          "text": "Count of findings reported by my security tool",
          "value_type": {
            "finding_note_names": [
              "providers/my-custom-tool/notes/my-custom-tool-findings-type"
            ],
            "kind": "FINDING_COUNT"
          }
        }
      ],
      "finding_note_names": [
        "providers/my-custom-tool/notes/my-custom-tool-findings-type"
      ],
      "section": "My Security Tools",
      "title": "My Security Tool Findings"
    },
    "context": {
      "account_id": "<account id>"
    },
    "create_time": "2018-09-04T11:49:36.056047Z",
    "create_timestamp": 1536061776056,
    "id": "custom-tool-card",
    "kind": "CARD",
    "long_description": "Details about why this is security risk to be fixed",
    "name": "<account id>/providers/my-custom-tool/notes/custom-tool-card",
    "provider_id": "my-custom-tool",
    "provider_name": "<account id>/providers/my-custom-tool",
    "reported_by": {
      "id": "my-custom-tool",
      "title": "My Security Tool"
    },
    "shared": true,
    "short_description": "security risk found by my custom tool",
    "update_time": "2018-09-04T11:49:36.056066Z",
    "update_timestamp": 1536061776056,
    "update_week_date": "2018-W36-2"
  }
  ```
  {: screen}

4. サービス・ダッシュボードにナビゲートし、作成したカードを確認します。

## 使用例
{: #custom-example}

`cloudkingdom` という名前の {{site.data.keyword.containershort_notm}} クラスター上で実行しているアプリケーションがあるとしましょう。アプリケーションの規模によっては、クラスター内に複数のポッドがあり、それらをすべて同時にモニターする場合があります。クラスターをモニターしてそれぞれ異なる脅威を検出する、複数のカスタム・ツールがある場合について考えましょう。クラスター内のポッドの 1 つが異常な量のデータを外部サーバーに送信し始めたら、できるだけ早くそのことを把握したいものです。データ転送をモニターするカスタム・ツールは、その問題を検出し、{{site.data.keyword.security-advisor_short}} に送信するでしょう。その問題を検出する別のカスタム統合が存在するなら、そこからも {{site.data.keyword.security-advisor_short}} に検出事項が送信されます。そして、{{site.data.keyword.security-advisor_short}} は、すべてのモニタリング・ツールから得た検出事項を単一のダッシュボードに表示します。これによって、ユーザーは、迅速にすべてのアラートの概要を確認し、問題を調査し、修復方法を知ることができます。


ペイロードの例:

```
{
	"note_name": "<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type",
	"kind": "FINDING",
	"remediation": "How to resolve Data leakage threat",
	"provider_id": "my-custom-tool",
	"id": "my-custom-tool-finding-2",
	"context": {
		"region": "location",
		"resource_id": "cluster crn",
		"resource_name": "cloudkingdom",
		"resource_type": "container",
		"service_name": "kubernetes service"
	},
	"finding": {
		"severity": "HIGH",
		"next_steps": [{
			"title": "Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities",
                        "url":"https://console.bluemix.net/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
