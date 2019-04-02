---

copyright:
  years: 2019
lastupdated: "2019-02-18"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}

# 疑難排解
{: #troubleshooting}

如果您在使用 {{site.data.keyword.security-advisor_long}} 時發生問題，請考慮使用這些技術來進行疑難排解及取得協助。
{: shortdesc}


## 取得協助及支援
{: #getting-help-and-support}



您可以搜尋資訊或透過討論區提問來尋求協助。您也可以開立支援問題單。使用討論區提問時，請標記您的問題，讓 {{site.data.keyword.Bluemix_notm}} 開發團隊能看到它。
{: shortdesc}

* 如果您有 {{site.data.keyword.security-advisor_short}} 的相關技術問題，請將問題張貼在 <a href="http://stackoverflow.com/" target="_blank">Stack Overflow <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。請務必包括 `security-advisor` 及 `ibm-cloud` 標籤。
* 若為服務及開始使用指示的相關問題，請使用 <a href="https://developer.ibm.com/" target="_blank">dW Answers <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 討論區。請務必包括 `security-advisor` 及 `ibm-cloud` 標籤。

如需取得支援的相關資訊，請參閱[如何取得我需要的支援](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support)。


## 您無法建立自訂解決方案出現項目
{: #ts-custom-occurrence}

{: tsSymptoms}
您嘗試建立自訂解決方案出現項目，但資訊未顯示在瀏覽器中，而且您未收到任何錯誤訊息。

{: tsCauses}
您發現已知問題。建立出現項目失敗，因為您選擇的名稱已存在。

{: tsResolve}
請為您的出現項目選擇另一個名稱。


## 錯誤：禁止名稱空間 "security-advisor-insights"
{: #ts-error-helm-install}

{: tsSymptoms}
嘗試安裝 Network 或 Activity Insights 時發生錯誤：

```
namespaces “security-advisor-insights” is forbidden: User “system:serviceaccount:kube-system:default” cannot get resource “namespaces” in API group “” in the namespace “security-advisor-insights”
```
{: screen}

{: tsCauses}
`kube-system` 預設服務帳戶沒有您叢集中的管理者存取權。

{: tsResolve}
安裝其中一個 Built-in Insights 供應項目之前，您必須安裝 Helm。您可以使用 [Kubernetes Service 整合文件](/docs/containers?topic=containers-integrations#helm)來安裝 Helm。


## 已知問題：未顯示部分 Network Insights 發現項目
{: #ts-network-analytics}

{: tsSymptoms}
當您透過儀表板或 API 來尋找 Network Insights 時，看不到所有發現項目類型。

{: tsCauses}
有些 Network Insights 發現項目類型只適用於 IBM Cloud Kubernetes Service 1.10 版或更舊版本。

{: tsResolve}
請使用 Kubernetes Service 1.10 版或更舊版本。
