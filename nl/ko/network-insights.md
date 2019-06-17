---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-06"

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


# Network Insights(베타)
{: #network}

{{site.data.keyword.security-advisor_long}}를 사용하면 {{site.data.keyword.containerlong_notm}} 클러스터에서 실행되는 잠재적으로 손상된 컨테이너에 대한 위협 인텔리전스, 동작 패턴 및 기계 학습을 기반으로 한 인사이트를 얻을 수 있습니다.
{: shortdesc}


## 작동 방법
{: #network-how-it-works}

Network Insights 기능은 {{site.data.keyword.security-advisor_short}} 서비스에 대한 추가 기능입니다. 이 기능이 구성되고 사용으로 설정되어 있는 경우 클러스터와 외부 엔티티 간의 클러스터 네트워크 통신(수신 및 발신 모두)을 수집하고 지속적으로 모니터 및 분석합니다.

정보의 플로우를 확인하려면 다음 이미지를 참조하십시오.

![Network Insights 플로우 다이어그램](images/network-insights-flow.png)

1. 계정 관리자는 Network Insights를 모니터할 각 클러스터에 설치할 수 있습니다.
2. 네트워크 플로우 로그는 Cloud Object Storage 버킷으로 전달되어 삭제할 때까지 저장됩니다. {{site.data.keyword.security-advisor_short}} GUI를 사용하여 버켓을 작성하면 서비스에서 로그를 볼 수 있도록 서비스 IAM 역할이 지정됩니다.
3. Network Insights가 사용으로 설정되면 COS 버킷의 원시 데이터가 의심스러운 동작에 대해 분석됩니다.
4. 가능한 보안 문제를 플래그 지정하면 찾은 결과가 찾은 결과 데이터베이스로 전달됩니다.
5. 찾은 결과는 다음 3개의 카드로 서비스 대시보드에 표시됩니다.
  * 의심스러운 인바운드 트래픽
  * 의심스러운 아웃바운드 트래픽
  * 트래픽의 이상 항목


## 데이터 수집
{: #network-data}

{{site.data.keyword.security-advisor_short}}는 클러스터와 외부 엔티티 간에 발생하는 네트워크 플로우 정보를 수집하는 데 사용되는 콜렉터를 제공합니다.
{: shortdesc}

수집된 원시 데이터는 저장되는 기간을 판별할 수 있는 Cloud Object Storage 버킷에 저장됩니다. 수집한 데이터를 소유하고 관리한다는 것은 데이터를 저장하고 보호하고 삭제할 책임이 있음을 의미합니다. {{site.data.keyword.security-advisor_short}}는 90일 동안 찾은 결과를 유지보수합니다. 그 기간 동안 결과는 서비스 대시보드의 Network Insights 카드에 표시됩니다. 따라서 90일 후 대시보드에 더 이상 찾은 결과가 표시되지 않지만 원시 데이터는 스토리지에 여전히 있을 수 있습니다.

다음 정보가 수집됩니다.

* 통신 중인 피어의 IP 주소
* 사용하는 포트
* 사용 중인 프로토콜 세트
* 전송되는 데이터의 양
* 프로토콜에 특정한 매개변수 세트
* 다양한 시간소인

교환되는 실제 데이터는 수집되지 않습니다.
{: tip}

보안 관점에서 법적 또는 회사 요구사항에 따라 삭제할 수 있으면 수집된 데이터는 제거하는 것이 좋습니다. 자세한 정보는 [오브젝트 삭제](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion)를 참조하십시오.


## 네트워크: 의심스러운 인바운드 트래픽
{: #network-suspicious-inbound}

{{site.data.keyword.security-advisor_short}}는 서비스 대시보드의 **의심스러운 인바운드 트래픽** 카드에서 클러스터를 조사하고 손상시키려는 외부 클라이언트의 시도를 알려줍니다.
{: shortdesc}


IBM X-Force가 봇넷의 일부로서 스캐너로 사용되는 배포 악성코드로 분류한 클라이언트의 동작 패턴, 암호 화폐 채굴 또는 익명화 서비스에 대한 동작 패턴은 모두 지속적으로 모니터됩니다. 해당 유형의 클라이언트가 모니터되는 클러스터에 접근하고 알람 동작이 표시되면 네트워크 인사이트에서 찾은 결과를 발행합니다.


카드에는 두 가지 주요 위험 지표(KRI)가 있습니다.

* 의심스러운 클라이언트에 의한 정찰: 클러스터에 액세스하는 클라이언트와 관련된 찾은 결과를 포함합니다.
* 의심스러운 클라이언트에 의해 전송된 비정상적으로 큰 페이로드: 클라이언트와 클러스터 간에 전송되는 데이터 볼륨과 관련된 찾은 결과를 포함합니다. 비정상적인 페이로드는 클러스터의 특성이 아닙니다.


찾은 결과에는 다음과 같은 의심스러운 클라이언트가 포함될 수 있습니다.

* 클러스터에 비정상적인 대량의 데이터를 전송합니다.
* 클러스터 서비스에 비정상적인 대량의 요청을 수행합니다.
* 클러스터를 조사하기 위해 수행하는 시도 횟수만큼 클러스터를 대상으로 지정하는 것으로 보입니다.



## 네트워크: 의심스러운 아웃바운드 트래픽
{: #network-suspicious-outbound}

{{site.data.keyword.security-advisor_short}}는 {{site.data.keyword.containershort_notm}} 클러스터에서 실행되는 잠재적으로 손상된 컨테이너를 서비스 대시보드의 **의심스러운 아웃바운드 트래픽** 카드로 알려줍니다.
{: shortdesc}

서비스는 IBM X-Force에 의해 봇넷의 파트, 스캐너로서 사용되는 악성코드를 배포하는 것으로 분류되는 클라이언트의 동작 패턴, 암호 화폐 채굴 또는 익명화 서비스에 대한 동작 패턴을 지속적으로 모니터합니다. 모니터되는 클러스터의 컨테이너가 의심되는 피어에 접근하고 알람 작동을 표시하면 네트워크 인사이트에서 찾은 결과를 발행합니다.

카드에는 두 가지 주요 위험 지표(KRI)가 있습니다.

* 의심스러운 서버에 아웃바운드 접근: 서버에 액세스하는 클러스터와 관련된 찾은 결과.
* 의심스러운 서버와 교환되는 비정상적으로 큰 페이로드: 클러스터와 서버 간에 전송되는 데이터 볼륨과 관련된 찾은 결과.


찾은 결과에는 다음과 같은 컨테이너가 포함될 수 있습니다.

* 비정상적인 대량의 데이터를 의심스러운 서버로 전송합니다. 
* 의심스러운 서버에서 대량의 데이터를 추출합니다. 
* 의심스러운 서버에 대해 대량의 요청을 수행합니다. 


## 네트워크: 트래픽의 이상 항목(시범)
{: #network-anomalies}

이 시범 기능에서 {{site.data.keyword.security-advisor_short}}는 네트워크를 모니터하여 동작 패턴을 학습합니다. 패턴이 형성되면 비정상으로 표시되는 모든 동작은 플래그 지정되고 서비스 대시보드의 **트래픽의 이상 항목** 카드에 요약됩니다.
{: shortdesc}

컨테이너와 피어 간의 동작 패턴을 모니터링하여 정상 컨테이너 동작 모델이 작성됩니다. 모델이 캡처되면 특정 변경사항이 모니터됩니다. 알람이 변경되면 Network Insights는 찾은 결과를 발행합니다.

카드에는 두 가지 주요 위험 지표(KRI)가 있습니다.

* 정상적인 정찰 또는 데이터 교환 활동보다 높음: 클러스터와 외부 피어 간에 발견된 비정상 상호작용과 관련된 찾은 결과.
* 새 서버에 아웃 바운드 접근: 클러스터가 접근하는 새로 발견된 서버와 관련된 찾은 결과.

찾은 결과에는 다음 사항이 포함될 수 있습니다.  

* 이전에 접근하지 않았던 서버에 접근하는 컨테이너
* 특정 피어로 또는 피어에서 정상 상태보다 훨씬 많은 데이터를 전송하거나 사용하는 컨테이너
* 특정 컨테이너의 조사 레벨이 현저하게 증가함

모델이 개발되면 학습된 모델의 편차가 발견되고 알람이 변경되면 Network Insights는 Security Advisor 대시보드에 찾은 결과를 게시합니다. 찾은 결과는 "네트워크: 트래픽의 이상 항목" 카드에 요약됩니다. 카드에는 두 가지 주요 위험 지표(KRI)가 있습니다. "정상적인 정찰 또는 데이터 교환 활동보다 높음" KRI는 클러스터와 외부 피어 간에 발견된 비정상 상호작용과 관련된 찾은 결과를 계산합니다. "새 서버에 아웃 바운드 접근" KRI는 클러스터에 의한 새로 발견된 서버 접근과 관련된 찾은 결과를 계산합니다.   

## 다음 단계
{: #network-next}

시작할 준비가 되셨습니까? [Network Insights 사용](/docs/services/security-advisor?topic=security-advisor-setup-network)을 참조하십시오!
