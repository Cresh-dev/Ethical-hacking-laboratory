_Tag:_ #cybersecurity  #hacking #bluetooth

---
==Il Bluetooth è una tecnologia di comunicazione wireless a corto raggio progettata per lo scambio di dati a basso consumo energetico tra dispositivi vicini, come smartphone, laptop, cuffie, wearable e sistemi IoT==. Opera nella banda di frequenza ISM a **2.4 GHz** e supporta svariate funzionalità, tra cui lo streaming audio, lo scambio di file, la sincronizzazione e la connettività delle periferiche wireless. Tuttavia, il fatto che i dispositivi Bluetooth pubblicizzino continuamente la propria presenza per stabilire relazioni di fiducia introduce inevitabilmente **rischi di sicurezza e privacy**, specialmente se non configurati correttamente.

## Livelli di Sicurezza

Le specifiche prevedono quattro livelli di sicurezza principali:

- **Livello 1 (Nessuna sicurezza):** Non prevede né autenticazione né cifratura. Massima comodità di connessione, ma esposizione totale a intercettazioni (eavesdropping) e accessi non autorizzati.
- **Livello 2 (Accoppiamento non autenticato con cifratura):** Utilizza il modello _Just Works_ senza conferma da parte dell'utente. Offre una riservatezza di base ma una scarsa protezione contro gli attacchi Man-in-the-Middle (MITM).
- **Livello 3 (Accoppiamento autenticato con cifratura):** Richiede un'interazione esplicita dell'utente (es. inserimento di passkey), garantendo comunicazioni cifrate e una forte protezione MITM.
- **Livello 4 (Connessioni LE autenticate con cifratura):** Sfrutta le moderne _LE Secure Connections_ con cifratura a 128 bit e metodi di accoppiamento autenticati avanzati (come il confronto numerico).

## Modalità Operative di Visibilità

La visibilità di un dispositivo definisce la sua superficie di attacco iniziale:

- **Discoverable:** Il dispositivo è permanentemente visibile a chiunque effettui una scansione.    
- **Limited Discoverable:** Il dispositivo è visibile solo per un breve intervallo temporale (es. 30 secondi), dopodiché torna invisibile.
- **Nondiscoverable:** Il dispositivo è impostato in modalità invisibile, impedendo agli altri di trovarlo tramite scansione ordinaria.

## Minacce e Tecniche di Attacco Comune

Le vulnerabilità Bluetooth derivano spesso da configurazioni deboli, falle nei meccanismi di pairing o bug nell'implementazione dei protocolli. Le minacce si dividono principalmente in:

1. **Furto di dati e violazione della privacy:** Accesso non autorizzato a informazioni sensibili (contatti, SMS, registri chiamate).
2. **Dirottamento del dispositivo (Device Hijacking):** Controllo remoto del dispositivo senza interazione dell'utente (es. attivazione del microfono).
3. **Abuso di servizi telefonici e SMS:** Utilizzo del dispositivo della vittima per effettuare chiamate o inviare SMS a pagamento.
4. **Propagazione di Malware:** Diffusione di software malevolo tramite richieste di scambio file malevole.
5. **Vulnerabilità dello Stack e del Protocollo:** Sfruttamento di bug firmware per causare Denial-of-Service (DoS) o l'esecuzione di codice remoto.

### Glossario delle Tecniche di Attacco

- **Bluejacking:** Invio di messaggi non richiesti (spam) a dispositivi vicini; è fastidioso ma generalmente non distruttivo.
- **Bluesniffing:** Attività di ricognizione per scoprire ed enumerare i dispositivi Bluetooth nelle vicinanze, inclusi quelli nascosti.    
- **Bluesmacking:** Un attacco DoS eseguito inviando pacchetti malformati o di dimensioni eccessive per mandare in crash il sistema target.
- **Bluesnarfing:** Sfruttamento dello stack Bluetooth per accedere e sottrarre dati sensibili dal dispositivo della vittima senza autorizzazione.
- **Bluebugging:** L'attaccante guadagna il controllo totale del dispositivo target all'insaputa dell'utente.

## Strumenti di Analisi ed Enumerazione (BlueZ)

==Nei sistemi Linux, lo stack protocollare ufficiale è BlueZ. Esso mette a disposizione diverse utility, sia storiche che moderne, fondamentali sia per la gestione legittima che per l'hacking==:

- `hciconfig`: Configura le interfacce Bluetooth locali (analogo a _ifconfig_ per le reti IP).
- `hcitool`: Strumento di interrogazione per raccogliere metadati come nomi, ID di dispositivo, classi e clock offset.
- `hcidump`: Consente di sniffare e analizzare il traffico dei pacchetti Bluetooth.
- `bluetoothctl`: Strumento interattivo moderno via CLI che supporta flussi di lavoro sia per il Bluetooth classico che per il BLE (Bluetooth Low Energy).

### Il Flusso di Ricognizione

La fase di Information Gathering segue passaggi precisi:

1. **Attivazione dell'interfaccia:** Si abilita l'adattatore locale con il comando `sudo hciconfig hci0 up`.
2. **Scansione dei target:** Si cercano i dispositivi visibili tramite `hcitool scan` (restituisce il MAC address e il nome dell'host) o `hcitool inq` (restituisce clock offset e classe del dispositivo).
3. **Enumerazione dei servizi:** Identificato il MAC della vittima, si usa il Service Discovery Protocol (SDP) tramite `sdptool browse <MAC_ADDRESS>` per mappare i servizi esposti.

Con il moderno `bluetoothctl`, è possibile avviare un'interfaccia interattiva che permette di gestire in tempo reale accoppiamenti (`pair`), connessioni (`connect`), autorizzazioni (`trust`), e disconnessioni o rimozioni dei target (`disconnect`, `remove`).

### Spoofing dell'identità

==Spooftooph è un tool (ormai quasi deprecato) orientato al MAC Spoofing. Permette di modificare via riga di comando l'indirizzo MAC, la classe e il nome della propria interfaccia per impersonare un altro dispositivo legittimo della vittima==.

## Best Practices di Mitigazione

Per difendersi dalle minacce descritte, il documento evidenzia cinque regole fondamentali di sicurezza:

1. **Modificare i PIN predefiniti:** Evitare l'uso di codici di accoppiamento banali o di fabbrica (es. 0000 o 1234).
2. **Utilizzare la modalità Non-Discoverable:** Mantenere il Bluetooth invisibile quando non si è attivamente impegnati in un accoppiamento legittimo, riducendo la visibilità agli scan ostili.
3. **Monitorare i dispositivi accoppiati:** Revisionare con frequenza la lista dei dispositivi associati in passato, eliminando immediatamente quelli sconosciuti o obsoleti.
4. **Verificare rigorosamente le richieste di pairing:** Non accettare mai alla cieca nuove richieste di connessione, rifiutando qualsiasi sorgente ignota.    
5. **Mantenere aggiornati sistemi e librerie:** Applicare tempestivamente le patch di sicurezza del sistema operativo e dello stack Bluetooth per correggere le falle logiche e protocollari (come, appunto, la CVE-2023-45866).