# Stage e branding {#stage-branding}

Questa pagina spiega come configurare lo stage per gli ospiti, personalizzare la visualizzazione dei testi e gestire gli sfondi e i preset.

## Contenuto {#contents}

- [Configurare lo stage](#configure-the-stage)
- [Personalizzare lo schermo del palco](#customize-the-stage-display)
- [Personalizzazione grafica](#custom-branding)
- [Preset dei testi](#lyric-presets)
- [Modificare i testi dal controllo della coda](#modify-lyrics-from-queue-control)
- [Utilizzare un iPhone o un iPad come schermo del palco](#use-an-iphone-or-ipad-as-a-stage-display)

## Configurare lo stage {#configure-the-stage}

### 1. Configurare il codice QR dello stage {#1-configure-the-stage-qr-code}

Per agevolare l'accesso degli ospiti, configura un codice QR con un URL raggiungibile dai loro dispositivi. Se non vuoi rendere pubblica la coda, può essere utile configurare un [proxy inverso con controllo degli accessi](server-administration.md#restrict-access-to-guest-wi-fi-users).

![Schermo del palco](../assets/images/stage.webp)

1. Imposta l'[URL del codice QR nella pagina Impostazioni](../configuration/stage.md#stage-qr-url).
2. Nella pagina `/stage`, premi ++q++ per mostrare il codice QR sullo schermo.
3. Premi **+** o **-** per modificarne le dimensioni.

### 2. Configurare la lobby del palco {#2-configure-the-stage-lobby}

Quando la coda è vuota, l'applicazione mostra una schermata d'attesa predefinita con un video e un segnale audio in loop. Puoi sostituirli con i tuoi contenuti per personalizzare lo stage.

![Lobby predefinita](../assets/images/tasks/stage-lobby.webp)

Imposta l'[URL del contenuto della schermata d'attesa nella pagina Impostazioni](../configuration/stage.md#stage-lobby-media-url).

Ecco due modi per creare o scegliere i media di lobby:

<div class="grid cards" markdown>

-   __Scarica un video da YouTube__

    ---

    Segui la guida [Aggiungere alla coda un video karaoke già pronto](karaoke-tasks.md#queue-a-premade-karaoke-video) per scaricare un video nella libreria.

    - Puoi [rinominare il video](create-ai-karaoke.md), per esempio in `stage-lobby.mp4`, così sarà facile individuarlo.

-   __Crea un video con Remotion__

    ---

    [Remotion](https://www.remotion.dev/) è un framework per creare video tramite codice. È facile da configurare rispetto ai programmi di editing video professionali e può essere usato con l'assistenza di agenti IA.

</div>

## Personalizzare la visualizzazione dello stage {#customize-the-stage-display}

La pagina dello stage offre molte opzioni per personalizzare i testi. Dopo aver installato i preset predefiniti, scegline uno come punto di partenza e adattalo all'ambiente in cui userai lo stage.

![Testo della canzone](../assets/images/presets.webp)

!!! note "I testi sullo stage richiedono la modalità a schermo intero"

    Per non mostrare controlli che distraggono durante la riproduzione, i testi appaiono solo quando lo stage è a schermo intero.

Puoi chiedere a un assistente IA di generare un file JSON con un preset personalizzato. Usa questo prompt come punto di partenza:

??? tip "Prompt da copiare e incollare"

    ```text
    You are a creative designer for a karaoke stage display. Generate one complete,
    valid JSON object for a custom lyrics preset. The design brief is:

    [Describe the venue, mood, audience, colors to use or avoid, and whether the
    lyrics will often be Chinese, Latin-script, or mixed.]

    Prioritize visual aesthetics and projected-screen legibility: choose a cohesive
    font, color scheme, typography weight, spacing, line hierarchy, and outline
    that still read clearly over a moving music video. Make the active lyric color
    visually distinct without using low-contrast combinations. Use restrained
    neighbor-line opacity and scale so the current line is obvious from a distance.

    Include the JSON with the following keys and values:
    fontPreset, customFontFamily, customFontWeight, sizeVw, lineWidthPct,
    lineGapVw, neighborLineScalePct, neighborLineOpacityPct, textColor,
    activeColor, outlineColor, outlineWidth, previousLines, nextLines,
    lineBehavior, animation, backgroundMediaEnabled, backgroundMediaPath,
    backgroundMediaOpacityPct.

    Rules:
    - Use one of fontPreset: custom, karaoke_cjk, readable_cjk, system_cjk, serif_cjk.
    - For custom fonts, choose a real Google Fonts family and one supported
      weight from 300, 400, 500, or 700. Otherwise set customFontFamily to an empty
      string and customFontWeight to 700.
    - Use #RRGGBB colors only.
    - Keep values within: sizeVw 3.2-8.8; lineWidthPct 60-100; lineGapVw 0.2-2;
      neighborLineScalePct and neighborLineOpacityPct 30-100; outlineWidth 2-14;
      previousLines and nextLines 0-3; backgroundMediaOpacityPct 10-100.
    - Use rolling, rolling_scroll, or fixed_group for lineBehavior; use slide,
      crop, fade, or none for animation.
    - The crop animation is preferred for classic karaoke scrolling.
    - Do not use a background image or video in this generated design: set
      backgroundMediaEnabled to false and backgroundMediaPath to an empty string.
    - If the user has specified a JSON object with backgroundMediaEnabled, keep
      that value and change the other values to match the design brief.
    ```

Per altri dettagli tecnici, consulta [`custom_presets.md`](https://raw.githubusercontent.com/vttc08/demucs-karaoke-app/main/custom_presets.md).

### Tipografia e layout {#typography-and-layout}

**Tipo di carattere** indica il font usato per i testi. Puoi scegliere tra centinaia di font tramite [Google Fonts](https://fonts.google.com/).

**Gruppo di font personalizzato** indica la famiglia Google Fonts da caricare. Il nome del font distingue tra maiuscole e minuscole.

??? warning "I nomi dei font distinguono tra maiuscole e minuscole"

    `Roboto` e `roboto` sono nomi diversi. Se il nome è scritto in modo errato, il font non verrà caricato. Per usare Google Fonts è inoltre necessaria una connessione Internet.

**Spessore del font personalizzato** controlla lo spessore del carattere. Puoi scegliere tra Light, Regular, Medium e Bold.

![Lyrics layout](../assets/images/lyrics-layout.webp){ width="700" }

**Dimensione del testo** controlla la dimensione principale dei testi, espressa in unità di larghezza della finestra.

**Larghezza massima** imposta la larghezza massima delle righe come percentuale della larghezza dello stage.

**Interlinea** controlla lo spazio tra le righe visibili, espresso in unità di larghezza della finestra.

**Colore del testo** imposta il colore dei testi non evidenziati.

**Colore del testo attivo** imposta il colore della riga o della parola corrente.

**Colore del bordo** imposta il colore del contorno che rende il testo più leggibile.

**Spessore del bordo** controlla lo spessore del contorno del testo.

![Linee liriche vicine](../assets/images/lyrics-neighbor.webp){ width="700" }

**Righe precedenti** indica quante righe mostrare prima di quella corrente. Questa impostazione non si applica a `fixed_group`.

**Righe successive** indica quante righe mostrare dopo quella corrente. Con `fixed_group`, il gruppo visibile contiene `1 + nextLines` righe.

**Dimensione del testo adiacente** controlla la dimensione delle righe precedenti e successive rispetto a quella corrente.

**Opacità del testo adiacente** controlla la trasparenza delle righe precedenti e successive.

### Animazioni e comportamento delle righe {#animation-and-line-behavior}

![Lyrics animazione](../assets/images/tasks/lyricsanimation.gif)

**Animazione** controlla la transizione del testo. `crop` riproduce più da vicino il classico effetto di scorrimento karaoke.

- `slide`: la parola attiva si ingrandisce e torna alle dimensioni normali durante la transizione.
- `crop`: la parola attiva viene rivelata da sinistra a destra, come in uno scorrimento.
- `fade`: la parola attiva appare gradualmente modificando la propria opacità.
- `none`: il colore della parola cambia senza effetti di transizione.

![Comportamento righe](../assets/images/tasks/lyricsbehavior.gif){ width="700" }

**Comportamento delle righe** controlla come avanza la porzione di testo visibile.

- `rolling` mantiene la riga corrente in una finestra definita da `previousLines` e `nextLines`. La riga corrente resta sempre nella stessa posizione.
- `rolling_scroll` usa la stessa finestra, ma la fa scorrere verso l'alto man mano che il testo avanza.
- `fixed_group` ignora `previousLines`, mostra un gruppo fisso di `1 + nextLines` righe e avanza solo quando la riga corrente esce dal gruppo.

### Supporti di sfondo {#background-media}

**File multimediale di sfondo** indica il percorso relativo all'immagine o al video mostrato sopra il video principale e dietro i testi. Per i dettagli, consulta [Personalizzazione grafica](#custom-branding).

**File multimediale di sfondo abilitato** determina se l'immagine o il video di sfondo viene mostrato dietro i testi.

**Opacità dello sfondo** controlla la trasparenza dell'immagine o del video. Puoi usarla per scurire uno sfondo troppo luminoso e rendere più leggibili i testi.

## Personalizzazione grafica {#custom-branding}

Con i testi allineati da WhisperX puoi aggiungere un'immagine o un video di sfondo. Puoi scurirlo per rendere i testi più leggibili, coprire contenuti sensibili oppure aggiungere una filigrana o un logo. Le immagini trasparenti, come i file `.png`, possono essere sovrapposte al video. Configura lo sfondo nelle impostazioni dei testi dello stage.

![Personalizzazione del marchio](../assets/images/branding.webp)

??? note "Disponibile solo dallo stage"

    Puoi cambiare l'immagine o il video di sfondo solo dalla pagina Stage. Dal controllo della coda puoi soltanto attivare o disattivare lo sfondo configurato. Salva sfondi diversi in preset e passa da uno all'altro quando necessario.

I preset predefiniti includono due immagini di sfondo:

- `black.png`: sfondo nero.
- `branding1.png`: sfondo generico con testo e logo, a scopo dimostrativo.

## Preset dei testi {#lyric-presets}

Le personalizzazioni dei testi vengono salvate in un file JSON. Puoi esportarlo, importarlo o condividerlo come preset.

Nella sezione **Preset**, scegli un preset e fai clic su **Applica** o **Elimina**. Per salvare le impostazioni correnti, fai clic su **Crea** e assegna un nome al preset. Usa **Aggiorna** per sostituire le impostazioni di un preset esistente.

Nella sezione **Trasferimento avanzato**, puoi importare o esportare le impostazioni correnti in formato JSON:

- **Scarica**: esporta le impostazioni correnti in un file JSON.
- **Applica**: applica le impostazioni contenute nel testo JSON.
- **Carica**: importa un file JSON e sostituisce le impostazioni correnti.

## Modificare i testi dal controllo della coda {#modify-lyrics-from-queue-control}

Puoi controllare da remoto la visualizzazione dello stage dalla pagina della coda, anche se le opzioni di personalizzazione sono limitate.

![Il controllo della coda lyrics](../assets/images/queue/queuelyrics.webp)

- **Testi**: mostra o nasconde i testi.
- **Sfondo**: mostra o nasconde l'immagine o il video configurato nelle impostazioni dello stage.
- **Schermo di destinazione**: scegli quale schermo controllare se ne sono collegati più di uno.
- **Preset**: seleziona un preset da applicare al posto delle impostazioni correnti.
- **Dimensione del testo** e **Larghezza massima**: modifica rapidamente questi due valori dalla pagina della coda.

??? note "Applica o sovrascrivi"

    **Applica** applica solo il preset e non modifica **Dimensione del testo** né **Larghezza massima**. Usa **Sovrascrivi** per modificare questi valori insieme al preset o alle impostazioni correnti.

## Usare iPhone o iPad come schermo dello stage {#use-an-iphone-or-ipad-as-a-stage-display}

I dispositivi Apple possono mostrare lo stage karaoke, ma i browser iOS e iPadOS presentano alcune limitazioni di riproduzione.

### Limitazioni di riproduzione audio {#audio-playback-limitations}

iOS e iPadOS non riproducono in modo affidabile due sorgenti multimediali contemporaneamente. Per evitare problemi, lo stage riproduce il video karaoke o la base strumentale e disattiva la traccia vocale.

- Al momento non esiste una soluzione alternativa. Quando l'applicazione rileva uno user agent iOS o iPadOS, disattiva la traccia vocale.

### Compatibilità video e audio {#video-and-audio-compatibility}

I video scaricati da YouTube sono spesso in formato VP9, mentre le tracce generate da Demucs usano MP3. Per velocizzare l'elaborazione, l'applicazione copia i flussi senza ricodificarli e non modifica il contenitore MP3 quando unisce il video e l'audio elaborato. Alcuni dispositivi Apple potrebbero non supportare VP9, MP3 o altri formati.

Per una migliore compatibilità:

- Imposta il [codec video di yt-dlp](../configuration/downloads.md#yt-dlp-video-codec) su `avc` per scaricare video H.264.
- Imposta il [codec audio di FFmpeg](../configuration/karaoke-processing.md#ffmpeg-audio-codec) su `aac` per ricodificare l'audio durante l'unione.
- Per gli sfondi personalizzati o i video in loop dello stage, usa formati compatibili, come video H.264 con audio AAC.

Queste impostazioni si applicano ai brani scaricati o elaborati da quel momento in poi. Prima di riprodurre sui browser Apple i brani già presenti, converti i video in H.264 con audio AAC.
