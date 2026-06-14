_Tag:_ #cybersecurity  #hacking #network #mitm #laboratorio

---
L'**Address Resolution Protocol (ARP)** mappa gli indirizzi IP (Layer 3) ai MAC address (Layer 2). ==Essendo un protocollo stateless e privo di autenticazione, si basa sulla fiducia intrinseca delle risposte ricevute==.

![[{651886FB-B6CA-47F3-9738-5109EBE5D356}.png]]

## Dinamica dell'Attacco Man-in-the-Middle

1. **Iniezione**: L'attaccante invia "ARP Reply" falsificate ai target (es. vittima e gateway).
2. **Avvelenamento della Cache**: I target aggiornano le proprie tabelle ARP associando l'IP del partner di comunicazione al MAC address dell'attaccante.
3. **Intercettazione**: ==Il traffico tra le vittime viene fisicamente instradato attraverso la macchina dell'attaccante, che può visualizzarlo, registrarlo o modificarlo prima di inoltrarlo==.

### Esercitazione

L'esercitazione pratica si concentra sull'uso di strumenti specifici per posizionarsi come "uomo nel mezzo" ([[Man in the Middle Attacks|Man-in-the-Middle]]) tra due macchine vittima in una LAN. Il processo si articola in cinque fasi principali:

1. **Verifica Preliminare:** Sulla macchina Windows si esegue il comando `arp -a` per visualizzare la tabella ARP originale e annotare le associazioni IP-MAC corrette prima dell'attacco.
2. **Configurazione di Ettercap:** Si avvia l'interfaccia grafica (`sudo ettercap -G`) e si seleziona l'interfaccia di rete corretta (tipicamente `eth0`).
3. **Host Discovery:** Si esegue una scansione della rete tramite l'icona della lente d'ingrandimento per identificare tutti gli host attivi nella sottorete.
4. **Targeting:** Si aggiunge l'IP del client Windows (`192.168.56.2`) al **Target 1**. Si aggiunge l'IP del server OWASP-BWA (`192.168.56.3`) al **Target 2**.
5. **Esecuzione dell'Attacco:** Si attiva l'**ARP Poisoning** dal menu MITM (icona del globo), selezionando l'opzione specifica per iniziare a inviare pacchetti falsificati.

![[Pasted image 20260331100124.png]]

#### Risultati e Analisi

L'efficacia dell'attacco viene confermata analizzando nuovamente la tabella ARP della vittima (Windows) con il comando `arp -a`:

- **Prima dell'attacco:** L'IP `192.168.56.3` (Server) è associato al suo MAC address reale.
- **Durante l'attacco:** L'IP `192.168.56.3` risulta ora associato allo stesso MAC address della macchina Kali dell'attaccante (`192.168.56.1`).
- **Conseguenza:** La vittima crede erroneamente che l'attaccante sia il server. Tutto il traffico destinato al server passa ora attraverso la macchina Kali, permettendo all'attaccante di intercettare dati sensibili come password o sessioni di chat.

#### Mitigazione e Prevenzione

Per contrastare lo sniffing e l'ARP poisoning si adottano diverse strategie:

- **Monitoraggio:** Ispezione regolare delle tabelle ARP (`arp -a`) per rilevare MAC duplicati.
- **Sicurezza degli Switch:**
    - **Port Security:** Limita il numero di MAC address per porta.
    - **DHCP Snooping:** Crea un database di binding IP-MAC fidati.
    - **Dynamic ARP Inspection (DAI):** Verifica i pacchetti ARP confrontandoli con il database del DHCP snooping.
- **Difesa Crittografica:** Uso di protocolli sicuri (**HTTPS, SSH, TLS**) per proteggere i dati anche in caso di intercettazione.