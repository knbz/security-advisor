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

# 파트너 솔루션
{: #setup-partner}

파트너 솔루션은 {{site.data.keyword.security-advisor_long}}와 다른 보안 도구 사이에 링크를 작성하여 보안을 강화하기 위한 방법입니다.
{: shortdesc}

## 통합 마법사
{: #wizard}

IBM Cloud 및 파트너 계정에 대한 관리 권한을 둘 다 보유한 관리자의 경우 통합 마법사를 사용하여 신속하게 시작하고 실행할 수 있습니다. 이 마법사에는 네 개의 기본 단계가 포함되어 있습니다.

* IBM Cloud 및 파트너 계정 연관 및 신뢰 설정
* 계정 간 인증 정보 및 URL과 같이 필요한 정보 복사
* 파트너 찾은 결과 메타데이터를 {{site.data.keyword.security-advisor_short}}에 설치
* 파트너의 찾은 결과를 {{site.data.keyword.security-advisor_short}}에 게시하여 해당 쌍을 유효성 검증

</br>

## NeuVector 통합
{: #neuvector}

**시작하기 전에**

* 통합하려는 파트너에 대한 계정이 있는지 확인하십시오.
* 파트너 서비스에 대한 통합 URL을 생성하기 위해 필요한 관리 권한이 있는지 확인하십시오.
* IBM Cloud에 대한 IAM 관리 액세스 권한 및 {{site.data.keyword.security-advisor_short}}에 대한 관리자 액세스 권한이 있는지 확인하십시오.

**통합**

1. 계정을 연결하십시오. 
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
