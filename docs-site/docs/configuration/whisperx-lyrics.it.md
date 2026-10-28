# Testi con WhisperX {#whisperx-lyrics}

![Impostazioni del testo di WhisperX](../assets/images/settings/whisperx-lyrics.webp){ width="400" }

WhisperX genera la sincronizzazione parola per parola a partire da testi non sincronizzati o in formato LRC. Durante l'elaborazione, l'applicazione principale invia i testi e la traccia vocale separata al servizio Demucs. WhisperX rileva la lingua, allinea il testo alla voce e restituisce un file JSON con i testi sincronizzati.

Usa questa pagina per configurare la lingua di WhisperX e il processo di allineamento. Puoi impostare questi valori anche tramite le [variabili d'ambiente](environments.md) se l'installazione richiede una configurazione fissa.

### Modello di trascrizione WhisperX {#whisperx-transcription-model}

Modello di trascrizione usato da WhisperX per rilevare la lingua. Il valore predefinito è `tiny`, consigliato perché il backend non deve trascrivere tutto l'audio quando la lingua è già nota.

### Lingua di allineamento WhisperX {#whisperx-alignment-language}

Lingua usata da WhisperX per l'allineamento. Inserisci un codice, per esempio `en` o `zh`.

??? note "Scegliere tra il rilevamento automatico e una lingua fissa"

    Attiva il rilevamento automatico se la libreria contiene brani in lingue diverse. WhisperX potrà così scegliere il modello di allineamento appropriato; questa opzione è in genere più semplice per gli ospiti meno esperti. Puoi comunque specificare una lingua per i singoli brani.

    Se la maggior parte dei brani della libreria è in una sola lingua, specificala manualmente. In questo modo eviti il rilevamento automatico e riduci il rischio che venga scelto il modello sbagliato, con un allineamento meno preciso. Quando la lingua è specificata, è possibile saltare la fase di trascrizione usata per rilevarla.

### Rilevare la lingua prima della trascrizione {#detect-language-before-transcription}

Attiva questa opzione per far rilevare a WhisperX la lingua dell'audio prima dell'allineamento e selezionare il modello appropriato.

### Utilizzare i tempi del testo sincronizzato {#use-synced-lyrics-timings}

Questa opzione è disattivata per impostazione predefinita e si consiglia di lasciarla così. WhisperX può usare righe LRC sincronizzate, come `[0:01.000] line`, che associano un timestamp a ogni riga e possono velocizzare l'allineamento.

Tuttavia, i testi provenienti da fonti esterne raramente sono sincronizzati con il video o l'audio usato per il karaoke. I relativi timestamp possono quindi peggiorare la precisione dell'allineamento parola per parola.

### Elenco dei modelli WhisperX da precaricare {#whisperx-preload-list}

Elenco, separato da virgole, dei modelli WhisperX da scaricare e caricare in anticipo. Il valore predefinito è `transcription=tiny,align=en`. Ogni voce usa il formato `type=model`, per esempio:

- `transcription=tiny`: precaricare il modello di trascrizione utilizzato per il rilevamento della lingua.
- `align=en`: precaricare il modello di allineamento inglese.
- `align=zh`: precaricare il modello di allineamento cinese.

I modelli devono essere scaricati prima di poter essere usati. Il pulsante **Precarica WhisperX** scarica e carica i modelli configurati prima del primo processo di elaborazione karaoke.
