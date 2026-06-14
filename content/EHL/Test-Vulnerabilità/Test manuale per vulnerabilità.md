_Tag:_ #cybersecurity  #hacking

---
==Sebbene gli scanner automatizzati ([[Vulnerability Assessment|VA]]) siano utili, il testing manuale è fondamentale perché gli strumenti automatici spesso non rilevano vulnerabilità complesse== come i **difetti di logica** o i **problemi di workflow**. L'analisi manuale permette una valutazione mirata basata sul contesto e sull'esperienza.

## Strumenti e Tecniche di Intercettazione

Per testare un'applicazione web, è necessario interagire con i parametri URL e intercettare le richieste inviate al server, specialmente quando esistono validazioni client-side (JavaScript) o parametri nascosti.

- **Hackbar v2:** Un'estensione per browser che funge da seconda barra degli indirizzi, utile per modificare parametri POST, bypassare reindirizzamenti e iniettare payload SQLi/XSS.
- **OWASP ZAP (Zed Attack Proxy):** Uno ==strumento avanzato che agisce come proxy locale (127.0.0.1:8080). Permette di mettere in pausa le richieste (Break), modificarle e inoltrarle al server==.
- **Burp Suite:** Alternativa professionale a ZAP, che include strumenti come _Repeater_ (per re-inviare richieste modificate) e _Intruder_ (per automazione di base).

## SQL Injection (SQLi) e Manipolazione del Database

L'**SQL Injection** rimane una delle minacce principali (OWASP Top 10) per le applicazioni web che utilizzano database. Si verifica quando ==l'input dell'utente viene concatenato direttamente nelle query SQL senza validazione==. Un attaccante può inserire frammenti di codice come `' OR '1'='1` per rendere la condizione della query sempre vera, estraendo così l'intero contenuto di una tabella. 

![[Screenshot 2026-05-16 at 11.12.29.png]]

## Cross-Site Scripting (XSS)

==L'XSS permette di iniettare codice JavaScript nel browser di altri utenti perché l'applicazione non sanifica correttamente l'input che viene poi visualizzato nella pagina==.

Esistono tre tipologie principali:

- **Reflected XSS**: Il codice malevolo è contenuto in un URL e viene "riflettuto" dal server nella risposta (es. un messaggio di benvenuto personalizzato).
- **Stored XSS (Persistent)**: Molto più pericoloso; il codice viene salvato sul database del server (es. in un commento) e colpisce chiunque visualizzi quella pagina.
- **DOM-based XSS**: La vulnerabilità risiede interamente nel codice JavaScript lato client e non passa necessariamente dal server.

L'impatto include il furto di cookie di sessione e il reindirizzamento degli utenti verso siti malevoli.

### Command Injection

Si verifica quando ==l'input dell'utente viene passato direttamente a comandi del sistema operativo==. Ad esempio, in un form di "Ping", concatenando un comando come `; cat /etc/passwd`, l'attaccante può leggere file sensibili del server o prenderne il controllo totale.

### Sicurezza dei Cookie

I siti web sono "stateless" (senza memoria): ogni richiesta HTTP è indipendente dalle altre. ==Per evitare che si debba inserire username e password a ogni click, il server genera un codice identificativo unico quando si effettua il login e lo invia al browser sotto forma di cookie==. 

![[Screenshot 2026-05-16 at 11.51.33.png]]

**PHPSESSID** è il nome standard che il linguaggio PHP dà al cookie di sessione. Quel lungo codice alfanumerico è la "chiave d'accesso". Chiunque possieda quel codice può farsi passare per l'utente senza bisogno delle sue credenziali (attacco chiamato **Session Hijacking** o dirottamento della sessione).

Per prevenire il furto di sessione, i cookie devono avere:

- **HTTPOnly flag**: Impedisce a JavaScript di leggere il cookie (mitiga XSS).
- **Secure flag**: Obbliga il browser a inviare il cookie solo su connessioni HTTPS cifrate.

> [!NOTE] Cosa succede se Secure flag non è settato
> Se questo flag non è impostato, un utente malintenzionato può eseguire un attacco man-in-the-middle (MiTM) e ottenere il cookie di sessione tramite HTTP, che lo fornisce in chiaro poiché HTTP è un protocollo in chiaro.

## SSL/TLS e Vulnerabilità Legacy

Sono state scoperte numerose vulnerabilità nell'implementazione e nella progettazione del protocollo SSL/TLS; pertanto, il test delle connessioni sicure diventa obbligatorio in qualsiasi penetration test di applicazioni web. L'analisi tramite **SSLScan** permette di individuare configurazioni deboli.

- **POODLE**: Un ==attacco che forza il downgrade della connessione a protocolli obsoleti come SSLv3 per decifrare le comunicazioni==.

![[Screenshot 2026-05-16 at 11.56.59.png]]

- **Heartbleed**: Una ==falla nell'estensione Heartbeat di OpenSSL che permette di leggere la memoria del server senza autenticazione==. Sebbene il laboratorio mostri che il server DVWA possa non essere vulnerabile, è un test fondamentale da eseguire con script Nmap.