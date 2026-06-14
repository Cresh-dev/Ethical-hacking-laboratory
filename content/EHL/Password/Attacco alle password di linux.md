_Tag:_ #cybersecurity  #hacking #laboratorio

---
Tramite **OpenSSL**, viene generato un hash SHA-512 (con salt) della password "chill". Successivamente, utilizzando **Hashcat** in modalità dizionario (`-a 0`) e specificando il tipo di hash (`-m 1800`), la stringa viene decifrata in pochissimi secondi sfruttando la corrispondenza nel dizionario `rockyou.txt`.

![[Screenshot 2026-05-18 at 16.21.15.png]]
