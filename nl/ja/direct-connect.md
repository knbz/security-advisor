---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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


# 直接接続
{: #setup-custom-gui}

{{site.data.keyword.security-advisor_short}} には、既存のカスタム・セキュリティー・ツールを統合できます。オープン・ソースのサービス、カスタム開発したサービス、サード・パーティーのサービス、どのようなサービスでも統合できます。{{site.data.keyword.security-advisor_short}} から他のツールへの直接接続を作成するには、URL を統合リストに追加します。 リンクを作成することで、使用しているすべてのツールを簡単にモニターできます。
{: shortdesc}


## 始めに
{: #custom-before-gui}

統合を追加するには、その前に、統合しようとしているパートナーのアカウントをまず用意する必要があります。

{{site.data.keyword.security-advisor_short}} は、パートナー・サービスに関連した資格情報を永続化しません。 エンタープライズ・ユーザーが、{{site.data.keyword.cloud_notm}} とビジネス・パートナーの両方に対して SAML を使用して認証しなければなりません。
{: note}

## 接続の構成
{: #custom-configure-connection}

1. セキュリティー・ツールにログインし、固有の URL を取得します。

2. コンソールを使用して {{site.data.keyword.cloud_notm}} にログインします。

3. **「カスタム統合 (Custom Integrations)」>「直接接続 (Direct Connection)」**をクリックします。 画面が表示されます。

  1. ソリューションに名前を付けます。 英数字、空白文字、ダッシュ (-) のみを名前で使用できます。

  2. `www.<website>.<domain>` の形式で、ソリューションの URL を入力します。

  3. ツールを表わすアイコンまたはイメージをアップロードします。

  4. **「接続」**をクリックして、構成を完了します。 {{site.data.keyword.security-advisor_short}} は、サービス ID、API 鍵、アカウント ID、メタデータなどの、統合に必要な成果物を作成します。 `writer` 役割が割り当てられます。
