_Tag:_ #cybersecurity  #hacking

---
==In questa situazione viene simulato uno scenario reale di penetration testing, partendo dai risultati di una precedente fase di [[Vulnerability Assessment]] (VA) eseguita con scanner automatici== (come [[VA con Greenbone OpenVAS (GVM)|Greenbone]] o [[VA con Nessus|Nessus]]). Per comprendere i vettori di attacco, viene analizzato il flusso di interazione _request-response_ a tre livelli (architettura _3-Tier_) che si attiva digitando un URL:

1. **Risoluzione DNS:** Il nome di dominio viene convertito nell'indirizzo IP corrispondente.
2. **Richiesta HTTP:** Il browser client invia la richiesta al Web Server di Front-End (Tier 1).
3. **Smistamento del Contenuto:** Il web server distingue la natura della risorsa:
    - I contenuti _statici_ (es. HTML) vengono restituiti direttamente.
    - I contenuti _dinamici_ (es. PHP, JSP, ASP) vengono inoltrati al motore applicativo.
4. **Logica di Business (Tier 2):** L'Application Server elabora il codice lato server.
5. **Accesso ai Dati (Tier 3):** Se necessario, l'applicazione interroga il Database backend per recuperare le informazioni e generare dinamicamente la risposta , la quale viene infine inviata al browser per il _rendering_ all'utente.

![[Screenshot 2026-05-21 at 09.54.02.png]]

## Vettori di Attacco e Flaws Applicativi

==Un vettore di attacco rappresenta il percorso o la metodologia usata per ottenere un accesso non autorizzato, sfruttando principalmente la gestione insicura dell'input utente per consegnare un payload malevolo==. L'esempio classico è l'**SQL Injection (SQLi)**, in cui un input non validato permette l'iniezione di codice SQL che altera le query eseguite dal database, portando all'esposizione di dati sensibili o al bypass dell'autenticazione. Le vulnerabilità delle applicazioni web derivano da errori di progettazione, configurazioni errate o pratiche di codifica insicure, e sono classificate in 5 macro-aree:

- **Flaws di Autenticazione**: Riguardano i meccanismi di verifica dell'identità dell'utente. Includono:
	- Messaggi di errore espliciti che permettono l'enumerazione degli utenti.
	- Policy sulle password deboli ed esposizione a attacchi di forza bruta.
	- Procedure di recupero credenziali insicure (es. domande di sicurezza prevedibili).
	- Trasmissione di credenziali su canali cifrati inadeguati (mancanza di HTTPS).
	- Difetti logici complessivi (_Broken Authentication_) per bypassare il login.
- **Flaws di Autorizzazione**: Regolano l'accesso alle risorse una volta autenticati. Si manifestano tramite:
	- _Insecure Direct Object References (IDOR):_ Modifica manuale dei parametri identificativi nelle richieste per accedere a risorse altrui.
	- _Privilege Escalation:_ Sia verticale (ottenere privilegi amministrativi) sia orizzontale (accedere ai dati di utenti di pari livello).
	- Mancanza di controlli di accesso a livello di singola funzione (_Function-level access control_).
- **Flaws di Gestione della Sessione**: Riguardano il tracciamento dello stato dell'utente. Comprendono:
	- Session ID deboli o prevedibili (soggetti a dirottamento/hijacking).
	- Riutilizzo del Session ID senza rigenerazione (esposizione a _Session Fixation_).
	- Assenza di timeout per inattività o mancata invalidazione del token al logout.
	- Vulnerabilità a _Cross-Site Request Forgery (CSRF)_, dove la sessione attiva viene sfruttata per far compiere all'utente azioni non intenzionali.
- **Flaws di Validazione dell'Input**: Causati dall'assenza di filtri sull'input utente:
	- _Injection Flaws:_ L'input viene interpretato come comando di backend (es. SQLi).
	- _Cross-Site Scripting (XSS):_ Esecuzione di script malevoli nel browser della vittima.
	- _Malicious File Upload:_ Caricamento di file eseguibili mascherati da file legittimi per ottenere l'esecuzione di codice sul server.
- **Altri Flaws Sistemici**:
	- Errori di _Configuration Management_ (servizi esposti, default insicuri).
	- Cifratura debole o algoritmi crittografici obsoleti.
	- Utilizzo di componenti, librerie o framework terzi affetti da vulnerabilità note.

## Metodologia di Hacking e Penetration Testing

L'hacking delle applicazioni web richiede un **approccio strutturato** e non la mera esecuzione di tool automatici. La metodologia si articola in sei fasi:

1. **Analisi dell'applicazione:** Comprensione delle funzionalità e delle tecnologie sottostanti.
2. **Identificazione dei punti di ingresso e uscita:** Mappatura dei flussi di dati (input/output).
3. **Scomposizione dei componenti:** Analisi approfondita dell'architettura client-server.
4. **Manual testing:** Interazione diretta e manuale con gli input per scoprire falle logiche complesse.
5. **Scansione automatica di sicurezza:** Uso di tool per una rapida identificazione di misconfigurazioni e falle note.
6. **Rimozione dei falsi positivi:** Fase critica di validazione manuale dei risultati dei tool.

## Mitigazioni e Strategie di Difesa

Si conclude introducendo un framework difensivo dettagliato per contrastare le vulnerabilità censite:

|**Categoria del Flaw**|**Principali Strategie di Mitigazione**|
|---|---|
|**Autenticazione**|Enforce di password complesse; memorizzazione tramite _salted hashing_; messaggi di errore generici; validazione _dual-side_ (client e server); implementazione di Multi-Factor Authentication (MFA); adozione di protocolli sicuri (OAuth, OpenID, SSO).|
|**Autorizzazione**|Applicazione del principio del _least privilege_; implementazione di controlli di accesso basati sui ruoli (RBAC); utilizzo di token sicuri (JWT); validazione dell'autorizzazione ad ogni singola richiesta.|
|**Session Management**|Generazione di session ID robusti e imprevedibili; impostazione di timeout per inattività; distruzione della sessione al logout e rigenerazione del token ad ogni cambio di privilegio; implementazione di difese anti-CSRF.|
|**Input Validation**|Validazione e sanificazione rigorosa (lato client e server); adozione di _allowlist_; codifica dell'output (_output encoding_); controllo dei tipi e scansione malware sui file caricati; utilizzo obbligatorio di _prepared statements_ e query parametrizzate contro le injection.|
|**Auditing & Logging**|Tracciamento dei log per gli eventi critici (autenticazione, modifiche ai dati) arricchiti da metadati (timestamp, IP, utente); protezione dei file di log da manomissioni e prevenzione di attacchi DoS basati sulla saturazione dei log.|