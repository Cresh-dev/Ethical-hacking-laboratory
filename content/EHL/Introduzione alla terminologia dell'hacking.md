_Tag:_ #cybersecurity  #hacking

---
Per operare nel settore, dobbiamo stabilire delle definizioni chiare per i concetti cardine:

- **Asset**: Qualsiasi risorsa o dato che deve essere protetto.
- **Vulnerabilità**: Una falla o debolezza intrinseca in un asset.
- **Minaccia (Threat)**: Un potenziale pericolo che potrebbe sfruttare una vulnerabilità.
- **Exploit**: Lo strumento o metodo utilizzato per trarre vantaggio da una vulnerabilità.
- **Rischio**: L'impatto derivante dalla compromissione di un asset, calcolato formalmente tramite l'equazione: $Risk=Threat×Vulnerabilities×Impact$

## Vulnerability Assessment (VA) vs. Penetration Test (PT)

Poniamo l'attenzione sulla differenza tra queste due attività:

- Il [[Vulnerability Assessment|Vulnerability Assessment]] è una ==scansione sistematica e ampia, volta a identificare tutte le potenziali debolezze di un sistema senza necessariamente sfruttarle. Utilizza strumenti come Nessus, OpenVas o Nmap==.
- Il [[Penetration Test|Penetration Test]] è invece un ==attacco simulato, più intrusivo e mirato, che mira a dimostrare se una vulnerabilità può essere effettivamente sfruttata per compromettere l'asset==.

### Vulnerability assessment

Come già detto in precedenza il Vulnerability assessment è una scansione che ha l'obiettivo di identificare gli assets che sono esposti ad attacchi e altre entità che possono causare un danno potenziale. 

#### Security process

Il cuore operativo della sicurezza si articola in un processo ciclico composto da quattro fasi fondamentali:

1. **Vulnerability Identification (Testing):** Identificazione sistematica delle debolezze tramite scansioni automatizzate, penetration test e analisi del codice, allineandosi a framework come **OWASP Testing Guide** o **NIST SP 800-115**.
2. **Vulnerability Analysis:** Analisi delle cause profonde (Root Cause) e dei componenti influenzati. Si utilizzano classificazioni come **CWE** (Common Weakness Enumeration), modelli di minaccia come **STRIDE** e mappature verso le tecniche **MITRE ATT&CK**.
3. **Risk Assessment:** Prioritizzazione delle vulnerabilità. Vengono usati modelli come **CVSS** (Common Vulnerability Scoring System) o framework ISO/IEC 27005. Per contesti minori, si cita il modello **DREAD** ($Damage, Reproducibility, Exploitability, Affected Users, Discoverability$).
4. **Remediation:** Fase finale di chiusura delle falle di sicurezza (patching, configurazione, aggiornamento).

Il risultato finale di una VA non è un sistema già messo in sicurezza, ma un **Documento di Analisi**. Questo report contiene:

- L'elenco delle vulnerabilità scoperte.
- La gravità di ogni falla (spesso calcolata tramite il punteggio **CVSS**).
- I suggerimenti tecnici affinché il cliente possa risolvere i problemi in autonomia.

#### Tipi di VA

