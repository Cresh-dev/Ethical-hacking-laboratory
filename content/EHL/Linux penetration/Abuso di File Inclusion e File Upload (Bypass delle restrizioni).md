_Tag:_ #cybersecurity  #hacking #laboratorio

---
Il livello di sicurezza di DVWA viene innalzato (impostato su _medium_), introducendo una convalida di base che blocca i file non riconosciuti come immagini. Per superare questo ostacolo, l'attaccante combina il File Upload con una vulnerabilità di Local File Inclusion (LFI). Poiché il server controlla solo l'estensione del file, l'attaccante crea uno script PHP malevolo (webshell) e uno script di supporto per rinominare i file, salvandoli entrambi con estensione `.jpg` in modo da ingannare il filtro di caricamento. Una volta caricati, tramite l'exploit LFI l'attaccante esegue prima il finto file `.jpg` che rinomina la webshell nella sua estensione originaria `.php`, e successivamente richiama quest'ultima per poter eseguire comandi di sistema arbitrari tramite parametri HTTP GET o POST (Remote Code Execution).

![[Pasted image 20260614143421.png]]