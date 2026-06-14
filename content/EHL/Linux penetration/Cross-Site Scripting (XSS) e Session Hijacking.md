_Tag:_ #cybersecurity  #hacking #laboratorio

---
==L'obiettivo di questa esercitazione è sfruttare una vulnerabilità XSS di tipo persistente (Stored XSS) per sottrarre i cookie di sessione di un utente legittimo==. L'attaccante ospita un semplice script PHP (un "cookie collector") sulla propria macchina per salvare i dati rubati. Successivamente, inietta un payload in JavaScript all'interno della sezione commenti dell'applicazione vulnerabile. 

![[Pasted image 20260608103854.png]]

Quando una vittima naviga in quella pagina, il codice malevolo viene eseguito dal suo browser in modo asincrono (tramite `XMLHttpRequest`), inviando in background il cookie di sessione all'attaccante. Acquisito il cookie (il `PHPSESSID`), l'attaccante utilizza un proxy come OWASP ZAP per intercettare il proprio traffico HTTP, rimpiazzare il proprio cookie con quello rubato e autenticarsi nel sistema con i privilegi della vittima (Session Hijacking).

![[Pasted image 20260608104010.png]]