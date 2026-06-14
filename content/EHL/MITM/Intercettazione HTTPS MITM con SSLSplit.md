_Tag:_ #cybersecurity  #hacking #mitm #network #laboratorio

---
==L'attacco [[Man in the Middle Attacks|MITM]] viene esteso al canale cifrato mediante l'applicativo SSLSplit, un proxy trasparente orientato alla scomposizione delle sessioni SSL/TLS==.

![[Screenshot 2026-05-18 at 10.36.52.png]]

La procedura operativa consta dei seguenti passaggi:

1. **Generazione della CA locale:** Sulla macchina dell'attaccante viene generata una chiave privata RSA a 4096 bit e un certificato CA auto-firmato (`ca.crt`) fittizio (avente come attributi geografici ed organizzativi _Bari, Poliba_).
2. **Configurazione del Kernel e di Netfilter:** Viene abilitato l'IP forwarding a livello kernel Linux per consentire il routing dei pacchetti. Successivamente, mediante la suite `iptables`, vengono definite regole nella tabella NAT per reindirizzare forzatamente tutto il traffico TCP originariamente destinato alle porte standard 80 (HTTP) e 443 (HTTPS) verso le porte di ascolto locali del proxy (rispettivamente la 8080 e la 8443).
3. **Esecuzione dello Splitting:** Viene avviato SSLSplit in modalità trasparente puntando alla CA locale e indicando le directory di logging. Durante l'handshake TLS, SSLSplit intercetta la richiesta del browser e presenta un certificato contraffatto firmato dalla CA dell'attaccante. Se il browser accetta l'autorità o la connessione viene forzata, l'attaccante è in grado di terminare la crittografia, visionare e memorizzare i flussi informativi in chiaro in appositi file di log cronologici (es. `connections.log` e log di sessione), per poi cifrare nuovamente i dati e inoltrarli al server reale.