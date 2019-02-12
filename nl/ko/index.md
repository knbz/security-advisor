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

# 튜토리얼 시작하기
{: #index}

{{site.data.keyword.security-advisor_long}}를 사용하는 경우 하나의 중앙 집중식 대시보드를 통해 즉시 {{site.data.keyword.Bluemix_notm}}의 보안 포스처를 확인할 수 있습니다.
{:shortdesc}

{{site.data.keyword.security-advisor_short}}는 모든 {{site.data.keyword.Bluemix_notm}} 계정에서 기본적으로 사용으로 설정되어 있는 클라우드 서비스입니다. 따라서 서비스의 인스턴스를 프로비저닝할 필요가 없습니다.
{: tip}

이 서비스는 다양한 소스로부터 보안 정보를 수신하며 서비스 대시보드에 사용자의 주의가 필요한 보안 경보 또는 취약성을 표시합니다. 대시보드에는 즉시 사용 가능한 몇 가지 미리 채워진 카드가 있습니다. 이러한 찾은 결과는 IBM Cloud의 보안 서비스로부터 제공되지만 동일한 위치에서 모든 보안 도구에 액세스할 수 있도록 카드 또는 사용자 정의 파트너 솔루션을 추가할 수 있습니다.

사전 통합 찾은 결과를 통해 다음과 같은 항목을 모니터할 수 있습니다.

- {{site.data.keyword.cloudcerts_long_notm}}으로 관리하는 인증서
- {{site.data.keyword.registrylong_notm}}에 저장되는 컨테이너 이미지의 취약성

IBM Cloud Kubernetes Service 클러스터에서 실행되는 의심스러운 클라이언트 및 잠재적으로 손상된 컨테이너에 대한 인사이트를 얻을 수도 있습니다. 이 기능이 구성되고 사용으로 설정되어 있는 경우 클러스터 네트워크 통신(수신 및 발신 모두)을 수집하고 위협 인텔리전스에 따라 지속적으로 모니터 및 분석합니다. [네트워크 분석](network-analytics.html)을 읽어서 자세한 정보를 확인할 수 있습니다.

</br>

## 서비스 대시보드에 도달
{: #dashboard}

시작할 준비가 되셨습니까? 다음 방법 중 하나를 사용하여 서비스 대시보드에 도달할 수 있습니다.

<ul>
  <li>타일 사용:
    <ol>
      <li><a href="https://console.cloud.ibm.com/catalog/" target="_blank">{{site.data.keyword.Bluemix_notm}}<img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>에 로그인하십시오.</li>
      <li>**카탈로그**로 이동한 후 **보안 및 ID**를 클릭하십시오.</li>
      <li>{{site.data.keyword.security-advisor_short}} 타일을 선택하십시오. Vulnerability Advisor 및 Certificate Manager와 같은 사전 구성된 통합 도구에 대한 보안 정보를 볼 수 있는 대시보드가 열립니다.</li>
    </ol>
  </li>
  <li>메뉴 사용:
    <ol>
      <li><a href="https://console.cloud.ibm.com" target="_blank">{{site.data.keyword.Bluemix_notm}}<img src="../../icons/launch-glyph.svg" alt="외부 링크 아이콘"></a>에 로그인하십시오.</li>
      <li>대시보드에서 햄버거 메뉴를 클릭하여 옵션을 펼치십시오.</li>
      <li>**보안**을 클릭하십시오. 보안 대시보드의 개요가 열립니다.</li>
      <li>탐색에서 **시작하기**를 클릭하여 서비스에 대한 일반 개요 정보를 확인하거나 조치에서 서비스를 확인하여 알아보려는 경우 **대시보드**를 클릭하십시오.</li>
    </ol>
  </li>
</ul>

사전 통합 찾은 결과에 아무 정보도 표시되지 않습니까? {{site.data.keyword.security-advisor_short}}에서 모니터할 인증서 또는 이미지가 없을 수 있습니다. [사전 통합 서비스 활용](setup.html)에서 대시보드 카드를 채우기 위해 {{site.data.keyword.security-advisor_short}}에 필요한 항목에 대한 자세한 정보를 확인하십시오.

</br>

## 다음 단계
{: #next}

대시보드가 작동 중임을 확인했으면 {{site.data.keyword.security-advisor_short}}가 어떻게 도움이 되는지에 대한 [자세한 정보](about.html)를 확인하십시오. 서비스가 개발됨에 따라 서비스에 대한 아이디어를 제공하기 위해 [developerWorks](ts_index.html)를 사용하여 사용자 피드백을 전송할 수도 있습니다.

</br>

## 가용성
{: #availability}

현재는 댈러스 및 런던 지역에서 {{site.data.keyword.security-advisor_short}}를 활용할 수 있습니다.
