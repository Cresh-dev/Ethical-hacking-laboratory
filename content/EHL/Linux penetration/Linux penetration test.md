_Tag:_ #cybersecurity  #hacking

---
==Lo scopo principale è illustrare l'intero processo di validazione e sfruttamento delle vulnerabilità precedentemente identificate su un sistema bersaglio ==(in questo caso la macchina virtuale _OWASP Broken Web Applications_) utilizzando una piattaforma di attacco basata su _Kali Linux_.

Si può delineare una precisa metodologia operativa strutturata in cinque fasi sequenziali:

1. **Analisi del report di Vulnerability Assessment (VA):** Studio dei risultati ottenuti da strumenti di scansione come [[VA con Greenbone OpenVAS (GVM)|Greenbone]] o [[VA con Nessus|Nessus]].
2. **Selezione della vulnerabilità target:** Scelta della falla da investigare in base alla gravità e alla reale possibilità di sfruttamento.
3. **Raccolta di informazioni sull'exploit:** Ricerca e studio di dettagli tecnici e codice di exploit pubblicamente disponibili.
4. **Esecuzione dello sfruttamento (Exploitation):** Tentativo pratico di attaccare il sistema vulnerabile all'interno di un ambiente sicuro e isolato.
5. **Documentazione dei risultati:** Registrazione delle prove del successo dell'attacco, dell'impatto reale e dei dettagli tecnici all'interno di un apposito report di penetration testing.