_Tag:_ #cybersecurity  #hacking

---
Nel panorama della sicurezza informatica moderna, si assiste a una transizione fondamentale: il passaggio da una prospettiva prettamente offensiva (_black hat perspective_), focalizzata sullo studio e sullo sfruttamento delle vulnerabilità, a una visione difensiva integrata. La cybersecurity contemporanea si configura come un **processo continuo e dinamico** di attacco, difesa e costante miglioramento, abbandonando l'idea obsoleta che la sicurezza possa essere garantita dall'adozione di un singolo prodotto o strumento.

## Ruoli Chiave e Modelli Organizzativi: Il SOC

L'architettura difensiva aziendale si fonda su competenze umane specializzate e sinergiche, i cui ruoli principali includono:

- **CISO (Chief Information Security Officer):** Responsabile della governance, della gestione del rischio e della strategia globale di sicurezza.
- **Security Architect e Security Administrator:** Il primo progetta l'integrazione dei controlli di sicurezza negli ambienti IT; il secondo ne gestisce le configurazioni operative e le policy di accesso.
- **Cybersecurity Specialist e Analisti:** Monitorano l'infrastruttura e rispondono tempestivamente agli incidenti in tempo reale.
- **Penetration Tester ed Ethical Hacker:** Figure autorizzate che simulano attacchi reali per individuare falle prima che vengano sfruttate da attori malevoli.
- **Cryptanalyst:** Specializzato nella ricerca di debolezze nei sistemi crittografici.

==Il fulcro operativo di queste figure è il Security Operations Center (SOC). Definito sia come team sia come infrastruttura logica, il SOC opera in continuità (24/7) con l'obiettivo di prevenire, rilevare e mitigare le minacce informatiche==.

### Evoluzione Storica del SOC

La struttura del SOC ha attraversato cinque generazioni storiche dettate dall'evoluzione tecnologica:

1. **1ª Generazione (Detect - anni 2000):** Basata sull'adozione iniziale di IDS (Intrusion Detection Systems) e raccolta rudimentale di log di sistema (_syslog_).
2. **2ª Generazione (Prevent - anni 2005):** Centralizzazione delle funzioni di Security Operations e nascita dei primi modelli APT (_Advanced Persistent Threat_).
3. **3ª Generazione (Respond - anni 2010):** Focalizzazione su indagine, contenimento ed exfiltration detection (introduzione di sistemi DLP e packet capture).
4. **4ª Generazione (Comply - anni 2015):** Spinta dalla compliance normativa; introduzione di Next-Generation Firewall (NGFW), Endpoint Detection and Response (EDR) e primi modelli di Machine Learning, con conseguente fenomeno dell'_alert fatigue_ (sovraccarico di falsi positivi per gli analisti).
5. **"Next Gen" (Automation - anni 2020):** Automatizzazione dei task ripetitivi, team snelli focalizzati sulla proattività (_cyber threat hunting_) e sfruttamento avanzato della telemetria.

## Strategie Architetturali: Dal Modello Perimetrale al Zero Trust

==I modelli di difesa tradizionali consideravano la rete aziendale divisa rigidamente in una zona interna "fidata" (_Trusted_) e una esterna "non fidata" (_Untrusted_), affidando la sicurezza a barriere perimetrali (Firewall e VPN). Questo approccio è fallimentare di fronte alle minacce moderne, poiché non contrasta i movimenti laterali degli attaccanti o le minacce interne (_Malicious Insiders_)==. Il paradigma moderno è il **Zero Trust Approach**, governato dal principio cardine **"never trust, always verify"**.

![[Screenshot 2026-05-19 at 11.02.28.png]]

In un ecosistema Zero Trust, la fiducia intrinseca è considerata una vulnerabilità. I pilastri di questa architettura prevedono:

