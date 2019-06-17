---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

keywords: Centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

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


# カスタム検出事項
{: #setup_custom}

{{site.data.keyword.security-advisor_short}} には、既存のカスタム・セキュリティー・ツールを統合できます。オープン・ソースのサービス、カスタム開発したサービス、サード・パーティーのサービス、どのようなサービスでも統合できます。サード・パーティーの検出事項を統合すれば、すべてのセキュリティー・ツールとすべての検出事項を 1 つのダッシュボードで利用して、重要なセキュリティー・イベントをモニターできるようになります。
{: shortdesc}


## 始めに
{: #custom-before-api}

サード・パーティー・ツールの検出事項を統合する前に、以下の前提条件を必ず満たしてください。

1. 使用するユーザー ID またはサービス ID に、**管理者**の [IAM 役割](https://cloud.ibm.com/iam#/users)が割り当てられていることを確認します。

2. {{site.data.keyword.cloud_notm}} にログインします。

  ```
  ibmcloud login
  ```
  {: codeblock}

3. アカウント ID を取得します。 サービスの役割について詳しくは、[{{site.data.keyword.security-advisor_short}} のアクセス・ポリシー](/docs/iam?topic=iam-iammanidaccser#iammanidaccser)を参照してください。

  ```
  ibmcloud account list org-account ORG_NAME [--guid]
  ```
  {: codeblock}

4. Identity and Access Management (IAM) トークンを取得します。 このトークンを、各 API 要求の `--header` で使用します。

  ```
  ibmcloud iam oauth-tokens
  ```
  {: codeblock}

  IAM トークンの有効期限は 60 分です。 詳しくは、API キーを使用して[新規トークンを自動的に取得する](/docs/iam?topic=iam-iamtoken_from_apikey)方法を参照してください。
  {: tip}



## 検出事項と KRI のインポート
{: #custom-adding}

API は、Grafeas に似た成果物メタデータ仕様に従って、セキュリティー・ツールやサービスから報告された検出事項の重要なメタデータを保管、照会、取得します。重要リスク指標 (KRI) を構成するための API を使用することで、統合を実現できます。
{: shortdesc}


### ステップ 1: 新しい検出事項タイプの登録
{: #custom-register-finding}

新しい検出事項タイプを登録するには、注記を作成します。注記を作成するには、[Findings API](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external} を使用します。プロバイダー ID としてサービス ID を使用してプロセスを自動化する場合は、必ず、カスタム・ツールを識別する固有のプロバイダー ID を選択してください。

要求の例:

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{  \"kind\": \"FINDING\",  \"short_description\": \"My security tool finding\",  \"long_description\": \"Longer description of what the security tool found.\",  \"provder_id\": \"my-custom-tool\",  \"id\": \"my-custom-tool-findings-type\",  \"reported_by\": {    \"id\": \"my-custom-tool\",    \"title\": \"My custom security tool\"  } ,  \"finding\": {    \"severity\": \"MEDIUM\",    \"next_steps\": [      {        \"title\": \"Explain why it's reported as a risk.\"      }    ]  }}"
```
{: codeblock}

| 一般 | 説明 | 
|:-----------------|:-----------------|
| `note_name` | `<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type`  |
| `kind` | `FINDING` |
| `remediation` | 問題を解決するために取るべき手順。|
| `provider_id` | カスタム・セキュリティー・ツール。|
| `id` | セキュリティー・ツールで検出されたこのタイプの検出事項の ID。|
{: class="simple-tab-table"}
{: caption="表 1. コマンドの一般的な構成要素について" caption-side="top"}
{: #registerfindingtable1}
{: tab-title="General"}
{: tab-group="register"}

| context | 説明 | 
|:-----------------|:-----------------|
| `region` | 検出事項が検出された場所。 |
| `resource_id` | リソースの ID。|
| `resource_name` | リソースの名前。|
| `resource_type` | リソースのタイプ。|
| `service_name` | サービスの名前。|
{: class="simple-tab-table"}
{: caption="表 2. コマンドのコンテキストの構成要素について" caption-side="top"}
{: #registerfinding2}
{: tab-title="Context"}
{: tab-group="register"}

| 検出事項 | 説明 | 
|:-----------------|:-----------------|
| `severity` | この検出事項が表す緊急性のレベル。 |
| `next_steps` | 問題に対処するために取れる手順。|
| `url` | 検出事項の詳細がわかる URL。|
{: class="simple-tab-table"}
{: caption="表 3. コマンドの検出事項の構成要素について" caption-side="top"}
{: #registerfinding3}
{: tab-title="Finding"}
{: tab-group="register"}

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

応答の一部として戻された注記の名前を覚えておいてください。 この例では、その値は `/providers/my-custom-tool/notes/my-custom-tool-findings-type` です。 次のステップでこの値を使用します。
{: tip}


### ステップ 2: 検出事項の通知
{: #custom-post-findings}

[オカレンス](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Occurrences/createOccurrence){: external}を作成して、検出事項を KRI またはイベントとしてセキュリティー・アドバイザーのダッシュボードに通知します。

カードごとに 2 つの KRI を定義できます。
{: note}

要求の例: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/occurrences"  -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"note_name\": \"<account id>/providers/my-custom-tool/notes/my-custom-tool-findings-type\",    \"kind\": \"FINDING\",    \"remediation\": \"How to resolve Data leakage threat\",    \"provider_id\": \"my-custom-tool\",    \"id\": \"my-custom-tool-finding-2\",    \"context\": {        \"region\": \"location\",        \"resource_id\": \"cluster crn\",        \"resource_name\": \"cloudkingdom\",        \"resource_type\": \"container\",        \"service_name\": \"kubernetes service\"    },    \"finding\": {        \"severity\": \"HIGH\",        \"next_steps\": [{            \"title\": \"Investigate which process are running in your cluster. If you suspect one of your pods was hacked, restart it, and look for image vulnerabilities\",                        \"url\":\"https://cloud.ibm.com/containers-kubernetes/clusters\"        }],                \"short_description\": \"One of the pods in your cluster appears to be leaking an excessive amount of data\",                \"long_description\": \"One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod's normal behavior\"    }}"
```
{: codeblock}

| 一般 | 説明 | 
|:-----------------|:-----------------|
| `kind` | `FINDING` |
| `short_description` | 検出事項について要約した簡単な説明 (数ワード)。|
| `long_description` | 検出事項について詳しく述べた長い説明。|
| `provider_id` | カスタム・セキュリティー・ツール。|
| `id` | セキュリティー・ツールで検出されたこのタイプの検出事項の ID。|
{: class="simple-tab-table"}
{: caption="表 2. コマンドの一般的な構成要素について" caption-side="top"}
{: #postfinding1}
{: tab-title="General"}
{: tab-group="post"}

| 報告元 | 説明 | 
|:-----------------|:-----------------|
| `id` | 検出事項を報告したセキュリティー・ツールの ID。 |
| `title` | 検出事項を報告したセキュリティー・ツールのタイトル。|
{: class="simple-tab-table"}
{: caption="表 2. コマンドの報告元の構成要素について" caption-side="top"}
{: #postfinding2}
{: tab-title="Reported by"}
{: tab-group="post"}

| 検出事項 | 説明 | 
|:-----------------|:-----------------|
| `severity` | この検出事項が表す緊急性のレベル。 |
| `next_steps` | 問題に対処するために取れる手順。|
| `title` | 検出事項のタイトル。|
{: class="simple-tab-table"}
{: caption="表 3. コマンドの検出事項の構成要素について" caption-side="top"}
{: #postfinding3}
{: tab-title="Finding"}
{: tab-group="post"}


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

### ステップ 3: 表示するカードの定義
{: #custom-define-card}

[注記](https://us-south.secadvisor.cloud.ibm.com/findings/v1/docs/#/Findings_Notes/createNote){: external}を作成して、ダッシュボードで検出事項をカードに表示する方法を定義します。

要求の例: 

```
curl -X POST "https://us-south.secadvisor.cloud.ibm.com/findings/v1/<account_id>/providers/my-custom-tool/notes" -H  "accept: application/json" -H  "Authorization: <IAM_token>" -H  "Content-Type: application/json" -d "{    \"kind\": \"CARD\",        \"provider_id\": \"my-custom-tool\",    \"id\": \"custom-tool-card\",    \"short_description\": \"Security risk found by my custom tool\",        \"long_description\": \"More detailed description about why this security risk needs to be fixed\",    \"reported_by\": {      \"id\": \"my-custom-tool\",      \"title\": \"My security tool\"    },        \"card\": {      \"section\": \"My security tools\",      \"order\": 1 ,     \"title\": \"My security tool findings\",      \"subtitle\": \"My security tool\",      \"finding_note_names\": [        \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"      ],      \"elements\": [        {          \"kind\": \"NUMERIC\",          \"text\": \"Count of findings reported by my security tool\",          \"default_time_range\": \"1d\",          \"value_type\": {            \"kind\": \"FINDING_COUNT\",            \"finding_note_names\": [              \"providers/my-custom-tool/notes/my-custom-tool-findings-type\"            ]          }        }      ]    }  }"
```
{: codeblock}

| 一般 | 説明 | 
|:-----------------|:-----------------|
| `provider_id` | セキュリティー・ツールの ID。|
| `id` | セキュリティー・ツールで検出されたこのタイプの検出事項の ID。|
| `short_description` | 検出事項について要約した簡単な説明 (数ワード)。|
| `long_description` | 検出事項について詳しく述べた長い説明。|
{: class="simple-tab-table"}
{: caption="表 1. コマンドの一般的な構成要素について" caption-side="top"}
{: #definecard1}
{: tab-title="General"}
{: tab-group="card"}

| 報告元 | 説明 | 
|:-----------------|:-----------------|
| `id` | 検出事項を報告したセキュリティー・ツールの ID。 |
| `title` | 検出事項を報告したセキュリティー・ツールのタイトル。|
{: class="simple-tab-table"}
{: caption="表 2. コマンドの報告元の構成要素について" caption-side="top"}
{: #definecard2}
{: tab-title="Reported by"}
{: tab-group="card"}

| カード | 説明 | 
|:-----------------|:-----------------|
| `section` | カードを表示するセクション。カスタム・セクションの数は最大 3 つ、各セクション内のカードは最大 6 つです。最大文字数: 30  |
| オプション: `order` | 指定したセクション内でこのカードを表示する順番。1 から 6 までの範囲で順番を指定します。別のカードに既に適用されている番号を選択すると、作成は失敗します。「指定された順番はセクション内のその他のカードで既に使用されている」というエラー・メッセージを受け取ります。現在のカード数に 1 を加えた数より大きい順番を指定すると、カードの作成は失敗します。例えば、現在のカード数が 2 の場合に、新たに作成するカードの順番として 5 と指定することはできません。カードは合計 3 つであるからです。カードの順番を指定しない場合は、割り当てたセクションにアルファベット順に並べられます。|
| `title` | カードに表示するタイトル。最大文字数: 28  |
| `subtitle` | カードに表示するサブタイトル。最大文字数: 30  |
| `finding_note_names` | `providers//notes/my-custom-tool-findings-type` |
{: class="simple-tab-table"}
{: caption="表 3. コマンドのカードの構成要素について" caption-side="top"}
{: #definecard3}
{: tab-title="Card"}
{: tab-group="card"}

| elements: value_type | 説明 | 
|:-----------------|:-----------------|
| `kind` | `NUMERIC`、`TIME_SERIES`、`BREAKDOWN` のオプションがあります。|
| `text` | 表示するテキスト。 kind が `NUMERIC` の場合、最大文字数は 60 です。kind が `TIME_SERIES` または `BREAKDOWN` の場合、最大文字数は 65 です。|
| `default_time_range` | 確認対象の時間。値は日単位で設定します。現在は `1d`、`2d`、`3d`、`4d` から選択できます。|
| `value_type` | エレメントの種類。kind が `NUMERIC` の場合、このフィールドは `value_type` で、カード 1 つあたりのエレメントは最大 4 つです。kind が `TIME_SERIES` または `BREAKDOWN` の場合、このフィールドは `value_types` です。`TIME_SERIES` または `BREAKDOWN` の最大数は両方とも 1 です。数値項目のみの場合は、カード 1 つあたりのエレメントは最大 4 つです。組み合わせを使用する場合は、最大 2 つの数値項目と 1 つの時系列または明細を使用できます。時系列と明細の両方を同じカード内で使用することはできません。値のタイプを時系列の配列として定義する場合は、項目は最大 3 つまでです。|
| `value_type`: `kind` | 値のタイプ。`KRI` と `FINDING_COUNT` のオプションがあります。|
| `value_type`: `finding_note_names` | `kind` が `FINDING_COUNT` の場合、カードに表示する検出事項の名前。配列として指定します。|
| `value_type`: `kri_note_names` | `kind` が `FINDING_COUNT` の場合、カードに表示する検出事項の名前。配列として指定します。|
| `value_type`: `text` | エレメント・タイプのテキスト。文字の最大数は 22 です。|
{: class="simple-tab-table"}
{: caption="表 4. コマンドのエレメントの構成要素について" caption-side="top"}
{: #definecard4}
{: tab-title="Elements"}
{: tab-group="card"}

応答の例:

```
{
"author": {
  "account_id": "account id",
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
  "order": "1",
  "title": "My Security Tool Findings",
  "subtitle": "My Security Tool",
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


## 使用例
{: #custom-example}

`cloudkingdom` という名前の {{site.data.keyword.containershort_notm}} クラスター上で実行しているアプリケーションがあるとしましょう。 アプリケーションの規模によっては、クラスター内に複数のポッドがあり、それらをすべて同時にモニターする必要がある場合があります。クラスターをモニターしてそれぞれ異なる脅威を検出する、複数のカスタム・ツールがある場合について考えましょう。 クラスター内のポッドの 1 つが異常な量のデータを外部サーバーに送信し始めたら、できるだけ早くそのことを把握したいものです。 データ転送をモニターするカスタム・ツールは、その問題を検出し、{{site.data.keyword.security-advisor_short}} に送信するでしょう。 その問題を検出する別のカスタム統合が存在するなら、そこからも {{site.data.keyword.security-advisor_short}} に検出事項が送信されます。 そして、{{site.data.keyword.security-advisor_short}} は、すべてのモニタリング・ツールから得た検出事項を単一のダッシュボードに表示します。 これによって、ユーザーは、迅速にすべてのアラートの概要を確認し、問題を調査し、修復方法を知ることができます。

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
                        "url":"https://cloud.ibm.com/containers-kubernetes/clusters"
		}],
                "short_description": "One of the pods in your cluster appears to be leaking an excessive amount of data",
                "long_description": "One of the pods in your cluster is approaching external servers and sending them data in volumes that exceed that pod’s normal behavior"
	}
}
```
{: screen}
