# Elaborazione karaoke {#karaoke-processing}

Durante l'elaborazione, l'applicazione principale invia al servizio Demucs l'audio oppure, quando opportuno, il video completo. Demucs separa la voce dalla base strumentale e restituisce entrambe le tracce. L'applicazione principale usa FFmpeg per unire la base strumentale al video originale e salva la traccia vocale separatamente. L'avanzamento di Demucs viene comunicato tramite SSE (eventi inviati dal server).

Usa questa pagina per configurare il motore di separazione, il formato di output e i limiti di elaborazione. Puoi impostare questi valori anche tramite le [variabili d'ambiente](environments.md) se l'installazione richiede una configurazione fissa.

![Impostazioni di Karaoke Processing](../assets/images/settings/karaoke-processing.webp){ width="400" }

## Servizio Demucs {#demucs-service}

### URL del servizio di separazione {#separation-service-url}

URL del servizio Demucs.

### Chiave API del servizio di separazione {#separation-service-api-key}

Una chiave API opzionale per il servizio Demucs.

??? warning "Chiave API fortemente consigliata per i servizi Demucs esposti pubblicamente"

    Chi si trova dietro CG-NAT può usare Cloudflare Tunnel o un servizio simile per rendere Demucs raggiungibile dall'applicazione principale, anche quando il servizio è ospitato da un amico o un familiare. Se però Demucs è esposto su Internet, chiunque potrebbe usarlo: per questo è fortemente consigliato configurare una chiave API. Per i dettagli, consulta la sezione [Variabili d'ambiente del servizio Demucs](environments.md#demucs-service).

## Opzioni di separazione {#separation-options}

### Backend di separazione {#separation-backend}

Scegliere `Demucs` o `Sherpa+Spleeter`.

### Modello Demucs {#demucs-model}

Modello usato per la separazione. Il valore predefinito è `htdemucs`, che offre un compromesso tra qualità e velocità. Sono disponibili anche:

- `htdemucs`: prima versione di Hybrid Transformer Demucs, addestrata su MusDB e 800 brani.
- `htdemucs_ft`: versione ottimizzata di `htdemucs`. La separazione richiede circa quattro volte più tempo, ma la qualità può essere leggermente migliore. Usa lo stesso insieme di dati di addestramento.
- `htdemucs_6s`: versione a sei sorgenti di `htdemucs`, che include anche pianoforte e chitarra.
- `hdemucs_mmi`: Hybrid Demucs v3, riaddestrato su MusDB e 800 brani.
- `mdx`: modello addestrato solo su MusDB HQ e vincitore della categoria A della sfida MDX.
- `mdx_extra`: modello addestrato con dati aggiuntivi, compreso il set di test di MusDB; si è classificato secondo nella categoria B della sfida MDX.
- `mdx_q`, `mdx_extra_q`: versioni quantizzate dei modelli precedenti.
- `SIG`: modello singolo della raccolta SIG.

### Modello Sherpa+Spleeter {#sherpaspleeter-model}

Il valore predefinito è `fp16`. Puoi scegliere tra `int8`, `fp16` e `fp32`. Il modello `int8` è il più veloce e compatto, ma può offrire una qualità inferiore rispetto a `fp16` e `fp32`.

### Dispositivo {#device}

Scegli `cuda` o `cpu` in base alle funzionalità del servizio Demucs.

- Se scegli `cuda` ma il servizio Demucs non lo supporta, oppure usi il backend Sherpa+Spleeter che funziona solo su CPU, la separazione passa alla CPU.

## Opzioni di output {#output-options}

### Formato di output delle tracce separate {#stem-output-format}

Scegli `mp3` o `wav`. `mp3` è consigliato per ridurre i tempi di trasferimento e lo spazio occupato.

### Bitrate MP3 delle tracce separate {#mp3-stem-bitrate}

Bitrate delle tracce separate in MP3. Il valore predefinito è `320`. Se la connessione al servizio Demucs è lenta, puoi scegliere un valore inferiore, per esempio `128–160`.

### Codec audio FFmpeg {#ffmpeg-audio-codec}

Codec audio usato da FFmpeg per unire la base strumentale al video originale. Se il campo è vuoto, l'audio viene copiato senza ricodifica. Imposta un codec solo se i dispositivi usati per lo stage hanno problemi a riprodurre il video risultante. Per esempio, i dispositivi iOS supportano `aac`.

## Limiti di elaborazione {#processing-limits}

### Soglia per l'invio diretto dei file multimediali (MB) {#separation-direct-media-cutoff-mb}

Se il file multimediale è più piccolo di questo valore, l'applicazione principale invia il video direttamente a Demucs senza scaricarlo né estrarne prima l'audio. Il valore predefinito è `500`. Se la connessione al servizio Demucs è lenta, puoi scegliere un valore inferiore, per esempio `20–50`.

### Intervallo di polling alternativo (secondi) {#separation-fallback-poll-interval-seconds}

Intervallo con cui l'app controlla gli aggiornamenti di avanzamento del servizio Demucs se la connessione SSE si interrompe. Il valore predefinito è `1.0` secondo.
