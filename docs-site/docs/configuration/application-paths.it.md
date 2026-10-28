# Percorsi dell'applicazione {#application-paths}

![Impostazioni dei percorsi di applicazione](../assets/images/settings/application-paths.webp){ width="400" }

Questa sezione consente di scegliere dove salvare i file multimediali e temporanei e quali programmi esterni usare. Puoi configurare queste impostazioni anche tramite le [variabili d'ambiente](environments.md), se l'installazione richiede percorsi fissi.

### Percorso dei file multimediali {#media-path}

La cartella in cui vengono salvati i file multimediali scaricati, caricati ed elaborati. L'applicazione la crea quando necessario. L'utente che esegue il servizio deve poterla leggere e scrivere.

### Percorso della cache {#cache-path}

La cartella dei download temporanei, dei risultati dell'elaborazione, delle miniature e degli altri file di cache. Quando non servono più, questi file possono essere eliminati dalla sezione [Strumenti](tools.md).

### Percorso di yt-dlp {#yt-dlp-path}

Il percorso o il nome dell'eseguibile di yt-dlp. L'applicazione cerca prima nell'ambiente virtuale attivo e poi nel `PATH` del sistema.

### Percorso di Deno {#deno-path}

Nelle installazioni Docker il percorso di Deno è già configurato. Si consiglia di installare Deno per evitare problemi di download con yt-dlp.

Percorso facoltativo dell'eseguibile Deno, usato da yt-dlp per eseguire JavaScript esterno. Lascialo vuoto per mantenere il comportamento predefinito di yt-dlp. Impostalo se la sorgente video richiede un runtime JavaScript esterno.

### Percorso di FFmpeg {#ffmpeg-path}

Il percorso o il nome dell'eseguibile FFmpeg, necessario per l'estrazione audio, la conversione dei file multimediali e altre elaborazioni.