- **Microperimetri:** Segmentazione della rete in zone isolate attorno a singoli asset o carichi di lavoro, impedendo lo spostamento laterale in caso di compromissione di un nodo.
- **Conoscenza dei Dati e Applicazioni:** Mappatura dei flussi applicativi e definizione di regole di filtraggio basate sulle applicazioni piuttosto che sulle porte di rete.
- **Verifica dell'Identità:** Autenticazione continua basata su Identity Provider, contesto del dispositivo e MFA (Multi-Factor Authentication) sia per gli utenti sia per gli account di servizio (_least privilege_).
- **Ispezione Totale del Traffico:** Analisi del traffico dati, inclusa la decifratura dei flussi TLS/SSL tramite infrastrutture PKI dedicate e moduli HSM (_Hardware Security Module_) per la gestione sicura delle chiavi.
- **Logging Onnipresente:** Registrazione e correlazione centralizzata di ogni richiesta di rete, accesso ai file o attività email, fondamentale per l'analisi investigativa.

## Tassonomia delle Soluzioni Tecnologiche di Sicurezza

La protezione dell'infrastruttura richiede una suite integrata di strumenti specializzati operanti a diversi livelli dello stack ISO/OSI:

- **IDS/IPS:** Sistemi di monitoraggio passivi (generazione di allarmi) o attivi/inline (blocco del traffico malevolo) basati su analisi delle firme (_misuse_) o delle anomalie comportamentali.
- **Next-Generation Firewall (NGFW):** Superano il filtraggio stateless basato su porte e protocolli, introducendo la _Deep Packet Inspection (DPI)_ a livello applicativo e l'integrazione nativa di moduli di intrusion prevention. Gli NGFW utilizzano "profili di sicurezza" specifici:
    - _Antivirus:_ Motori basati su flussi di pacchetti in tempo reale che scansionano malware all'interno di eseguibili, PDF o file compressi.
    - _Anti-Spyware:_ Bloccano le comunicazioni outbound dei client infetti verso i server di Command and Control (C2).
    - _Vulnerability Protection:_ Rilevano ed eradicano i tentativi di exploit inbound (es. buffer overflow, esecuzioni di codice illecito).
    - _URL Filtering:_ Monitorano e controllano gli accessi web HTTP/HTTPS basandosi su categorie reputazionali.
- **Web Application Firewall (WAF):** Dispositivi verticali per la protezione delle applicazioni web a livello Layer 7 (ispezione di conversazioni HTTP/HTTPS, XML, SOAP), specificamente tarati per mitigare le minacce della _OWASP Top Ten_.
- **Sandbox:** Ambienti virtualizzati e isolati (OS Windows, Linux, ecc.) in cui "detonare" file ed allegati sospetti per analizzarne il comportamento in sicurezza, cruciali contro malware zero-day.

### Sicurezza degli Endpoint ed Evoluzione dei Sistemi di Rilevamento

==I sistemi antivirus tradizionali basati esclusivamente su firme sono considerati obsoleti: la rapidità con cui il malware moderno muta (l'82% rimane attivo per meno di un'ora e il 70% appare una sola volta) rende inefficace questo approccio statistico==. Si è passati quindi ai sistemi di **Advanced Endpoint Protection (AEP)** e alle piattaforme **EDR/XDR**. Queste tecnologie sfruttano l'**Advanced Machine Learning (ML)** e l'**Intelligenza Artificiale (AI)** per analizzare enormi dataset di telemetria e monitorare il comportamento di script e file. L'EDR registra gli eventi degli endpoint in un database centrale per attività di investigazione ; l'**XDR (Extended Detection & Response)** estende questa visibilità proattiva correlando i dati cross-domain tra reti, endpoint e infrastrutture cloud.

## Automazione, Threat Intelligence e Gestione degli Incidenti

L'efficacia della difesa moderna è legata alla capacità di orchestrare i componenti di sicurezza tramite piattaforme **SOAR (Security Orchestration, Automation and Response)** e **SIEM (Security Information and Event Management)**.

### Il Ruolo del SIEM e del SOAR

![[Pasted image 20260529181513.png]]

