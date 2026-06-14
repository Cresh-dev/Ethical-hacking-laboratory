_Tag:_ #cybersecurity  #hacking #mitm

---
==Un attacco Man-in-the-Middle (MITM) si configura quando un agente malevolo si posiziona in modo trasparente e furtivo all'interno del canale di comunicazione tra due entità legittime (tipicamente un client e un server)==. La dinamica dell'attacco si sviluppa in tre fasi macroscopiche:

1. **Interruzione del canale originale:** L'attaccante altera la normale comunicazione tra le vittime inserendosi nel percorso di scambio dei dati.
2. **Intercettazione e inoltro (Relay):** I messaggi di entrambe le parti vengono intercettati e poi reindirizzati al reale destinatario, preservando l'illusione di un'interazione diretta e non compromessa.
3. **Manipolazione (opzionale):** Una volta stabilita la persistenza nel canale, l'attaccante può alterare i dati in transito in tempo reale, abilitando attività ostili quali il furto di credenziali o l'hijacking delle sessioni.

## Intercettazione nei Canali Cifrati: SSL Bumping, SSL Inspection e SSLSplit

==L'avvento del protocollo HTTPS, protetto dalla suite crittografica TLS, garantisce riservatezza e integrità, impedendo la lettura del payload da parte di dispositivi intermediari. Questo introduce un trade-off sotto il profilo della network security: se da un lato protegge gli utenti, dall'altro acceca gli strumenti di monitoraggio difensivo (IDS/IPS, firewall)==.

Per ovviare a ciò negli ambienti aziendali o per scopi di auditing, si ricorre a due modelli controllati:

1. **SSL Bumping (Transparent Interception):** ==Il dispositivo intermedio intercetta l'handshake e, conoscendo o potendo derivare i parametri crittografici/chiavi simmetriche di sessione del server, decifra il traffico in modo trasparente dal punto di vista del client==.
2. **SSL Inspection (Explicit Proxy Model):** ==Il proxy termina la sessione TLS del client e ne instaura una nuova, distinta, verso il server di destinazione. Ciò impone che la macchina client consideri fidata (tramite installazione locale) una Certification Authority (CA) radice di proprietà dell'intermediario, che provvederà a generare certificati dinamici e falsificati impersonando il server web target==.