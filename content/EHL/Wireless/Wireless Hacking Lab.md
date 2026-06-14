_Tag:_ #cybersecurity  #hacking #network 

---
Prima di sferrare qualsiasi attacco, un penetration tester deve mappare il territorio. Questa pratica prende il nome di **Wardriving**, una tecnica di ricognizione wireless che consiste nel mappare le reti Wi-Fi nelle vicinanze scansionando, in modo attivo o passivo, gli Access Point. L'obiettivo primario non è l'intrusione immediata, ma la raccolta sistematica di metadati tecnici essenziali: SSID (il nome della rete), tipologia di crittografia, potenza del segnale (RSSI) e informazioni sull'hardware (come il MAC address e il vendor). 

 ![[Screenshot 2026-06-06 at 07.35.39.png]]

==Per operare senza essere rilevato e senza doversi connettere a un Access Point, l'attaccante deve impostare la propria interfaccia di rete in Monitor Mode==. Questo stato, attivabile tramite tool come `airmon-ng`, permette alla scheda di catturare tutto il traffico aereo 802.11 (management, control e data frames) indipendentemente dal destinatario effettivo. Gli strumenti principali utilizzati in questa fase sono:

- **Airodump-ng:** un ==catturatore attivo di pacchetti focalizzato sui frame raw 802.11, utilizzato per collezionare Vettori di Inizializzazione (IV) del WEP e handshake WPA/WPA2==.
- **Wireshark:** ==impiegato per un'analisi approfondita del traffico crittografato e dei protocolli di autenticazione direttamente sui file catturati==.

## L'Arsenale del Penetration Tester: I Tool di Laboratorio

Il laboratorio si avvale di una serie di strumenti specializzati, ognuno progettato per colpire uno specifico anello debole della catena di sicurezza wireless.

### Kismet: L'Occhio Invisibile

Mentre Airodump-ng è ottimo per la cattura attiva, **Kismet** è il ==principe della scansione totalmente passiva. Operando al Livello 2 del protocollo 802.11, funge da rilevatore di reti, sniffer e sistema di Intrusion Detection (IDS). La sua forza risiede nel non trasmettere alcuna richiesta di "probe" (sonda) nell'aria, rendendolo invisibile ai sistemi di difesa bersaglio==. Una delle sue capacità più avanzate di "Network Intelligence" è l'analisi delle associazioni client-dispositivo: Kismet è in grado di mappare in tempo reale quali utenti sono connessi a un determinato router, identificando sia i client wireless che quelli cablati dedotti sulla stessa rete, il tutto senza richiedere alcuna autenticazione. È accessibile tramite un'interfaccia web locale.

### PixieWPS e Reaver: L'Assedio al WPS

Il Wi-Fi Protected Setup (WPS) rappresenta spesso una grave vulnerabilità architetturale nei router domestici. Il laboratorio introduce due tool per comprometterlo:

- **PixieWPS:** È un ==software leggero scritto in C che esegue un attacco di tipo Pixie Dust==. Questo strumento non cerca di indovinare il PIN provando tutte le combinazioni, ma sfrutta vulnerabilità crittografiche o valori prevedibili (scarsa entropia) nella generazione del PIN WPS da parte del router. L'enorme vantaggio è che permette un recupero del PIN _offline_ in tempi ridottissimi, evitando i blocchi del router.
- **Reaver:** Adotta un approccio più tradizionale basato sulla forza bruta _online_. ==Reaver interagisce continuamente con l'Access Point testando sistematicamente tutti i possibili PIN dell'AP==. Pur essendo efficace su router configurati male o con WPS attivo di default, è un attacco di lunga durata che richiede un'interazione sostenuta con il bersaglio. Entrambi i tool, una volta violato il PIN, permettono di recuperare la vera password WPA/WPA2 della rete.

### Wifite e Fern WiFi Cracker: L'Automazione

Poiché un attacco completo richiede la concatenazione di decine di comandi complessi, entrano in gioco i framework di automazione:

