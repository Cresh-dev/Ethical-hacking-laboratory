_Tag:_ #cybersecurity  #hacking #laboratorio 

---
Il flusso di questo attacco prevede l'uso di [[Exploitation con Metasploit|Metasploit]] Framework:

- Viene configurato l'exploit `adobe_utilprintf` per generare un file PDF malevolo, assegnandogli un nome apparentemente legittimo (es. `UpgradeInstructions.pdf`) per ingannare la vittima.
- Viene integrato un payload di tipo `reverse_tcp`, che istruisce il sistema infetto a stabilire una connessione verso la macchina dell'attaccante.
- Prima della consegna del file, l'attaccante configura un "listener" (tramite il modulo `multi/handler` di Metasploit) in attesa della connessione in ingresso.
- Quando la vittima apre il file, il PDF innesca la vulnerabilità: l'interfaccia grafica potrebbe mostrare solo una schermata grigia bloccata, ma nel frattempo il codice malevolo apre una shell remota sulla macchina dell'attaccante, compromettendo il sistema.

A livello prettamente accademico, questo particolare exploit richiede un ambiente applicativo specifico e datato, come Adobe 8 in esecuzione su Windows XP SP3.