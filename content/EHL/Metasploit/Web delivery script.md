_Tag:_ #cybersecurity  #hacking #metasploit #laboratorio

---
Il **Web Delivery Script**, è un modulo estremamente versatile che permette di distribuire ed eseguire payload tramite linguaggi di scripting (come PHP, Python o PowerShell). ==Questo scenario è fondamentale per comprendere gli attacchi "fileless", dove il codice malevolo non viene necessariamente salvato sul disco rigido della vittima, riducendo drasticamente le tracce lasciate==.

## Funzionamento e Architettura

Il modulo `exploit/multi/script/web_delivery` trasforma la macchina attaccante in un server web temporaneo. Invece di inviare un file eseguibile, il server ospita uno script che, una volta richiamato, scarica ed esegue il payload direttamente nella memoria del processo del linguaggio scelto (in questo caso PHP).

## Configurazione del Modulo e del Payload

La fase di setup richiede precisione nella scelta dei target e dei protocolli:

- **Selezione del Modulo**: Si utilizza `use exploit/multi/script/web_delivery`.
- **Scelta del Payload**: Viene impostato `generic/shell_reverse_tcp`. A differenza di Meterpreter, questa è una shell di comando più semplice ma molto efficace per stabilire la prima connessione.
- **Targeting**: È necessario specificare la tecnologia presente sulla vittima (es. `set target PHP`) affinché [[Exploitation con Metasploit|Metasploit]] generi il codice corretto.
- **Parametri di Rete**: Come sempre, si definiscono `LHOST` (IP attaccante) e `LPORT` (porta di ascolto, es. 1234).

## Esecuzione sul Target

Una volta lanciato l'exploit, Metasploit genera un comando "one-liner" (una singola riga di codice) che deve essere eseguito sulla macchina vittima.

- **Il Comando PHP**: Il comando utilizza `file_get_contents` per recuperare il payload dall'URL dell'attaccante ed `eval()` per eseguirlo istantaneamente.
- **Installazione Dipendenze**: Sulla macchina Ubuntu target, è necessario che sia presente il pacchetto `php-cli` per interpretare il comando.

## Gestione della Sessione e Post-Exploitation

Dopo che la vittima ha eseguito il comando, l'attaccante riceve una notifica di sessione aperta.

- **Interazione**: Si possono visualizzare le sessioni con `show sessions` ed entrarvi con `sessions -i <ID>`.
- **Comandi Diretti**: È possibile eseguire comandi sulla vittima senza entrare in modalità interattiva, ad esempio per leggere file sensibili come `/etc/shadow` o verificare la configurazione di rete con `ifconfig`.