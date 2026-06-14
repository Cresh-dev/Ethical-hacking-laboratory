_Tag:_ #cybersecurity  #hacking #laboratorio

---
Il processo di esfiltrazione e cracking prevede:

1. **Estrazione dei registri:** Copia dei file _SAM_ e _SYSTEM_ (Windows protegge parte dei dati nel SAM usando chiavi memorizzate nell’hive **SYSTEM**) tramite prompt dei comandi con privilegi amministrativi.
2. **Esfiltrazione sicura:** Trasferimento dei file su una macchina Kali Linux tramite protocollo SCP/SSH.
3. **Dumping degli Hash:** Utilizzo del tool `impacket-secretsdump` per estrarre la stringa NTLM pura.
4. **Cracking con John the Ripper:** Pulizia del dataset (tramite comandi `grep` e `awk`) e successiva sottomissione del file a **John the Ripper** impostando il formato specifico (`--format=NT`).

> [!NOTE] Cosa è Impacket
> Impacket è una raccolta di strumenti Python usati in penetration testing e analisi di protocolli di rete Windows. `impacket-secretsdump` (spesso chiamato semplicemente `secretsdump.py`) è uno degli strumenti inclusi. Serve a **estrarre credenziali e segreti da sistemi Windows**.
> 

John the Ripper si distingue anche per la modalità **Single Crack**, che genera variazioni e mutazioni basate sul nome utente stesso (es. modifiche di capitalizzazione), basandosi sul presupposto che gli utenti tendano a creare password correlate alla propria identità.
