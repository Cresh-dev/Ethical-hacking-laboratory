_Tag:_ #cybersecurity  #hacking #laboratorio

---
==Per le applicazioni web, viene impiegato Burp Suite come proxy di intercettazione per manipolare le richieste HTTP POST dirette ai form di login==. All'interno del modulo _Intruder_, viene impostato l'attacco di tipo **Cluster Bomb**. Questa modalità permette di definire più set di payload (es. Set 1: Lista di Username; Set 2: Lista di Password) combinandoli in modo combinatorio (prodotto cartesiano dei due set) per testare sistematicamente ogni associazione possibile. Il successo dell'attacco viene confermato dall'analisi delle risposte HTTP (es. un codice di stato `302 Found` o la presenza di cookie di sessione specifici come `Logged-In-User: admin`).

### Hydra e gli attacchi a livello di rete

Per i servizi infrastrutturali o protocolli di rete come **RDP** (Remote Desktop) o **SMB** (Samba), si utilizza **Hydra**, un brute-forcer online multi-protocollo in grado di parallelizzare le richieste di autenticazione fino a trovare la credenziale valida.