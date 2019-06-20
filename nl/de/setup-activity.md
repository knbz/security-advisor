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

Mit {{site.data.keyword.security-advisor_long}} können Sie Ihre {{site.data.keyword.cloud_notm}} Activity Tracker-Protokolle fortlaufend überwachen, um unbefugtes oder verdächtiges Verhalten und ebensolche Änderungen in Ihren Ressourcen zu erkennen. Sie können Regelpakete verwenden, die vom Service bereitgestellt werden und auf bewährten Verfahren (Best Practices) basieren, oder Sie können eigene angepasste Regeln erstellen.
{: shortdesc}


## Vorbereitende Schritte
{: #activity-prereq}

Um Activity Insights verwenden zu können, müssen Sie sicherstellen, dass die folgenden Voraussetzungen erfüllt sind.

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
{: #activity-setup-cos}

Durch Verwendung der {{site.data.keyword.security-advisor_short}}-GUI können Sie eine neue COS-Instanz und ein entsprechendes Bucket erstellen.

1. Navigieren Sie im Service-Dashboard zur Registerkarte **Integrationen**.

2. Schalten Sie die Option **Analyse inaktiviert** im Feld 'Activity Insights' zu **Analyse aktiviert** um.

3. Klicken Sie auf **Zur Konfiguration wechseln**.

4. Klicken Sie im Abschnitt für Voraussetzungen auf **COS-Instanz und -Bucket erstellen**. Ihre COS-Instanz und das zugehörige Bucket werden unter Beachtung der entsprechenden Namenskonvention und mit IAM-Berechtigungen automatisch erstellt. Die Bucketinformationen werden angezeigt.

Wenn Sie über eine bereits vorhandene Instanz von COS mit zugehörigem Bucket verfügen, stellen Sie sicher, dass diese die Namenskonvention `sa.<konto-ID>.telemetric.<COS-region>` verwendet. Damit der Service die in Ihrer COS-Instanz gespeicherten Daten lesen kann, ist es erforderlich, mit {{site.data.keyword.cloud_notm}} IAM die [Service-zu-Service-Autorisierung](/docs/iam?topic=iam-serviceauth) einzurichten. Legen Sie für `source` `{{site.data.keyword.security-advisor_short}}` als Quelle und für `target` Ihre COS-Instanz als Ziel fest. Weisen Sie die IAM-Rolle `Leseberechtigter` (Reader) zu.


## {{site.data.keyword.security-advisor_short}}-Komponenten installieren
{: #activity-install-components}

Sie können einen Agenten installieren, um Auditflussprotokolle aus Ihrem {{site.data.keyword.cloud_notm}}-Konto zu erfassen. Die Protokolle werden in Ihrer Cloud Object Storage-Instanz gespeichert, wo Sie Activity Insights aktivieren können, damit Ihre Protokolle auf verdächtiges Verhalten analysiert werden.
{: shortdesc}

1. Klonen Sie das folgende Repository auf Ihrem lokalen System.

  ```
  git clone https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Wechseln Sie in den Ordner `security-advisor-activity-insights`.

3. Extrahieren Sie die`.tar`-Datei durch Ausführen des folgenden Befehls:

  ```
  tar -xvf security-advisor-activity-insights.tar
  ```
  {: codeblock}

4. Wechseln Sie in den Ordner `security-advisor-activity-insights`.
5. Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-CLI an. Gehen Sie gemäß den Eingabeaufforderungen in der CLI vor, um die Anmeldung abzuschließen.

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

6. Legen Sie den Kontext für Ihren Cluster fest.

  1. Rufen Sie den Befehl zum Festlegen der Umgebungsvariablen ab und laden Sie die Konfigurationsdateien für Kubernetes herunter.

    ```
    ibmcloud ks cluster-config <clustername_oder_-ID>
    ```
    {: codeblock}

  2. Kopieren Sie die Ausgabe ab einschließlich `export` und fügen Sie sie in Ihrem Terminal ein, um die Umgebungsvariable `KUBECONFIG` festzulegen.

7. Installieren Sie Helm unter Berücksichtigung der [Dokumente zur Integration des Kubernetes-Service](/docs/containers?topic=containers-helm).

8. Optional: [Aktivieren Sie TLS](https://github.com/helm/helm/blob/master/docs/tiller_ssl.md){: external}. Wenn Sie Ihre Workstation für die Abwicklung der Installation von Analysekomponenten in mehreren Clustern verwenden und TLS aktiviert ist, müssen Sie sicherstellen, dass die TLS-Konfigurationen auf dem neuesten Stand sind und mit dem aktuellen Cluster übereinstimmen, in dem Sie die Komponenten installieren möchten.

9. Führen Sie den folgenden Befehl aus, um Insights zu installieren. Der Befehl überprüft die Namenskonvention Ihres Buckets, erstellt geheime Kubernetes-Schlüssel, aktualisiert die Werte mit Ihrer Cluster-GUID und stellt Activity Insights bereit.

  ```
  ./activity-insight-install.sh <COS-region> <COS-API-schlüssel> <AT-region> <konto-API-schlüssel> <kontobereiche>
  ```
  {: codeblock}

  Falls Sie einen Fehler feststellen, versuchen Sie, den Befehl `helm init --upgrade` auszuführen.
  {: tip}

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
    <tr>
      <td><code>AT-region</code></td>
      <td>Die Region, in der Sie Ihre COS-Instanz und das Bucket erstellt haben. Folgende Optionen sind möglich: <code>us-south</code> und <code>eu-gb</code>.</td>
    </tr>
    <tr>
      <td><code>konto-API-schlüssel</code></td>
      <td>Der Plattform-API-Schlüssel für Ihr {{site.data.keyword.cloud_notm}}-Konto.</td>
    </tr>
    <tr>
      <td><code>kontobereiche</code></td>
      <td>Eine durch Kommas getrennte Liste der Bereichs-GUIDs für ihr {{site.data.keyword.cloud_notm}}-Konto.</td>
    </tr>
  </table>


## Regelpakete zu COS hinzufügen
{: #activity-adding-rules}

Ein Regelpaket ist eine JSON-Datei, die eine Liste von -Regeln enthält, die überwacht werden sollen. Die Regelpakete können Sie wahlweise herunterladen, Sie können aber auch [eigene Regelpakete erstellen](/docs/services/security-advisor?topic=security-advisor-activity#activity-packages). Die {{site.data.keyword.security-advisor_short}}-Engine überprüft, ob die einzelnen Regeln jeweils auch der korrekten Syntax entsprechen.
{: shortdesc}

1. Klonen Sie das folgende Repository, um mehrere voreingestellte Regelpakete zu erhalten. Auf Ihrem lokalen System wird ein Ordner namens `security-advisor-activity-insights` erstellt.

  ```
  https://github.com/ibm-cloud-security/security-advisor-activity-insights.git
  ```
  {: codeblock}

2. Erstellen Sie lokal einen Ordner mit dem Namen `IBM.rules/activities`.

3. Kopieren Sie die JSON-Dateien aus `security-advisor-activity-insights/security-advisor-ata-rule-packages` in `IBM.rules/activities`.

4. Navigieren Sie zu Ihrem {{site.data.keyword.cloud_notm}}-Dashboard und wählen Sie die COS-Serviceinstanz aus, die Activity Insights zugeordnet ist.

5. Wählen Sie im Service-Dashboard auf der Registerkarte **Buckets** das Bucket aus, das Activity Insights zugeordnet ist.

5. Klicken Sie auf der Homepage der COS-Instanz auf **Hochladen** und wählen Sie **Ordner** aus.

6. Wenn eine entsprechende Eingabeaufforderung angezeigt wird, klicken Sie auf **Aspera Connect-Client installieren**. Wird keine derartige Aufforderung angezeigt, ist der Client bereits installiert. Falls Sie den Client installieren mussten, wiederholen Sie nach Abschluss der Installation Schritt 5.

7. Wählen Sie den Ordner *IBM.rules/activities* aus.

8. Klicken Sie auf **Hochladen**.

Möchten Sie Ihre eigenen Pakete verwenden? Verwenden Sie eine der JSON-Dateien als Leitfaden und erstellen Sie Regeln, die den Anforderungen Ihres Unternehmens entsprechen. Nachdem Sie die Datei erstellt haben, fügen Sie sie in Ihrer COS-Instanz zum Ordner *IBM.rules/activities* hinzu. Weitere Informationen zu den einzelnen Typen von Regeln und Formatierungen enthält [Regelpakete verstehen](/docs/services/security-advisor?topic=security-advisor-activity).
{: tip}


## Komponenten löschen
{: #activity-delete}

Wenn Sie Activity Insights nicht mehr benötigen, können Sie die Servicekomponenten aus Ihrem Cluster löschen.
{: shortdesc}

1. Löschen Sie die Servicekomponenten mit Helm. Wenn TLS aktiviert ist, stellen Sie sicher, dass Sie das Flag `-tls` verwenden.

  ```
  helm del --purge activity-insights [--tls]
  ```
  {: codeblock}

2. Löschen Sie die geheimen Schlüssel für Kubernetes.

  ```
  kubectl delete ns security-advisor-insights
  ```
  {: codeblock}
