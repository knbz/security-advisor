---

copyright:
  years: 2018
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}

# {{site.data.keyword.security-advisor_short}} 정보
{: #about}

{{site.data.keyword.security-advisor_long}}를 통해 관리자는 클라우드 애플리케이션 및 워크로드의 보안 문제를 찾고, 우선순위를 지정하고, 관리할 수 있습니다.
{:shortdesc}

{{site.data.keyword.Bluemix_notm}}에서 {{site.data.keyword.security-advisor_short}}는 Vulnerability Advisor와 같은 인사이트 서비스 및 {{site.data.keyword.cloudcerts_short}}를 중앙 집중화합니다. 보안 이벤트를 관리하고 분석을 적용하여 {{site.data.keyword.Bluemix_notm}}에서 보안 관리 프로세스를 통합하고 개선하는 찾은 결과를 작성할 수 있습니다.

{{site.data.keyword.security-advisor_short}}는 의심스러운 클라이언트 및 서버 IP와의 통신을 발견할 수 있는 넷 플로우 데이터를 수집하도록 {{site.data.keyword.containerlong_notm}} 클러스터에서 실행되는 평가판 네트워크 분석 기능도 제공합니다. 

## 주요 개념
{: #concepts}

{{site.data.keyword.security-advisor_short}} 관련 작업을 수행하는 동안 사용할 수 있는 다른 개념에 대해 자세히 알아보십시오.
{: shortdesc}

<dl>
  <dt>찾은 결과</dt>
    <dd>찾은 결과는 원시 이벤트가 처리될 때 작성되는 우선순위 보안 문제입니다. 찾은 결과는 문제에 대한 사용자, 정보, 시간 및 위치를 식별하는 데 필요한 정보의 주요 부분으로 구성됩니다. 보안 관리자로서 {{site.data.keyword.security-advisor_short}} 찾은 결과를 사용하여 발견된 상황에 대한 우선순위를 지정하고 이 상황에 반응할 수 있습니다. </dd>
  <dt>핵심성과지표(KPI)</dt>
    <dd>핵심성과지표(KPI)는 찾은 결과의 값이 서비스 및 워크로드의 특정 보안 제어에 대한 허용 가능한 성능의 범위를 벗어날 때 트리거됩니다. </dd>
  <dt>참고</dt>
    <dd>분석 중인 동안 발견한 찾은 결과를 분류하기 위해 참고를 작성할 수 있습니다. 참고는 다른 제공자 간에 여러 번 작성될 수 있습니다. </dd>
  <dt>발생</dt>
    <dd>발생에서는 참고의 제공자별 세부사항에 대해 설명합니다. 발생에는 취약성 세부사항, 조치방안 단계 및 기타 일반 정보가 포함됩니다. </dd>
  <dt>서비스 CRN</dt>
    <dd>서비스 CRN은 찾은 결과에 포함된 {{site.data.keyword.Bluemix_notm}} 서비스를 식별합니다. </br>
      <table>
        <tr>
          <th>찾은 결과의 유형</th>
          <th>CRN</th>
        </tr>
        <tr>
          <td>{{site.data.keyword.cloudcerts_short}}</td>
          <td>서비스 인스턴스 CRN입니다. </td>
        </tr>
        <tr>
          <td>네트워크 분석</td>
          <td>Kubernetes 클러스터 CRN입니다.</td>
        </tr>
        <tr>
          <td>Vulnerability Advisor</td>
          <td>컨테이너 CRN입니다.</td>
        </tr>
      </table></dd>
    <dt>리소스 CRN</dt>
      <dd>리소스 CRN은 찾은 결과에 포함된 특정 리소스를 식별합니다.</dd>
</dl>
