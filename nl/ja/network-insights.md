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


# Network Insights (ベータ版)
{: #network}

{{site.data.keyword.security-advisor_long}} を使用すると、{{site.data.keyword.containerlong_notm}}・クラスター上で実行される、危険にさらされている可能性のあるコンテナーに関する洞察を、脅威インテリジェンス、動作パターン、および機械学習に基づいて得ることができます。
{: shortdesc}


## 仕組み
{: #network-how-it-works}

Network Insights 機能は、{{site.data.keyword.security-advisor_short}} サービスへのアドオンです。この機能を有効にして構成すると、クラスターと外部エンティティーの間のクラスター・ネットワーク通信 (着信と発信の両方) が収集されて、継続的にモニターされ、分析されます。

以下の画像を参照して、情報の流れを確認してください。

![Network Insights のフロー・ダイアグラム](images/network-insights-flow.png)

1. アカウント管理者が、モニターする各クラスターに Network Insights をインストールすることができます。
2. ネットワーク・フローのログは Cloud Object Storage バケットに転送され、削除するまでそこに保管されます。{{site.data.keyword.security-advisor_short}} GUI を使用してバケットを作成すると、このサービスでログを参照できるように、サービス間 IAM 役割が割り当てられます。
3. Network Insights が有効になっていれば、COS バケット内の生データが分析され、疑わしい動作がないか確認されます。
4. セキュリティー上の問題の可能性がある事項にフラグが立てられると、その検出事項が Findings データベースに転送されます。
5. 検出事項はサービス・ダッシュボードの次の 3 枚のカードに表示されます。
  * 疑わしいインバウンド・トラフィック (Suspicious inbound traffic)
  * 疑わしいアウトバウンド・トラフィック (Suspicious outbound traffic)
  * トラフィックの異常 (Anomalies in traffic)


## データの収集
{: #network-data}

{{site.data.keyword.security-advisor_short}} には、クラスターと外部エンティティーの間で発生するネットワーク・フローの情報を収集するために使用される収集機能が備えられています。
{: shortdesc}

収集された生データは Cloud Object Storage バケットに保管されます。ここでの保管期間はお客様が決定することができます。収集されたデータは、お客様が所有し、管理します。つまり、それらのデータの保管、保護、削除を行う責任があります。{{site.data.keyword.security-advisor_short}} は検出事項を 90 日間維持します。この期間は、サービス・ダッシュボード内の Network Insights カードに結果が表示されます。したがって、90 日が経過すると、検出事項はダッシュボードに表示されなくなりますが、生データはまだストレージに残っている可能性があります。

以下の情報が収集されます。

* 通信しているピアの IP アドレス
* ピアが使用しているポート
* 使用されているプロトコルのセット
* 転送されたデータ量
* プロトコル固有のパラメーターのセット
* 各種タイム・スタンプ。

**交換される実際のデータは収集されません。**

セキュリティー上の観点では、収集データは、法的または社内の要求事項に照らして削除が可能であれば、消去するのが賢明です。詳しくは、[オブジェクトの削除](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion)を参照してください。
{: tip}


## ネットワーク: 疑わしいインバウンド・トラフィック
{: #network-suspicious-inbound}

外部クライアントによりクラスターの調査や暗号漏えいが試みられた場合は、{{site.data.keyword.security-advisor_short}}により、サービス・ダッシュボードの**「疑わしいインバウンド・トラフィック」**カード上でそれらの試みが通知されます。
{: shortdesc}


暗号通貨のマイニングや匿名化サービスのためにスキャナーとして、またはネットボットの一部として使用されるマルウェアを配布している、と IBM X-Force が分類するクライアントの動作パターンがすべて継続的にモニターされます。そうしたクライアントがモニター対象のクラスターに接近し、警戒すべき動作が見られた場合、Network Insights は検出事項を送出します。


このカードには、以下の 2 つの重要リスク指標 (KRI) が示されます。

* 疑わしいクライアントによる偵察 (Reconnaissance by suspicious clients): クラスターにアクセスするクライアントに関連した検出事項が含まれます。
* 疑わしいクライアントによって送信された異常に大きいペイロード (Abnormally large payloads sent by suspicious clients): クライアントとクラスターの間で送信されたデータのボリュームに関連した検出事項が含まれます。クラスターにそぐわないペイロードはすべて異常なペイロードです。


検出事項には、次のような疑わしいクライアントが含まれる可能性があります。

* クラスターに対して異常に多量のデータを送信する。
* クラスター・サービスに対して異常に多数の要求を実行する。
* クラスターの調査を試みた回数から、クラスターを標的にしていると思われる。



## ネットワーク: 疑わしいアウトバウンド・トラフィック
{: #network-suspicious-outbound}

危険にさらされている可能性があるコンテナーが {{site.data.keyword.containershort_notm}}・クラスターで実行されている場合は、{{site.data.keyword.security-advisor_short}} により、サービス・ダッシュボードの**「疑わしいアウトバウンド・トラフィック」**カードでそのことが通知されます。
{: shortdesc}

暗号通貨のマイニングや匿名化サービスのためにスキャナーとして、またはネットボットの一部として使用されるマルウェアを配布している、と IBM X-Force が分類するクライアントにアクセスするコンテナーの動作パターンが、サービスによって継続的にモニターされます。モニター対象のクラスター上のコンテナーが疑わしいピアに接近し、警戒すべき動作が見られた場合、Network Insights は検出事項を送出します。

このカードには、以下の 2 つの重要リスク指標 (KRI) が示されます。

* 疑わしいサーバーへのアウトバウンド・アプローチ (Outbound approaches to suspicious servers): そうしたサーバーに接近するクラスターに関連した検出事項。
* 疑わしいサーバーと交換された異常に大きいペイロード (Abnormally large payloads exchanged with suspicious servers): クラスターとサーバーの間で送信されたデータのボリュームに関連した検出事項。


検出事項には、次のようなコンテナーが含まれる可能性があります。

* 疑わしいサーバーに対して異常に多量のデータを送信する。
* 疑わしいサーバーから多量のデータを取り出す。
* 疑わしいサーバーに対して多数の要求を実行する。


## ネットワーク: トラフィックの異常 (試験段階)
{: #network-anomalies}

試験段階にあるこの機能では、{{site.data.keyword.security-advisor_short}} がネットワークをモニターして動作パターンを学習します。パターンが形成されると、異常と思われる動作はすべてフラグが立てられて、サービス・ダッシュボードの**「トラフィックの異常」**カードで要約されます。
{: shortdesc}

通常のコンテナー動作のモデルは、コンテナーとそのピアの間の動作パターンをモニターすることによって作成されます。モデルが取り込まれると、特定の変更がないかモニターされます。警戒すべき変化が見られた場合、Network Insights は検出事項を送出します。

このカードには、以下の 2 つの重要リスク指標 (KRI) が示されます。

* 通常よりも頻繁な偵察またはデータ交換アクティビティー (Higher than normal reconnaissance or data exchange activity): クラスターと外部ピアとの間で検出された異常な対話に関連した検出事項。
* 新しいサーバーへのアウトバウンド・アプローチ (Outbound approach to a new server): クラスターが接近する、新しく検出されたサーバーに関連した検出事項。

検出事項には、次のものが含まれる可能性があります。  

* これまで一度も接触したことがないサーバーに接近するコンテナー
* 特定のピアとの間で通常よりかなり多くのデータを送信または消費しているコンテナー
* 特定の事柄の調査の程度の顕著な増大

 モデルが開発された後、学習したモデルからの逸脱が検出され、警戒すべき変化が見られた場合、Network Insights がセキュリティー・アドバイザー・ダッシュボードに検出事項を表示します。検出事項は「ネットワーク: トラフィックの異常 (Network: Anomalies in Traffic)」カードで要約されます。このカードには、以下の 2 つの重要リスク指標 (KRI) が示されます。1 つは「通常よりも頻繁な偵察またはデータ交換アクティビティー (Higher than normal reconnaissance or data exchange activity)」という KRI で、これはクラスターと外部ピアとの間で検出された異常な対話に関連した検出事項をカウントします。もう 1 つは「新しいサーバーへのアウトバウンド・アプローチ (Outbound approach to a new server)」という KRI で、これは新しく検出されたサーバーへのクライアントの接近に関連した検出事項をカウントします。  

## 次のステップ
{: #network-next}

さあ始めましょう。 [Network Insights の使用可能化](/docs/services/security-advisor/setup-network.html)をご覧ください。
