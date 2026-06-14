_Tag:_ #cybersecurity  #hacking

---
La **fase di Information Gathering** (raccolta informazioni), nota anche come **Reconnaissance** (ricognizione), è definita come il ==primo e fondamentale stadio di un processo di hacking o [[Penetration Test|penetration test]]. Il principio cardine è che una maggiore quantità di dati sul bersaglio aumenta proporzionalmente le probabilità di successo dell'exploitation==. La raccolta informazioni si divide in due metodologie principali:

- **Active Information Gathering (Attiva):** ==Prevede un'interazione diretta con il target==. Permette di scoprire porte aperte, servizi attivi e sistemi operativi. Tuttavia, è una tecnica "rumorosa" poiché genera log ed è facilmente rilevabile da sistemi di difesa come **IDS, IPS e firewall**.
- **Passive Information Gathering (Passiva):** ==Non vi è interazione diretta con i sistemi del bersaglio==. Si utilizzano fonti esterne come motori di ricerca, social media (LinkedIn, Facebook) e siti web terzi. È il metodo raccomandato per la sua discrezione, non lasciando tracce sui log del target.

## Tecniche e Strumenti di Ricognizione Attiva

Si può tener conto di diverse tecniche per ottenere dati tecnici specifici.

### Identificazione di Dominio e Server

- **Whois:** Database che contiene informazioni sulla proprietà dei domini e contatti email degli amministratori (utili per il social engineering). Accessibile tramite riga di comando o siti come `whois.domaintools.com`.
- **Reverse IP Lookup:** Tecnica per individuare altri siti web ospitati sullo stesso server fisico del bersaglio. Strumenti comuni: `yougetsignal.com` o `reverseip.domaintools.com`.
- **Localizzazione IP:** Utilizzo del comando `ping` per ottenere l'indirizzo IP del server e successiva geolocalizzazione tramite strumenti come `ip-tracker.org`.

![[Screenshot 2026-06-05 at 23.51.55.png]]

### Analisi della Rete (Traceroute)

Il **Traceroute** serve per l'orientamento di rete, ovvero per mappare la topologia, firewall e load balancer. Funziona incrementando il campo **TTL (Time To Live)** dei pacchetti IP.

> [!NOTE] Approfondimento sul traceroute
> Il traceroute permette di vedere il percorso esatto che i dati compiono dal computer del mittente al server di destinazione, identificando ogni router, firewall o load balancer incontrato lungo la strada. Il segreto del traceroute sta in un campo dell'intestazione del pacchetto IP chiamato **TTL (Time To Live)**. Esso ha il seguente funzionamento:
> 1. **L'invio**: Il computer invia un pacchetto con un valore **TTL=1**.
> 2. **Il primo "Hop"**: Quando il pacchetto raggiunge il primo router (chiamato "hop"), il router diminuisce il TTL di 1. Poiché il valore diventa zero, il router scarta il pacchetto e invia al mittente un messaggio di errore ("ICMP Time Exceeded").
> 3. **L'identificazione**: Ricevendo l'errore, il computer registra l'indirizzo IP di quel router e il tempo impiegato.
> 4. **La progressione**: Il processo si ripete aumentando il TTL a 2, poi a 3, e così via, finché il pacchetto non raggiunge la destinazione finale.

- **ICMP Traceroute:** Default in Windows, ma spesso bloccato dai firewall (causando "Request timed out"). Questi dispositivi sono configurati per identificare e **bloccare le richieste ICMP Echo** (i pacchetti del traceroute), impedendoti di mappare il resto della rete oltre quel punto.
- **TCP/UDP Traceroute:** Spesso bypassano le restrizioni perché simulano traffico applicativo normale. Il TCP traceroute invia pacchetti SYN, mentre quello UDP invia pacchetti a porte alte inutilizzate.

#### Interazione con DNS

Il DNS è una delle fonti più ricche di informazioni.

- **Nslookup e Dig:** Strumenti per interrogare i record DNS (es. `MX` per i server mail, `NS` per i name server, `A` per gli indirizzi IP). `Dig` è considerato più flessibile e potente di `nslookup`.   
- **Reverse DNS Lookup:** Mappa un indirizzo IP a un nome di dominio interrogando i record PTR.
- **DNS Cache Snooping:** Tecnica per determinare se un record è presente nella cache di un server DNS, rivelando se un utente ha visitato recentemente un determinato sito. Può essere **non-ricorsiva** (più precisa, impostando "Recursion Desired" a 0) o **ricorsiva** (analizzando la diminuzione del valore TTL).

### Scansione di Vulnerabilità e Servizi

Esistono vari tool a disposizione per effettuare una scansione della rete e dei servizi con i relativi protocolli messi a disposizione sul sistema. ==È cruciale identificare la presenza di un WAF, IDS o IPS prima di iniziare un attacco per evitare il ban dell'IP==. Nmap dispone di script specifici come `http-waf-detect` per questo scopo.

#### Port Scanning con Nmap

**Nmap** è lo strumento standard per identificare host attivi e servizi. Di seguito sono elencati i Flag principali:

- `-sV`: Identifica le versioni dei servizi tramite banner-grabbing.
- `-O`: Tenta di indovinare il sistema operativo.
- `-Pn`: Salta il test di ping se si sa già che l'host è attivo.
- `-v`: Modalità verbosa per dettagli maggiori.

#### Altre Analisi

- **SSL/TLS Scanning:** Strumenti come `openssl s_client`, `nmap --script ssl-enum-ciphers` o servizi online (SSLLabs) permettono di verificare la corretta installazione dei certificati e la robustezza dei cifrari.
- **Google Dorking:** Utilizzo di operatori di ricerca avanzati (es. `filetype:pdf`, `intitle:"index of"`) per trovare informazioni sensibili, file di configurazione o directory pubbliche indicizzate da Google.
- **Analisi del Codice Sorgente:** L'esame manuale dell'HTML/JS di una pagina permette di comprendere la logica di programmazione e individuare librerie di terze parti o vulnerabilità palesi.