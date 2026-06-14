_Tag:_ #cybersecurity  #hacking #network

---
Il **Network Sniffing** è una ==tecnica di attacco passiva o attiva che consiste nell'intercettazione, cattura e analisi dei pacchetti dati trasmessi su una rete cablata (Ethernet) o wireless (Wi-Fi)==.

- **Finalità:** Raccolta di informazioni sensibili per scopi di ricognizione o per facilitare attacchi successivi.
- **Target:** Protocolli insicuri o non crittografati che espongono credenziali e dati di configurazione, come **HTTP, SMTP, POP3, IMAP, FTP e TELNET** .
- **Ambiente:** L'attacco è più agevole in reti locali (LAN) o Wi-Fi rispetto a reti vaste come Internet.

Per eseguire lo sniffing, le interfacce di rete devono operare in modalità specifiche per superare i filtri hardware standard:

1. **Non-Promiscuous (Default):** L'interfaccia elabora solo pacchetti destinati al proprio MAC address, broadcast o multicast.
2. **Promiscuous Mode:** L'interfaccia accetta tutti i pacchetti circolanti sul segmento di rete, indipendentemente dalla destinazione.
3. **Monitor Mode (Wireless):** Specifica per le reti Wi-Fi (IEEE 802.11), permette di catturare non solo i dati ma anche i frame di gestione (handshake, probe request) senza essere associati a un Access Point.

## Sniffing Passivo

L'attaccante si limita all'ascolto senza interagire con la rete. È tipico delle reti basate su **Hub** (dove il traffico è inviato a tutte le porte) o delle reti **Wireless** (mezzo trasmissivo condiviso).

## Sniffing Attivo

Necessario in reti basate su **Switch**, dove il traffico è veicolato in modo mirato verso la porta di destinazione. Le tecniche principali includono:

- **MAC Flooding:** Inondazione dello switch con migliaia di MAC address falsi per saturare la tabella CAM. Una volta piena, lo switch può degradare a modalità "hub", inoltrando il traffico a tutte le porte. Strumenti come Macof (da dsniff) possono generare migliaia di falsi MAC voci al minuto.
- **DHCP Attacks:** Include il _DHCP Starvation_ (esaurimento degli IP disponibili) e il _Rogue DHCP Server_ (per fornire gateway e DNS falsi).
- **Spoofing & Poisoning**: Manipolazione di ARP e DNS per deviare il flusso dei dati.

## Ecosistema di Tool e Protocolli Vulnerabili

- **Wireshark / Tcpdump**: Standard per l'analisi dei protocolli e la decrittazione (se le chiavi sono note).
- **BetterCAP / Ettercap**: Suite avanzate per l'esecuzione di attacchi MITM, manipolazione traffico HTTP/HTTPS e sniffing di credenziali.
- **Dsniff**: Specializzato nell'estrazione di password da protocolli legacy.

## Protocolli ad Alto Rischio

I protocolli privi di cifratura nativa sono i target primari:

- **Web/Email**: HTTP (senza SSL), SMTP, POP3, IMAP.
- **Trasferimento File e Accesso Remoto**: FTP e TELNET.