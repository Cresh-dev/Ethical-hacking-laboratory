_Tag:_ #cybersecurity  #hacking #mitm #network #laboratorio

---
La metodologia classica per l'esecuzione di un attacco MITM all'interno di una rete locale (LAN) si basa sul protocollo di risoluzione degli indirizzi tramite [[ARP Poisoning|ARP Poisoning]]. ==Questa tecnica mira a corrompere le tabelle ARP delle vittime inviando risposte fasulle, associando l'indirizzo MAC dell'attaccante all'IP dei legittimi interlocutori==. Quando il client effettua un'autenticazione su una connessione non crittografata (HTTP) , l'analizzatore di protocollo **Wireshark** consente di esaminare i pacchetti catturati sull'interfaccia di rete. L'adozione di protocolli in chiaro espone l'intero payload in formato human-readable : filtrando il traffico per la richiesta `POST /dvwa/login.php`, è possibile intercettare direttamente i parametri di autenticazione (`username` e `password`) trasmessi in testo chiaro (clear text).

### Manipolazione Attiva del Traffico con Filtri Ettercap

Oltre alla mera intercettazione passiva, si ha una tecnica di **modifica del payload in tempo reale**. Utilizzando il software **Ettercap**, viene definito e compilato un filtro personalizzato basato su espressioni regolari (`regex-replace-filter.filter`).

Il codice del filtro agisce nel modo seguente:

- Intercetta i pacchetti TCP destinati alla porta 80 del server OWASP-BWA.
- All'individuazione di una richiesta `POST` diretta alla pagina di login, modifica dinamicamente il campo `Content-Length` nell'header HTTP per prevenire errori di parsing lato server.
- Applica una sostituzione sul corpo della richiesta tramite espressione regolare, rimpiazzando qualsiasi username digitato dall'utente con la stringa fissa `username=admin&`.

Durante il test, l'utente digita intenzionalmente credenziali arbitrarie (es. username `pippo`, password `admin`). Il filtro di Ettercap intercetta la transazione in transito e sostituisce l'identificativo con `admin`. Di conseguenza, il server riceve ed elabora la stringa alterata, garantendo all'attaccante un accesso autenticato con i privilegi amministrativi superiori dell'account `admin`.