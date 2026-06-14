_Tag:_ #cybersecurity  #hacking #pt

---
==Il Penetration Testing viene definito come una simulazione autorizzata e controllata di un attacco informatico, il cui obiettivo primario è identificare e valutare le debolezze di sicurezza di un sistema prima che possano essere sfruttate da criminali informatici==. A differenza delle analisi teoriche, il PT ha un approccio pratico che dimostra come le vulnerabilità possano essere sfruttate nel mondo reale, fornendo preziose indicazioni per migliorare la sicurezza complessiva di un'organizzazione. Esistono due macro-categorie principali di test: il penetration test _esterno_ e quello _interno_. 

![[Screenshot 2026-04-28 at 18.39.11.png]]

Il test esterno simula un attacco proveniente dall'esterno dell'organizzazione, concentrandosi sui servizi esposti pubblicamente su Internet, come i server web, i gateway VPN e i server di posta. Al contrario, il test interno parte dal presupposto che l'attaccante abbia già ottenuto un accesso alla rete aziendale, permettendo di valutare i danni che potrebbero derivare da una minaccia interna o da un dispositivo compromesso.

> [!NOTE] Le regole di ingaggio
> Un passaggio critico e preliminare per qualsiasi test è la definizione del perimetro d'azione, ovvero lo "scoping", insieme alle Regole d'Ingaggio (RoE). Questo serve a stabilire con precisione quali sistemi (come firewall, DNS, server web) possono essere testati e quali devono essere rigorosamente evitati per non incorrere in interruzioni operative o conseguenze legali.

## Lifecycle

Operativamente, il ciclo di vita di un Penetration Test è strutturato in tre fasi distinte:

1. **Fase di Pre-Attack**: È lo stadio preparatorio in cui si firmano i documenti legali (come gli Accordi di Non Divulgazione o NDA), si definisce il perimetro di intervento e si avviano le prime attività di ricognizione, raccogliendo informazioni iniziali sul bersaglio in maniera attiva e passiva.
2. **Fase di Attack**: Rappresenta il nucleo dell'operazione, in cui si interagisce attivamente con i sistemi per scovare e sfruttare le debolezze. Questa fase è ulteriormente suddivisa in cinque passaggi sequenziali:
    - **Perimeter testing**: si analizzano i confini esterni della rete per individuare porte aperte e servizi accessibili dall'esterno.
    - **Enumerating devices**: si instaura una connessione attiva per estrarre informazioni sensibili, come nomi utente o servizi in esecuzione. In questo contesto si può evidenziare la vulnerabilità di protocolli basati su testo in chiaro come l'FTP, che risultano facilmente intercettabili rispetto a protocolli binari più complessi.
    - **Acquiring targets**: si cerca di ottenere un accesso iniziale al sistema sfruttando credenziali deboli o vulnerabilità note per "mettere un piede" nella porta.
    - **Escalating privileges**: una volta dentro, l'attaccante cerca di elevare i propri privilegi per passare da un utente base a un account amministratore, essenziale per ottenere il controllo totale del sistema e accedere a dati sensibili.
    - **Execute and implant**: l'ultimo passo dell'attacco prevede l'esecuzione di comandi, l'installazione di backdoor per mantenere l'accesso (persistenza) o il movimento laterale nella rete, utilizzando exploit di natura locale o remota.        
3. **Fase di Post-Attack**: Conclusa la simulazione, è fondamentale ripristinare il sistema allo stato originale ripulendolo da ogni file o strumento caricato durante i test. L'attività finale e più importante per il cliente consiste nell'analizzare i risultati e produrre un report dettagliato con le scoperte effettuate.

> [!NOTE] Differenza tra Exploit
> - **Exploit Remoti (Remote exploits):** Permettono agli attaccanti di ottenere l'accesso ai sistemi senza che vi sia stata alcuna interazione o accesso precedente. In questo scenario, l'attaccante si trova completamente all'esterno del sistema bersaglio (ad esempio su Internet) e scaglia l'attacco contro un servizio esposto (come un server web o un servizio di posta) per riuscire a penetrare.
> - **Exploit Locali (Local exploits):** Vengono invece utilizzati da utenti che possiedono già un accesso esistente al sistema. L'obiettivo di questi exploit non è entrare dall'esterno, ma **elevare i propri privilegi** (privilege escalation). L'attaccante, avendo già ottenuto un accesso base (magari tramite credenziali deboli rubate nella fase precedente), usa l'exploit locale per trasformare il suo account limitato in un account di amministratore (o utente "root"), ottenendo così il controllo totale.

## Sistema di comunicazione tra esperti

==Bisogna capire come ultimo passo come fanno gli esperti di tutto il mondo a comprendersi quando parlano di un difetto del software. Essi usano un vocabolario standard chiamato CVE, che assegna a ogni falla un "codice fiscale" univoco. Usano un database governativo chiamato NVD, che prende quel codice e ti dice tutto su quel difetto, compreso un punteggio (il CVSS) per indicare quanto è grave e urgente da sistemare.==