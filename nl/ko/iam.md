---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

{:new_window: target="_blank"}
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



# 서비스 액세스 관리
{: #service-access}

계정 소유자로서 사용자는 {{site.data.keyword.cloud_notm}} Identity and Access Management(IAM)를 사용하여 {{site.data.keyword.security-advisor_long}}의 인스턴에 대한 액세스를 관리할 수 있습니다. 다른 사용자마다 다른 액세스 레벨을 작성하는 계정 내에 정책을 설정하여 {{site.data.keyword.security-advisor_short}}의 각 인스턴스가 보안 설정되었는지 확인할 수 있습니다.
{: shortdesc}

IAM에 대한 자세한 정보는 [IAM 액세스](/docs/iam?topic=iam-userroles)를 참조하십시오.

## {{site.data.keyword.security-advisor_short}} 액세스 정책
{: #access}

계정에서 {{site.data.keyword.security-advisor_short}} 서비스의 인스턴스에 액세스하는 모든 사용자에게 IAM 사용자 역할이 정의된 액세스 정책이 지정되어야 합니다. 정책은 사용자가 해당 특정 서비스 인스턴스의 컨텍스트 내에서 수행할 수 있는 조치를 결정합니다.
{: shortdesc}

조치는 서비스에서 수행될 수 있는 오퍼레이션으로서 {{site.data.keyword.cloud_notm}} 서비스를 통해 사용자 정의 및 정의됩니다. 그런 다음 조치는 IAM 서비스 사용자 역할에 맵핑됩니다. 다음 표에서 조치 및 {{site.data.keyword.security-advisor_short}}에 필요한 권한이 맵핑됩니다.

<table><caption>어떤 역할이 어떤 조치를 수행할 수 있습니까?</caption>
  <col width="40%">
  <col width="40%">
  <col width="20%">
  <tr>
    <th>조치</th>
    <th>설명</th>
    <th>역할</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>보안 보고서 찾은 결과를 봅니다.</td>
    <td>독자</br>작성자</br>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>보안 보고서 찾은 결과를 생성합니다.</td>
    <td>작성자</br>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>보안 보고서 찾은 결과를 삭제합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>보안 보고서 찾은 결과를 편집합니다.</td>
    <td>작성자</br>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>메타데이터를 봅니다.</td>
    <td>독자</br>작성자</br>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>메타데이터를 삭제합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>메타데이터를 생성합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>메타데이터를 업데이트합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>서비스에 추가된 사용자 정의 솔루션을 읽습니다.</td>
    <td>독자</br>작성자</br>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>서비스에 사용자 정의 솔루션을 추가합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>서비스 내 기존 사용자 정의 솔루션을 업데이트합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>서비스에서 사용자 정의 솔루션을 삭제합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>서비스 인스턴스에 추가한 파트너 솔루션을 확인합니다.</td>
    <td>독자</br>작성자</br>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>서비스 인스턴스에 파트너 솔루션을 추가합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>서비스 인스턴스에서 파트너 솔루션을 업데이트합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>서비스 인스턴스에서 파트너 솔루션을 삭제합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.enable</code></td>
    <td>서비스에서 제공하는 네트워크 인사이트를 사용으로 설정합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.network-insights.disable</code></td>
    <td>서비스에서 제공하는 네트워크 인사이트를 사용 안함으로 설정합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.enable</code></td>
    <td>서비스에서 제공하는 활동 인사이트를 사용으로 설정합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.activity-insights.disable</code></td>
    <td>서비스에서 제공하는 활동 인사이트를 사용 안함으로 설정합니다.</td>
    <td>관리자</td>
  </tr>
  <tr>
    <td><code>security-advisor.insights-cos.create</code></td>
    <td>Network 및 Activity Insights의 {{site.data.keyword.security-advisor_short}}를 통해 Cloud Object Storage 인스턴스를 작성합니다.</td>
    <td>관리자</td>
  </tr>
</table>

## API 맵핑
{: #api-map}

해당 역할이 API와 일치하는 방법은 무엇입니까? 서비스 조치가 API에 맵핑되는 방법을 보려면 다음 표를 확인하십시오.


|메소드 | 엔드포인트                                                                  | 서비스 조치                  |
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