- **SIEM:** ==Funge da strato di visibilità centrale. Raccoglie i log da fonti eterogenee, ne esegue la correlazione in tempo reale e storicizzata==, e supporta la prioritizzazione degli incidenti tramite cruscotti di monitoraggio e KPI.
- **SOAR:** ==Risolve il problema dei tempi di reazione manuali (che per un singolo evento di analisi di un indicatore di compromesso o aggiornamento di un ticket richiedono mediamente 25 minuti) riducendoli a circa 1 minuto tramite l'automazione==. Il SOAR opera su tre direttrici:
    - _Orchestration:_ Coordinamento e attivazione centralizzata dello stack di prodotti di sicurezza tramite flussi logici (Playbook e Runbook).
    - _Automation:_ Esecuzione automatica di script ed integrazioni API per compiti standardizzati.
    - _Response:_ Case management, reportistica e collaborazione del team durante l'incidente.

![[Pasted image 20260529181448.png]]

==Questi strumenti sono alimentati dalla Cyber Threat Intelligence (CTI), ovvero la raccolta e l'analisi sistematica del ciclo di vita e delle metodologie operative degli avversari. L'output più pragmatico della CTI è la condivisione bidirezionale in tempo reale degli Indicators of Compromise (IoC) (indirizzi IP malevoli, domini, URL, hash di file), che consente una difesa coordinata e istantanea dell'intera infrastruttura aziendale.==

## Sistemi di Mitigazione avanzata contro Attacchi DDoS

La garanzia della disponibilità dei servizi informatici (_Availability_) richiede contromisure specifiche contro gli attacchi di tipo _Distributed Denial of Service_ (DDoS), sia volumetrici sia applicativi.

### Attacchi volumetrici

Questa tipologia di minaccia mira a saturare la larghezza di banda o a esaurire le risorse di calcolo dei dispositivi di rete del bersaglio attraverso l'invio di volumi di traffico insostenibili.

- **SYN Flood:** ==Un attacco che colpisce direttamente il livello di trasporto sfruttando il meccanismo di handshake a tre vie del protocollo TCP. L'attaccante esaurisce le risorse del server bersaglio inizializzando innumerevoli connessioni che vengono lasciate intenzionalmente incomplete (half-open connections)==.
- **TCP Flood:** Una tecnica di forza bruta che mira a sopraffare il target inondandolo con un volume eccessivo di traffico TCP generico.
- **UDP Flood:** Consiste nell'invio massivo e continuo di pacchetti UDP verso porte specifiche o casuali del sistema vittima, costringendolo a consumare risorse elaborative per gestire le richieste.
- **ICMP Flood:** Satura la capacità di banda della rete inviando una raffica ininterrotta di richieste ping (pacchetti ICMP Echo Request).
- **IGMP Flood:** Prende di mira i protocolli di gestione dei gruppi multicast, inviando traffico anomalo con lo scopo di sovraccaricare e bloccare i dispositivi di instradamento della rete.

### Minacce Estese (Amplificazione e Livello Applicativo)

Questa categoria comprende tecniche più asimmetriche e sofisticate. Invece di generare tutto il traffico malevolo alla fonte, queste minacce sfruttano configurazioni errate di protocolli terzi, pacchetti malformati o logiche di livello applicativo per massimizzare il danno:

- **Attacchi di Amplificazione (Reflection):**
    - **NTP Amplification:** Sfrutta server NTP (Network Time Protocol) esposti e configurati in modo non sicuro. L'attaccante invia piccole richieste con l'IP sorgente falsificato (spoofato) della vittima, inducendo il server NTP a rispondere con pacchetti di dimensioni molto maggiori, amplificando così il volume di traffico riversato sul bersaglio.
    - **DNS Amplification:** ==Segue il medesimo principio geometrico dell'attacco NTP, ma utilizza le risposte dei server DNS per generare e riflettere volumi di traffico sproporzionati verso il target==.
- **Attacchi tramite Pacchetti Malformati e Layer 7:**
    - **Ping of Death:** Consiste nell'invio di pacchetti ICMP malformati o di dimensioni superiori alla MTU (Maximum Transmission Unit) consentita. L'obiettivo non è saturare la banda, ma causare errori di frammentazione che portano al crash o alla destabilizzazione dei sistemi operativi bersaglio.
    - **HTTP Flood (Layer 7):** ==Opera al livello applicativo del modello OSI. Prende di mira le risorse delle applicazioni web inviando enormi quantità di richieste HTTP (GET o POST) che, ad un'analisi perimetrale superficiale, risultano del tutto indistinguibili dal traffico legittimo==.

