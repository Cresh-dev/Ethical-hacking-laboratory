_Tag:_ #cybersecurity  #hacking #va #laboratorio

---
==Nessus è uno scanner commerciale (nel lab si usa la versione _Essentials_) noto per la sua profondità e il vasto database di firme sempre aggiornato==.

## Fasi dell'attività:

- **Installazione:** Si scarica il pacchetto `.deb` per Debian/Kali e si avvia il servizio `nessusd`. L'interfaccia è raggiungibile su `https://localhost:8834`.
- **Requisiti:** Richiede risorse hardware significative (almeno 4 GB di RAM e 4 core CPU) per funzionare correttamente.
- **Analisi dei Risultati:** Il laboratorio mostra come Nessus classifichi le vulnerabilità per severità (Critical, High, Medium, Low, Info).
- **Esempio di Vulnerabilità:** Viene analizzato il plugin #20007 che rileva il supporto ai protocolli obsoleti **SSL 2.0 e 3.0**. Il report spiega che questi sono soggetti ad attacchi man-in-the-middle e downgrade (come POODLE) e raccomanda di disabilitarli a favore di TLS 1.2 o superiore.

![[Screenshot 2026-06-05 at 23.54.12.png]]