_Tag:_ #cybersecurity  #hacking #laboratorio

---
==Questa esercitazione si concentra sulla vulnerabilità di File Upload sull'applicazione web DVWA. Questo tipo di falla si verifica quando il server non valida in modo adeguato aspetti come il nome, il tipo, il contenuto o la dimensione dei file caricati dagli utenti==. L'attaccante sfrutta questa mancanza per caricare una _webshell_ o una _backdoor_. Nello specifico, utilizzando il tool _msfvenom_ (componente del framework [[Exploitation con Metasploit|Metasploit]]), viene generato un payload in PHP che instaura una _reverse shell_ verso la macchina attaccante. 

![[Pasted image 20260529214724.png]]

Una volta caricato il file malevolo (es. `msf_shell.php`), l'attaccante si mette in ascolto tramite Metasploit o _netcat_ e innesca il payload richiamando l'URL del file caricato tramite il browser, ottenendo così l'accesso al sistema.

![[Pasted image 20260614143620.png]]