---

copyright:
  years: 2018
lastupdated: "2018-11-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
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



情報を検索したりフォーラムで質問したりして役に立つ情報を得ることができます。また、サポート・チケットを開くことができます。 フォーラムを利用して質問するときは、{{site.data.keyword.Bluemix_notm}} 開発チームの目に留まるように、質問にタグを付けてください。
{: shortdesc}

* {{site.data.keyword.security-advisor_short}} に関する技術的質問がある場合は、<a href="http://stackoverflow.com/search?q=ibm+" target="_blank">スタック・オーバーフロー <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> に質問を投稿してください。必ず、`security-advisor` と `ibm-cloud` のタグを含めてください。
* サービスや開始手順についての質問は、<a href="https://developer.ibm.com/answers/search.html?f=&type=question&redirect=search%2Fsearch&sort=relevance&q=appid%20[bluemix]" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="外部リンク・アイコン"></a> フォーラムをご利用ください。 必ず、`security-advisor` と `ibm-cloud` のタグを含めてください。

サポートの利用方法について詳しくは、[必要なサポートを利用するには](/docs/get-support/howtogetsupport.html#getting-customer-support)を参照してください。

</br>

## カスタムのソリューション・オカレンスを作成できない
{: #custom-occurrence}

{: tsSymptoms}
カスタムのソリューション・オカレンスを作成してみましたが、ブラウザーに情報が表示されず、エラー・メッセージも表示されません。

{: tsCauses}
これは既知の問題です。既に存在する名前を選択したために、オカレンスの作成に失敗しています。

{: tsResolve}
オカレンスに別の名前を選択してください。

</br>

## KPI が表示されない
{: #kpi-missing}

{: tsSymptoms}
**「不審なサーバー IP (Suspicious server IPs)」**カードに KPI が表示されません。

{: tsCauses}
これは既知の問題です。

{: tsResolve}
この問題は解決できません。

</br>

## 最新の詳細情報が表示されない
{: #latest-details}

{: tsSymptoms}
**「脆弱性のあるイメージ (Images with vulnerabilities)」**カードに最新の詳細情報が表示されません。

{: tsCauses}
これは既知の問題です。時折、ページの初回ロード時に情報が更新されないことがあります。

{: tsResolve}
この問題を解決するには、「リフレッシュ」アイコンをクリックしてください。