- **Network-based scans (Scansioni di rete):** Si concentrano sull'infrastruttura. Servono a identificare sistemi vulnerabili sia su reti cablate che wireless, cercando falle che potrebbero essere sfruttate per attacchi di rete.
- **Host-based scans (Scansioni basate sull'host):** Invece di guardare la rete "dall'esterno", queste analizzano i singoli server o workstation. Offrono una visione profonda delle configurazioni interne e della **cronologia delle patch** (per vedere cosa non è stato aggiornato).
- **Wireless network scans (Scansioni di reti wireless):** Analizzano i punti di accesso (Access Point). Servono a trovare "rogue access points" (punti di accesso non autorizzati o maligni) e a verificare che la crittografia Wi-Fi sia configurata bene.
- **Application scans (Scansioni di applicazioni):** Testano il software e i siti web per trovare bug di programmazione o configurazioni errate che potrebbero esporre dati sensibili.
- **Database scans (Scansioni di database):** Si focalizzano sui server dove risiedono i dati (es. SQL). L'obiettivo è prevenire attacchi distruttivi come la **SQL Injection**.
- **Web application scanners:** Strumenti specifici che simulano modelli di attacco noti contro le applicazioni web per vedere come reagiscono.
- **Protocol scanners:** Cercano protocolli obsoleti o vulnerabili (come un vecchio protocollo di trasferimento file non criptato), porte aperte non necessarie e servizi di rete a rischio.
- **Network scanners (Mappatura):** Aiutano a visualizzare l'intera rete. Servono a scovare anomalie come indirizzi IP "vaganti", pacchetti contraffatti (_spoofed_) o traffico sospetto proveniente da un singolo host.

#### Tools

| **Strumento** | **Tipo / Modello**       | **Funzionalità Principale** | **Caratteristiche Chiave**                                                           |
| ------------- | ------------------------ | --------------------------- | ------------------------------------------------------------------------------------ |
| **Nessus**    | Commerciale              | Scanner di vulnerabilità    | Architettura a plugin; database aggiornato regolarmente; report dettagliati.         |
| **OpenVas**   | Open-source              | Scanner di vulnerabilità    | Versione open-source derivata dal codice originale di Nessus; incluso in Kali Linux. |
| **Nexpose**   | Vulnerability Management | Gestione e prioritizzazione | Sviluppato da Rapid7; aiuta a rilevare, valutare e dare priorità alle vulnerabilità. |
| **Nmap**      | Network Scanning         | Network Discovery           | Identifica porte aperte, servizi attivi e protocolli; rileva servizi non sicuri.     |
| **Nikto**     | Open-source              | Web Server Scanner          | Rileva vulnerabilità web, configurazioni errate e componenti obsoleti sui server.    |

### Penetration testing

Un test di penetrazione è un metodo per valutare la sicurezza di un sistema o di una rete, simulando un'attacco da una risorsa malevola conosciuta come cracker.

#### Confronto: VA vs PT

![[{4B4B1A84-8C30-431B-9C29-3F24B2E4C588}.png]]

Possiamo evidenziare differenze nette tra **Vulnerability Assessment (VA)** e **Penetration Test (PT)**:

| Caratteristica | Vulnerability Assessment (VA)        | Penetration Test (PT)                         |
| -------------- | ------------------------------------ | --------------------------------------------- |
| **Focus**      | Ampio (analisi completa dei sistemi) | Mirato (specifici vettori di attacco)         |
| **Natura**     | Non intrusivo, programmato           | Intrusivo, spesso imprevedibile per il target |
| **Obiettivo**  | Identificare potenziali debolezze    | Dimostrare la fattibilità di un exploit (PoC) |
| **Output**     | Report dettagliato con risk rating   | Risultato binario: attacco riuscito o fallito |
| **Conoscenza** | Prevalentemente White/Gray Box       | Spesso Black Box                              |

#### Categorie di penetration test

- **Black Box:** Nessuna informazione previa sul target.
- **White Box:** Conoscenza completa del sistema.
- **Gray Box:** Conoscenza parziale.

#### Metodologie e Regole di Ingaggio (RoE)

Per garantire la professionalità, ogni test deve essere preceduto da un accordo formale (**Rules of Engagement**) che definisca:

- **Autorizzazione ("Permission to hack"):** Un documento legale che protegge il tester da ripercussioni penali.
- **Nondisclosure Agreement (NDA):** Per proteggere la riservatezza delle scoperte.
- **Scope (Perimetro):** Definizione esatta di quali IP, domini o sedi fisiche possono essere testati.
- **Timeline e Milestone:** Date di inizio/fine e obiettivi intermedi (spesso monitorati tramite diagrammi di GANTT).
- **Tecniche proibite:** Ad esempio, il divieto di eseguire test di Denial of Service (DoS) che potrebbero interrompere i servizi aziendali.

## Metodologie 

Citiamo le principali metodologie internazionali:

- **OSSTMM:** Un manuale open source per testare e misurare la sicurezza operativa. È molto dettagliato ma complesso da implementare quotidianamente.
- **NIST SP 800-115:** Una metodologia più agile divisa in 4 fasi: **Planning** (pianificazione), **Discovery** (ricerca informazioni), **Attack** (tentativi di accesso ed escalation dei privilegi) e **Reporting**.
- **OWASP:** Lo standard de facto per la sicurezza web, fondamentale per testare vulnerabilità come SQL Injection e XSS.

## Tipologie di Test e Analisi del Report

L'ethical hacking si applica a diversi domini, ognuno con sfide specifiche:

- **Network PT:** Test di IP pubblici (esterno) o della rete locale (interno, spesso tramite VPN o presenza fisica).
- **Web & Mobile Application PT:** Analisi di siti web e app Android/iOS, oggi i target più comuni a causa della sensibilità dei dati trattati.
- **Social Engineering:** Attacchi mirati alle persone tramite phishing o tecniche di manipolazione per convincerli a rivelare password o cliccare su link malevoli.
- **Physical PT:** Test della sicurezza fisica (serrature, badge RFID, videosorveglianza) per accedere ai data center.

## Reporting

Il report è il prodotto finale del lavoro del pentester. Deve contenere:

1. **Executive Summary:** Un riassunto non tecnico per il management che descrive il livello di rischio globale.
2. **Vulnerability Report:** Dettagli tecnici su ogni falla trovata, classificata per severità (Critical, High, Medium, Low).
3. **Remediation Report:** Istruzioni chiare per gli amministratori di sistema su come risolvere i problemi (es. aggiornamento di patch, modifiche al codice, implementazione di Web Application Firewall).