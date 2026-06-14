_Tag:_ #cybersecurity  #hacking #network #mitm #laboratorio

---
Il **DNS Poisoning** (o DNS Spoofing) è una ==tecnica di attacco informatico che mira a corrompere l'integrità del sistema di risoluzione dei nomi (DNS), manipolando le risposte in modo che un client risolva un dominio legittimo verso un indirizzo IP malevolo controllato dall'attaccante==.

![[Pasted image 20260331110021.png]]

## Dinamica dell'Attacco Man-in-the-Middle

1. **Iniezione**: L'attaccante invia "ARP Reply" falsificate ai target (es. vittima e gateway).
2. **Avvelenamento della Cache**: I target aggiornano le proprie tabelle ARP associando l'IP del partner di comunicazione al MAC address dell'attaccante.
3. **Intercettazione**: Il traffico tra le vittime viene fisicamente instradato attraverso la macchina dell'attaccante, che può visualizzarlo, registrarlo o modificarlo prima di inoltrarlo. Nel seguente caso, l'attaccante effettua un DNS poisoning per dirottare la navigazione delle vittima a proprio piacere.
### Esercitazione

Si simula un ambiente di rete locale (**LAN 192.168.56.0/24**) per dimostrare un attacco di **Man-in-the-Middle (MITM)** finalizzato al DNS Spoofing. Componenti della rete:

- **DNS Server (Ubuntu VM):** IP `192.168.56.4`. Gestisce il dominio autoritativo `provahacking.it`.
- **Vittima (Windows VM):** IP `192.168.56.2`. Client che effettua le richieste DNS.
- **Attaccante (Kali Linux):** IP `192.168.56.1`. Intercetta il traffico e ospita un server web malevolo (Apache).
- **Web Server Legittimo (OWASP-bwa):** IP `192.168.56.3`. Destinazione reale del traffico web.

#### Configurazione del Server DNS (Bind9)

Il server DNS è implementato su Ubuntu utilizzando **Bind9**. La configurazione si articola in tre file principali:

- **`named.conf.options`**: Definisce le impostazioni globali. Il server è configurato per ascoltare sulla porta 53, permettere query da qualsiasi client (`allow-query { any; }`) e inoltrare richieste ignote ai DNS di Google (`8.8.8.8`).
- **`named.conf.local`**: Definisce le zone di competenza:
    - **Forward Lookup Zone**: Associa nomi a IP (es. `provahacking.it` → `192.168.56.4`).
    - **Reverse Lookup Zone**: Associa IP a nomi (es. `192.168.56.3` → `www.provahacking.it`).
- **Zone File (`provahacking.it`)**: Contiene i record risorsa (RR):
    - **SOA (Start of Authority)**: Informazioni autoritative e numero seriale per gli aggiornamenti.
    - **A Records**: Mappatura diretta tra hostname e IP.
    - **Glue Records**: Necessari per fornire l'IP dei Name Server interni al dominio stesso, evitando risoluzioni circolari.

#### Dinamica dell'Attacco

l **DNS Poisoning** consiste nel manipolare le risposte DNS affinché un client risolva un dominio legittimo verso un IP malevolo. Fasi dell'attacco:

1. **Preparazione del Fake Web Server**: Su Kali viene avviato **Apache2** con una pagina `index.html` modificata che avverte l'utente della redirezione avvenuta.
2. **Configurazione di Ettercap**: Si modifica il file `etter.dns` inserendo la riga: `www.provahacking.it A 192.168.56.1`. Questo istruisce il tool a falsificare la risposta DNS puntando verso Kali.
3. **Esecuzione MITM**: Tramite **Ettercap**, si avvia un attacco di **ARP Poisoning** posizionandosi tra il Client (Target 1) e il DNS Server (Target 2).
4. **Attivazione Plugin**: Si abilita il plugin `dns_spoof`.
5. **Risultato**: Quando la vittima cerca `www.provahacking.it`, Ettercap intercetta la query e risponde con l'IP dell'attaccante prima del server reale. La vittima visualizza il sito malevolo pur vedendo l'URL corretto nel browser.


> [!NOTE] Perché si usa l'ARP poisoning anche nel DNS poisoning
> L'**ARP Poisoning** è necessario perché opera al **Layer 2** per dirottare fisicamente i pacchetti in una rete commutata. Senza di esso, lo switch inoltrerebbe la query DNS direttamente al server reale, rendendo il traffico invisibile all'attaccante. Sintesi dell'attacco:
> - **ARP Poisoning**: Associa l'IP del server DNS (`192.168.56.4`) al MAC address dell'attaccante nella cache della vittima.
> - **Intercettazione**: Lo switch invia la query DNS a Kali invece che al server Ubuntu.
> - **DNS Poisoning**: Kali riceve la richiesta e invia una risposta falsa con il proprio IP (`192.168.56.1`) prima che il server reale possa intervenire.
> - **Risultato**: Il server legittimo non riceve mai la richiesta perché il flusso fisico dei dati è stato deviato a monte

#### Mitigazione e Prevenzione

Per contrastare queste vulnerabilità, possono essere adottate diverse strategie:

- **Network Security**: Utilizzo di switch hardware per isolare i domini di collisione e implementazione di **DHCP Snooping** per prevenire ARP Spoofing.
- **Policy di sistema**: Disabilitazione della modalità promiscua sulle schede di rete.
- **Crittografia**: Utilizzo di protocolli sicuri come **SSH** o **IPsec** per proteggere il traffico sensibile.
- **DNS over HTTPS (DoH)**: Cifratura delle query DNS tramite HTTPS, impedendo ad attaccanti locali di leggere o modificare le richieste di risoluzione nomi.