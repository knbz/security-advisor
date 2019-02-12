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

# 통합
{: #about}

{{site.data.keyword.security-advisor_long}}를 사용하여 IBM Cloud 서비스 및 파트너 솔루션에서 찾은 결과를 통합하거나 고유한 사용자 정의 통합을 작성할 수 있습니다.
{: shortdesc}


## 사전 통합 찾은 결과
{: #built-in}

{{site.data.keyword.security-advisor_long}}를 사용하는 경우 기본 제공 서비스 기능을 통해 잠재적인 문제에 대한 인사이트를 얻을 수 있습니다.
{: shortdesc}


{{site.data.keyword.security-advisor_short}}는 다음과 같은 서비스와 즉시 통합할 수 있습니다.

* Certificate Manager(인증서 만기 및 수명과 관련된 알림)
* Vulnerability Advisor(이미지 취약성 및 구성 문제에 대한 세부사항)

자세한 정보는 [사전 통합 서비스 활용](setup.html)을 참조하십시오!

</br>

## 파트너 솔루션
{: #partner}

파트너 솔루션은 {{site.data.keyword.security-advisor_short}}와 IBM Cloud 외부의 다른 보안 도구 사이에 링크를 작성하여 IBM Cloud 배치의 보안을 강화하기 위한 방법입니다.
{: shortdesc}

{{site.data.keyword.security-advisor_short}}는 현재 다음과 같은 회사와 파트너 관계를 맺고 있습니다.

### NeuVector
{: #neuvector}

[NeuVector](https://neuvector.com/)는 컨테이너 네트워크 트래픽을 모니터 및 보호할 수 있도록 해주는 Kubernetes 및 Red Hat OpenShift를 위한 고도로 통합되고 자동화된 보안 플랫폼을 제공합니다. 특히 동-서 네트워크 트래픽에 해당됩니다.

NeuVector를 통해 네트워크 위협 및 위반을 발견하여 컨테이너 기반 애플리케이션에 대한 공격을 차단할 수 있습니다. 모니터링을 통해 컨테이너 및 호스트에서 권한 상승, 포트 스캔, 역방향 쉘 및 의심스러운 파일 시스템 활동을 발견하여 악용 및 유출을 차단할 수 있습니다.

통합 마법사를 사용하여 NeuVector 인스턴스를 설정하는 방법에 대한 정보는 [파트너 솔루션](partners.html)을 참조하십시오.

귀하는 파트너로서 솔루션을 {{site.data.keyword.security-advisor_short}}와 통합하는 데 관심이 있습니까? Matt Ward(wardm@us.ibm.com)를 통해 IBM 팀에 문의하십시오!
{: tip}

</br>

## 사용자 정의 통합
{: #custom}

이미 사용 중인 보안 도구가 있을 수도 있습니다. 모든 보안 정보가 하나의 위치에서 중앙 집중화되도록 해당 도구를 {{site.data.keyword.security-advisor_short}} 대시보드와 통합할 수 있습니다!
{: shortdesc}

**고유한 도구를 GUI와 통합**

{{site.data.keyword.security-advisor_short}} GUI를 사용하여 {{site.data.keyword.security-advisor_short}} 대시보드에서 모든 항목에 대한 하나의 액세스 지점을 보유하도록 내부 및 외부 보안 도구에 책갈피를 지정할 수 있습니다. 예를 들어 PagerDuty에 책갈피를 지정할 수 있습니다.

**고유한 도구를 API와 통합**

찾은 결과 API를 사용하여 사용자 정의 보안 도구에서 찾은 결과를 {{site.data.keyword.security-advisor_short}} 대시보드에 통합할 수 있습니다. 여기에는 API 기반 상호작용을 지원하는 보안 경보 또는 찾은 결과를 생성하는 사용자 정의 또는 파트너 도구가 포함될 수 있습니다.

**시작하기**

API를 활용하고 GUI를 통해 책갈피를 작성하는 방법에 대한 권장 사례는 [사용자 정의 보안 도구](/docs/services/security-advisor/custom.html)를 참조하십시오.

</br>


## 네트워크 분석(미리보기)
{: #network-analytics}

네트워크 분석 미리보기를 사용하는 경우 {{site.data.keyword.containerlong_notm}} 클러스터 통신을 모니터하여 위협을 발견할 수 있습니다.
{: shortdesc}

클러스터 통신에서 클러스터로부터 송수신되는 의심스러운 통신을 모니터하는 경우 IBM X-Force에서 제공되는 위협 인텔리전스를 활용하여 잠재적인 문제에 플래그를 지정할 수 있습니다. 의심스러운 통신이 식별되면 사용자가 검토 및 평가할 수 있는 찾은 결과가 실행됩니다.

네트워크 분석에 대한 자세한 정보는 [네트워크 분석](network-analytics.html)을 참조하십시오.
{: tip}

</br>
</br>
