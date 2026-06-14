_Tag:_ #cybersecurity  #hacking #laboratorio

---
Utilizzando **Hackbar** all'interno dell'ambiente Damn Vulnerable Web Application (DVWA), è possibile estrarre username e hash delle password degli utenti. La mitigazione richiede la sanificazione degli input e un'architettura a tre livelli (UI, Application, Data) basata sul modello **Zero Trust**.

## Intercettazione e Proxy: OWASP ZAP

Quando le protezioni lato client (come JavaScript) impediscono l'invio di input malevoli direttamente dal browser, si ricorre a un **intercepting proxy** come **OWASP ZAP**. Il software agisce tra il browser e il server:

1. Si configura il browser per puntare al proxy (`127.0.0.1:8080`).
2. Si attiva il "Break" (icona rossa) per bloccare ogni richiesta.
3. Si manipolano i dati (es. cambiare username o password) e si inoltra il pacchetto "manomesso" al server. Un'alternativa professionale molto diffusa è **Burp Suite**.

![[Screenshot 2026-05-16 at 11.13.42.png]]