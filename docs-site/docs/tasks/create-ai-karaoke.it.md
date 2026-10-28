# Creare karaoke con l'IA {#create-ai-karaoke}

L'applicazione offre diversi modi per creare karaoke: puoi scaricare un video musicale oppure elaborare i tuoi file. Scegli la procedura più adatta ai contenuti che hai a disposizione e ai tempi di elaborazione che preferisci.

## Indice {#contents}

- [Creare karaoke da un video musicale](#create-karaoke-from-a-music-video)
- [Creare karaoke da un video con testo](#create-karaoke-from-a-lyrics-video)
- [Creare karaoke dai file caricati](#create-karaoke-from-uploaded-files)
- [Modificare un video esistente](#modify-existing-video)
- [Aggiungere una traccia vocale a un video karaoke già pronto](#add-vocals-to-a-premade-karaoke-video)
- [Karaoke più rapido o in condizioni ideali](#fastest-or-best-case-karaoke)

## Creare karaoke da un video musicale {#create-karaoke-from-a-music-video}

Creare un karaoke da un video musicale offre l'esperienza più coinvolgente e permette di personalizzare lo stile dei testi. È anche l'opzione che richiede più elaborazione. Per ridurre i tempi, usa il servizio Demucs su un dispositivo dotato di GPU.

![AI Karaoke flusso completo](../assets/images/tasks/ai-karaoke-fullflow.webp){ width="800" }

### 1. Selezionare un video musicale {#1-select-a-music-video}

1. Cerca un brano e scegli un video musicale tra i risultati.
2. L'applicazione non rileva automaticamente se il video contiene già i testi o una versione karaoke. Per questo attiva sia la separazione vocale sia l'elaborazione dei testi.

### 2. Configurare le opzioni karaoke {#2-configure-karaoke-options}

La schermata di configurazione prima dell'invio offre queste opzioni:

- **Allineamento parole con WhisperX** crea testi sincronizzati parola per parola.
- **Ricomponi righe del testo** imposta il numero massimo di caratteri per riga prima di andare a capo.
    - Per le lingue dell'Asia orientale, in particolare cinese, giapponese e coreano (CJK), modifica invece `Max CJK Chars`.
- **Lingua specifica per il brano** indica la lingua da usare per l'[allineamento con WhisperX](../configuration/whisperx-lyrics.md#whisperx-alignment-language). Se la imposti, il rilevamento automatico e la lingua predefinita configurata vengono ignorati.

### 3. Aggiungere il testo {#3-add-lyrics}

WhisperX ha bisogno dei testi per sincronizzarli con la musica. L'applicazione li cerca usando i provider configurati.

- L'app usa Last.fm per ricavare il titolo e l'artista dal titolo del video. Se i dati non sono corretti, inseriscili manualmente prima di cercare i testi.
- Il pulsante **Google** apre una nuova scheda e cerca `Artist - Title lyrics`. Copia i testi trovati nell'**editor dei testi**.
- Puoi anche caricare un file di testo `.lrc` o `.txt`.
- Se necessario, correggi i testi prima di aggiungere il brano alla coda.

??? tip "Convertire i testi LRC in TTML"

    Per alcuni brani è possibile ottenere una versione TTML (Timed Text Markup Language), spesso da Apple Music. Il TTML contiene già la sincronizzazione parola per parola e può fornire tempi precisi senza dover usare WhisperX.

    Se il recupero non riesce o restituisce testi errati, usa il risultato standard.

### 4. Elaborare il brano {#4-process-the-song}

Dopo aver aggiunto il brano alla coda, l'applicazione scarica il video, separa la voce, sincronizza i testi e prepara il contenuto karaoke per lo stage.

!!! note "Se l'elaborazione non riesce"

    Se il download non riesce, consulta la [guida ai problemi di yt-dlp](../troubleshooting/index.md#yt-dlp-fails-to-download). Se la separazione della voce è molto lenta, consulta [questa guida](../troubleshooting/index.md#vocal-separation-is-very-slow). Per problemi di allineamento o sincronizzazione dei testi, consulta le guide sui [problemi di allineamento di WhisperX](../troubleshooting/index.md#whisperx-alignment-fails-or-takes-too-long) e sui [testi WhisperX non sincronizzati](../troubleshooting/index.md#whisperx-lyrics-are-poorly-synchronized).

## Creare karaoke da un video con testo {#create-karaoke-from-a-lyrics-video}

![Esempio di testo video](../assets/images/tasks/lyricsvideo.webp)

Su YouTube sono disponibili molti video con i testi delle canzoni. In genere i testi non sono sincronizzati parola per parola, ma questi video possono offrire uno stile visivo e uno sfondo che alcuni utenti preferiscono. Inoltre non richiedono l'allineamento con WhisperX, riducendo i tempi di elaborazione sui dispositivi senza GPU.

### 1. Selezionare un video con testo {#1-select-a-lyrics-video}

1. Cerca un brano e scegli un video con il testo tra i risultati.
2. L'applicazione riconosce questo tipo di video e avvia solo la separazione vocale, senza elaborare i testi.

### 2. Attendere la separazione delle tracce vocali {#2-wait-for-vocal-separation}

Il brano deve essere elaborato per separare la voce. Al termine, compare sullo stage.

??? note "I video con testo non sono sempre sincronizzati correttamente"

    Questi video sincronizzano di solito i testi per riga, non parola per parola. Spesso privilegiano gli effetti visivi e le animazioni, perciò il cambio di riga potrebbe non coincidere esattamente con la musica. La differenza si nota meno se conosci bene il brano.

!!! note "Se l'elaborazione non riesce"

    Se il download non riesce, consulta la [guida ai problemi di yt-dlp](../troubleshooting/index.md#yt-dlp-fails-to-download). Se la separazione della voce non riesce o è molto lenta, consulta [questa guida](../troubleshooting/index.md#vocal-separation-is-very-slow).

## Creare karaoke dai file caricati {#create-karaoke-from-uploaded-files}

Creare un karaoke da un file MP3 caricato con copertina può offrire un'esperienza coinvolgente e personalizzabile. Il caricamento è utile anche se l'applicazione non riesce a scaricare un video, se un altro dispositivo o una rete sono più adatti al download o se vuoi usare un file della tua libreria.

![Carica pilota automatico](../assets/images/media/autopilot.gif)

### 1. Caricare un file {#1-upload-a-file}

Fai clic sull'area di caricamento oppure trascina un file al suo interno.

??? tip "Caricamento con Autopilot"

    Autopilot configura le opzioni karaoke predefinite con un solo clic:

    - Ricava i dati del brano dal nome del file.
    - Cerca e scarica i testi.
    - Inserisce il titolo, l'artista e le opzioni di elaborazione karaoke.
    - Imposta le opzioni di elaborazione consigliate prima dell'invio.

### 2. Configurare il brano caricato {#2-configure-the-uploaded-song}

Per le opzioni relative ai testi, consulta [Creare karaoke da un video musicale](#create-karaoke-from-a-music-video). Le stesse opzioni sono disponibili anche durante il caricamento.

### 3. Scegliere dove aggiungere il brano {#3-choose-where-to-add-the-song}

Attiva **Aggiungi alla coda** per aggiungere il brano alla coda e riprodurlo sullo stage al termine dell'elaborazione. Lascialo disattivato per salvare il brano elaborato nella libreria e usarlo in seguito.

!!! note "Se l'elaborazione non riesce"

    Se la separazione della voce non riesce o è molto lenta, consulta [questa guida](../troubleshooting/index.md#vocal-separation-is-very-slow). Se l'allineamento non riesce, richiede troppo tempo o produce una sincronizzazione imprecisa, consulta le guide sui [problemi di allineamento di WhisperX](../troubleshooting/index.md#whisperx-alignment-fails-or-takes-too-long) e sui [testi WhisperX non sincronizzati](../troubleshooting/index.md#whisperx-lyrics-are-poorly-synchronized).

## Modificare un video esistente {#modify-existing-video}

L'applicazione può separare la voce e sincronizzare i testi dei file già presenti nella libreria.

![Creare karaoke con l’IA dai file esistenti](../assets/images/media/create.gif)

### 1. Aprire l'editor multimediale {#1-open-the-media-editor}

1. Apri la pagina Media e fai clic su **Modifica** accanto al file.
2. Nella pagina **Modifica dettagli multimediali**, modifica il titolo, l'artista o le opzioni di elaborazione karaoke.
3. Usa **Automatico** per ricavare titolo e artista dal nome del file tramite Last.fm.
    - Attiva **Rinomina sul disco** se vuoi modificare anche il nome del file.

??? note "Rinominare sul disco"

    Il file viene rinominato solo dopo aver fatto clic su **Rinomina**. Se vuoi modificare solo il titolo, l'artista o il nome del file senza elaborare i contenuti, non modificare **Karaoke IA** né **Sincronizzazione del testo**.

### 2. Configurare il testo e l'elaborazione {#2-configure-lyrics-and-processing}

Per le opzioni relative ai testi, consulta [Creare karaoke da un video musicale](#create-karaoke-from-a-music-video).

??? note "Sincronizzazione del testo e WhisperX"

    Se **Sincronizzazione del testo** è attiva e hai fornito i testi, l'applicazione salva il file senza eseguire WhisperX. Per creare testi sincronizzati, modifica di nuovo il file e attiva **Allineamento WhisperX**.

!!! note "Se l'elaborazione non riesce"

    Se la separazione della voce non riesce o è molto lenta, consulta [questa guida](../troubleshooting/index.md#vocal-separation-is-very-slow). Se l'allineamento non riesce, richiede troppo tempo o produce una sincronizzazione imprecisa, consulta le guide sui [problemi di allineamento di WhisperX](../troubleshooting/index.md#whisperx-alignment-fails-or-takes-too-long) e sui [testi WhisperX non sincronizzati](../troubleshooting/index.md#whisperx-lyrics-are-poorly-synchronized).

## Aggiungere una traccia vocale a un video karaoke già pronto {#add-vocals-to-a-premade-karaoke-video}

Se preferisci lo stile dei testi di un video karaoke già pronto, per esempio di Sing King, ma vuoi una traccia vocale per esercitarti, puoi aggiungerla al video tramite l'applicazione.

??? tip "L'allineamento automatico della traccia vocale richiede vocal-sync"

    Se sono soddisfatti i requisiti, l'applicazione può sincronizzare automaticamente la base strumentale del video karaoke con quella estratta dal video musicale originale. Installa l'extra [vocal-sync](../getting-started/linux.md#1-prepare-the-application-and-dependencies) oppure usa l'[immagine Docker vocal-sync](../getting-started/docker.md#3-configure-the-environment) per attivare la sincronizzazione automatica.

### 1. Aprire Vocal Sync {#1-open-vocal-sync}

1. Apri la pagina Media e fai clic su **Modifica**.
2. Seleziona **Aggiungi traccia vocale** per aprire la pagina di sincronizzazione vocale.

![Aggiungi voce](../assets/images/addvocals.webp)

### 2. Preparare e allineare la traccia vocale {#2-prepare-and-align-the-vocals}

1. Cerca il brano su YouTube oppure carica i tuoi file, quindi fai clic su **Prepara**.
2. L'applicazione separa la voce dalla base strumentale e prepara un'anteprima.
    - Se `vocal-sync` è disponibile, lo scarto temporale viene calcolato automaticamente.
3. Usa i pulsanti **+** e **-** per regolare lo scarto, quindi fai clic su **Anteprima** per verificare il risultato.
    - Un valore **+** ritarda la voce. Aumentalo se la voce parte prima della base strumentale.
    - Un valore **-** anticipa la voce. Riducilo se la voce parte dopo la base strumentale.

??? note "Utilizzare Anteprima per la riproduzione"

    Non usare i controlli del lettore multimediale: riproducono solo il video originale. Per questa verifica, usa **Anteprima** e **Interrompi**.

### 3. Salvare il risultato {#3-commit-the-result}

Quando la sincronizzazione ti soddisfa, fai clic su **Salva definitivamente**.

## Karaoke più rapido o in condizioni ideali {#fastest-or-best-case-karaoke}

Anche senza una GPU CUDA per il servizio Demucs puoi usare le funzioni di IA con la CPU.

### Elaborazione con la CPU {#cpu-processing}

Usa [Sherpa+Spleeter](../configuration/karaoke-processing.md#separation-backend) per separare la voce.

- Il servizio Demucs scarica i modelli in base alla tua [configurazione](../configuration/environments.md#processing-and-model-settings).
- Sherpa+Spleeter funziona bene sulla CPU ed è molto più veloce di Demucs.
- La qualità della separazione è inferiore a quella di Demucs.

### Passaggio al testo TTML {#ttml-lyrics-upgrade}

Non esiste un'alternativa semplice a WhisperX per i dispositivi con la sola CPU. La separazione di un brano di tre minuti può richiedere da uno a due minuti. Per alcuni brani, però, è disponibile una versione TTML dei testi. Il TTML contiene già la sincronizzazione parola per parola e non richiede l'elaborazione con WhisperX.

I timestamp TTML si basano in genere sulla versione ufficiale del brano, non su una specifica versione del video. Non sono quindi adatti ai video musicali con introduzioni, finali o altre modifiche: i testi potrebbero risultare fuori sincrono. Usa il TTML per video che mantengono la durata originale del brano o per file MP3 caricati; usa WhisperX se il video contiene sezioni aggiuntive.

Combinare `Sherpa+Spleeter` con testi TTML offre la soluzione più rapida sui dispositivi con la sola CPU.
