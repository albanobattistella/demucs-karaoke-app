# Modifica dei file multimediali {#media-editing}

Gli strumenti di editing multimediale sono disponibili per gli amministratori dai controlli di modifica della libreria multimediale.

## Strumento di taglio senza perdita di qualità {#lossless-trimmer}

![Strumento di taglio senza perdita di qualità](../assets/images/videotrimmer.webp){ width="600" }

Alcuni brani karaoke hanno introduzioni lunghe, loghi o schermate finali. I video musicali possono contenere anche sezioni senza musica. Per ottenere il risultato migliore, usa lo **Strumento di taglio senza perdita di qualità** per rimuovere queste parti.

Quando usi lo strumento di taglio senza perdita di qualità, anche i sidecar associati, come i testi e la traccia vocale, vengono tagliati per mantenere la sincronizzazione. Lo strumento usa gli I-frame (keyframe) e taglia il video quasi istantaneamente, senza ricodificarlo né ridurne la qualità.

## Editor del testo {#lyrics-editor}

![Editor del testo](../assets/images/subtitleeditor.webp){ width="600" }

Il risultato di WhisperX potrebbe richiedere qualche correzione. L'**Editor dei testi** lo esporta in formati standard per i sottotitoli karaoke, che puoi modificare con un programma esterno. Sono supportati i formati ASS e SRT.

<div class="grid cards" markdown>
- :material-subtitles:{ .lg .middle } __Formato ASS__

    ---

    ASS supporta la sincronizzazione karaoke. Ogni riga viene convertita nel formato standard, con tag `\k` e codici temporali.

    Modificare i file ASS con [Aegisub](https://aegisub.org/).

- :material-subtitles-outline:{ .lg .middle } __Formato SRT__

    ---

    SRT è un formato di sottotitoli molto diffuso. Ogni parola viene esportata come una riga di sottotitoli.

    Modifica i file SRT con [Subtitle Edit](https://www.nikse.dk/SubtitleEdit/).
</div>

## Aggiungere una traccia vocale {#add-vocals}

![Aggiungi traccia vocale](../assets/images/addvocals.webp){ width="600" }

Una traccia vocale guida può essere utile per esercitarsi con un video karaoke già pronto. La funzione **Aggiungi traccia vocale** permette di cercare un brano su YouTube o caricarlo, quindi di estrarne la voce con Demucs.

Se è installato il [pacchetto aggiuntivo vocal-sync](../tasks/create-ai-karaoke.md), la traccia vocale estratta può essere sincronizzata automaticamente con il video karaoke originale. Per la procedura completa, consulta la sezione **Aggiungere una traccia vocale a un video karaoke già pronto**. Puoi anche regolare manualmente la traccia per anticiparla o ritardarla.