- **Wifite:** È uno ==script di orchestrazione automatizzata==. Con un solo comando (es. `sudo wifite --dict rockyou.txt`), il tool scansiona le reti, individua il bersaglio, lancia automaticamente pacchetti di deautenticazione per forzare la disconnessione dei client, cattura l'handshake e avvia i tool di cracking esterni per decifrare la password. Gestisce persino lo spoofing (mascheramento) del MAC address dell'attaccante.
- **Fern WiFi Cracker:** Scritto in Python, offre un'==alternativa basata su un'interfaccia grafica (GUI), rendendo i flussi di lavoro complessi molto più accessibili==. Fern non solo automatizza gli attacchi WEP, WPA e WPS, ma integra anche capacità di attacco Man-In-The-Middle (MITM). Permette inoltre di lanciare comodamente dalla sua interfaccia attacchi WEP avanzati e specifici, come il _Fragmentation Attack_, _Chop-Chop_, _Caffe-Latte_ e _Hirte_ (utilizzati per recuperare keystream e forzare la generazione di IV manipolando vulnerabilità architetturali del WEP).
 
## Attacchi a Reti con Crittografia WEP

==Il protocollo WEP (Wired Equivalent Privacy) è ampiamente superato, ma il suo studio rimane un pilastro didattico fondamentale per via delle sue debolezze strutturali legate al meccanismo di scheduling delle chiavi e ai Vettori di Inizializzazione (IV) troppo corti e prevedibili==. La compromissione di WEP avviene in due scenari:

**1. Scenario ad Alto Traffico (Cattura Passiva)**: In presenza di una rete WEP in cui gli utenti generano molto traffico, l'attaccante può limitarsi ad ascoltare. Attraverso `airodump-ng`, si accumulano decine di migliaia di frame crittografati e i relativi IV. Raggiunto un numero statisticamente rilevante di pacchetti, si utilizza `aircrack-ng` per effettuare un'analisi statistica sugli IV catturati, arrivando a decifrare la password in chiaro in tempi rapidissimi.
**2. Scenario a Basso Traffico (Iniezione di Pacchetti)**: Nel mondo reale, una rete potrebbe essere accesa ma inattiva, rendendo impossibile la semplice raccolta passiva. In questo caso si forza attivamente l'Access Point a generare traffico. Poiché l'AP risponde solo ai client associati, l'attaccante utilizza `aireplay-ng` per effettuare una **Fake Authentication**, associando il proprio MAC address a quello del router. Avvenuta la finta autenticazione, si lancia un attacco **ARP Replay**: un pacchetto ARP legittimo precedentemente intercettato viene reiniettato migliaia di volte verso l'AP. Questo stimola l'AP a inviare innumerevoli pacchetti crittografati di risposta, portando l'attaccante ad accumulare velocemente i circa 20.000-25.000 IV necessari per lanciare la decodifica finale con `aircrack-ng`.

![[Pasted image 20260614150714.png]]

### Attacchi a Reti con Crittografia WPA/WPA2 (Personal)

==A differenza del WEP, il WPA2 non è vulnerabile ad attacchi diretti agli algoritmi crittografici (essendo basato su AES). Il focus si sposta quindi sulla variante WPA2-Personal, in cui tutti i client di una singola rete si basano su una Pre-Shared Key (PSK) o password comune. La vulnerabilità risiede nello scambio iniziale di autenticazione, noto come 4-Way Handshake==. 

![[Pasted image 20260606074710.png]]

Poiché questo scambio avviene solo quando un client si connette, l'attaccante deve provocarlo attivamente. Ciò si ottiene tramite un attacco di **Deauthentication**: si sfruttano frame di gestione 802.11 (che per natura in WPA/WPA2 non sono crittografati) per inviare falsi comandi di disconnessione al client bersaglio. Il client, vedendosi espulso dalla rete, tenterà automaticamente una riconnessione, eseguendo nuovamente il 4-Way Handshake. Avendo `airodump-ng` in ascolto, l'attaccante cattura la conversazione e ne salva i file. A quel punto si abbandona l'attacco di rete per passare al cracking offline: si fornisce l'handshake e un dizionario di password comuni (come _rockyou.txt_) al tool `aircrack-ng`, che tenta un attacco di forza bruta estraendo le chiavi PMK derivate dal dizionario fino a trovare la corrispondenza esatta.

![[Pasted image 20260614150811.png]]

