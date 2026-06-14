_Tag:_ #cybersecurity  #hacking #laboratorio

---
==Questo scenario affronta le vulnerabilità del software Tiki Wiki CMS Groupware (versione 1.9.5), identificato dal report [[Vulnerability Assessment|VA]] come un prodotto giunto alla fine del suo ciclo di vita (End of Life) e quindi privo di aggiornamenti di sicurezza==. 

![[Pasted image 20260529215029.png]]

Vengono analizzati due exploit specifici. 
## CVE-2006-5702

![[Pasted image 20260529215729.png]]

Il primo, identificato dalla CVE-2006-5702, è una _Information Disclosure_ che permette a un utente non autenticato di estrarre le credenziali del database MySQL causando un errore mirato tramite la variabile `sort_mode`.

![[Pasted image 20260529215530.png]]

## CVE-2007-5423

![[Pasted image 20260529215813.png]]

Il secondo attacco (CVE-2007-5423) è una grave vulnerabilità di _Remote PHP Code Execution_ causata dalla mancata sanitizzazione degli input dell'utente nello script `tiki-graph_formula.php`. 

![[Pasted image 20260529215838.png]]

Quest'ultimo viene sfruttato utilizzando uno specifico modulo di [[Exploitation con Metasploit|Metasploit]] 5 (eseguito in un container Docker per questioni di compatibilità) che garantisce all'attaccante l'apertura di molteplici sessioni di command shell.