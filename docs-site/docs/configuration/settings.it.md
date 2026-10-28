# Pagina delle impostazioni {#settings-page}

!!! note "La pagina delle impostazioni è riservata agli amministratori"

    La pagina Impostazioni è disponibile solo dopo l'accesso come amministratore. Crea un account amministratore durante la configurazione iniziale.

![Pagina delle impostazioni](../assets/images/settings.webp){ width="800" }

Usa le [impostazioni consigliate](#recommended-settings) come punto di partenza e consulta le guide delle singole sezioni per maggiori dettagli.

<div class="grid cards" markdown>
-   __[Elaborazione karaoke](karaoke-processing.md)__

    ---

    Configura il motore di separazione, il formato di output e i limiti di elaborazione.

-   __[Testi con WhisperX](whisperx-lyrics.md)__

    ---

    Configura la trascrizione, l'allineamento, la sincronizzazione dei testi e il precaricamento dei modelli.

-   __[Percorsi dell'applicazione](application-paths.md)__

    ---

    Scegli i percorsi dei file multimediali, della cache e degli eseguibili.

-   __[Download](downloads.md)__

    ---

    Configura i download di yt-dlp, il proxy, la ricerca parallela e i provider di testi.

-   __[Palco](stage.md)__

    ---

    Configura i collegamenti dello stage, il contenuto mostrato quando la coda è vuota e il volume predefinito della traccia vocale.

-   __[Strumenti](tools.md)__

    ---

    Controlla la connessione e lo spazio di archiviazione, aggiorna yt-dlp o libera memoria sul servizio Demucs remoto.
</div>

## Impostazioni consigliate {#recommended-settings}

Questi valori sono un buon punto di partenza per un'esperienza karaoke fluida. Puoi adattarli all'hardware, alla rete e al flusso di lavoro che preferisci.

### Elaborazione karaoke {#karaoke-processing}

- **Motore di separazione:** `demucs`. Se il backend Demucs non ha accesso a una GPU, usa `Sherpa+Spleeter`.
- **Soglia per l’invio diretto dei file multimediali (MB):** `500`. Impostare un valore inferiore, ad esempio `20–50`, se la connessione di rete a Demucs è lenta.
- **Bitrate MP3 delle tracce separate**: `320`. Impostare questo valore più basso, ad esempio `128-160`, se la connessione di rete a Demucs è lenta.

### Testi con WhisperX {#whisperx-lyrics}

- **Rilevamento della lingua prima dell'allineamento:** attivato.
- **Usa i timestamp dei testi sincronizzati:** disattivato.

### Download {#downloads}

- **Ricerca parallela su YouTube:** abilitata.

### Palco {#stage}

- **URL del codice QR dello stage:** imposta l'indirizzo della pagina della coda.
- **URL del contenuto mostrato quando la coda è vuota:** imposta il percorso del file multimediale o della cache da usare.

Queste impostazioni sono adatte a molte configurazioni. Puoi modificarle in qualsiasi momento dalla pagina Impostazioni.
