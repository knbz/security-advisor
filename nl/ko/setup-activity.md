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

# Activity Insights
{: #setup-activity}

{{site.data.keyword.security-advisor_long}}를 사용하면 {{site.data.keyword.cloud_notm}} Activity Tracker 로그를 지속적으로 모니터하여 리소스의 권한이 없거나 의심스러운 동작 또는 변경사항을 식별할 수 있습니다. 서비스가 제공하는 우수 사례를 기반으로 한 규칙 패키지를 사용하거나 사용자 고유의 사용자 정의 규칙을 작성할 수 있습니다.
{: shortdesc}


## 시작하기 전에
{: #activity-prereq}

Activity Insights를 시작하려면 다음 필수 소프트웨어가 있어야 합니다.

- Windows 10에서 작업하고 있는 경우 Linux용 Windows Subsystem을 활성화하고 [Ubuntu 쉘](https://win10faq.com/install-run-ubuntu-bash-windows-10/){: external}을 설치하십시오. 
- yq CLI 설치:
  * [macOS와 Windows 10의 경우](http://mikefarah.github.io/yq/){: external}.
  * CentOS, Red Hat 및 Ubuntu의 경우 다음 명령을 실행하여 버전 1.15를 설치하십시오.
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- 업데이트된 cURL 바이너리: CentOS와 Red Hat의 경우 `yum update -y nss curl libcurl`을 실행하여 업데이트할 수 있습니다.
- [{{site.data.keyword.cloud_notm}} CLI 및 필수 플러그인](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
- [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external} v1.10.11 이상
- [Kubernetes Helm(패키지 관리자)](/docs/containers?topic=containers-helm) v2.9.0 이상.
- 표준 Kubernetes 클러스터 버전 v1.10.11 이상


## COS 버킷 작성
{: #activity-setup-cos}

{{site.data.keyword.security-advisor_short}} GUI를 사용하여 새 COS 인스턴스 및 버킷을 작성할 수 있습니다.

1. 서비스 대시보드의 **통합** 탭으로 이동하십시오.

2. Activity Insights 상자의 **분석 사용 안함**을 **분석 사용**으로 전환하십시오.

3. **설정으로 이동**을 클릭하십시오.

4. 전제조건 섹션에서 **COS 인스턴스 및 버킷 작성**을 클릭하십시오. 적절한 이름 지정 규칙 및 IAM 권한으로 COS 인스턴스와 버킷이 자동으로 작성됩니다. 버킷 정보가 표시됩니다.

COS 및 버킷의 기존 인스턴스가 있으면 다음 이름 지정 규칙을 사용하는지 확인하십시오. `sa.<account_id>.telemetric.<cos_region>`. 서비스가 COS 인스턴스에 저장된 데이터를 읽도록 하려면 {{site.data.keyword.cloud_notm}} IAM을 사용하여 [service-to-service 권한](/docs/iam?topic=iam-serviceauth)을 설정하십시오. `소스`를 `{{site.data.keyword.security-advisor_short}}`로 `대상`을 COS 인스턴스로 설정하십시오. `독자` IAM 역할을 지정하십시오.


## {{site.data.keyword.security-advisor_short}} 컴포넌트 설치
{: #activity-install-components}

{{site.data.keyword.cloud_notm}} 계정에서 감사 플로우 로그를 수집하는 에이전트를 설치할 수 있습니다. 로그는 Activity Insight를 사용하여 로그에서 의심스러운 동작을 분석할 수 있는 Cloud Object Storage 인스턴스에 저장됩니다.
{: shortdesc}

1. 다음 저장소를 로컬 시스템에 복제하십시오.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. `security-advisor-activity-insights` 폴더로 변경하십시오.

3. 다음 명령을 실행하여 `.tar` 파일을 추출하십시오.

  ```
  tar -xvf security-advisor-activity-insights.tar
  ```
  {: codeblock}

4. `security-advisor-activity-insights` 폴더로 변경하십시오.
5. {{site.data.keyword.cloud_notm}} CLI에 로그인하십시오. CLI의 프롬프트에 따라 로그인을 완료하십시오.

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>지역</th>
      <th> 엔드포인트</th>
    </tr>
    <tr>
      <td>영국</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>미국 남부</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

6. 클러스터에 컨텍스트를 설정하십시오.

  1. 명령을 가져와 환경 변수를 설정하고 Kubernetes 구성 파일을 다운로드하십시오.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. `가져오기`로 시작하는 출력을 복사한 후 터미널에 붙여넣어 `KUBECONFIG` 환경 변수를 설정하십시오.

7. [Kubernetes Service 통합 문서](/docs/containers?topic=containers-helm)를 사용하여 Helm을 설치하십시오.

8. 선택사항: [TLS를 사용으로 설정하십시오](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. 워크스테이션을 사용하여 다중 클러스터의 분석 컴포넌트 설치를 처리하고 TLS가 사용으로 설정된 경우 TLS 구성이 현재 상태이고 컴포넌트를 설치하려는 현재 클러스터와 일치하는지 확인하십시오.

9. 다음 명령을 실행하여 해당 인사이트를 설치하십시오. 이 명령은 버킷의 이름 지정 규칙을 유효성 검증하고, Kubernetes 시크릿을 작성하고, 클러스터 GUID로 값을 업데이트하며 Activity Insights를 배치합니다.

  ```
  ./activity-insight-install.sh <cos_region> <cos_api_key> <at_region> <account_api_key> <account_spaces>
  ```
  {: codeblock}

  오류가 발생하면 `helm init --upgrade`를 실행하십시오.
  {: tip}

  <table>
    <tr>
      <th>변수</th>
      <th>설명</th>
    </tr>
    <tr>
      <td><code>cos_region</code></td>
      <td>COS 인스턴스가 배치된 지역입니다. 옵션으로는 <code>us-south</code> 및 <code>eu-gb</code>가 있습니다.</td>
    </tr>
    <tr>
      <td><code>cos_api_key</code></td>
      <td>COS 인스턴스와 버킷에 액세스하기 위해 작성된 [API 키](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials)입니다. 키에는 플랫폼 역할 `작성자`가 있어야 합니다.</td>
    </tr>
    <tr>
      <td><code>at_region</code></td>
      <td>COS 인스턴스와 버킷을 작성한 지역입니다. 옵션으로는 <code>us-south</code> 및 <code>eu-gb</code>가 있습니다.</td>
    </tr>
    <tr>
      <td><code>account_api_key</code></td>
      <td>{{site.data.keyword.cloud_notm}} 계정에 대한 플랫폼 API 키입니다.</td>
    </tr>
    <tr>
      <td><code>account_spaces</code></td>
      <td>쉼표로 구분된 {{site.data.keyword.cloud_notm}} 계정의 영역 GUID 목록입니다.</td>
    </tr>
  </table>


## COS에 규칙 패키지 추가
{: #activity-adding-rules}

규칙 패키지는 모니터하려는 규칙 목록이 포함된 JSON 파일입니다. 규칙 패키지를 다운로드하거나 [사용자 고유의 규칙 패키지를 작성](/docs/services/security-advisor?topic=security-advisor-activity#activity-packages)할 수 있습니다. {{site.data.keyword.security-advisor_short}} 엔진은 각 규칙이 올바른 구문을 준수하는지 유효성 검증합니다.
{: shortdesc}

1. 다음 저장소를 복제하여 몇 가지 사전된 설정 규칙 패키지를 가져오십시오. `security-advisor-activity-insights`라는 이름의 폴더가 로컬 시스템에 작성됩니다. 

  ```
  https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. 로컬에서 이름이 `IBM.rules/activities`인 폴더를 작성하십시오. 

3. JSON 파일을 `security-advisor-activity-insights/security-advisor-ata-rule-packages`에서 `IBM.rules/activities`로 복사하십시오. 

4. {{site.data.keyword.cloud_notm}} 대시보드로 이동하여 Activity Insights와 연관된 COS 서비스 인스턴스를 선택하십시오.

5. 서비스 대시보드의 **버킷** 탭에서 Activity Insights와 연관된 버킷을 선택하십시오.

5. COS 인스턴스 홈 페이지에서 **업로드**를 클릭하고 **폴더**를 선택하십시오.

6. 프롬프트가 표시되면 **Aspera Connect 클라이언트 설치**를 클릭하십시오. 프롬프트가 표시되지 않으면 클라이언트가 이미 설치되어 있습니다. 클라이언트를 설치해야 하는 경우 설치가 완료되면 5단계를 반복하십시오.

7. *IBM.rules/activities* 폴더를 선택하십시오.

8. **업로드**를 클릭하십시오.

사용자 고유의 패키지를 사용하시겠습니까? 안내서대로 JSON 파일 중 하나를 사용하여 조직의 요구사항에 충족하는 규칙을 작성하십시오. 파일을 작성한 후 COS 인스턴스의 *IBM.rules/activities* 폴더에 추가하십시오. 규칙의 유형 및 형식화에 대한 자세한 정보는 [규칙 패키지 이해](/docs/services/security-advisor?topic=security-advisor-activity)를 참조하십시오.
{: tip}


## 컴포넌트 삭제
{: #activity-delete}

Activity Insights를 더 이상 사용할 필요가 없으면 클러스터에서 서비스 컴포넌트를 삭제할 수 있습니다.
{: shortdesc}

1. Helm을 사용하여 서비스 컴포넌트를 삭제하십시오. TLS가 사용으로 설정된 경우 `-tls` 플래그를 사용해야 합니다.

  ```
  helm del --purge activity-insights [--tls]
  ```
  {: codeblock}

2. Kubernetes 시크릿을 삭제하십시오.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}
