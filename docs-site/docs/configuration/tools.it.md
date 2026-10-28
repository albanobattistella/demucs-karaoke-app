# Strumenti {#tools}

![Impostazioni strumenti](../assets/images/settings/tools.webp){ width="400" }

Usa questa sezione per controllare la rete, lo spazio di archiviazione e la connessione a Demucs. Prima di verificare Demucs, l'app salva le impostazioni correnti. Queste funzioni sono disponibili solo agli amministratori.

### Informazioni sul proxy {#proxy-info}

Seleziona **Verifica proxy** per controllare la connessione in uscita tramite `ipinfo.io/json`. Il risultato mostra l'indirizzo IP rilevato, la posizione e l'organizzazione, e può aiutarti a verificare se viene usato il proxy configurato.

### Utilizzo dello spazio di archiviazione {#storage-usage}

Seleziona **Verifica archiviazione** per stimare lo spazio occupato dai file multimediali, dalla cache e dal database SQLite. Il risultato mostra anche il totale.

### Pulire la cache e il database {#clean-cache-and-database}

Seleziona **Pulisci cache e DB** per rimuovere i file temporanei della cache e i record obsoleti del database. Questa operazione non elimina i file multimediali dalla cartella configurata. Controlla comunque il risultato prima di usare un elemento che potrebbe essere stato segnalato come mancante.

### Verificare Demucs {#check-demucs}

Utilizzare questa opzione per verificare la connettività al servizio Demucs dopo aver aggiunto o modificato l'URL o la chiave API di Demucs.

La verifica di Demucs salva prima le impostazioni correnti, compresi l'URL e la chiave API, quindi controlla la connessione al servizio. Se il salvataggio non riesce, la verifica non viene avviata.

### Eseguire la pulizia della memoria di Demucs {#run-demucs-gc}

Avvia manualmente la pulizia della memoria sul servizio Demucs. L'operazione scarica dalla memoria i modelli Demucs e WhisperX per liberare la VRAM.
