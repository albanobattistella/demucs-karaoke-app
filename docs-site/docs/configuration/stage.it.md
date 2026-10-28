# Palco {#stage}

![Impostazioni del palco](../assets/images/settings/stage.webp){ width="400" }

Usa questa sezione per configurare l'URL associato al codice QR dello stage, il contenuto mostrato quando la coda è vuota e il volume predefinito della traccia vocale. Puoi impostare questi valori anche tramite le [variabili d'ambiente](environments.md) se l'installazione richiede una configurazione fissa.

### URL del codice QR del palco {#stage-qr-url}

URL codificato nel codice QR mostrato sullo stage. Se il campo è vuoto, l'applicazione usa il nome host corrente per creare l'indirizzo.

### URL del file multimediale per la schermata di attesa {#stage-lobby-media-url}

URL facoltativo del contenuto riprodotto in loop quando la coda è vuota. Usa un URL `/media/...` relativo alla cartella dei file multimediali, per esempio `/media/stage-lobby.mp4`.

### Volume predefinito della traccia vocale {#default-vocals-volume}

Volume della traccia vocale applicato quando la pagina Stage o Queue viene caricata dopo un riavvio. Inserisci un valore percentuale da `0` a `100`. Durante la riproduzione puoi comunque regolare il volume dalla pagina Stage.

La variabile d'ambiente equivalente `STAGE_VOCALS_VOLUME_DEFAULT` utilizza un valore decimale da `0.0` a `1.0`; per esempio, `0.46` rappresenta `46%`.
