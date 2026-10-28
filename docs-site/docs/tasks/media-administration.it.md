# Gestione dei file multimediali {#media-administration}

Usa questa guida per gestire i file multimediali importati, correggere la sincronizzazione dei testi, modificare i video e convertire i file karaoke CD+G.

## Indice {#contents}

- [Importare ed esportare file multimediali](#import-and-export-media)
- [Correggere la sincronizzazione dei testi WhisperX](#resynchronize-inaccurate-whisperx-lyrics)
- [Modificare la durata del video e i metadati](#adjust-video-duration-and-metadata)
- [Convertire i formati CD+G](#modernize-cdg-formats)

## Importare ed esportare file multimediali {#import-and-export-media}

Puoi scaricare tutti i file creati dall'applicazione per il karaoke, comprese le tracce vocali separate e i testi JSON sincronizzati parola per parola.

1. Apri `/media` e fai clic su **Modifica**.
2. Espandi **Gestione file**.
3. Fai clic su **Scarica ZIP** per scaricare tutti i file oppure scarica quelli singolarmente.
4. Quando non servono più, elimina i sidecar, come la traccia vocale e i testi.

L'opzione **Scarica ZIP** crea un archivio dei file del brano. Puoi caricarlo tramite `/upload` per ripristinare i file karaoke da un backup.

Per i file gestiti esternamente, l'applicazione si aspetta che i sidecar seguano questa convenzione di denominazione:

- `<song>.mp4`: il video principale. Se importi un video con la voce separata, deve contenere solo la base strumentale.
- `<song>.vocals.mp3`: la traccia vocale separata.
- `<song>.json` o `<song>.lrc`: il file con i testi sincronizzati parola per parola o in formato standard. Il file JSON dovrebbe essere generato da WhisperX.

Esempio di JSON generato da WhisperX:

```json
{"segments":[{"start":0.0,"end":5.0,"text":"Hello, world!","words":[{"start":0.0,"end":2.5,"word":"Hello"},{"start":2.5,"end":5.0,"word":"world"}]}]}
```

??? note "JSON non generato da WhisperX"

    Se il file JSON non rispetta il formato di WhisperX o non è valido, l'applicazione non potrà elaborarlo e i testi non verranno visualizzati.

Copiali nella cartella `media`, quindi avvia una scansione della libreria per rilevare i nuovi brani.

## Correggere la sincronizzazione dei testi WhisperX {#resynchronize-inaccurate-whisperx-lyrics}

??? note "Problemi gravi di sincronizzazione"

    Se i testi scorrono troppo velocemente o sono fuori sincrono, WhisperX potrebbe aver rilevato la lingua sbagliata e scelto un modello di allineamento non adatto. Elimina il file JSON esistente e ripeti l'allineamento specificando manualmente la lingua.

    1. Nell'editor multimediale, espandi **Gestione file** ed elimina il file JSON dei testi.
    2. Attiva **Sincronizzazione del testo** e **Allineamento WhisperX**.
    3. Inserisci manualmente il codice della lingua in **Sostituzione lingua WhisperX**.

Per problemi minori — per esempio, una parola che termina troppo presto o resta visibile fino al ritornello successivo — puoi correggere la sincronizzazione con strumenti di terze parti. Usa il file `.vocals.mp3` come riferimento. Dopo aver apportato le modifiche, carica il file corretto: il server lo convertirà di nuovo in JSON.

![Editor sottotitoli](../assets/images/subtitleeditor.webp)

### Sincronizzazione karaoke in ASS {#ssa-karaoke-timing}

L'applicazione esporta un file di sottotitoli `.ass` con la sincronizzazione karaoke. Puoi usare [Aegisub](https://aegisub.org/) per correggere i tempi ed esportare il file `.ass` aggiornato.

- [Tutorial sulla sincronizzazione karaoke](https://aegisub.org/docs/latest/karaoke_timing_tutorial/)
- [YouTube: lezione 10 di Aegisub — creare un video karaoke](https://www.youtube.com/watch?v=4YTIaMeKXts)

![Aegisub](../assets/images/sysadmin/aegisub.gif)

1. Importa in Aegisub la traccia vocale e il file `.ass`.
2. Attiva **Karaoke Timing** per modificare i tempi parola per parola.
3. Disattivalo per modificare i tempi delle righe.
4. Fai clic con il tasto sinistro per impostare il punto iniziale della riga e con quello destro per impostare il punto finale.
5. Premi la barra spaziatrice per ascoltare l'anteprima insieme alla traccia vocale.

Questa non è una guida completa ad Aegisub.

### Modificare la temporizzazione delle parole in SRT {#srt-word-editing}

L'applicazione esporta anche un file di sottotitoli `.srt` con indicatori temporali, in cui ogni parola è una voce separata. Puoi usare [Subtitle Edit](https://www.nikse.dk/SubtitleEdit/) per correggere i tempi ed esportare il file `.srt` aggiornato.

![Subtitle Edit](../assets/images/sysadmin/subtitleedit.gif)

1. Importa in Subtitle Edit la traccia vocale e il file `.srt`.
2. Subtitle Edit dovrebbe generare automaticamente la forma d'onda della voce.
3. Trascina gli orari di inizio e fine di ogni parola per modificarne la sincronizzazione.

??? warning "Non eliminare le righe `//wx:meta` e `//wx:time`"

    Questi marcatori sono necessari per ricostruire la sincronizzazione parola per parola.

### Dividere e unire le righe {#split-and-merge-lines}

![Righe di testo lunghe](../assets/images/sysadmin/long-lyrics.webp)

A volte i testi originali hanno righe troppo lunghe che, andando a capo, occupano più righe sullo schermo karaoke. Per mantenere uniforme il numero di caratteri per riga, modifica il layout o ricomponi il testo.

Puoi attenuare il problema [riducendo la dimensione del testo o aumentando la larghezza massima](stage-and-branding.md#typography-and-layout).

Ci sono due modi per risolvere le linee:

1. Durante l'elaborazione con WhisperX, imposta **Ricomponi righe del testo** nella pagina [Creare karaoke con l'IA](create-ai-karaoke.md). Il valore predefinito è 36 caratteri per riga in inglese e 12 per CJK. Scegli un limite adatto ai tuoi preset e alle tue personalizzazioni.
2. Dividi una riga lunga nel punto più naturale oppure unisci più righe brevi.

![Dividi e unisci](../assets/images/sysadmin/merge-split.gif)

1. Nell'**Editor dei testi**, scegli **Dividi e unisci** per aprire l'editor dedicato.
2. Usa **Elaborazione automatica** e imposta **Lunghezza massima riga** per dividere automaticamente le righe che superano il limite.
3. Fai clic su una parola per dividere la riga dopo quella parola.
4. Fai clic su **Unisci con la riga sotto** per unire la riga corrente con quella successiva.
5. Se necessario, annulla le modifiche.

## Modificare la durata del video e i metadati {#adjust-video-duration-and-metadata}

Quando scarichi un video da YouTube, vengono mantenuti i metadati predefiniti: il titolo corrisponde a quello del video, l'artista non è specificato e il nome del file è `<video_title>.mp4`. Questi valori potrebbero non essere adatti a organizzare la libreria. Inoltre, il video viene scaricato senza modifiche e molti video karaoke contengono introduzioni o finali che interrompono la riproduzione senza pause.

Per modificare i metadati del video, consulta [Modificare un video esistente](create-ai-karaoke.md).

### Taglio senza perdita di qualità {#lossless-trim}

??? note "Il taglio senza perdita di qualità non può essere esatto"

    L'applicazione usa gli I-frame per tagliare il video senza ricodificarlo. Il taglio deve quindi avvenire in corrispondenza di un I-frame. Non è possibile scegliere un punto preciso: l'applicazione seleziona l'I-frame più vicino.

??? warning "Il taglio è irreversibile"

    Il taglio è irreversibile: una volta tagliato il video, l'originale viene eliminato.

![Taglio senza perdita di qualità](../assets/images/videotrimmer.webp)

L'editor mostra una timeline con tutti gli I-frame disponibili per il taglio.

- Trascina le maniglie per impostare l'inizio e la fine. Ciascuna si aggancia all'I-frame più vicino.
- Inserisci l'ora di inizio e di fine negli appositi campi. L'applicazione sceglie un I-frame che include ciascun punto indicato.
- Riproduci il video e usa i pulsanti **:material-rewind: Imposta** e **:material-fast-forward: Imposta** per segnare i punti di inizio e fine.
- Usa i pulsanti **:material-rewind:** e **:material-fast-forward:** per passare all'I-frame precedente o successivo.
- Usa i pulsanti **:material-rewind: Salta** e **:material-fast-forward: Salta** per vedere l'anteprima del taglio tra i punti iniziale e finale.

??? tip "Velocizzare l'anteprima con le scorciatoie da tastiera"

    - ++i++ / ++o++: imposta i punti iniziale e finale del taglio;
    - ++bracket-left++ / ++bracket-right++: passa da un fotogramma chiave rilevato all'altro;
    - ++comma++ / ++period++: sposta la testina di riproduzione di un fotogramma;
    - ++home++ / ++end++: vai all'inizio o alla fine.

## Convertire i formati CD+G {#modernize-cdg-formats}

CD+G è un formato karaoke tradizionale che visualizza i testi come immagini. Spesso viene usato insieme a un file MP3 nel formato MP3+G. Lo schermo dello stage può riprodurre la grafica CD+G, ma non è possibile [tagliare](#lossless-trim) un file CD+G senza prima convertirlo in video.

Se provi ad aprire nell'editor di taglio senza perdita di qualità un file CD+G, l'applicazione propone di usare **Transcodifica in MP4**.

- Nell'editor, converti la grafica CD+G in un video MP4 con l'audio MP3.
- Seleziona **Sostituisci il file CD + G originale dopo la creazione dell'MP4** per creare un nuovo elemento multimediale mantenendo anche l'elemento CD+G originale.
