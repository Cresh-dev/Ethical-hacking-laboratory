_Tag:_ #cybersecurity  #hacking #laboratorio

---
==Questo laboratorio dimostra un attacco di tipo Denial of Service (DoS) contro il server web Apache versione 2.2.14, affetto dalla vulnerabilità CVE-2011-3192==. 

![[Pasted image 20260529220016.png]]

Questa debolezza risiede nel modo in cui il server gestisce l'header HTTP "Range". L'invio di richieste appositamente manipolate con molteplici intervalli di byte sovrapposti forza il server a un consumo spropositato di memoria e cicli CPU, portando a instabilità del sistema. L'attacco viene condotto utilizzando l'apposito modulo ausiliario `apache_range_dos` di [[Exploitation con Metasploit|Metasploit]], ed è possibile constatarne l'efficacia monitorando l'allocazione anomala delle risorse sul server bersaglio tramite il comando di sistema `top`.

![[Pasted image 20260529220100.png]]


