---

copyright:
  years: 2017, 2018
lastupdated: "2018-12-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# {{site.data.keyword.cloudaccesstrailshort}} 이벤트
{: #at_events}

{{site.data.keyword.cloudaccesstrailshort}} 서비스를 사용하여 {{site.data.keyword.security-advisor_long}} 서비스 인스턴스에서 구성된 사용자 초기화 활동을 보고, 관리하고, 감사할 수 있습니다.
{: shortdesc}

서비스 작동 방법에 대한 자세한 정보는 [{{site.data.keyword.cloudaccesstrailshort}} 문서](/docs/services/cloud-activity-tracker/index.html)를 참조하십시오.


## 이벤트 확인 위치
{: #monitor}

이벤트는 이벤트가 생성되는 {{site.data.keyword.Bluemix_notm}} 지역에서 사용 가능한 {{site.data.keyword.cloudaccesstrailshort}} **계정 도메인**에 사용할 수 있습니다.

1. {{site.data.keyword.Bluemix_notm}} 계정에 로그인하십시오.
2. 카탈로그의 {{site.data.keyword.security-advisor_long}}의 인스턴스와 동일한 계정에서 {{site.data.keyword.cloudaccesstrailshort}} 서비스의 인스턴스를 프로비저닝하십시오.
3. {{site.data.keyword.cloudaccesstrailshort}} 대시보드의 **관리** 탭에서 **Kibana에서 보기**를 클릭하십시오.
4. 로그를 보려는 시간 프레임을 설정하십시오. 기본값은 15분입니다.
5. **사용 가능한 필드** 목록에서 **유형**을 클릭하십시오. **활동 트래커**의 돋보기 아이콘을 클릭하여 서비스로 추적되는 항목으로만 로그를 제한하십시오.
6. 사용 가능한 기타 필드를 사용하여 검색 범위를 좁힐 수 있습니다.

로그를 볼 계정 소유자가 아닌 사용자의 경우 프리미엄 플랜을 사용해야 합니다. 다른 사용자가 이벤트를 볼 수 있으려면 [계정 이벤트를 보도록 권한 부여](/docs/services/cloud-activity-tracker/how-to/grant_permissions.html#grant_permissions)를 참조하십시오.
{: tip}

## 이벤트 목록
{: #events}

{{site.data.keyword.cloudaccesstrailshort}}에 전송된 이벤트의 목록을 보려면 다음 표를 확인하십시오.
{: shortdesc}

<table>
  <tr>
    <th>조치</th>
    <th>설명</th>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>이전에 작성된 참고를 봅니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>참고를 작성합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>참고를 삭제합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>참고를 업데이트합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>하나 이상의 발생을 봅니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>발생을 작성합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>발생을 삭제합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>발생을 업데이트합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>서비스에 추가된 사용자 정의 솔루션을 읽습니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>서비스에 사용자 정의 솔루션을 추가합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>서비스 내 기존 사용자 정의 솔루션을 업데이트합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>서비스에서 사용자 정의 솔루션을 삭제합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>서비스 인스턴스에 추가한 파트너 솔루션을 확인합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>서비스 인스턴스에 파트너 솔루션을 추가합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>서비스 인스턴스에서 파트너 솔루션을 업데이트합니다.</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>서비스 인스턴스에서 파트너 솔루션을 삭제합니다.</td>
  </tr>
</table>
