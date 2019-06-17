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


# 통합
{: #integrations}

{{site.data.keyword.security-advisor_long}}를 사용하여 기타 기본 제공 인사이트 또는 파트너 솔루션을 통합하거나 고유한 사용자 정의 통합을 작성할 수 있습니다.
{: shortdesc}


## 사전 통합 찾은 결과
{: #integrate-built-in}

{{site.data.keyword.security-advisor_long}}를 사용하는 경우 기본 제공 서비스 기능을 통해 잠재적인 문제에 대한 인사이트를 얻을 수 있습니다.
{: shortdesc}


{{site.data.keyword.security-advisor_short}}는 다음과 같은 서비스와 즉시 통합할 수 있습니다.

* Certificate Manager(인증서 만기 및 수명과 관련된 알림)
* Vulnerability Advisor(이미지 취약성 및 구성 문제에 대한 세부사항)

자세한 정보는 [사전 통합 서비스 활용](/docs/services/security-advisor?topic=security-advisor-setup-services)을 참조하십시오!

</br>

## 파트너 통합
{: #integrate-partner}

파트너 통합은 {{site.data.keyword.security-advisor_short}}와 IBM과 함께 작업하는 다른 보안 도구 간의 연결을 작성하여 {{site.data.keyword.cloud_notm}} 배치의 보안을 강화하기 위한 방법입니다.
{: shortdesc}

현재 {{site.data.keyword.security-advisor_short}} 파트너에는 NeuVector와 Twistlock이 포함됩니다. 

귀하는 파트너로서 솔루션을 {{site.data.keyword.security-advisor_short}}와 통합하는 데 관심이 있습니까? Matt Ward(wardm@us.ibm.com)를 통해 IBM 팀에 문의하십시오!
{: tip}

### NeuVector
{: #integrate-neuvector}

[NeuVector](https://neuvector.com){: external}는 컨테이너 네트워크 트래픽을 모니터 및 보호할 수 있도록 해주는 Kubernetes 및 Red Hat OpenShift를 위한 고도로 통합되고 자동화된 보안 플랫폼을 제공합니다. 특히 동-서 네트워크 트래픽에 해당됩니다.

NeuVector를 통해 네트워크 위협 및 위반을 발견하여 컨테이너 기반 애플리케이션에 대한 공격을 차단할 수 있습니다. 모니터링을 통해 컨테이너 및 호스트에서 권한 상승, 포트 스캔, 역방향 쉘 및 의심스러운 파일 시스템 활동을 발견하여 악용 및 유출을 차단할 수 있습니다.

NeuVector 인스턴스 설정에 대한 도움말은 [파트너 솔루션](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-neuvector)을 참조하십시오.


### Twistlock
{: #integrate-twistlock}

[Twistlock](https://www.twistlock.com){: external}은 사용자 환경 전체에서 취약한 이미지의 배치를 방지하여 SDLC 전반에 걸쳐 위험을 방지할 수 있습니다. 자동화된 사용자 정의 정책 적용은 애플리케이션 라이프사이클의 모든 단계에서 완전한 제어를 제공합니다. Twistlock Intelligence Stream은 30개 이상의 업스트림 프로젝트, 상업 소스 및 Twistlock 연구소의 자체 연구를 통해 직접 취약성 정보를 수집하고 집계합니다.

가장 정확한 데이터를 사용하여 스택의 모든 계층을 처리할 수 있으므로 정확한 가시성과 가장 낮은 오탐지율을 확보할 수 있습니다.Twistlock은 이 데이터를 인터넷에 어떤 컨테이너가 노출되어 있는지, 높은 권한으로 실행되는지, 다른 보안 완화 기능이 있는지 등 실제 배치 지식과 결합하므로 사용자는 특정 환경에서 가장 심각한 취약점을 항상 확인할 수 있습니다.

Twistlock 인스턴스 설정에 대한 도움말은 [파트너 솔루션](/docs/services/security-advisor?topic=security-advisor-setup-partner#setup-twistlock)을 참조하십시오.
</br>


## 사용자 정의 통합
{: #integrate-custom}

이미 사용 중인 보안 도구가 있을 수도 있습니다. 모든 보안 정보가 하나의 위치에서 중앙 집중화되도록 해당 도구를 {{site.data.keyword.security-advisor_short}} 대시보드와 통합할 수 있습니다!
{: shortdesc}

### 직접 연결 작성
{: #create-direct-connect}

{{site.data.keyword.security-advisor_short}} GUI를 사용하여 {{site.data.keyword.security-advisor_short}} 대시보드에서 모든 항목에 대한 하나의 액세스 지점을 보유하도록 내부 및 외부 보안 도구에 책갈피를 지정할 수 있습니다. 예를 들어 PagerDuty에 책갈피를 지정할 수 있습니다.

### 고유한 도구를 API와 통합
{: #integrate-tools-api}

찾은 결과 API를 사용하여 사용자 정의 보안 도구에서 찾은 결과를 {{site.data.keyword.security-advisor_short}} 대시보드에 통합할 수 있습니다. 여기에는 API 기반 상호작용을 지원하는 보안 경보 또는 찾은 결과를 생성하는 사용자 정의 또는 파트너 도구가 포함될 수 있습니다.

## 기본 제공 인사이트
{: #integrate-insights}

기본 제공 인사이트를 통해 클러스터 및 계정 로그를 지속적으로 모니터링하여 잠재적인 문제를 발견할 수 있습니다. 네트워크 트래픽과 사용자 활동을 모니터링하여 {{site.data.keyword.cloud_notm}} 리소스가 보호된 상태로 유지되는지 확인할 수 있습니다.
{: shortdesc}

### Network Insights(베타)
{: #integrate-network-insights}

Network Insights(베타)를 사용하면 클러스터와 외부 엔티티 간의 클러스터 네트워크 통신(수신 및 발신 모두)을 모니터하고 분석할 수 있습니다. 서비스는 통합된 위협 인텔리전스 및 이상 항목 발견을 사용하여 정찰 공격 및 잠재적으로 손상된 자산을 식별할 수 있습니다. 자세한 정보는 [Network Insights](/docs/services/security-advisor?topic=security-advisor-network)를 참조하십시오.

### Activity Insights(미리보기)
{: #integrate-activity-insights}

Activity Insights(미리보기)를 사용하면 규칙 패키지를 사용하여 {{site.data.keyword.cloud_notm}} Activity Tracker 로그를 지속적으로 모니터하여 사용자 또는 앱에서 수행된 권한이 없거나 의심스러운 활동을 식별할 수 있습니다. 보안 우수 사례를 기반으로 한 서비스에서 제공하는 규칙 패키지를 사용하거나 사용자 요구사항에 맞게 규칙을 사용자 정의할 수 있습니다. 자세한 정보는 [Activity Insights](/docs/services/security-advisor?topic=security-advisor-activity)를 참조하십시오.
