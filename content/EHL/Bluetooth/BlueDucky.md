_Tag:_ #cybersecurity  #hacking #bluetooth #laboratorio

---
==In questo scenario il problema risiede nella gestione degli host HID all'interno di BlueZ (e nelle implementazioni Android non patchate, in particolare dalle versioni 11 alla 14). La falla permette a una periferica Bluetooth non autenticata (l'attaccante) di forzare la creazione di una connessione cifrata fingendosi un dispositivo di input (come una tastiera HID)==. Poiché i sistemi operativi tendono a fidarsi implicitamente delle tastiere, l'attaccante può **iniettare sequenze di tasti arbitrarie** senza che l'utente debba confermare l'accoppiamento (comportamento assimilabile a un attacco _zero-click_). L'attacco richiede la prossimità fisica (~100 metri), estendibile con antenne speciali. Ha un punteggio CVSS di **6.2 (Medium)**.

## Il Framework BlueDucky

==BlueDucky è un tool di exploitation open-source scritto in Python che automatizza l'attacco legato alla CVE-2023-45866. Il suo funzionamento emula una tastiera "rogue" che invia payload di digitazione una volta stabilito il canale==. Il flusso operativo di laboratorio prevede:

1. Il setup delle dipendenze di sistema (librerie di sviluppo Bluetooth, pacchetti BlueZ e moduli Python come `pybluez` e `pydbus`).
2. La compilazione dello strumento di basso livello `bdaddr` direttamente dai sorgenti ufficiali di BlueZ per manipolare l'indirizzo dell'adattatore.
3. L'esecuzione del framework (`sudo python3 BlueDucky.py`), che effettua una scansione dell'ambiente per elencare i dispositivi vulnerabili (come mostrato nei log del laboratorio su smartphone Android come il _Galaxy Note4_).
4. La selezione e l'invio di un file di script (payload della tastiera). Nel caso pratico documentato, viene caricato `browser_payload.txt`. Il tool forza l'abilitazione di parametri come il Simple Secure Pairing (`btmgmt ssp on`), modifica la classe del dispositivo locale e simula la pressione di combinazioni di tasti (`GUI d`, `CTRL L`) per aprire automaticamente il browser della vittima e navigare su un URL specifico (es. `www.poliba.it`).