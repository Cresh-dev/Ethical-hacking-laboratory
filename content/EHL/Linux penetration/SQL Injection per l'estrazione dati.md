_Tag:_ #cybersecurity  #hacking #laboratorio 

---
==Questo laboratorio consiste nello sfruttamento di una vulnerabilità SQL Injection (SQLi) per estrarre informazioni sensibili==. 

![[Pasted image 20260608104031.png]]

Procedendo per gradi, l'attaccante inizia inserendo la clausola `ORDER BY` per comprendere la struttura della query e capire il numero esatto di colonne interrogate dal database. Successivamente, tramite l'operatore `UNION SELECT`, viene interrogato il database di sistema `information_schema`. Questo permette di effettuare una vera e propria mappatura del database bersaglio, estraendo nomi di schema, tabelle e colonne utili. Rintracciati i campi corretti, l'attacco si conclude con l'estrazione diretta degli _username_ e delle rispettive _password_ conservate nel database, queste ultime memorizzate sotto forma di hash MD5.

![[Pasted image 20260614143840.png]]