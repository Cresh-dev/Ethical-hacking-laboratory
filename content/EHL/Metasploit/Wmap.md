_Tag:_ #cybersecurity  #hacking #scanning #laboratorio

---
==Wmap, un potente modulo== di [[Exploitation con Metasploit|Metasploit]] derivato dallo strumento SQLMap, ==concepito per automatizzare la scansione di vulnerabilità web==. A differenza delle precedenti esercitazioni mirate a un singolo servizio, Wmap analizza l'intera applicazione web alla ricerca di falle comuni. In questo scenario, il target è **WackoPicko**, un'applicazione volutamente vulnerabile inclusa nella suite _OWASP Broken Web Applications_, utilizzata per scopi didattici.

## Inizializzazione e Caricamento

L'attività inizia con l'integrazione del modulo all'interno dell'ambiente Metasploit.

- **Comando**: `load wmap`.
- **Caratteristica**: Wmap non è un singolo modulo, ma un'estensione che richiama automaticamente diversi moduli ausiliari di scansione per identificare vulnerabilità come SQL Injection, Directory Traversal o file esposti.

## Configurazione del Target

Prima di scansionare, è necessario registrare il sito nel database di Metasploit.

- **Creazione del sito**: Si utilizza `wmap_sites -a` seguito dall'URL (es. `http://192.168.56.3/WackoPicko/`) per aggiungere il target alla lista.
- **Selezione dell'ID**: Ogni sito aggiunto riceve un identificativo numerico (ID). Tramite `wmap_sites -l` si visualizzano i siti e con `wmap_targets -d 0` si definisce quale ID debba essere l'obiettivo della scansione attuale.

## Esecuzione della Scansione

La fase di analisi viene avviata con il comando `wmap_run -e`.

- **Processo**: Wmap inizia a testare il sito utilizzando vari moduli per mappare la struttura (crawler) e testare i vettori di attacco.
- **Tempistiche**: Come indicato nel documento, questa operazione può richiedere dai 5 ai 20 minuti, a seconda delle risorse della macchina virtuale, poiché esegue numerose richieste HTTP per ogni potenziale vulnerabilità.

## Analisi dei Risultati

A differenza di strumenti con interfaccia grafica (come [[VA con Greenbone OpenVAS (GVM)|Greenbone]]), Wmap restituisce risultati "raw" (grezzi) direttamente nella console.

- **Visualizzazione**: Il comando `wmap_vulns -l` elenca le falle identificate.
- **Esempi di scoperte**: Nell'esercitazione vengono rilevati elementi critici come:
    - **Directory trovate**: Cartelle esposte (es. `/doc/` o `/007/`).
    - **File sensibili**: Identificazione di file di versionamento come `/.svn/entries`, che possono rivelare informazioni sulla struttura del codice sorgente.