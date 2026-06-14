_Tag:_ #cybersecurity  #hacking #va #laboratorio

---
==OpenVAS è una suite open-source che utilizza un motore di scansione per rilevare vulnerabilità tramite i Network Vulnerability Tests (NVTs)==.

## Fasi della configurazione:

- **Installazione e Setup:** Si installa su Kali Linux tramite i comandi `gvm-setup` e `gvm-check-setup`. È un processo lungo perché deve scaricare database pesanti (SCAP, CERT, GVMD_DATA).
- **Accesso:** Una volta avviato il servizio con `gvm-start`, si accede a un'interfaccia web (Greenbone Security Assistant) all'indirizzo `https://127.0.0.1:9392/`.
- **Configurazione del Target:**
    1. Si registra l'IP della macchina vittima (192.168.56.3).
    2. Si crea una **Port List** personalizzata (TCP: 22, 80, 139, 143, 443, 445, 5001, 8080, 8081) basata su una scansione preliminare con Nmap.
- **Esecuzione:** Si crea un "Task" di scansione utilizzando la configurazione **Full and fast**. Al termine, lo strumento genera un report con i risultati.

