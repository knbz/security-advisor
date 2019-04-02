---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-13"

keywords: centralized security, security management, alerts, security risk, insights, threat detection

subcollection: security-advisor

---

{:new_window: target="_blank"}
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


# 파트너
{: #setup-partner}

IBM Business Partner 통합은 서드파티 제공업체의 주요 경보와 찾은 결과를 {{site.data.keyword.security-advisor_long}} 대시보드에 가져오는 방법입니다. 이러한 파트너들은 사용자를 위한 통합 환경을 구체화하고 단순화하며 안내를 돕기 위해 IBM과 협력해 오고 있습니다.
{: shortdesc}

## 시작하기 전에
{: #partner-before}

파트너 통합을 시작하기 전에 다음 전제조건을 충족하는지 확인하십시오.

* 통합하려는 파트너에 대한 계정이 있는지 확인하십시오.
* 파트너 서비스에 대한 통합 URL을 생성하기 위해 필요한 관리 권한이 있는지 확인하십시오.
* {{site.data.keyword.cloud_notm}}에 대한 IAM 관리 액세스 권한 및 {{site.data.keyword.security-advisor_short}}에 대한 관리자 액세스 권한이 있는지 확인하십시오.

## 통합 마법사
{: #wizard}

{{site.data.keyword.cloud_notm}} 및 파트너 계정에 대한 관리 권한을 둘 다 보유한 관리자의 경우 통합 마법사를 사용하여 신속하게 시작하고 실행할 수 있습니다. 이 마법사에는 네 개의 기본 단계가 포함되어 있습니다.

* {{site.data.keyword.cloud_notm}} 및 파트너 계정 연관 및 신뢰 설정
* 계정 간 인증 정보 및 URL과 같이 필요한 정보 복사
* 파트너 찾은 결과 메타데이터를 {{site.data.keyword.security-advisor_short}}에 설치
* 파트너의 찾은 결과를 {{site.data.keyword.security-advisor_short}}에 게시하여 해당 쌍을 유효성 검증


## NeuVector 통합
{: #setup-neuvector}

NeuVector를 통해 네트워크 위협 및 위반을 발견하여 컨테이너 기반 애플리케이션에 대한 공격을 차단할 수 있습니다. 모니터링을 통해 컨테이너 및 호스트에서 권한 상승, 포트 스캔, 역방향 쉘 및 의심스러운 파일 시스템 활동을 발견하여 악용 및 유출을 차단할 수 있습니다.
{: shortdesc}

NeuVector를 {{site.data.keyword.security-advisor_short}}와 통합하려면 다음 단계를 사용하십시오.

1. **통합 > 파트너 통합**으로 이동하고 제공된 옵션에서 **NeuVector**를 선택하십시오.
2. 계정을 연결하십시오. 
  1. 연결의 이름을 제공하십시오.
  2. NeuVector 대시보드 URL을 제공하십시오.
  3. NeuVector 구성 URL을 제공하십시오.
    1. NeuVector에 로그인한 후 설정으로 이동하십시오.
    2. **URL 생성**을 클릭하십시오.
    3. URL을 복사하여 {{site.data.keyword.security-advisor_short}} 통합 마법사에 붙여넣으십시오.
  4. **다음**을 클릭하십시오.
3. 서비스에서 서비스 ID 및 API 키를 생성할 수 있는 권한을 부여했는지 확인한 후 **다음**을 클릭하여 해당 서비스와의 연결을 작성하십시오.
4. **완료**를 클릭하십시오.
5. 서비스 대시보드로 이동하여 NeuVector에서 {{site.data.keyword.containershort_notm}}로 전송한 테스트 찾은 결과를 검토하십시오.



## Twistlock 통합
{: #setup-twistlock}

환경에 취약한 이미지의 배치를 중지하여 위험을 방지할 수 있습니다. 자동화된 사용자 정의 정책 적용으로 Twistlock은 애플리케이션 라이프사이클의 모든 단계에서 완전한 제어를 제공합니다.
{: shortdesc}

파트너 연결을 구성하면 대시보드에 Twistlock의 찾은 결과를 요약하는 두 개의 카드가 작성됩니다.

**Twistlock 런타임**으로 두 가지 주요 위험 지표(KRI)가 도입되었습니다. 

* 총 인시던트 및 감사: 클라우드 고유 워크로드에서 인시던트 또는 감사와 관련된 찾은 결과.
* 총 방화벽 감사: 방화벽 문제와 관련된 찾은 결과.

**Twistlock 취약점**으로 하나의 KRI가 도입되었습니다.

* 새 취약점이 있는 총 이미지 수: 컨테이너 이미지에서 발견된 취약점과 관련된 찾은 결과.

Twistlock 문서에서 회사에 대한 자세한 정보를 확인할 수 있습니다.

### Twistlock 구성
{: #configure-twistlock}

1. {{site.data.keyword.security-advisor_short}} 대시보드에서 **통합 > 파트너 통합**으로 이동하고 제공된 옵션에서 **Twistlock**을 선택하십시오.

2. **예, 내 계정으로 {{site.data.keyword.security-advisor_short}}에 연결**을 클릭하십시오.

  계정이 없으면 **아니오, 계정 작성을 위한 도움말 > 계정 작성**을 클릭하십시오. 정보를 기입하면 시작을 위해 Twishlock 영업 팀이 연락합니다.
  {: note}

3. 연결 이름을 제공하십시오. 이름이 계정에 고유하고 영숫자 문자, 공백 또는 하이픈만 사용해야 합니다.

4. 선택사항: 구성 URL이 없으면 **등록 URL 생성**을 클릭하고 Twistlock 문서로 이동하여 URL 작성 방법을 알아보십시오. 문서에 대한 액세스 권한을 확보하려면 라이센스와 함께 제공되는 Twistlock 토큰을 사용해야 합니다.

5. Twistlock 대시보드에서 **관리 > 경보** 탭으로 이동하여 **프로파일 추가**를 클릭하십시오.

6. **제공자** 드롭 다운에서 **{{site.data.keyword.security-advisor_short}}**를 선택하십시오.

7. **복사**를 클릭하여 제공된 구성 URL을 사용하십시오.

8. {{site.data.keyword.security-advisor_short}} 대시보드로 돌아가서 **Twistlock 구성 URL 입력** 상자에 URL을 붙여넣으십시오.

9. **파트너 연결**을 클릭하십시오.

### 연결 확인
{: #twistlock-verify}

1. {{site.data.keyword.security-advisor_short}} 대시보드에서 Twistlock 카드가 예상대로 표시되는지 확인하십시오.

2. Twistlock 대시보드에서 **경보** 탭을 새로 고치십시오. {{site.data.keyword.security-advisor_short}} 연결이 표시됩니다. 연결을 여십시오.

3. 알림을 받으려는 경보 유형이 선택되어 있는지 확인한 후 **확인**을 클릭하여 연결을 완료하십시오.
