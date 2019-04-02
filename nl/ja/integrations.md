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


# 統合
{: #integrations}

{{site.data.keyword.security-advisor_long}} では、他の組み込み洞察機能やパートナー・ソリューションを統合したり、独自のカスタム統合を作成したりできます。
{: shortdesc}


## 事前統合済みの検出事項
{: #integrate-built-in}

{{site.data.keyword.security-advisor_long}} を使用すると、組み込みサービス機能により潜在的な問題に関する知見を得ることができます。
{: shortdesc}


{{site.data.keyword.security-advisor_short}} には、以下のものがあらかじめ統合されています。

* 証明書の有効期限とライフサイクルに関連した通知を行う証明書マネージャー。
* イメージの脆弱性と構成の問題に関する詳細を示す脆弱性アドバイザー。

詳細については、[事前統合済みのサービスの活用](/docs/services/security-advisor?topic=security-advisor-setup-services)を確認してください。

</br>

## パートナー統合
{: #integrate-partner}

パートナー統合を利用すると、{{site.data.keyword.security-advisor_short}} と、IBM と連携する他のセキュリティー・ツールとの間に接続を作成して、シームレスなユーザー・エクスペリエンスを実現し、{{site.data.keyword.cloud_notm}} デプロイメントのセキュリティーを強化できます。
{: shortdesc}

現行の {{site.data.keyword.security-advisor_short}} のパートナーとして、Neuvector と Twistlock が挙げられます。

ご自分のソリューションと {{site.data.keyword.security-advisor_short}} との統合に関心があるパートナーですか? wardm@us.ibm.com で Matt Ward と連絡を取ってチームに接触してください。
{: tip}

### NeuVector
{: #integrate-neuvector}

[NeuVector](https://neuvector.com/) は、Kubernetes と Red Hat OpenShift 用の高度に統合された自動化セキュリティー・プラットフォームを提供するので、コンテナー・ネットワーク・トラフィックのモニターや保護を行うことができます。 特に、East-West (端末相互間) ネットワーク・トラフィックが該当します。

NeuVector を使用すると、ネットワークの脅威や違反を検出し、コンテナー・ベースのアプリケーションへの攻撃を防ぐことができます。モニターにより、コンテナーやホスト内で root 権限の昇格、ポート・スキャン、リバース・シェル、不審なファイル・システム・アクティビティーを検出して、悪用や突破を防ぐことができます。

NeuVector インスタンスのセットアップについては、[パートナー・ソリューション](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-neuvector)を参照してください。


### Twistlock
{: #integrate-twistlock}

Twistlock は、脆弱なイメージがお客様の環境にデプロイされないようにすることで、SDLC 全体にわたってリスクを防止する、独自の機能を備えています。自動化されたカスタム・ポリシーの適用によって、アプリケーション・ライフサイクルの全ステージで完全な管理を実現します。Twistlock Intelligence Stream は、脆弱性情報を 30 以上のアップストリーム・プロジェクト、商業ソース、Twistlock Labs の専有リサーチ結果から直接収集して集約します。

お客様のテクノロジー・スタックのあらゆる層に及ぶ詳細データを収集することに注力して、正確な可視性を実現し、誤検出を最小限に抑えます。Twistlock はそれらのデータと、実際のデプロイメントに関する情報 (例えば、どのコンテナーが、インターネット上に公開されているか、高い特権によって実行されているか、他のセキュリティー対策を実装しているか) とを組み合わせて、お客様の特定の環境内で最も重大な脆弱性を常に把握できるようにします。

Twistlock インスタンスのセットアップについては、[パートナー・ソリューション](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-twistlock)を参照してください。
</br>


## カスタム統合
{: #integrate-custom}

既存のセキュリティー・ツールに頼っている場合もあります。 そのツールを {{site.data.keyword.security-advisor_short}} ダッシュボードと統合して、すべてのセキュリティー情報を 1 か所に集中させることができます。
{: shortdesc}

### 直接接続の作成
{: #create-direct-connect}

{{site.data.keyword.security-advisor_short}} GUI を使用して、内外のセキュリティー・ツールにブックマークを付け、{{site.data.keyword.security-advisor_short}} ダッシュボードからすべてのものへの単一アクセス・ポイントを設けることができます。 例えば、PagerDuty にブックマークを付けることもできます。

### 独自のツールと API との統合
{: #integrate-tools-api}

Findings API を使用して、カスタム・セキュリティー・ツールからの検出事項を {{site.data.keyword.security-advisor_short}} ダッシュボードに統合できます。 セキュリティー・アラートや検出事項を生成し、API ベースの操作をサポートしている、あらゆるカスタム・ツールやパートナー・ツールが対象になります。

## 組み込みの洞察機能
{: #integrate-insights}

組み込みの洞察機能を利用すると、お客様のクラスターおよびアカウントのログを継続的にモニターして、潜在的な問題を検出することができます。ネットワーク・トラフィックとユーザー・アクティビティーをモニターすることで、{{site.data.keyword.cloud_notm}} リソースが保護された状態を保つことができます。
{: shortdesc}

**Network Insights (ベータ)**

Network Insights (ベータ) を使用すると、Kubernetes クラスターと外部エンティティーとの間のクラスター・ネットワーク通信 (着信と発信の両方) をモニターし、分析することができます。このサービスは、脅威インテリジェンスと異常検出の統合を利用することで、偵察攻撃や、悪用のおそれのある資産を特定できます。詳しくは、[Network Insights](/docs/services/security-advisor?topic=security-advisor-network) を確認してください。

**Activity Insights (プレビュー)**

Activity Insights (プレビュー) を使用すると、{{site.data.keyword.cloud_notm}} Activity Tracker ログを継続的にモニターし、ルール・パッケージに基づいて、ユーザーまたはアプリによる無許可のアクティビティーまたは不審なアクティビティーを特定することができます。ルール・パッケージについては、このサービスで提供されている、セキュリティー上のベスト・プラクティスに基づいたものを使用することができます。あるいは、お客様のニーズに合ったルールをカスタマイズすることもできます。詳しくは、[Activity Insights](/docs/services/security-advisor?topic=security-advisor-activity) を確認してください。
