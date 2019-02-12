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

# パートナー・ソリューション
{: #setup-partner}

パートナー・ソリューションを使用すると、{{site.data.keyword.security-advisor_long}} とその他のセキュリティー・ツールとの間にリンクを作成して、セキュリティーを拡張できます。
{: shortdesc}

## 統合ウィザード
{: #wizard}

IBM Cloud とパートナーの両方のアカウントに管理権限がある管理者は、統合ウィザードを使用して素早く稼働を開始できます。このウィザードには以下の 4 つの基本手順があります。

* 信頼を確立し、IBM Cloud アカウントとパートナー・アカウントを関連付ける
* アカウント間で資格情報や URL などの必須情報をコピーする
* パートナーの検出事項メタデータを {{site.data.keyword.security-advisor_short}} 内にインストールする
* パートナーからの検出事項を {{site.data.keyword.security-advisor_short}} に送付して、ペア化を検証する

</br>

## NeuVector の統合
{: #neuvector}

**始める前に**

* 統合しようとしているパートナーのアカウントがあることを確認してください。
* パートナー・サービスの統合 URL の生成に必要な管理許可があることを確認してください。
* IBM Cloud への IAM 管理アクセス権限と {{site.data.keyword.security-advisor_short}} への管理者アクセス権限があることを確認してください。

**統合**

1. アカウントを接続します。
  1. 接続の名前を指定します。
  2. NeuVector ダッシュボード URL を指定します。
  3. NeuVector 構成 URL を指定します。
    1. NeuVector にログインして設定にナビゲートします。
    2. **「URL の生成 (Generate URL)」**をクリックします。
    3. URL を {{site.data.keyword.security-advisor_short}} 統合ウィザードにコピー・アンド・ペーストします。
  4. **「次へ」**をクリックします。
3. サービスがサービス ID と API 鍵を生成するための許可を付与していることを確認し、**「次へ」**をクリックしてそのサービスとの接続を作成します。
4. **「完了」**をクリックします。
5. サービス・ダッシュボードに進み、NeuVector により {{site.data.keyword.containershort_notm}} に送信されたテストの検出結果を確認します。
