---

copyright:
  years: 2019
lastupdated: "2019-02-18"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# トラブルシューティング
{: #troubleshooting}

{{site.data.keyword.security-advisor_long}} の操作中に問題が生じた場合は、ここに示すトラブルシューティング手法やヘルプの利用手法を検討してください。
{: shortdesc}


## ヘルプおよびサポートの利用
{: #getting-help-and-support}



情報を検索したりフォーラムで質問したりして役に立つ情報を得ることができます。 また、サポート・チケットを開くことができます。 フォーラムを利用して質問するときは、{{site.data.keyword.Bluemix_notm}} 開発チームの目に留まるように、質問にタグを付けてください。
{: shortdesc}

* {{site.data.keyword.security-advisor_short}} に関する技術的質問がある場合は、<a href="http://stackoverflow.com/" target="_blank">スタック・オーバーフロー <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> に質問を投稿してください。 必ず、`security-advisor` と `ibm-cloud` のタグを含めてください。
* サービスや開始手順についての質問は、<a href="https://developer.ibm.com/" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> フォーラムをご利用ください。 必ず、`security-advisor` と `ibm-cloud` のタグを含めてください。

サポートの利用方法について詳しくは、[必要なサポートを利用するには](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)を参照してください。


## カスタムのソリューション・オカレンスを作成できない
{: #ts-custom-occurrence}

{: tsSymptoms}
カスタムのソリューション・オカレンスを作成してみましたが、ブラウザーに情報が表示されず、エラー・メッセージも表示されません。

{: tsCauses}
これは既知の問題です。 既に存在する名前を選択したために、オカレンスの作成に失敗しています。

{: tsResolve}
オカレンスに別の名前を選択してください。


## エラー: 名前空間 "security-advisor-insights" は禁止されている
{: #ts-error-helm-install}

{: tsSymptoms}
Network Insights または Activity Insights のインストールを試みると、次のエラーが発生します。

```
名前空間“security-advisor-insights”は禁止されています: ユーザー“system:serviceaccount:kube-system:default”は名前空間“security-advisor-insights”の API グループ“”のリソース“namespaces”を取得できません
```
{: screen}

{: tsCauses}
デフォルトのサービス・アカウント `kube-system` に、クラスターでの管理アクセス権がありません。

{: tsResolve}
Insights の標準装備オファリングの 1 つをインストールする前に、Helm をインストールする必要があります。[Kubernetes サービス統合文書](/docs/containers?topic=containers-integrations#helm)を使用して Helm をインストールできます。


## 既知の問題: 一部の Network Insights 検出事項が表示されない
{: #ts-network-analytics}

{: tsSymptoms}
ダッシュボードまたは API を介して Network Insights の検索をするときに、一部の検出事項タイプが表示されません。

{: tsCauses}
Network Insights の検出事項タイプのいくつかは、IBM Cloud Kubernetes サービス・バージョン 1.10 以下でのみ機能します。

{: tsResolve}
Kubernetes サービス・バージョン 1.10 以下を使用してください。