### Meccanismi di Protezione a Livello di Trasporto (SYN Protection)

Per proteggere il protocollo TCP dall'esaurimento delle risorse causato da connessioni parzialmente aperte (_half-open connections_), si utilizzano due tecniche di validazione dell'IP sorgente:

1. **Transparent Proxy Authentication (SYN-Cookie):** ==L'appliance di sicurezza intercetta il pacchetto SYN della sorgente e risponde con un pacchetto SYN-ACK contenente un "cookie" crittografico nei campi sequenza. Se il client è legittimo, risponderà con un ACK contenente il cookie validato==, completando l'autenticazione ed inserendo l'IP in una tabella di white-list prima di permettere l'effettivo _delayed binding_ della sessione con il server reale.

![[Pasted image 20260529182349.png]]

2. **Safe Reset Authentication:** ==Il sistema invia al client un pacchetto di ACK contenente un numero di sequenza volutamente non valido. Un client conforme agli standard RFC risponderà inviando un pacchetto di RST e rieseguendo un nuovo tentativo di handshake SYN entro 3 secondi==. Verificata la corrispondenza temporale e strutturale di questa risposta, la sorgente viene considerata autentica.

![[Pasted image 20260529182403.png]]

### Meccanismi di Protezione Comportamentale (HTTP e DNS)

- **Behavioral Analysis HTTP:** V==iene stabilita una linea di base (baseline) del traffico HTTP legittimo. In caso di deviazioni anomale, il sistema applica una tecnica di Selective Challenge inviando codici di reindirizzamento HTTP 302 Redirection (con annesso Set-Cookie) o script JavaScript==. I browser reali eseguono lo script o onorano il redirect inserendosi automaticamente tra le sorgenti lecite, mentre i bot degli attaccanti falliscono la sfida e vengono bloccati.

![[Pasted image 20260529182313.png]]

- **Behavioral Analysis DNS:** Sfrutta i controlli di conformità RFC. ==Un client DNS legittimo, di fronte a un mancato soddisfacimento di una richiesta entro una finestra temporale, ritrasmetterà la medesima query==. Attraverso un meccanismo di _Selective Discard_, il sistema monitora la frequenza e la conformità di queste risposte alzando il punteggio reputazionale del client, preservando così l'esperienza utente ed evitando l'alerting asimmetrico.

![[Pasted image 20260529182328.png]]

### Mitigazione Volumetrica tramite BGP Diversion

Quando il volume dell'attacco DDoS satura la banda passante della rete locale di destinazione, l'attivazione di difese on-premise diventa impossibile. Si ricorre quindi alla **BGP Diversion (BGP Rerouting)**. ==Sfruttando le tabelle di instradamento del Border Gateway Protocol, l'intero traffico destinato alla rete della vittima viene deviato verso infrastrutture cloud distribuite o centri di pulizia dedicati, noti come Scrubbing Centers. All'interno dello scrubbing center il traffico viene analizzato, i pacchetti malevoli vengono scartati e solo il traffico "pulito" (_clean traffic_) viene reinstradato (tramite tunnel dedicati) verso il datacenter originario, garantendo la continuità operativa del servizio==.

![[Pasted image 20260529182418.png]]

### Network Access Control (NAC)

Il **Network Access Control (NAC)** è un processo di sicurezza che implementa specifiche politiche (policy) all'interno dell'infrastruttura di rete con l'obiettivo di controllare e regolamentare l'accesso da parte di dispositivi e utenti.

L'applicazione di queste direttive di controllo si basa tipicamente su due parametri di verifica:

- **Autenticazione:** La validazione dell'identità del dispositivo e/o dell'utente che sta tentando di connettersi alla rete.
- **Stato dell'Endpoint:** La valutazione dello stato di configurazione del dispositivo terminale (endpoint) al momento della richiesta di accesso, per assicurarsi che rispetti i requisiti di sicurezza previsti.