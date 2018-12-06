---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-23"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# サービス・アクセスの管理
{: #service-access}

アカウント所有者は、{{site.data.keyword.Bluemix_notm}} の ID およびアクセス管理 (IAM) を使用して、{{site.data.keyword.security-advisor_long}} のインスタンスへのアクセスを管理できます。ユーザーに応じたアクセス・レベルになるようにアカウント内にポリシーを設定することで、{{site.data.keyword.security-advisor_short}} の各インスタンスを保護できます。
{: shortdesc}

IAM について詳しくは、[IAM のアクセス権限](/docs/iam/users_roles.html)を参照してください。

## {{site.data.keyword.security-advisor_short}} アクセス・ポリシー
{: #access}

アカウント内の {{site.data.keyword.security-advisor_short}} のサービス・インスタンスにアクセスする各ユーザーに、IAM ユーザー役割を定義したアクセス・ポリシーを割り当てる必要があります。このポリシーにより、その特定のサービス・インスタンスのコンテキストでユーザーが実行できるアクションが決まります。
{: shortdesc}

アクションは、{{site.data.keyword.Bluemix_notm}} サービスで実行が許可された操作として、そのサービスでカスタマイズおよび定義されます。 そして、アクションは IAM サービスのユーザー役割にマップされます。以下の表では、{{site.data.keyword.security-advisor_short}} のアクションと必要な許可がマップされています。

<table><caption>どの役割がどのアクションを実行できるか?</caption>
  <col width="43%">
  <col width="42%">
  <col width="5%">
  <col width="5%">
  <col width="5%">
  <tr>
    <th>アクション</th>
    <th>説明</th>
    <th>リーダー</th>
    <th>ライター</th>
    <th>管理者</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>セキュリティー・レポートの検出事項を表示する。</td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>セキュリティー・レポートの検出事項を生成する。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>セキュリティー・レポートの検出事項を削除する。</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>セキュリティー・レポートの検出事項を編集する。</td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>メタデータを表示する。</td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>メタデータを削除する。</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>メタデータを生成する。</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>メタデータを更新する。</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>サービスに追加されたカスタム・ソリューションを読み取る。</td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>サービスにカスタム・ソリューションを追加する。</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>サービス内の既存のカスタム・ソリューションを更新する。</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>サービスからカスタム・ソリューションを削除します。</td>
    <td> </td>
    <td> </td>
    <td><img src="images/confirm.png" width="32" alt="フィーチャーを使用可能" style="width:32px;" /></td>
  </tr>
</table>

## API マッピング
{: #api-map}

これらの役割と API はどのように対応するのでしょうか? 以下の表で、サービス・アクションと API の対応関係を確認してください。


| 方法 | エンドポイント                                                                  |  サービス・アクション                  |
|--------|---------------------------------------------------------------------------|----------------------------------|
| POST   | /v1/{account_id}/graph                                                    | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.delete |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}/note | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}/occurrences      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.delete |
