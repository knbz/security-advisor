---

copyright:
  years: 2019
lastupdated: "2019-06-06"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# トラブルシューティング
{: #troubleshooting}

{{site.data.keyword.security-advisor_long}} の操作中に問題が生じた場合は、ここに示すトラブルシューティング手法やヘルプの利用手法を検討してください。
{: shortdesc}


## ヘルプおよびサポートの利用
{: #getting-help-and-support}



情報を検索したりフォーラムで質問したりして役に立つ情報を得ることができます。 また、サポート・チケットを開くことができます。 フォーラムを利用して質問するときは、{{site.data.keyword.cloud_notm}} 開発チームの目に留まるように、質問にタグを付けてください。
{: shortdesc}

  * {{site.data.keyword.security-advisor_short}} に関する技術的質問がある場合は、[スタック・オーバーフロー](https://stackoverflow.com/){: external}に質問を投稿してください。必ず、`security-advisor` と `ibm-cloud` のタグを含めてください。

  * サービスに関する質問や入門的な説明については、[dW Answers](https://developer.ibm.com/){: external} フォーラムをご利用ください。必ず、`security-advisor` と `ibm-cloud` のタグを含めてください。


サポートの利用方法について詳しくは、[必要なサポートを利用するには](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)を参照してください。


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
Insights の標準装備オファリングの 1 つをインストールする前に、Helm をインストールする必要があります。 [Kubernetes サービス統合文書](/docs/containers?topic=containers-helm)を使用して Helm をインストールできます。


## 既知の問題: カスタムのソリューション・オカレンスを作成できない
{: #ts-custom-occurrence}

{: tsSymptoms}
カスタムのソリューション・オカレンスを作成してみましたが、ブラウザーに情報が表示されず、エラー・メッセージも表示されません。

{: tsCauses}
使用中の名前を選択したために、オカレンスの作成に失敗しました。

{: tsResolve}
オカレンスに別の名前を選択してください。

## 既知の問題: 直接接続のイメージが更新されない
{: #ts-known-cant-edit}

{: tsSymptoms}
直接接続のイメージをアップロードまたは編集すると、イメージがすぐには表示されません。

{: tsCauses}
これは、イメージが正しく表示されないという既知の問題です。

{: tsResolve}
更新したイメージを表示するには、ブラウザーを更新してください。直接接続のテーブルに新しいイメージが表示されます。

