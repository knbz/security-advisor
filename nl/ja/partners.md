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


# パートナー
{: #setup-partner}

IBM ビジネス・パートナー統合は、サード・パーティー・プロバイダーからの重要なアラートと検出事項を {{site.data.keyword.security-advisor_long}} ダッシュボードに取り込む手段となります。それらのパートナーと IBM は連携してお客様のための統合エクスペリエンスを構築し、シンプルにして、その方向性を決めています。
{: shortdesc}

## 始めに
{: #partner-before}

パートナーの統合を開始する前に、以下の前提条件を必ず満たしてください。

* 統合しようとしているパートナーのアカウントがあることを確認してください。
* パートナー・サービスの統合 URL の生成に必要な管理権限があることを確認してください。
* {{site.data.keyword.cloud_notm}} に対する IAM 管理アクセス権限と、{{site.data.keyword.security-advisor_short}} に対する管理者アクセス権限があることを確認してください。

## 統合ウィザード
{: #wizard}

{{site.data.keyword.cloud_notm}} とパートナーの両方のアカウントに管理権限がある管理者は、統合ウィザードを使用して素早く稼働を開始できます。このウィザードには以下の 4 つの基本手順があります。

* 信頼を確立し、{{site.data.keyword.cloud_notm}} アカウントとパートナー・アカウントを関連付ける
* アカウント間で資格情報や URL などの必須情報をコピーする
* パートナーの検出事項メタデータを {{site.data.keyword.security-advisor_short}} 内にインストールする
* パートナーからの検出事項を {{site.data.keyword.security-advisor_short}} に送付して、ペア化を検証する


## NeuVector の統合
{: #setup-neuvector}

NeuVector を使用すると、ネットワークの脅威や違反を検出し、コンテナー・ベースのアプリケーションの攻撃を防ぐことができます。 モニターにより、コンテナーやホスト内での root 権限の昇格、ポート・スキャン、リバース・シェル、不審なファイル・システム・アクティビティーを検出して、悪用や突破を防ぐことができます。
{: shortdesc}

NeuVector を {{site.data.keyword.security-advisor_short}} に統合するには、以下の手順に従います。

1. **「統合」>「パートナー統合」**とナビゲートし、表示されたオプションから**「NeuVector」**を選択します。
2. アカウントを接続します。
  1. 接続の名前を指定します。
  2. NeuVector ダッシュボード URL を指定します。
  3. NeuVector 構成 URL を指定します。
    1. NeuVector にログインして設定にナビゲートします。
    2. **「URL の生成 (Generate URL)」**をクリックします。
    3. URL を {{site.data.keyword.security-advisor_short}} 統合ウィザードにコピー・アンド・ペーストします。
  4. **「次へ」**をクリックします。
3. サービスがサービス ID と API 鍵を生成するための許可を付与していることを確認し、**「次へ」**をクリックしてそのサービスとの接続を作成します。
4. **「完了」**をクリックします。
5. サービス・ダッシュボードに進み、NeuVector により {{site.data.keyword.containershort_notm}} に送信されたテストの検出事項を確認します。



## Twistlock の統合
{: #setup-twistlock}

脆弱なイメージがご使用の環境にデプロイされないようにして、リスクを回避することができます。Twistlock は、自動化されたカスタム・ポリシーの適用によって、アプリケーション・ライフサイクルの全ステージで完全な管理を実現します。
{: shortdesc}

パートナー接続を構成すると、Twistlock からの検出事項が要約された 2 つのカードがダッシュボードに作成されます。

**「Twistlock ランタイム (Twistlock Runtime)」** には、以下の 2 つの重要リスク指標 (KRI) が示されます。

* インシデントと監査の合計数 (Total incidents and audits): クラウド・ネイティブのワークロードにおけるインシデントまたは監査に関連した検出事項。
* ファイアウォール監査の合計数 (Total firewall audits): ファイアウォールの問題に関連した検出事項。

**「Twistlock 脆弱性 (Twistlock vulnerabilities)」**には、以下の 1 つの KRI が示されます。

* 新規脆弱性があるイメージの合計数 (Total images with new vulnerabilities): コンテナー・イメージで検出された脆弱性に関連した検出事項。

この企業について詳しくは、Twistlock の資料を参照してください。

### Twistlock の構成
{: #configure-twistlock}

1. {{site.data.keyword.security-advisor_short}} ダッシュボードで、**「統合」>「パートナー統合」**とナビゲートし、表示されたオプションから**「Twistlock」**を選択します。

2. **「はい、マイ・アカウントを {{site.data.keyword.security-advisor_short}} に接続します」**をクリックします。

  アカウントをまだお持ちでない場合は、**「いいえ、今アカウントを作成します」>「アカウントの作成」**をクリックします。お客様の情報を入力してください。Twistlock の営業チーム担当者がお客様に連絡して、開始できるようにします。
  {: note}

3. 接続に名前を付けます。お客様のアカウントに固有の名前を付けてください。また、英数字、スペース、またはハイフンのみを使用してください。

4. オプション: 構成 URL がまだない場合は、**「登録 URL の生成」**をクリックして Twistlock の資料に移動し、URL の作成方法を確認します。その資料にアクセスするための、お客様のライセンスに付属する Twistlock トークンがあることを確認してください。

5. Twistlock ダッシュボードで、**「管理 (Manage)」>「アラート (Alerts)」**タブとナビゲートし、**「プロファイルの追加 (Add profile)」**をクリックします。

6. **「プロバイダー (Provider)」**ドロップダウンから**「{{site.data.keyword.security-advisor_short}}」**を選択します。

7. 指定された構成 URL を使用するために**「コピー (Copy)」**をクリックします。

8. {{site.data.keyword.security-advisor_short}} ダッシュボードに戻り、この URL を**「Twistlock 構成 URL の入力 (Enter the Twistlock configuration URL)」**ボックスに貼り付けます。

9. **「パートナーの接続」**をクリックします。

### 接続の検証
{: #twistlock-verify}

1. {{site.data.keyword.security-advisor_short}} ダッシュボードで、Twistlock カードが適切に表示されているか確認します。

2. Twistlock ダッシュボードで、**「アラート (Alerts)」**タブを最新表示します。{{site.data.keyword.security-advisor_short}} 接続が表示されます。その接続を開いてください。

3. 通知させるアラート・タイプがチェックされていることを確認してから、**「検証 (Verify)」**をクリックし、接続が完了したことを確認します。
