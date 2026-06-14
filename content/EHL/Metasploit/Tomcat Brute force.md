_Tag:_ #cybersecurity  #hacking #metasploit #brute-force #laboratorio

---
==Questa esercitazione si concentra sul Brute Force contro l'interfaccia di gestione di Apache Tomcat, un server ampiamente utilizzato per l'esecuzione di applicazioni web basate su Java (come Servlet e JSP)==. In questo scenario, l'obiettivo non è sfruttare una vulnerabilità del codice, ma colpire una configurazione debole o predefinita del sistema.

## Il Target: Tomcat Manager

Apache Tomcat include un'applicazione chiamata **Web Application Manager**, che permette agli amministratori di gestire l'intero ciclo di vita delle applicazioni sul server (avvio, arresto, aggiunta e rimozione).

- È frequente trovare questa interfaccia esposta pubblicamente.
- Il rischio principale risiede nell'utilizzo di credenziali di default (es. `admin:admin` o `tomcat:s3cret`) o facilmente indovinabili.

## Selezione del Modulo: `tomcat_mgr_login`

Per questa operazione non si usa un exploit, ma un modulo **Auxiliary**, specificamente un componente di scansione.

- **Comando**: `use auxiliary/scanner/http/tomcat_mgr_login`.
- **Funzionamento**: Il modulo esegue un attacco di tipo "dictionary-based brute force", testando sistematicamente diverse combinazioni di username e password contro il pannello di login di Tomcat.
- **Wordlist**: Di default, il framework utilizza liste predefinite, ma è possibile configurare dizionari personalizzati per aumentare l'efficacia.

> [!NOTE] Cosa sono i moduli ausiliari e come differiscono dagli exploit
> In **msfconsole**, un **modulo ausiliario** (Auxiliary module) è una categoria di strumenti che, a differenza degli exploit, non ha come obiettivo primario l'esecuzione di codice malevolo tramite una vulnerabilità, ma serve a supportare le diverse fasi di un attacco. Mentre un exploit è progettato per "consegnare" un payload (come Meterpreter) al target, i moduli ausiliari eseguono un'azione specifica e terminano il compito senza necessariamente stabilire una sessione interattiva.

## Configurazione dei Parametri Tecnici

Prima del lancio, è necessario definire i parametri che bilanciano velocità e furtività:

- **RHOSTS**: L'indirizzo IP del server target (es. 192.168.56.3).
- **THREADS**: Definisce il numero di tentativi di login simultanei. Un valore come `5` è un compromesso ottimale: aumenta la velocità senza sovraccaricare eccessivamente la macchina attaccante o saturare la rete del target.
- **BRUTEFORCE_SPEED**: Regola l'aggressività dell'attacco (da 0 a 5). Un valore pari a `3` garantisce un ritmo ragionevole senza generare un traffico così rumoroso da attivare immediatamente i sistemi di rilevamento (IDS/IPS).

## Esecuzione e Analisi dei Risultati

Lanciando l'attacco con il comando `run`, [[Exploitation con Metasploit|Metasploit]] inizia a testare le combinazioni.

- **Analisi dell'Output**: La console mostrerà una serie di `LOGIN FAILED` per ogni tentativo errato.
- **Successo**: Quando il modulo identifica le credenziali corrette, stampa un messaggio evidenziato: `[+] Login Successful: root:owaspbwa`.
- **Conseguenze**: Una volta ottenute le credenziali del Manager, un attaccante potrebbe caricare un file `.war` malevolo (una "Web Shell") per ottenere il controllo completo del server, trasformando un accesso amministrativo in una compromissione totale del sistema.