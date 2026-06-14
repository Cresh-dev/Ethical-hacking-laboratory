_Tag:_ #cybersecurity  #hacking

---
==Gli attacchi lato client (Client-side attacks) rappresentano un cambio di paradigma rispetto alle tradizionali vulnerabilità lato server. Invece di mirare alla compromissione dei sistemi di backend o all'estrazione di dati archiviati, l'attenzione si sposta sull'ambiente dell'utente, rendendo il contesto di esecuzione (come browser e applicazioni) la superficie di attacco primaria==. In questo scenario, il server funge prevalentemente da mero meccanismo di consegna per i contenuti o le interazioni malevole.

### L'Ingegneria Sociale (Social Engineering)

==L'ingegneria sociale è una componente fondamentale degli attacchi lato client, la quale sfrutta il fattore umano anziché le vulnerabilità tecniche del software==. L'attaccante manipola la percezione e la fiducia della vittima impersonando entità legittime per bypassare i controlli di sicurezza. Esistono diverse metodologie per condurre questo tipo di attacco:

- **Phishing e varianti**: ==Il Phishing è la tecnica più comune e consiste nell'invio di messaggi fraudolenti per rubare credenziali o dati sensibili==. Può essere mirato a un singolo individuo o organizzazione (Spear Phishing), rivolto a bersagli di alto profilo gestionale (Whaling), oppure veicolato tramite chiamate vocali (Vishing) o SMS (Smishing).
- **Manipolazione fisica e situazionale**: Include il ==Pretexting, ovvero la creazione di scenari fittizi per ottenere fiducia== (es. finto supporto IT), e il ==Baiting, che attira la vittima con incentivi malevoli come software gratuiti o chiavette USB infette==.
- **Intrusione e scambio**: Comprendono il ==Tailgating, che consiste nell'accedere ad aree fisiche riservate seguendo una persona autorizzata==, e il ==Quid Pro Quo, dove l'attaccante offre un beneficio o un servizio in cambio di credenziali di accesso==.

==Per massimizzare le probabilità di successo, l'attaccante deve condurre una ricerca meticolosa sul bersaglio (utilizzando tool come Maltego per analizzare social network e forum) al fine di costruire un pretesto credibile==. L'efficacia della truffa aumenta adottando la terminologia specifica dell'ambiente della vittima, replicando accuratamente loghi aziendali, registrando domini contraffatti molto simili agli originali e sfruttando argomenti controversi o di interesse specifico per l'utente.

### Simulazioni di Attacco: Credential Harvesting e Phishing Avanzato

Per simulare le minacce legate al fattore umano, i professionisti della sicurezza utilizzano il **Social-Engineer Toolkit (SET)**, un framework open-source sviluppato da TrustedSec. Questo strumento permette di creare scenari realistici implementando vettori di attacco come lo Spear Phishing e la clonazione di siti web.