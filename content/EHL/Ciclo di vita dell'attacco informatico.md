_Tag:_ #cybersecurity  #hacking

---
Il ciclo si divide in due macro-fasi: l'ottenimento dell'**Accesso Non Autorizzato** (fasi 1-3) e l'**Uso Non Autorizzato** del sistema (fasi 4-6).

![[{836227C2-8C33-44E1-B1E2-2FB65719269D}.png]]

## 1. Ricognizione

- **L'attacco:** L'aggressore studia il bersaglio cercando informazioni su social media (LinkedIn, Twitter), siti aziendali o scansionando la rete in cerca di vulnerabilità.
- **Prevenzione:** Ispezionare il traffico di rete per rilevare scansioni anomale e limitare le informazioni sensibili pubblicate online (organigrammi, dettagli tecnici, ecc.).
## 2. Militarizzazione e Consegna

- **L'attacco:** Viene creato un "payload" (codice malevolo) personalizzato per il target. Metodi comuni includono email di [[Client side attack|spear phishing]], link dannosi o chiavette USB infette.
- **Prevenzione:** Utilizzare filtri URL, bloccare siti rischiosi e istruire costantemente gli utenti a riconoscere email o allegati sospetti.
## 3. Sfruttamento

- **L'attacco:** L'attaccante sfrutta una vulnerabilità specifica (es. in un software come Office o in un servizio web come [[Test manuale per vulnerabilità#SQL Injection (SQLi) e Manipolazione del Database|SQL injection]]) per ottenere il primo accesso al sistema.
- **Prevenzione:** Gestione rigorosa delle "patch" (aggiornamenti di sicurezza), valutazione continua delle vulnerabilità e protezione avanzata degli endpoint.
## 4. Installazione

- **L'attacco:** Una volta entrato, l'aggressore installa malware per garantire la persistenza (poter rientrare anche dopo un riavvio), scalare i privilegi e prepararsi a muoversi lateralmente nella rete.
- **Prevenzione:** Implementare un modello [[Hacker's Enemies#Strategie Architetturali Dal Modello Perimetrale al Zero Trust|Zero Trust]] con zone di sicurezza controllate e limitare i privilegi di amministratore locale per gli utenti.
## 5. Comando e Controllo

- **L'attacco:** Viene stabilito un canale di comunicazione tra i dispositivi infetti e l'infrastruttura dell'attaccante per inviare istruzioni o rubare dati (es. tramite keylogger o cattura schermo).
- **Prevenzione:** Monitoraggio del DNS, blocco delle comunicazioni verso URL malevoli noti e isolamento degli host compromessi tramite "sinkholing".
## 6. Azioni sugli Obiettivi

- **L'attacco:** È la fase finale in cui l'aggressore raggiunge il suo scopo: esfiltrazione di dati, furto di proprietà intellettuale, lancio di ransomware o distruzione di infrastrutture.
- **Prevenzione:** Utilizzo di strumenti di [[Hacker's Enemies#Automazione, Threat Intelligence e Gestione degli Incidenti|Threat Intelligence]] per dare la caccia agli indicatori di compromissione (IoC) e controllo granulare dei trasferimenti di file.