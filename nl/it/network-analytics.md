---

copyright:
  years: 2018
lastupdated: "2018-11-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}

# Analisi di rete 
{: #network-analytics}

Con {{site.data.keyword.security-advisor_long}}, puoi ottenere informazioni approfondite sulla comunicazione di rete potenzialmente rischiosa correlata ai tuoi cluster {{site.data.keyword.containerlong_notm}}. Per visualizzare un'anteprima di questa funzionalità di analisi di rete, seleziona **Preview** dalla [pagina Introduzione](https://console.bluemix.net/security/advisor/#!/overview) a {{site.data.keyword.security-advisor_short}}.
{: shortdesc}

La funzione di anteprima dell'analisi di rete è composta da tre moduli:

1. **Agent di raccolta del flusso di rete**: installato nel tuo cluster, l'agent raccoglie le informazioni di rete e le invia al backend di analisi. Leggi ulteriori informazioni sulla [raccolta dei dati](#data-collection).

2. **Backend di analisi**: il backend di analisi identifica la comunicazione sospetta in entrata e in uscita dal cluster. Utilizza il rilevamento delle minacce offerto da [IBM X-Force![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/security/xforce) per interpretare le informazioni di rete. Quando viene identificata una comunicazione sospetta, il backend di analisi monitora quelle comunicazioni e trasmette le ricerche sulla sicurezza al dashboard {{site.data.keyword.security-advisor_short}}.

3. **Dashboard {{site.data.keyword.security-advisor_short}}**: le ricerche e i KPI dal backend di analisi sono presentati in due schede:

   - **Client sospetti (traffico in entrata)**: questa scheda mostra i KPI e le ricerche che riguardano i client esterni sospetti che accedono al cluster, come ad esempio quando un membro di una botnet si avvicina a uno degli IP del cluster. Ulteriori informazioni sui [client sospetti](#suspicious-clients).
   - **IP server sospetti (traffico in uscita)**: questa scheda mostra i KPI e le ricerche che hanno [IP server sospetti](#suspicious-server-ips) che vengono eseguiti come parte del cluster. Esempio: quando il cluster si avvicina a un server sospetto per la distribuzione di malware.


## Raccolta dei dati 
{: #data-collection}

{{site.data.keyword.security-advisor_short}} può essere utile per proteggere il tuo cluster aggiungendo il monitoraggio della rete. Distribuito come parte del tuo cluster, il monitoraggio di rete viene utilizzato per fornire informazioni sulle comunicazioni del cluster. Per abilitare le analisi di rete, vengono raccolte le informazioni sul flusso di rete della comunicazione che si svolge tra il cluster e le entità esterne.
{: shortdesc}

Vengono raccolte le seguenti informazioni:

* L'indirizzo IP dei peer che stanno comunicando
* Le porte che utilizzano
* La serie dei protocolli che vengono utilizzati
* La quantità di dati che viene trasferita
* Una serie di parametri specifici del protocollo
* Varie date/ore.

**I dati effettivi che vengono scambiati non vengono raccolti**.

Il monitoraggio di rete viene utilizzato all'interno del cluster con altri servizi come ad esempio {{site.data.keyword.loganalysisshort_notm}} in modo che ti puoi concentrare sulla logica aziendale. Puoi controllare le informazioni approfondite sul monitoraggio di rete nel tuo dashboard {{site.data.keyword.security-advisor_short}}.


## Client sospetti (traffico in entrata) 
{: #suspicious-clients}

Il dashboard {{site.data.keyword.security-advisor_short}} include una scheda del client sospetto che riepiloga le varie informazioni sulle comunicazioni in cui il cluster agisce come un server che viene avvicinato da un client esterno.
{: shortdesc}

La comunicazione analizzata potrebbe produrre una ricerca come ad esempio:

- Un client esterno con bassa reputazione, come ad esempio un client noto per distribuire malware o associato a servizi anonimi, si avvicina ad uno degli URL o IP associati al cluster. Ad esempio, uno scanner si avvicina a una delle porte aperte del cluster. Tale comunicazione potrebbe suggerire una protezione inadeguata degli URL e degli IP associati al cluster ed anche dei rischi effettivi o in attesa.


## IP server sospetti (traffico in uscita) 
{: #suspicious-server-ips}

Il dashboard {{site.data.keyword.security-advisor_short}} include una scheda dell'IP server sospetto che riepiloga le varie informazioni sulle comunicazioni in cui il cluster agisce come un client che si avvicina a un server esterno.
{: shortdesc}

La comunicazione analizzata potrebbe produrre una ricerca come ad esempio:

- Un client dall'interno del cluster, come ad esempio la distribuzione del carico di lavoro, si avvicina a un nodo esterno con una bassa reputazione, come ad esempio una botnet. Tale comunicazione potrebbe suggerire che il carico di lavoro è compromesso e richiede attenzione da parte del tuo team di sicurezza. La ricerca varia in base alla reputazione del nodo esterno avvicinato, ai modelli di comunicazione e al livello di attendibilità offerto dal rilevamento.

- L'URL del cluster o uno dei suoi IP ha una bassa reputazione. Controlla una bassa reputazione per verificare che il cluster non sia compromesso o impegnato in attività non accettabili.
