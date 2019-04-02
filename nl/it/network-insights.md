---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-14"

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


# Network Insights (beta)
{: #network}

Con {{site.data.keyword.security-advisor_long}}, puoi ottenere delle informazioni approfondite basate su threat intelligence, modelli comportamentali e machine learning per contenitori potenzialmente compromessi eseguiti sui tuoi cluster {{site.data.keyword.containerlong_notm}}.
{: shortdesc}


## Modalità di funzionamento
{: #network-how-it-works}

La funzione Network Insights è un componente aggiuntivo del servizio {{site.data.keyword.security-advisor_short}}. Con la funzione abilitata e configurata, la comunicazione di rete del cluster, sia in entrata che in uscita, tra il tuo cluster ed entità esterne, viene raccolta e continuamente monitorata e analizzata. 

Controlla la seguente immagine per vedere il flusso di informazioni.

![Diagramma del flusso di Network Insights](images/network-insights-flow.png)

1. Come amministratore, puoi installare Network Insights in ogni cluster che vuoi monitorare.
2. I log del flusso di rete vengono inoltrati a un bucket Cloud Object Storage in cui vengono archiviati finché non decidi di eliminarli. Quando utilizzi la GUI {{site.data.keyword.security-advisor_short}} per creare il bucket, vengono assegnati dei ruoli IAM da servizio a servizio in modo che il servizio possa visualizzare i log.
3. Con Network Insights abilitato, i dati non elaborati nel tuo bucket COS vengono analizzati alla ricerca di comportamenti sospetti.
4. Quando viene segnalato un possibile problema di sicurezza, la ricerca viene inoltrata al database delle ricerche.
5. Le ricerche vengono visualizzate nel tuo dashboard del servizio su tre schede: 
  * Traffico in entrata sospetto
  * Traffico in uscita sospetto
  * Anomalie nel traffico


## Raccolta di dati
{: #network-data}

{{site.data.keyword.security-advisor_short}} fornisce un raccoglitore utilizzato per raccogliere le informazioni sul flusso di rete che si svolge tra un cluster ed entità esterne.
{: shortdesc}

I dati non elaborati che vengono raccolti sono archiviati in un bucket Cloud Object Storage dove puoi determinare la durata dell'archiviazione. Gestisci e controlli i dati raccolti, il che significa che sei responsabile della loro archiviazione, protezione ed eliminazione. {{site.data.keyword.security-advisor_short}} conserva le ricerche per 90 giorni. Durante tale periodo, i risultati vengono presentati sulle schede Network Insights nel dashboard del servizio. Pertanto, anche se non visualizzerai più la ricerca nel tuo dashboard dopo 90 giorni, i dati non elaborati potrebbero ancora essere presenti nell'archivio.

Vengono raccolte le seguenti informazioni:

* L'indirizzo IP dei peer che stanno comunicando
* Le porte che utilizzano
* La serie dei protocolli che vengono utilizzati
* La quantità di dati che viene trasferita
* Una serie di parametri specifici del protocollo
* Varie date/ore.

**I dati effettivi che vengono scambiati non vengono raccolti.**

Da un punto di vista della sicurezza, è una buona idea eliminare i tuoi dati raccolti quando i requisiti legali e aziendali consentono di eliminarli. Per ulteriori informazioni, vedi [Eliminazione degli oggetti](/docs/services/cloud-object-storage/info?topic=cloud-object-storage-security#deletion).
{: tip}


## Rete: Traffico in entrata sospetto
{: #network-suspicious-inbound}

{{site.data.keyword.security-advisor_short}} ti informa dei tentativi effettuati da client esterni di sondare e compromettere i cluster sulla scheda **Traffico in entrata sospetto** nel dashboard del servizio.
{: shortdesc}


I modelli comportamentali dei client classificati da IBM X-Force come distributori di malware utilizzati come scanner, che fanno parte di una botnet, per l'estrazione della criptovaluta o per i servizi di anonimazione, vengono tutti monitorati in modo continuo. Se questo tipo di client si avvicina al tuo cluster e mostra l'approccio di un cluster monitorato e di un comportamento allarmante, Network Insights emette una ricerca.


La scheda introduce due KRI (Key Risk Indicator):

* Ricognizione da client sospetti: include le ricerche correlate ai client che accedono al cluster.
* Payload grandi in modo anomalo inviati da client sospetti: include le ricerche correlate ai volumi di dati che vengono inviati tra i client e il cluster. Un payload anomalo è qualsiasi cosa che è inusuale per il tuo cluster.


Le ricerche potrebbero includere i client sospetti che:

* Inviano in modo anomalo grandi quantità di dati nel cluster.
* Eseguono un numero anomalo di richieste a un servizio del cluster.
* Si presentano al cluster in modo insolito con molti tentativi eseguiti per sondare il cluster.



## Rete: Traffico in uscita sospetto
{: #network-suspicious-outbound}

{{site.data.keyword.security-advisor_short}} ti informa di tutti i contenitori potenzialmente compromessi eseguiti sui tuoi cluster {{site.data.keyword.containershort_notm}} nella scheda **Traffico in uscita sospetto** nel dashboard del servizio.
{: shortdesc}

Il servizio monitora in modo continuo i modelli comportamentali dei contenitori che accedono ai client classificati da IBM X-Force come distributori di malware utilizzati come scanner, che fanno parte di una botnet, per l'estrazione della criptovaluta o per i servizi di anonimazione. Dopo che un contenitore su un cluster monitorato si avvicina a peer sospetti e presenta un comportamento allarmante, Network Insights emette una ricerca.

La scheda introduce due KRI (Key Risk Indicator):

* Approcci in uscita a server sospetti: ricerche correlate a un cluster che accede ai server.
* Payload grandi in modo anomalo scambiati con server sospetti: ricerche correlate ai volumi di dati che vengono inviati tra il cluster e i server.


Le ricerche potrebbero includere i contenitori che:

* inviano in modo anomalo grandi quantità di dati a un server sospetto 
* estraggono una grande quantità di dati da un server sospetto
* eseguono molte richieste a un server sospetto


## Rete: Anomalie nel traffico (sperimentale)
{: #network-anomalies}

In questa funzione sperimentale, {{site.data.keyword.security-advisor_short}} monitora la tua rete in cerca di informazioni sui modelli comportamentali. Dopo che i modelli sono stati creati, qualsiasi comportamento che sembra essere anomalo viene contrassegnato e riepilogato nel dashboard del servizio nella scheda **Anomalie nel traffico**.
{: shortdesc}

Un modello di un comportamento del contenitore normale viene creato monitorando i modelli comportamentali tra un contenitore e i propri peer. Quando viene acquisito il modello, viene monitorato alla ricerca di modifiche specifiche. Se viene presentata una modifica allarmate, Network Insights emette una ricerca.

La scheda introduce due KRI (Key Risk Indicator):

* Attività di scambio di dati o di ricognizione più elevata del normale: ricerche correlate a interazioni anomale rilevate tra il cluster e un qualsiasi peer esterno.
* Approccio in uscita a un nuovo server: ricerche correlate a nuovi server rilevati a cui si avvicina il cluster.

Una ricerca può includere:  

* contenitori che si avvicinano a server a cui non si sono mai avvicinati prima
* contenitori che inviano o utilizzano molti più dati del normale da o verso peer specifici
* uno specifico livello di rilevamento aumenta in modo significativo

 Dopo che il modello è stato sviluppato, vengono rilevate le deviazioni dal modello acquisito e quando si presenta una modifica allarmante, Network Insights pubblica una ricerca sul dashboard di Security Advisor. Le ricerche sono riepilogate nella scheda 'Rete: Anomalie nel traffico'. La scheda introduce due KRI (Key Risk Indicator). Il KRI 'Attività di scambio di dati o di ricognizione più elevata del normale' conta le ricerche correlate alle interazioni anomale rilevate tra il cluster e i peer esterni, mentre il KRI 'Approccio in uscita a un nuovo server' conta le ricerche correlate ai nuovi server rilevati a cui si avvicina il cluster.  

## Passi successivi
{: #network-next}

Pronto per iniziare? Consulta [Abilitazione di Network Insights](/docs/services/security-advisor/setup-network.html)!
