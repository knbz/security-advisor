---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-05"

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

# Network Insights
{: #setup-network}

{{site.data.keyword.security-advisor_long}}를 사용하는 경우 기계 학습, 학습된 패턴 및 위협 인텔리전스를 사용하여 작동을 모니터하여 {{site.data.keyword.containerlong_notm}} 클러스터에서 실행되고 있는 잠재적으로 손상된 컨테이너를 발견할 수 있습니다.
{: shortdesc}

2019년 1월 20일부터 Network Insights(베타)가 Network Analytics 기능을 대체합니다. 서비스 대시보드의 분석 카드는 모두 삭제되지만 찾은 결과는 결과 데이터베이스에 남아 있습니다.
{: important}

## 시작하기 전에
{: #network-prereq}

Network Analytics 기능을 현재 사용중인 경우 Network Insights를 설치하기 전에 [서비스 컴포넌트를 삭제](/docs/services/security-advisor?topic=security-advisor-setup-network#uninstall-analytics)해야 합니다. Network Insights를 시작하려면 다음 필수 소프트웨어가 있어야 합니다.

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
{: #network-setup-cos}

{{site.data.keyword.security-advisor_short}} GUI를 사용하여 새 COS 인스턴스 및 버킷을 작성할 수 있습니다.

1. 서비스 대시보드의 **통합** 탭에서 Network Insights 상자의 **분석 사용 안함**을 **분석 사용**으로 전환하십시오.

2. **설정으로 이동**을 클릭하십시오.

3. 전제조건 섹션에서 **COS 인스턴스 및 버킷 작성**을 클릭하십시오. 적절한 이름 지정 규칙 및 IAM 권한으로 COS 인스턴스와 버킷이 자동으로 작성됩니다.

COS 및 버킷의 기존 인스턴스가 있으면 다음 이름 지정 규칙을 사용하는지 확인하십시오. `sa.<account_id>.telemetric.<cos_region>`. 서비스가 COS 인스턴스에 저장된 데이터를 읽도록 하려면 {{site.data.keyword.cloud_notm}} IAM을 사용하여 [service-to-service 권한](/docs/iam?topic=iam-serviceauth)을 설정하십시오. `소스`를 `{{site.data.keyword.security-advisor_short}}`로 `대상`을 COS 인스턴스로 설정하십시오. `독자` IAM 역할을 지정하십시오.


## {{site.data.keyword.security-advisor_short}} 컴포넌트 설치
{: #network-install-components}

Kubernetes 클러스터에서 네트워크 플로우 로그를 수집하는 에이전트를 설치할 수 있습니다. 로그는 Cloud Object Storage 인스턴스에 저장됩니다. 그런 다음 Network Insights를 사용으로 설정하여 로그를 분석하고 의심스러운 네트워크 활동을 발견하여 경보할 수 있습니다.
{: shortdesc}

모니터하려는 각 클러스터에 대해 다음 설치를 반복하십시오.
{: note}

1. {{site.data.keyword.cloud_notm}} CLI에 로그인하십시오. CLI의 프롬프트에 따라 로그인을 완료하십시오.

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

2. 클러스터에 컨텍스트를 설정하십시오.

  1. 명령을 가져와 환경 변수를 설정하고 Kubernetes 구성 파일을 다운로드하십시오.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. `가져오기`로 시작하는 출력을 복사한 후 터미널에 붙여넣어 `KUBECONFIG` 환경 변수를 설정하십시오.

3. Kubernetes 클러스터의 버전을 가져오십시오. 

  ```
  kube_version=$(kubectl version --output json) echo $(echo $kube_version | yq r - serverVersion.major).$(echo $kube_version | yq r - serverVersion.minor)
  ```
  {: codeblock}

3. Kubernetes Service v1.12.x를 사용 중인 경우 다음 명령을 사용하여 Helm을 설치하십시오. 이 버전을 사용하지 않는 경우 [공용 액세스로 클러스터에서 Helm을 설정](/docs/containers?topic=containers-helm#public_helm_install)하기 위해 수행해야 하는 설치 단계를 Kubernetes 문서에서 확인하십시오. 

  1. 기존 배치를 삭제하십시오. 

    ```
    kubectl delete deployment tiller-deploy -n kube-system
    ```
    {: codeblock}

  2. Tiller RBAC 정책을 배치에 적용하십시오. 

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: codeblock}
  
  3. 클러스터에서 Helm을 초기화한 후 클러스터에 Tiller를 설치하십시오. 

    ```
    helm init --service-account tiller
    ```
    {: codeblock}

  4. `tiller-deploy` 팟(Pod)의 상태가 `running`인지 확인하여 설치에 성공했는지 확인하십시오. 

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: codeblock}

4. Kubernetes Service 버전이 1.11 이상인지 확인한 후 다음 저장소를 로컬 시스템으로 복제하십시오. 

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-network-insights.git
  ```
  {: codeblock}

  Kubernetes Service 버전 1.10은 더 이상 사용되지 않으므로 지원되지 않습니다. v1.10+가 이미 설치되어 있는 경우 다음 Helm 명령을 실행하여 분석기 팟(Pod)을 다시 시작하여 기존 이미지에서 취약점을 수정하십시오. `helm upgrade --recreate-pods network-insights`.
  {: deprecated}

5. `security-advisor-network-insights` 폴더로 변경하십시오.

6. `v1.10+` 디렉토리로 변경하십시오. 

7. 다음 명령을 실행하여 `.tar` 파일을 추출하십시오.

  ```
  tar -xvf security-advisor-network-insights.tar
  ```
  {: codeblock}

8. `security-advisor-network-insights` 폴더로 변경하십시오.

9. [Kubernetes Service 통합 문서](/docs/containers?topic=containers-helm)를 사용하여 Helm을 설치하십시오.

10. 선택사항: [TLS를 사용으로 설정하십시오](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. 워크스테이션을 사용하여 다중 클러스터의 분석 컴포넌트 설치를 처리하고 TLS가 사용으로 설정된 경우 TLS 구성이 현재 상태이고 컴포넌트를 설치하려는 현재 클러스터와 일치하는지 확인하십시오.

11. 다음 명령을 실행하여 Helm 차트와 종속 항목을 설치하십시오. 이 명령은 버킷이 올바른 이름 지정 규칙을 사용하고 있는지 유효성 검증하고, Kubernetes 시크릿을 작성하고, 클러스터 GUID로 값을 업데이트하며, Network Insights Helm 차트를 배치합니다. 오류가 발생하면 `helm init --upgrade`를 실행하십시오.

  ```
  ./network-insight-install.sh <cos_region> <cos_api_key>
  ```
  {: codeblock}

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
  </table>

{{site.data.keyword.security-advisor_short}} Network Insights에서 작동하도록 클러스터가 구성되었습니다!



## 컴포넌트 삭제
{: #network-delete}

Network Insights를 더 이상 사용할 필요가 없으면 클러스터에서 서비스 컴포넌트를 삭제할 수 있습니다.
{: shortdesc}

1. {{site.data.keyword.cloud_notm}} CLI에 로그인하십시오. CLI의 프롬프트에 따라 로그인을 완료하십시오.

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

2. 클러스터에 컨텍스트를 설정하십시오.

  1. 명령을 가져와 환경 변수를 설정하고 Kubernetes 구성 파일을 다운로드하십시오.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. `가져오기`로 시작하는 출력을 복사한 후 터미널에 붙여넣어 `KUBECONFIG` 환경 변수를 설정하십시오.

3. Helm을 사용하여 컴포넌트를 삭제하십시오.

  ```
  helm del --purge network-insights [--tls]
  ```
  {: codeblock}

4. Kubernetes 시크릿을 삭제하십시오.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}

에이전트를 제거할 각 클러스터의 프로세스를 삭제하십시오.
{: tip}

## Network Analytics 설치 제거
{: #uninstall-analytics}

Network Analytics의 베타 버전을 사용한 경우 새 버전을 설치하기 전에 이전 {{site.data.keyword.security-advisor_short}} 컴포넌트를 설치 제거해야 합니다. 서비스 컴포넌트가 포함된 각 클러스터마다 이 프로세스를 반복하십시오.
{: shortdesc}

1. {{site.data.keyword.cloud_notm}}에 로그인하십시오.

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
  ```
  {: codeblock}

2. 클러스터의 이름을 가져오려면 계정에 있는 모든 클러스터를 나열하십시오.

  ```
    ibmcloud ks clusters
  ```
  {: codeblock}

3. 클러스터에 컨텍스트를 설정하십시오.

  1. 명령을 가져와 환경 변수를 설정하고 Kubernetes 구성 파일을 다운로드하십시오.

    ```
    ibmcloud ks cluster-config <cluster_name_or_ID>
    ```
    {: codeblock}

  2. `가져오기`로 시작하는 출력을 복사한 후 터미널에 붙여넣어 `KUBECONFIG` 환경 변수를 설정하십시오.

4. 추출된 아카이브 위치로 이동하고 설치 제거 프로그램 스크립트를 실행하십시오.

  ```
    ./uninstall.sh
  ```
  {: codeblock}

5. 선택사항: 클러스터에서 Helm 서버 컴포넌트를 설치 제거하십시오.

  ```
    helm reset
  ```
  {: codeblock}
