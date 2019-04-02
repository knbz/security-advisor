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

# Activity Insights (プレビュー)
{: #activity}

{{site.data.keyword.security-advisor_long}} で {{site.data.keyword.cloud_notm}} Activity Tracker を使用すると、お客様の {{site.data.keyword.Bluemix_notm}} アカウントにおける不審なユーザー・アクティビティーを検出できます。
{: shortdesc}


## 仕組み
{: #activity-how}

Activity Insights フィーチャーは、{{site.data.keyword.security-advisor_short}} サービスに対するアドオンです。このフィーチャーを有効にして構成すると、ユーザーの行動がログに記録されて分析され、ルールに基づいて不審なアクティビティーが特定されます。デフォルトのルールを使用するか、お客様の組織に合ったカスタム・ルールを作成することができます。

以下の画像を参照して、情報の流れを確認してください。

![Activity Insights フロー・ダイアグラム](images/activity-insights-flow.png)

1. アカウント管理者は、Activity Insights をクラスターにインストールします。
2. このアドオンが 1 つのクラスターにインストールされていれば、アカウント全体の Activity Tracker ログをモニターできます。
3. アクティビティー・ログは、Cloud Object Storage バケットに転送されます。それらのログは、削除を決定するまで、そのバケットで保管されます。{{site.data.keyword.security-advisor_short}} GUI を使用してバケットを作成すると、このサービスでログを参照できるように、サービス間 IAM 役割が割り当てられます。
4. Activity Insights が有効であれば、COS バケット内の生データが、ルール (このサービスで事前定義されたルールか、お客様がカスタマイズしたルール) に基づいて分析されます。
5. セキュリティー上の問題の可能性がある事項にフラグが立てられると、その検出事項が Findings データベースに転送されます。
6. 検出事項は、サービス・ダッシュボードの **Activity Insights** カードに表示されます。

</br>

## データの収集
{: #activity-data}

Activity Tracker は、{{site.data.keyword.cloud_notm}} API に対するユーザーの操作を示すイベントを収集します。その後、ログを Object Storage バケットに保管して、さらに分析することができます。
{: shortdesc}

Activity Tracker は、{{site.data.keyword.cloud_notm}} API に対するユーザーの操作を示すイベントを収集します。

以下の情報を収集します。

* API 呼び出しのイニシエーターの IP アドレス
* 認証されたユーザー
* アクティビティー・タイプ
* アクティビティーの結果
* その他

収集された生データは Cloud Object Storage バケットに保管されます。ここでの保管期間はお客様が決定することができます。収集されたデータは、お客様が所有し、管理します。つまり、それらのデータの保管、保護、削除を行う責任があります。{{site.data.keyword.security-advisor_short}} は検出事項を 90 日間維持します。その期間は、サービス・ダッシュボード内の **Activity Insights** カードに結果が表示されます。したがって、90 日が経過すると、検出事項はダッシュボードに表示されなくなりますが、生データはまだストレージに残っている可能性があります。

一般に、セキュリティー上の観点では、収集データは、法的または社内の要求事項に照らして削除が可能であれば、消去するのが賢明です。詳しくは、[オブジェクトの削除](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion)を参照してください。
{: tip}

## Activity Insights: アクセス
{: #ai-access}

サービス・ダッシュボード内の Activity Insights カードには、ユーザーおよびサービスによる、予期しないまたは警戒すべきアカウント・アクティビティーの兆候の要約が示されます。通常とは異なるアクティビティーは、正当なユーザーおよびサービスによる誤操作である場合もあれば、アカウントの悪用を示している場合もあります。カードに表示される検出事項は、{{site.data.keyword.security-advisor_short}} に用意されているデフォルトのルール・パッケージに基づいて決まります。

このカードには、以下の 2 つの重要リスク指標 (KRI) が示されます。

* ID およびアクセス: Identity and Access Management (IAM) または App ID サービスに関連した検出事項。
* データおよび Kubernetes: Key Protect、Kubernetes Service、Cloud Object Storage、または Certificate Manager に関連した検出事項。


## ルール・パッケージについて
{: #activity-packages}

アカウント管理者は、ルール・パッケージを活用するとアカウントのモニタリングを素早く開始できます。
{: shortdesc}

このサービスは、以下を含む複数のサービスと関連付けられたルール・パッケージを提供しています。

* {{site.data.keyword.containerlong_notm}}
* {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM)
* {{site.data.keyword.cloudcerts_long_notm}}
* {{site.data.keyword.appid_long_notm}}
* {{site.data.keyword.keymanagementservicelong_notm}}
* {{site.data.keyword.cos_full_notm}} (COS)

現行のパッケージでは満たせないニーズがある場合はいつでも、既存のパッケージを更新するか、新規パッケージを作成して、組織に合ったルールを用意することができます。

### ルールとは
{: #ai-rule}

ルールは、条件と単一イベントの組み合わせです。ルール、またはルールの組み合わせを使用することで、{{site.data.keyword.security-advisor_short}} ダッシュボードに表示可能な検出事項をトリガーすることができます。

例:

```
	{
		"comment": "Dormant Rule: Very high risk App ID activity... ",
		"dormant": true,
		"conditions": { 	… },
		"event": { … }
		"type": "aggregate"
	}
```
{: screen}

ルールには、条件とイベントに加えて、以下の表に示すフィールドを含めることもできます。

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="電球アイコン"/> ルールの構成要素について</th>
	</tr>
	<tr>
		<td><code>comment</code></td>
		<td>常に無視されます。</td>
	</tr>
	<tr>
		<td><code>dormant</code></td>
		<td>ブール値フィールド。true の場合、無視されます。値が false または未定義の場合、ルールが使用されます。</td>
	</tr>
	<tr>
		<td><code>type</code></td>
		<td><code>aggregate</code>、<code>coincident</code>、<code>boolean</code> のオプションがあります。type に <code>aggregate</code> も <code>coincident</code> も割り当てられていない場合、<code>boolean</code> として評価されます。</td>
	</tr>
</table>

</br>

### 条件とは
{: #ai-condition}

基本となる条件 1 つは、3 つの要素から成る 1 つのビルディング・ブロックに相当します。これらのブロックは、`any` 演算子および `all` 演算子によって結び付けられます。ブロックをネストすることもできます。`all` 演算子は `and` と同等で、`any` は `or` と同等です。

例:

```
	"conditions": {
		"all": [{
			"any": [{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.group.delete"
			},
			{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.member.delete"
			}]
		}
	}
```
{: screen}

<table>
	<tr>
		<th colspan=2><img src="images/idea.png" alt="電球アイコン"/> 条件の構成要素について</th>
	</tr>
	<tr>
		<td><code>fact</code></td>
		<td>検査される Activity Tracker CADF イベント。</td>
	</tr>
	<tr>
		<td><code>operator</code></td>
		<td><code>equal</code>、<code>notEqual</code>、<code>lessThan</code>、<code>greaterThan</code>、<code>in</code>、<code>notIn</code> のオプションがあります。</td>
	</tr>
	<tr>
		<td><code>value</code></td>
		<td>アクションを定義する手段。value は通常、サービスとやり取りするために使用できる API 呼び出しに相当します。</td>
	</tr>
</table>

</br>

### イベントとは
{: #ai-event}

イベントは、`type` および `params.findingType` という 2 つのフィールドで構成されます。前者はルールの固有 ID で、`params.findingType` はサービスに送信される検出事項の名前です。検出事項の名前により、{{site.data.keyword.security-advisor_short}} ダッシュボードに検出事項を表示することが可能になります。

例:

```
	{
		"conditions": { 	… },
		"event": {
			"type": "IKS high risk API",
			"params": {"findingType": "IKS-high-risk"}
		}
	}
```
{: screen}


### ルール・タイプ: aggregate
{: #rule-aggregate}

aggregate ルール・タイプでは、特定の時間フレーム内における、あるアクションの発生回数がカウントされます。そして、定義されたしきい値を超えると、検出事項がトリガーされます。このルールは、しきい値と時間枠をブール条件に追加して定義します。ルールを定義するには、以下の複数の条件を満たす必要があります。

* ルール・タイプは `aggregate` でなければなりません。
* ルート条件に、以下の fact が含まれていなければなりません。

	```
	{
			"fact": "occurrences",
			"operator": [equal | greaterThan | greaterThanInclusive],
			"value": [positive number]
	},
	{
	    "fact": "withInLast",
	    "operator": "equal",
	    "value": "X [minutes|hours]",
	}
	```
	{: screen}

	説明
	* X = ゼロでない正整数
	* hours を選択した場合、最大値は 24
	* minutes を選択した場合、最大値は 1440

**例**

以下の例は、30 分以内に失敗した試行を 5 回カウントするルールを示しています。

```
{
    "conditions": {
        "all": [
            {
                "fact": "action",
                "operator": "equal",
                "value": "iam-identity.user-apikey.login"
            },
            {
                "fact": "reason",
                "operator": "equal",
                "value": 400,
                "path": ".reasonCode"
            },
            {
                "fact": "occurrences",
                "operator": "equal",
                "value": 5
            },
            {
                "fact": "withInLast",
                "operator": "equal",
                "value": "30 minutes",
            }
        ]
    },
    "event": {
        "type": "failed-login-attempts",
        "params": {
            "findingType": "failed-login-attempts",
        }
    },
    "type" : "aggregate"
}
```
{: screen}

### ルール・タイプ: coincident
{: #rule-coincident}

coincident ルール・タイプは、アクションをモニターして、ある時間枠内における同じアクションの発生回数を判定します。このルールは、基本的な条件のビルディング・ブロックのグループに時間枠を追加して定義します。coincident ルールでアクションの発生順序は意味を持ちませんが、このルールを定義するには、以下の複数の条件を満たす必要があります。

* ルール・タイプは `coincident` でなければなりません。
* ルート条件の種類は `all` でなければなりません。
* ルート条件に、以下の fact が含まれていなければなりません。

	```
	{
	    "fact": "actions",
	    "operator": "contains",
	    "value": [action value]
	},
	{
	    "fact": "withInLast",
	    "operator": "equal",
	    "value": "X [minutes|hours]",
	}
	```
	{: screen}

	説明
	* `fact` 値は複数でなければなりません。つまり、`action` ではなく、`actions` でなければなりません。
	* X = ゼロでない正整数
	* hours を選択した場合、最大値は 24
	* minutes を選択した場合、最大値は 1440


**例**

以下の例は、3 つの特定のアクションすべてが同時期 (30 分以内) に発生するという出来事を監視するルールを示しています。

```
{
    "conditions": {
        "all": [
            {
                "fact": "actions",
                "operator": "contains",
                "value": "iam-identity.user-apikey.login"
            },
            {
                "fact": "actions",
                "operator": "contains",
                "value": "kms.secrets.list"
            },
            {
                "fact": "actions",
                "operator": "contains",
                "value": "kms.secrets.read"
            },
            {
                "fact": "withInLast",
                "operator": "equal",
                "value": "30 minutes",
            }
        ]
    },
    "event": {
        "type": "key-protect-keys-read",
        "params": {
            "findingType": "key-protect-keys-read",
        }
    },
    "type" : "coincident"
}
```
{: screen}

### ルール・タイプ: boolean
{: #rule-boolean}

boolean ルールは、ブール条件とイベントで構成されます。boolean ルールは、危険性の高い API の使用、つまり、変更管理時間枠外での API の使用や、ホワイトリストにないイニシエーターによる API の使用をモニターするためによく使用されます。

ルールが `aggregate` としても `coincident` としても定義されていない場合、`boolean` ルールとして評価されます。

**例**

以下の例は、ホワイトリストにないユーザーによる、変更管理時間枠外のポリシーの削除を監視するルールを示しています。

```
{
	"conditions": {
		"all": [{
			"any": [{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.group.delete"
			},
			{
				"fact": "action",
				"operator": "equal",
				"value": "iam-groups.member.delete"
			}]
		},
		{
			"any": [{
				"fact": "event_time",
				"operator": "lessThan",
				"value": "0800"
			},
			{
				"fact": "event_time",
				"operator": "greaterThan",
				"value": "1700"
			}
			]
		},
		{
			"fact": "initiator",
			"path": ".name",
			"operator": "notIn",
			"value": ["example@ibm.com", "example2@ibm.com"]
		}
		]
	},
	"event": {
		"type": "account-delete",
		"params": {
			"findingType": "iam-delete-account-threshold"
		}
	}
```
{: screen}

boolean ルールについて詳しくは、<a href="https://github.com/CacheControl/json-rules-engine/blob/master/docs/rules.md" target="_blank">CacheControl の資料 <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> を確認してください。
{: tip}

## 次のステップ
{: #activity-next}

さあ始めましょう。 [Activity Insights の有効化](/docs/services/security-advisor?topic=security-advisor-setup-activity)を確認してください。
