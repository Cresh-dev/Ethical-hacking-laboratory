_Tag:_ #cybersecurity  #hacking

---
==Il password cracking è il processo di reverse-engineering o recupero di una password in chiaro a partire dalla sua rappresentazione cifrata o hash memorizzata in un sistema di autenticazione==. In ambito di [[Penetration Test|penetration testing]], l'obiettivo è identificare configurazioni errate o credenziali deboli.

Le metodologie d'attacco principali si dividono in:

- **Password Guessing:** Tentativi manuali o automatizzati basati su pattern prevedibili del comportamento utente.
- **Attacchi a Dizionario (Dictionary Attacks):** Uso di liste predefinite di password note o trapelate (es. il celebre file `rockyou.txt` integrato in Kali Linux).
- **Brute Force:** Computazione sistematica di tutte le combinazioni possibili di caratteri; garantisce la risoluzione a fronte di un elevato costo computazionale e temporale.
- **Rainbow Tables:** Utilizzo di tabelle di hash precomputate per accelerare il cracking di hash non protetti da "salt".
- **Approcci Ibridi:** Combinazione di dizionari e mutazioni algoritmiche (es. aggiunta di caratteri speciali o cifre a parole esistenti).

## Generazione di Wordlist Mirate

Spesso i dizionari generici sono insufficienti. Si ricorre quindi a strumenti di OSINT e scraping come **CeWL** (che esegue il crawling di siti web estrandone i termini per creare dizionari contestuali) o **Crunch** (per la generazione programmata di stringhe basate su set di caratteri e lunghezze specifiche).

## Sistemi di Autenticazione e Attacchi Offline

Gli attacchi offline si focalizzano sulla compromissione di hash estratti localmente dai sistemi target, sfruttando la massima potenza di calcolo hardware (CPU/GPU) senza interagire con la rete.

### L'Ambiente Linux

==Nei sistemi [[Basi di Linux|Linux]], le informazioni protette degli account risiedono nel file /etc/shadow, leggibile solo dall'utente root==. Ogni riga descrive un utente tramite campi separati da due punti (`:`). La password cifrata segue la sintassi standard:

$$\$id\$salt\$hash$$

![[Screenshot 2026-05-18 at 15.53.07.png]]

L'identificatore `\$id\$` definisce l'algoritmo di hashing utilizzato:

- `$1$`: MD5 (obsoleto e vulnerabile).
- `$5$ / $6$`: SHA-256 / SHA-512 (standard diffusi ma vulnerabili a calcolo parallelo).
- `$y$ / $7$`: **yescrypt** / **scrypt** (algoritmi moderni _memory-hard_, strutturati appositamente per resistere ad attacchi massivi tramite GPU o ASIC).

### L'Ambiente Windows: Il file SAM

==In Windows, i flussi di autenticazione locale sono gestiti dal database SAM (Security Account Manager)==, situato in `C:\Windows\System32\config\SAM`. Le voci del SAM includono il RID (Relative Identifier), l'insicuro hash LM (spesso nullo nei sistemi moderni) e l'**hash NTLM**. ==NTLM non implementa il salting, rendendosi intrinsecamente vulnerabile ad attacchi a dizionario e rainbow tables==.

## Attacchi Online e Manipolazione dei Protocolli Web

==A differenza dei modelli offline, gli attacchi online bersagliano servizi attivi in rete (es. form di login web, RDP, SMB). Le performance sono limitate dalla latenza di rete e dai controlli difensivi del server, esponendo l'attaccante a un elevato rischio di rilevamento==.

## Strategie Difensive e Mitigazione del Rischio

L'efficacia delle tecniche d'attacco evidenzia la necessità di solide politiche di sicurezza.

1. **Complessità e Lunghezza:** L'entropia della password è la prima difesa. I dati statistici (riferiti a una stazione di calcolo avanzata dotata di 12 GPU RTX 5090 contro un hash protetto da bcrypt) dimostrano che una password di soli numeri o di sole lettere minuscole viene violata istantaneamente fino a lunghezze elevate. Al contrario, l'adozione di stringhe alfanumeriche complesse con simboli e una lunghezza minima di 12-14 caratteri sposta il tempo teorico di cracking su scale temporali di milioni o miliardi di anni.

![[Screenshot 2026-05-19 at 09.35.03.png]]

2. **Esclusione del riutilizzo:** Evitare la replicazione delle credenziali su piattaforme diverse previene gli attacchi di _credential stuffing_ in caso di data breach.
3. **Password Manager:** Utilizzo di gestori crittografici per generare e mantenere password ad alta entropia.
4. **Autenticazione Multifattoriale (MFA):** Configurazione obbligatoria dell'MFA come secondo layer di sicurezza non legato alla robustezza della password testuale.
5. **Politiche di Account Lockout (e relativi rischi):** L'implementazione di soglie di blocco account (es. blocco dopo X tentativi falliti) contrasta i brute force online. Tuttavia, si evidenziano come tali policy espongano i sistemi ad attacchi di tipo **Denial of Service (DoS)**, permettendo a un utente malevolo di bloccare massivamente gli account legittimi della rete semplicemente inserendo credenziali errate.