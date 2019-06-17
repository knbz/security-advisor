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

Mit {{site.data.keyword.security-advisor_long}} können Sie unter Verwendung von maschinellem Lernen, erlernten Mustern und Informationen zu Sicherheitsbedrohungen das Verhalten überwachen, um potenziell beeinträchtigte Container zu ermitteln, deren Ausführung in Ihren {{site.data.keyword.containerlong_notm}}-Clustern erfolgt.
{: shortdesc}

Ab dem 20. Januar 2019 ersetzt Network Insights (Betaversion) das Network Analytics-Feature. Es werden alle Analysekarten in Ihrem Service-Dashboard gelöscht, doch die Untersuchungsergebnisse bleiben weiterhin in der Datenbank für Untersuchungsergebnisse erhalten.
{: important}

## Vorbereitende Schritte
{: #network-prereq}

Wenn Sie gegenwärtig das Network Analytics-Feature verwenden, müssen Sie die [Servicekomponenten löschen](/docs/services/security-advisor?topic=security-advisor-setup-network#uninstall-analytics), bevor Network Insights installiert werden kann. Um Network Insights verwenden zu können, müssen Sie sicherstellen, dass die folgenden Voraussetzungen erfüllt sind.

- Wenn Sie unter Windows 10 arbeiten, ist es erforderlich, das Windows-Subsystem für Linux zu aktivieren und eine [Ubuntu-Shell](https://win10faq.com/install-run-ubuntu-bash-windows-10/){: external} zu installieren.
- yq-CLI:
  * Installieren Sie die yq-CLI für [macOS und Windows 10](http://mikefarah.github.io/yq/){: external}.
  * Führen Sie für CentOS, Red Hat und Ubuntu die folgenden Befehle aus, um Version 1.15 zu installieren:
    ```
    wget https://github.com/mikefarah/yq/releases/download/1.15.0/yq_linux_amd64       
    mv yq_linux_amd64 yq   
    chmod +x yq    
    mv yq /usr/local/bin/     
    yq -V
    ```
    {: codeblock}     
- Aktualisierte cURL-Binärdatei: Für CentOS und Red Hat können Sie die Aktualisierung durch Ausführen von `yum update -y nss curl libcurl` vornehmen.
- [{{site.data.keyword.cloud_notm}}-CLI und erforderliche Plug-ins](/docs/cli?topic=cloud-cli-ibmcloud-cli#ibmcloud-cli)
- [Kubernetes-CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/){: external} der Version 1.10.11 oder höher
- [Kubernetes Helm (Paketmanager)](/docs/containers?topic=containers-helm) der Version 2.9.0 oder höher
- Standardmäßiger Kubernetes-Cluster der Version 1.10.11 oder höher

## COS-Bucket erstellen
{: #network-setup-cos}

Durch Verwendung der {{site.data.keyword.security-advisor_short}}-GUI können Sie eine neue COS-Instanz und ein entsprechendes Bucket erstellen.

1. Schalten Sie im Service-Dashboard auf der Registerkarte **Integrationen** die Option **Analyse inaktiviert** im Feld 'Network Insights' zu **Analyse aktiviert** um.

2. Klicken Sie auf **Zur Konfiguration wechseln**.

3. Klicken Sie im Abschnitt für Voraussetzungen auf **COS-Instanz und -Bucket erstellen**. Ihre COS-Instanz und das zugehörige Bucket werden unter Beachtung der entsprechenden Namenskonvention und mit IAM-Berechtigungen automatisch erstellt.

Wenn Sie über eine bereits vorhandene Instanz von COS mit zugehörigem Bucket verfügen, stellen Sie sicher, dass diese die folgende Namenskonvention verwendet: `sa.<konto-ID>.telemetric.<COS-region>`. Damit der Service die in Ihrer COS-Instanz gespeicherten Daten lesen kann, ist es erforderlich, mit {{site.data.keyword.cloud_notm}} IAM die [Service-zu-Service-Autorisierung](/docs/iam?topic=iam-serviceauth) einzurichten. Legen Sie für `source` `{{site.data.keyword.security-advisor_short}}` als Quelle und für `target` Ihre COS-Instanz als Ziel fest. Weisen Sie die IAM-Rolle `Leseberechtigter` (Reader) zu.


## {{site.data.keyword.security-advisor_short}}-Komponenten installieren
{: #network-install-components}

Sie können einen Agenten installieren, um Netzflussprotokolle aus Ihrem Kubernetes-Cluster zu erfassen. Die Protokolle werden in Ihrer Cloud Object Storage-Instanz gespeichert. Dann können Sie Network Insights aktivieren, damit Ihre Protokolle analysiert sowie verdächtige Netzwerkaktivitäten erkannt und an Sie gemeldet werden.
{: shortdesc}

Stellen Sie sicher, dass Sie die Installation für jeden Cluster wiederholen, der überwacht werden soll.
{: note}

1. Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-CLI an. Gehen Sie gemäß den Eingabeaufforderungen in der CLI vor, um die Anmeldung abzuschließen.

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Region</th>
      <th>Endpunkt</th>
    </tr>
    <tr>
      <td>Vereintes Königreich</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>Vereinigte Staaten (Süden)</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

2. Legen Sie den Kontext für Ihren Cluster fest.

  1. Rufen Sie den Befehl zum Festlegen der Umgebungsvariablen ab und laden Sie die Konfigurationsdateien für Kubernetes herunter.

    ```
    ibmcloud ks cluster-config <clustername_oder_-ID>
    ```
    {: codeblock}

  2. Kopieren Sie die Ausgabe ab einschließlich `export` und fügen Sie sie in Ihrem Terminal ein, um die Umgebungsvariable `KUBECONFIG` festzulegen.

3. Ermitteln Sie die Version Ihres Kubernetes-Clusters.

  ```
  kube_version=$(kubectl version --output json) echo $(echo $kube_version | yq r - serverVersion.major).$(echo $kube_version | yq r - serverVersion.minor)
  ```
  {: codeblock}

3. Bei der Verwendung von v1.12.x des Kubernetes-Service installieren Sie Helm mithilfe der folgenden Befehle. Ziehen Sie andernfalls die Kubernetes-Dokumentation zurate; hier finden Sie Informationen zu den Installationsschritten, die Sie ausführen müssen, damit [Helm in einem Cluster ohne öffentlichen Zugriff eingerichtet wird](/docs/containers?topic=containers-helm#public_helm_install).

  1. Löschen Sie alle bereits vorhandenen Bereitstellungen.

    ```
    kubectl delete deployment tiller-deploy -n kube-system
    ```
    {: codeblock}

  2. Wenden Sie die Tiller-RBAC-Richtlinien auf Ihre Bereitstellung an.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml
    ```
    {: codeblock}
  
  3. Initialisieren Sie Helm in Ihrem Cluster und installieren Sie Tiller in Ihrem Cluster.

    ```
    helm init --service-account tiller
    ```
    {: codeblock}

  4. Prüfen Sie, ob Ihre Installation erfolgreich war; stellen Sie hierfür sicher, dass der Status des Pods `tiller-deploy` `running` lautet.

    ```
    kubectl get pods -n kube-system -l app=helm
    ```
    {: codeblock}

4. Stellen Sie sicher, dass Ihr Kubernetes-Service Version 1.11 oder eine höhere Version aufweist und klonen Sie anschließend das folgende Repository auf Ihr lokales System.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-network-insights.git
  ```
  {: codeblock}

  Version 1.10 des Kubernetes-Service ist veraltet und wird nicht unterstützt. Wenn Sie bereits v1.10+ installiert haben, beheben Sie die Schwachstellen im vorhandenen Image; starten Sie hierfür die Analysefunktionspods erneut, indem Sie den folgenden Helm-Befehl ausführen: `helm upgrade --recreate-pods network-insights`.
  {: deprecated}

5. Wechseln Sie in den Ordner `security-advisor-network-insights`.

6. Wechseln Sie in das Verzeichnis `v1.10+`.

7. Extrahieren Sie die`.tar`-Datei durch Ausführen des folgenden Befehls:

  ```
  tar -xvf security-advisor-network-insights.tar
  ```
  {: codeblock}

8. Wechseln Sie in den Ordner `security-advisor-network-insights`.

9. Installieren Sie Helm unter Berücksichtigung der [Dokumente zur Integration des Kubernetes-Service](/docs/containers?topic=containers-helm).

10. Optional: [Aktivieren Sie TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. Wenn Sie Ihre Workstation für die Abwicklung der Installation von Analysekomponenten in mehreren Clustern verwenden und TLS aktiviert ist, müssen Sie sicherstellen, dass die TLS-Konfigurationen auf dem neuesten Stand sind und mit dem aktuellen Cluster übereinstimmen, in dem Sie die Komponenten installieren möchten.

11. Führen Sie den folgenden Befehl aus, um das Helm-Diagramm und seine Abhängigkeiten zu installieren. Der Befehl überprüft, ob Ihr Bucket die richtige Namenskonvention verwendet, erstellt geheime Schlüssel für Kubernetes, aktualisiert die Werte mit Ihrer Cluster-GUID und stellt das Helm-Diagramm von Network Insights bereit. Falls Sie einen Fehler feststellen, versuchen Sie, den Befehl `helm init --upgrade` auszuführen.

  ```
  ./network-insight-install.sh <COS-region> <COS-API-schlüssel>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Variable</th>
      <th>Beschreibung</th>
    </tr>
    <tr>
      <td><code>COS-region</code></td>
      <td>Die Region, in der Ihre COS-Instanz bereitgestellt wird. Folgende Optionen sind möglich: <code>us-south</code> und <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>COS-API-schlüssel</code></td>
      <td>Der [API-Schlüssel](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials), den Sie zum Zugreifen auf Ihre COS-Instanz und das Bucket erstellt haben. Dem Schlüssel muss die Plattformrolle des Autors (`Writer`) zugeordnet sein.</td>
    </tr>
  </table>

Sie haben Ihren Cluster erfolgreich für die Zusammenarbeit mit {{site.data.keyword.security-advisor_short}} Network Insights konfiguriert!



## Komponenten löschen
{: #network-delete}

Wenn Sie Network Insights nicht mehr benötigen, können Sie die Servicekomponenten aus Ihrem Cluster löschen.
{: shortdesc}

1. Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-CLI an. Gehen Sie gemäß den Eingabeaufforderungen in der CLI vor, um die Anmeldung abzuschließen.

  ```
  ibmcloud login -a cloud.ibm.com -r <region>
  ```
  {: codeblock}

  <table>
    <tr>
      <th>Region</th>
      <th>Endpunkt</th>
    </tr>
    <tr>
      <td>Vereintes Königreich</td>
      <td><code>eu-gb</code></td>
    </tr>
    <tr>
      <td>Vereinigte Staaten (Süden)</td>
      <td><code>us-south</code></td>
    </tr>
  </table>

2. Legen Sie den Kontext für Ihren Cluster fest.

  1. Rufen Sie den Befehl zum Festlegen der Umgebungsvariablen ab und laden Sie die Konfigurationsdateien für Kubernetes herunter.

    ```
    ibmcloud ks cluster-config <clustername_oder_-ID>
    ```
    {: codeblock}

  2. Kopieren Sie die Ausgabe ab einschließlich `export` und fügen Sie sie in Ihrem Terminal ein, um die Umgebungsvariable `KUBECONFIG` festzulegen.

3. Löschen Sie die Komponenten mit Helm.

  ```
  helm del --purge network-insights [--tls]
  ```
  {: codeblock}

4. Löschen Sie die geheimen Schlüssel für Kubernetes.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}

Stellen Sie sicher, dass Sie den Prozess für jeden Cluster löschen, von dem die Agenten entfernt werden sollen.
{: tip}

## Network Analytics deinstallieren
{: #uninstall-analytics}

Wenn Sie die Betaversion von Network Analytics verwendet haben, müssen Sie zunächst die alten {{site.data.keyword.security-advisor_short}}-Komponenten deinstallieren, bevor Sie die neuen Komponenten installieren können. Stellen Sie sicher, dass Sie diesen Prozess für jeden Cluster wiederholen, der Servicekomponenten enthält.
{: shortdesc}

1. Melden Sie sich bei {{site.data.keyword.cloud_notm}} an.

  ```
  ibmcloud login -a https://api.us-south.ibm.cloud.com --sso
  ```
  {: codeblock}

2. Listen Sie alle Cluster in Ihrem Konto auf, um den Namen des Clusters abzurufen.

  ```
  ibmcloud ks clusters
  ```
  {: codeblock}

3. Legen Sie den Kontext für Ihren Cluster fest.

  1. Rufen Sie den Befehl zum Festlegen der Umgebungsvariablen ab und laden Sie die Konfigurationsdateien für Kubernetes herunter.

    ```
    ibmcloud ks cluster-config <clustername_oder_-ID>
    ```
    {: codeblock}

  2. Kopieren Sie die Ausgabe ab einschließlich `export` und fügen Sie sie in Ihrem Terminal ein, um die Umgebungsvariable `KUBECONFIG` festzulegen.

4. Navigieren Sie zu der Position des extrahierten Archivs und führen Sie das Deinstallationsscript aus.

  ```
  ./uninstall.sh
  ```
  {: codeblock}

5. Deinstallieren Sie wahlweise die Helm-Serverkomponente im Cluster.

  ```
  helm reset
  ```
  {: codeblock}
