_Tag:_ #cybersecurity  #hacking #linux

---
Linux non è un sistema operativo completo, ma un **kernel** (il cuore del sistema) che gestisce hardware, memoria e processi. Attorno ad esso nascono le **distribuzioni** (come Ubuntu, Kali Linux o Debian), ognuna con scopi diversi.

- **Importanza per l'hacking:** È essenziale per la sua flessibilità, stabilità e compatibilità con i principali tool di sicurezza.
- **CLI (Command Line Interface):** L'interfaccia a riga di comando è lo strumento principale. Molti sistemi (server, container) non hanno un'interfaccia grafica e alcune funzioni avanzate sono accessibili solo via terminale.

## Struttura del File System

==Linux segue la filosofia "tutto è un file": non solo i documenti, ma anche l'hardware e i processi sono rappresentati come file.==

- **La radice (/):** È il punto di partenza di tutto il sistema.
- **Directory principali:**
    - `/bin`: Programmi comuni di sistema.
    - `/etc`: File di configurazione (simile al Pannello di Controllo di Windows).
    - `/home`: Cartelle personali degli utenti.
    - `/root`: Cartella personale dell'amministratore di sistema.
    - `/var/log`: Dove vengono salvati i registri (log) del sistema.

## Permessi dei File

La sicurezza di Linux si basa su una gerarchia di permessi applicata a tre livelli:

1. **Owner (u):** Il proprietario del file.
2. **Group (g):** Il gruppo assegnato al file.
3. **Other (o):** Tutti gli altri utenti.

I permessi sono di tre tipi: **Read (r)**, **Write (w)** ed **Execute (x)**. Possono essere espressi in modo simbolico (es. `chmod u+x`) o numerico (es. `755`, dove 7 è la somma di r=4, w=2, x=1).

## Gestione di Utenti e Password

- **Utenti:** ==Le informazioni sugli utenti sono in /etc/passwd. Gli utenti con UID 0 hanno privilegi di root (amministratore).==
- **Password:** ==Nei sistemi moderni, le password cifrate (hash) sono conservate in /etc/shadow, leggibile solo da root per motivi di sicurezza==.

![[Screenshot 2026-05-14 at 16.01.35.png]]

## Strumenti e Automazione

- **Comandi comuni:** `ls` (elenca file), `cd` (cambia directory), `pwd` (mostra posizione attuale), `cat` (legge file), `chmod` (cambia permessi).
- **Pipes (|):** Permettono di collegare i comandi tra loro, usando l'output di uno come input per l'altro (es: `ls | grep ".txt"`).
- **Cron Job:** Un'utilità per pianificare l'esecuzione automatica di comandi a orari o intervalli specifici.
- **Monitoraggio:** Comandi come `top` o `htop` permettono di vedere in tempo reale quali processi consumano più risorse (CPU, RAM).

### Servizi e Log

Il sistema gestisce i servizi tramite **systemd** e il comando **`systemctl`** (per avviare o fermare programmi come server web o database). I log, fondamentali per tracciare attività sospette o errori, possono essere consultati con **`journalctl`**.