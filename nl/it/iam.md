---

copyright:
  years: 2017, 2018
lastupdated: "2018-12-07"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}


# Gestione dell'accesso al servizio
{: #service-access}

Come proprietario di un account, puoi gestire l'accesso alle istanze di {{site.data.keyword.security-advisor_long}}, utilizzando {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM). Configurando le politiche nel tuo account creando diversi livelli di accesso per utenti differenti, puoi garantire che ogni istanza di {{site.data.keyword.security-advisor_short}} sia sicura.
{: shortdesc}

Per ulteriori informazioni su IAM, consulta [Accesso IAM](/docs/iam/users_roles.html).

## Politiche di accesso {{site.data.keyword.security-advisor_short}}
{: #access}

Ad ogni utente che accede a un'istanza del servizio {{site.data.keyword.security-advisor_short}} nel tuo account deve essere assegnata una politica di accesso con un ruolo utente IAM definito. La politica determina le azioni che un utente può eseguire all'interno del contesto di quella specifica istanza del servizio.
{: shortdesc}

Le azioni sono personalizzate e definite dal servizio {{site.data.keyword.Bluemix_notm}} come operazioni consentite nel servizio. Le azioni sono poi associate ai ruoli utente del servizio IAM. Nella seguente tabella, le azioni e le autorizzazioni richieste per {{site.data.keyword.security-advisor_short}} sono associate.

<table><caption>Quali ruoli possono eseguire quali azioni?</caption>
  <col width="40%">
  <col width="40%">
  <col width="20%">
  <tr>
    <th>Azione</th>
    <th>Spiegazione</th>
    <th>Ruolo</th>
  </tr>
  <tr>
    <td><code>security-advisor.findings.read</code></td>
    <td>Visualizza le ricerche del report di sicurezza.</td>
    <td>Lettore</br>Scrittore</br>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.write</code></td>
    <td>Genera le ricerche del report di sicurezza.</td>
    <td>Scrittore</br>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.delete</code></td>
    <td>Elimina le ricerche del report di sicurezza.</td>
    <td>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.findings.update</code></td>
    <td>Modifica le ricerche del report di sicurezza.</td>
    <td>Scrittore</br>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.read</code></td>
    <td>Visualizza i metadati.</td>
    <td>Lettore</br>Scrittore</br>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.delete</code></td>
    <td>Elimina i metadati.</td>
    <td>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.write</code></td>
    <td>Genera i metadati.</td>
    <td>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.metadata.update</code></td>
    <td>Aggiorna i metadati.</td>
    <td>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.read</code></td>
    <td>Leggi le soluzioni personalizzate che sono state aggiunte al servizio.</td>
    <td>Lettore</br>Scrittore</br>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.write</code></td>
    <td>Aggiungi una soluzione personalizzata al servizio.</td>
    <td>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.update</code></td>
    <td>Aggiorna una soluzione personalizzata esistente all'interno del servizio.</td>
    <td>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.custom-solution.delete</code></td>
    <td>Elimina una soluzione personalizzata dal servizio.</td>
    <td>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.read</code></td>
    <td>Visualizza le soluzioni partner che hai aggiunto alla tua istanza del servizio.</td>
    <td>Lettore</br>Scrittore</br>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.write</code></td>
    <td>Aggiungi una soluzione partner alla tua istanza del servizio.</td>
    <td>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.update</code></td>
    <td>Aggiorna una soluzione partner nella tua istanza del servizio.</td>
    <td>Gestore</td>
  </tr>
  <tr>
    <td><code>security-advisor.partner-solution.delete</code></td>
    <td>Elimina una soluzione partner dalla tua istanza del servizio.</td>
    <td>Gestore</td>
  </tr>
</table>

## Associazioni API
{: #api-map}

Come questi ruoli corrispondono all'API? Controlla la seguente tabella per vedere come le azioni del servizio sono associate all'API.


| Metodo | Endpoint                                                                  |  Azione del servizio                  |
|--------|---------------------------------------------------------------------------|----------------------------------|
| POST   | /v1/{account_id}/graph                                                    | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/notes                            | security-advisor.metadata.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/notes/{note_id}                  | security-advisor.metadata.delete |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}/note | security-advisor.findings.read   |
| POST   | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.write  |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences                      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/notes/{note_id}/occurrences      | security-advisor.findings.read   |
| GET    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.read   |
| PUT    | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.update |
| DELETE | /v1/{account_id}/providers/{provider_id}/occurrences/{occurrence_id}      | security-advisor.findings.delete |
