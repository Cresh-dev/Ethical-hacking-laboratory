_Tag:_ #cybersecurity  #hacking #network 

---
Le reti wireless sono diventate il metodo dominante di connettività grazie alla loro mobilità, facilità di implementazione e scalabilità. Tuttavia, a differenza delle reti cablate dove è richiesto un accesso fisico per intercettare i dati, ==le comunicazioni wireless avvengono su un mezzo condiviso (le frequenze radio). Questo significa che il segnale si estende oltre i confini fisici degli edifici, permettendo a qualsiasi dispositivo nelle vicinanze di monitorare o interagire con la rete==. Per comprendere gli attacchi, è fondamentale conoscere la terminologia di base:

- **Access Point (AP):** Il dispositivo che fornisce connettività wireless e fa da ponte verso la rete cablata.
- **SSID / ESSID:** Il nome logico della rete (l'ESSID si usa quando più AP condividono lo stesso nome per garantire la continuità del segnale).
- **BSSID:** L'indirizzo MAC univoco assegnato all'interfaccia di uno specifico Access Point.
- **Canale Wireless:** La specifica gamma di frequenze radio utilizzata per la comunicazione.

## Le Minacce Principali

La natura aperta delle onde radio espone le reti a diverse categorie di attacco:

- **Eavesdropping (Intercettazione):** La cattura di traffico in chiaro per ottenere informazioni sensibili.
- **Jamming:** L'uso di hardware speciale per bloccare completamente le comunicazioni radio nella zona, creando un Denial-of-Service (DoS).
- **MAC Spoofing:** Una tecnica (realizzabile tramite tool come SMAC) per alterare l'indirizzo MAC del proprio dispositivo e aggirare i filtri degli Access Point.
- **De-authentication Attack:** L'invio di pacchetti falsificati per disconnettere forzatamente un client legittimo dall'AP, utile per catturare gli handshake o reindirizzare le vittime.
- **Rogue Access Point e Evil Twin:** La creazione di AP falsi. In un attacco _Evil Twin_, l'aggressore clona perfettamente un AP legittimo per spingere gli utenti a connettersi, potendo così intercettare tutto il traffico (Man-in-the-Middle)

## L'Evoluzione della Sicurezza Wi-Fi

Poiché le comunicazioni sono facilmente intercettabili, i meccanismi di autenticazione e crittografia sono essenziali per proteggere la riservatezza e l'integrità dei dati. Le reti moderne adottano diversi modelli di autenticazione: aperta (tipica delle reti guest), basata su portale web, tramite chiave pre-condivisa (PSK) o in modalità Enterprise (con server RADIUS e protocollo 802.1X). Gli standard di sicurezza si sono evoluti nel tempo per risolvere vulnerabilità critiche:

- **WEP (Wired Equivalent Privacy):** Il primo standard. Utilizza l'algoritmo RC4 con chiavi statiche (40 o 104 bit) e un ==Vettore di Inizializzazione (IV) di soli 24 bit== inviato in chiaro. È crittograficamente debole e del tutto obsoleto.
- **WPA (Wi-Fi Protected Access):** Una soluzione temporanea che ha ==introdotto il protocollo TKIP per una migliore gestione delle chiavi==, pur mantenendo la compatibilità con l'hardware RC4.
- **WPA2 (IEEE 802.11i):** Lo standard solido e ampiamente adottato. ==Sostituisce il TKIP con la crittografia AES-CCMP==, offrendo ottime garanzie di sicurezza.
- **WPA3:** ==Lo standard moderno che migliora ulteriormente la robustezza contro gli attacchi offline==.

### Attacchi alle Reti WEP

Nel WEP, l'unione della chiave segreta e del Vettore di Inizializzazione (IV) genera un flusso di dati pseudo-casuale (PRGA) che viene usato per cifrare i pacchetti. Gli hacker sfruttano le debolezze di questo sistema in vari modi:

- **Attacco di Frammentazione (Fragmentation):** ==Sfrutta il comportamento prevedibile dei pacchetti per recuperare fino a 1500 byte di materiale crittografico (PRGA). Non recupera la chiave WEP, ma permette di forgiare e iniettare traffico malevolo==.
- **Attacco Chop-Chop:** ==Sfrutta le debolezze del controllo di integrità (ICV). L'attaccante rimuove o modifica l'ultimo byte di un pacchetto cifrato== e, in base alla reazione dell'Access Point, deduce il testo in chiaro byte per byte.
- **Café-Latte Attack:** ==Si tratta di un attacco "client-side", ovvero non prende di mira l'Access Point, ma un dispositivo isolato del client. L'attaccante cattura una richiesta ARP del client, la altera e gliela reinvia ripetutamente. Questo bombarda il client costringendolo a generare un'enorme quantità di risposte cifrate e prevedibili, fornendo all'attaccante il materiale necessario per craccare la chiave WEP==.
- **Hirte Attack:** ==È un'evoluzione diretta del Café-Latte. Supera il limite di dover fare affidamento esclusivo sui pacchetti ARP, permettendo di utilizzare qualsiasi pacchetto basato su protocollo IP==. Risulta molto più flessibile in reti dove il traffico ARP viene filtrato.

### Attacchi alle Reti WPA/WPA2 e WPS

Nonostante la crittografia AES, le reti WPA e WPA2 (specialmente in modalità Personal/PSK) sono vulnerabili ad attacchi basati sui dizionari, se le password scelte sono deboli.

- **Metodo Tradizionale:** ==Richiede che un utente legittimo sia connesso alla rete. L'attaccante invia un pacchetto di de-autenticazione per disconnettere l'utente; quando questi si riconnette, l'attaccante intercetta il "4-way handshake"==. Questo file catturato viene poi utilizzato per tentare di indovinare la password offline tramite brute-force.
- **Metodi Moderni:** Permettono di attaccare l'Access Point direttamente, senza bisogno che ci siano utenti connessi. ==L'attacco prende di mira elementi specifici del protocollo di autenticazione (EAPOL e RSN IE) per ottenere il materiale crittografico necessario al cracking offline==.
- **Attacco Pixie-Dust al WPS:** ==Il Wi-Fi Protected Setup (WPS) usa un PIN a 8 cifre per facilitare le connessioni. L'attacco Pixie-Dust sfrutta vulnerabilità nell'implementazione di questo protocollo per recuperare il PIN rapidamente e offline, compromettendo la rete==.

## Best Practice di Sicurezza Wi-Fi

Per proteggere adeguatamente un'infrastruttura wireless, il documento suggerisce diverse regole d'oro:

- Modificare sempre le credenziali di amministrazione di default dell'Access Point.
- Utilizzare standard crittografici forti, possibilmente WPA2 o WPA2 Enterprise.
- Mantenere costantemente aggiornato il firmware dell'Access Point per chiudere eventuali falle.
- Disabilitare la trasmissione del nome della rete (SSID broadcast) per nasconderla ad utenti indesiderati.
- Abilitare il filtraggio MAC, permettendo l'accesso solo a una lista pre-approvata di dispositivi.