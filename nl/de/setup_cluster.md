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

# Überwachung verdächtiger Clients und Server-IP-Adressen für einen Kubernetes-Cluster einrichten
{: #cluster_install}

Zum Testen des Network Analytics-Features installieren Sie die {{site.data.keyword.security-advisor_short}}-Komponenten in einem Cluster, der in {{site.data.keyword.containerlong_notm}} bereitgestellt wurde. Daraufhin werden Netzerkenntnisse und -alerts im {{site.data.keyword.security-advisor_short}}-Dashboard angezeigt.
{:shortdesc}

Dieses Feature von {{site.data.keyword.security-advisor_short}} ist eine Technologievorschau.
{: tip}

Möchten Sie mehr über Network Analytics in {{site.data.keyword.security-advisor_short}} erfahren? [Dann schauen Sie sich die Dokumentation an](network-analytics.html).


## Voraussetzungen
{: #cluster_prereqs}

Vorbereitende Schritte:

* Mac-, Linux- oder Windows 10-Entwicklerworkstation
  * Windows 10: [Linux-Subsystemfeature aktivieren](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
* [Node.js](https://nodejs.org/en/) 6 oder höher
* [jQ](https://stedolan.github.io/jq/download/)
* [{{site.data.keyword.Bluemix_notm}}-Befehlszeilenschnittstelle](https://console.bluemix.net/docs/cli/reference/bluemix_cli/get_started.html#getting-started) Version 0.6.5 oder höher
* [{{site.data.keyword.containerlong_notm}}-CLI-Plug-in](https://console.bluemix.net/docs/containers/cs_cli_install.html#cs_cli_install)
* [Kubernetes-CLI (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) Version 1.7 oder höher
* [Kubernetes Helm (Paketmanager)](https://docs.helm.sh/using_helm/#from-script)
* [Ein freier oder Standard-Kubernetes-Cluster](https://console.bluemix.net/containers-kubernetes/catalog/cluster) in der Region **US South** (Vereinigte Staaten, Süden) von {{site.data.keyword.Bluemix_notm}}
* Geben Sie den {{site.data.keyword.Bluemix_notm}}-Kontoeigner an, der die Installationsschritte ausführen soll.

## Beim Cluster anmelden
{: #login}

1.  Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an.

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  Listen Sie alle Cluster im Konto auf, um den Namen des Clusters abzurufen.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  Geben Sie den Cluster in der Befehlszeilenschnittstelle als Ziel an.

    ```
    ibmcloud ks cluster-config <Clustername-oder--ID>
    ```
    {: pre}

4.  Definieren Sie den Pfad zur lokalen Kubernetes-Konfigurationsdatei als Umgebungsvariable. Beispiel:

    ```
    export KUBECONFIG=/Users/<Benutzername>/.bluemix/plugins/container-service/clusters/<Clustername>/kube-config-prod-dal10-<Clustername>.yml
    ```
    {: pre}

5.  Richten Sie Helm im Cluster ein.

    ```
    helm init
    ```
    {: pre}

Lassen Sie dieses Befehlszeilenfenster geöffnet und fahren Sie fort.{: tip}

## Security Advisor-Komponenten installieren
{: #cluster_components}

Das Installationsprogramm muss vom {{site.data.keyword.Bluemix_notm}}-Kontoeigner ausgeführt werden.
{: tip}

Bei der Konfiguration von {{site.data.keyword.security-advisor_short}} finden die folgenden Aktionen statt:
1. Clustermetadaten werden erfasst und für die Installation der folgenden Elemente verwendet: IP-Adressen des Workerknotens, Teilnetze, Ingress-URL und -IP-Adresse, Cluster-CRN, Log Analysis-Endpunkt, Bereich und Token.
2. Eine IAM-Service-ID und ein IAM-API-Schlüssel werden in Ihrem {{site.data.keyword.Bluemix_notm}}-Konto erstellt, so dass Ihr Cluster von Security Advisor identifiziert werden kann. Wenn Sie über mehrere Cluster verfügen, führen Sie die Installation für jeden Cluster aus, um Service-IDs und API-Schlüssel für jeden Cluster zu generieren.
3. Die {{site.data.keyword.security-advisor_short}}-Komponenten werden im Namensbereich **monitoring** in Ihrem Cluster bereitgestellt.

<br/>
Gehen Sie wie folgt vor, um die {{site.data.keyword.security-advisor_short}}-Komponenten zu installieren:

1.  Laden Sie das [Installationspaket](https://github.com/IBM-Bluemix-Docs/security-advisor/blob/master/installation.tar.gz?raw=true) herunter und extrahieren Sie es.
2.  Navigieren Sie in demselben Befehlszeilenfenster wie im vorherigen Abschnitt zu der Position des extrahierten Archivs und installieren Sie das Paket.

    ```
    ./install.sh
    ```
    {: pre}

3.  Überprüfen Sie im [{{site.data.keyword.security-advisor_short}}-Dashboard](https://console.bluemix.net/security-advisor/#/dashboard), ob die Komponenten ordnungsgemäß installiert wurden.

## {{site.data.keyword.security-advisor_short}}-Komponenten entfernen
{: #cluster_uninstall}

Entfernen Sie die {{site.data.keyword.security-advisor_short}}-Komponenten aus Ihrem Cluster sowie die Service-ID und den API-Schlüssel aus Ihrem {{site.data.keyword.Bluemix_notm}}-Konto.

1.  Melden Sie sich bei {{site.data.keyword.Bluemix_notm}} an.

    ```
    ibmcloud login -a https://api.ng.bluemix.net --sso
    ```
    {: pre}

2.  Listen Sie alle Cluster im Konto auf, um den Namen des Clusters abzurufen.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

3.  Geben Sie den Cluster in der Befehlszeilenschnittstelle als Ziel an.

    ```
    ibmcloud ks cluster-config <Clustername-oder--ID>
    ```
    {: pre}

4.  Definieren Sie den Pfad zur lokalen Kubernetes-Konfigurationsdatei als Umgebungsvariable. Beispiel:

    ```
    export KUBECONFIG=/Users/<Benutzername>/.bluemix/plugins/container-service/clusters/<Clustername>/kube-config-prod-dal10-<Clustername>.yml
    ```
    {: pre}

5.  Navigieren Sie zu der Position des extrahierten Archivs und führen Sie das Deinstallationsscript aus.

    ```
    ./uninstall.sh
    ```
    {: pre}

6.  Deinstallieren Sie wahlweise die Helm-Serverkomponente im Cluster.

    ```
    helm reset
    ```
    {: pre}
