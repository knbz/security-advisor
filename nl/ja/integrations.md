---

copyright:
  years: 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# 統合
{: #about}

{{site.data.keyword.security-advisor_long}} を使用して、IBM Cloud サービスまたはパートナー・ソリューションからの検出事項を統合したり、独自のカスタム統合を作成したりできます。
{: shortdesc}


## 事前統合済みの検出事項
{: #built-in}

{{site.data.keyword.security-advisor_long}} を使用すると、組み込みサービス機能により潜在的な問題に関する知見を得ることができます。
{: shortdesc}


すぐに使用可能な {{site.data.keyword.security-advisor_short}} は、以下のものと統合します。

* 証明書の有効期限とライフサイクルに関連した通知を行う証明書マネージャー。
* イメージの脆弱性と構成の問題に関する詳細を示す脆弱性アドバイザー。

詳細については、[事前統合済みのサービスの活用](setup.html)を確認してください。

</br>

## パートナー・ソリューション
{: #partner}

パートナー・ソリューションを使用すると、{{site.data.keyword.security-advisor_short}} と IBM Cloud 外の他のセキュリティー・ツールとの間にリンクを作成して、IBM Cloud デプロイメントに関するセキュリティーを強化できます。
{: shortdesc}

現在、{{site.data.keyword.security-advisor_short}} は以下の会社とのパートナーになっています。

### NeuVector
{: #neuvector}

[NeuVector](https://neuvector.com/) は、Kubernetes と Red Hat OpenShift 用の高度に統合された自動化セキュリティー・プラットフォームを提供するので、コンテナー・ネットワーク・トラフィックのモニターや保護を行うことができます。特に、East-West (端末相互間) ネットワーク・トラフィックが該当します。

NeuVector を使用すると、ネットワークの脅威や違反を検出し、コンテナー・ベースのアプリケーションの攻撃を防ぐことができます。モニターにより、コンテナーやホスト内で root 権限の昇格、ポート・スキャン、リバース・シェル、不審なファイル・システム・アクティビティーを検出して、悪用や突破を防ぐことができます。

統合ウィザードを使用して NeuVector インスタンスをセットアップする方法の詳細については、[パートナー・ソリューション](partners.html)を参照してください。

ご自分のソリューションと {{site.data.keyword.security-advisor_short}} との統合に関心があるパートナーですか? wardm@us.ibm.com で Matt Ward と連絡を取ってチームに接触してください。
{: tip}

</br>

## カスタム統合
{: #custom}

既存のセキュリティー・ツールに頼っている場合もあります。そのツールを {{site.data.keyword.security-advisor_short}} ダッシュボードと統合して、すべてのセキュリティー情報を 1 か所に集中させることができます。
{: shortdesc}

**独自のツールと GUI との統合**

{{site.data.keyword.security-advisor_short}} GUI を使用して、内外のセキュリティー・ツールにブックマークを付け、{{site.data.keyword.security-advisor_short}} ダッシュボードからすべてのものへの単一アクセス・ポイントを設けることができます。例えば、PagerDuty にブックマークを付けることもできます。

**独自のツールと API との統合**

Findings API を使用して、カスタム・セキュリティー・ツールからの検出事項を {{site.data.keyword.security-advisor_short}} ダッシュボードに統合できます。セキュリティー・アラートや検出事項を生成するカスタム・ツールやパートナー・ツールで、API ベースの対話もサポートするものがこれに該当します。

**開始**

API を活用する方法や GUI でブックマークを作成する方法のお勧めについて詳しくは、[カスタム・セキュリティー・ツール](/docs/services/security-advisor/custom.html)を参照してください。

</br>


## ネットワーク分析 (プレビュー)
{: #network-analytics}

プレビュー版のネットワーク分析を使用すると、{{site.data.keyword.containerlong_notm}} クラスター通信をモニターして脅威を検出できます。
{: shortdesc}

クラスターとの間の不審な通信を対象にクラスター通信をモニターすると、IBM X-Force で提供されている脅威インテリジェンスを活用して潜在的な問題にフラグを立てることができます。不審な通信が特定されると検出事項が発行されるので、確認したり評価したりできます。

ネットワーク分析に関する詳細情報については、[ネットワーク分析](network-analytics.html)を参照してください。
{: tip}

</br>
</br>
