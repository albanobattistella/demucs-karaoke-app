# Provider di testi personalizzati {#custom-lyrics-providers}

L'app può caricare i propri fornitori di testi di fallback in runtime da file o directory Python locali.

![Esempio di provider di testi](../assets/images/custom.webp)

## Configurazione {#configuration}

Impostare `LYRICS_PROVIDER_CUSTOM_PATHS` su un elenco separato da virgole di cartelle o file Python:

```bash
LYRICS_PROVIDER_CUSTOM_PATHS=/app/custom_lyrics,/mnt/shared/lyrics_provider.py
```

### Docker {#docker}

Per Docker, è necessario montare i file del provider personalizzato nel contenitore e impostare `LYRICS_PROVIDER_CUSTOM_PATHS` utilizzando i percorsi del contenitore.

```yaml
      LYRICS_PROVIDER_CUSTOM_PATHS: /app/custom_lyrics
    volumes:
      - ./custom_lyrics:/app/custom_lyrics
```

## Contratto {#contract}

Usa AI per scrivere un provider di testi personalizzato (copia questo modello)
```
I want you to help me write a custom lyrics provider plugin based on the specifications. Please fetch this URL https://raw.githubusercontent.com/vttc08/demucs-karaoke-app/refs/heads/main/custom_lyrics_providers.md for which explains the requirements and structure of a custom lyrics provider, including the InferredSong input and the LyricsPayload output. I will include information about the lyrics provider I want to create, and you will generate the code for it. Please ensure that the generated code adheres to the requirements and structure outlined in the provided documentation.
```

Ogni modulo provider dovrebbe definire una classe provider, di solito chiamata `LyricsProvider` o `CustomLyricsProvider`.

Forma richiesta:

```python
from services.lyrics_types import InferredSong, LyricsPayload


class LyricsProvider:
    name = "hello-world"

    async def fetch(self, inferred_song: InferredSong, **kwargs):
        # Implement your lyrics lookup, proxy rotation, caching, etc. here.
        return LyricsPayload()
```

Requisiti:

- La classe deve essere costruibile senza argomenti.
- `name` deve essere una stringa non vuota.
- `fetch(...)` deve essere `async` (le funzioni sincrone possono essere avvolte con `asyncio.to_thread`).
- `fetch(...)` riceve i metadati delle canzoni normalizzati come `inferred_song`.
- Il loader passa anche suggerimenti per parole chiave come `title` e `artist`, quindi le implementazioni personalizzate dovrebbero accettare `**kwargs` anche se non li usano.
- Se un provider non riesce a trovare una corrispondenza, deve restituire `None`. 

## Input della funzione (`InferredSong`) {#function-input-inferredsong}

`InferredSong` è la descrizione delle canzoni più ripulita e con i migliori sforzi dell'app. Per le ricerche di testi, i campi importanti sono:

- `title`: il titolo della canzone normalizzata
- `artist`: il nome dell'artista normalizzato, quando l'app potrebbe dedurre uno
- `source`: da dove provengono i metadati, ad esempio YouTube o un altro percorso di input


## Tipo restituito (`LyricsPayload` o `None`) {#return-type-lyricspayload-or-none}


`LyricsPayload` è il tipo di reso strutturato per i provider che vogliono essere più espliciti.

Contiene:

- `lyrics`: il testo stesso (stringa a più righe)
- `is_synced`: se i testi hanno timestamp
- `provider`: un nome di provider breve, come `hello-world`
- `inferred_song`: l'`InferredSong` utilizzato per la ricerca
- `provider_score`: punteggio di confidenza utilizzato internamente quando l'app confronta più partite di fallback
    - gamma compresa tra 0 e 250, dove più alto è meglio
- `provider_details`: metadati aggiuntivi opzionali per il debug o l'uso futuro
- `alternatives`: tupla opzionale dei valori `LyricsAlternative` quando il provider
  in grado di offrire un'altra rappresentazione dello stesso risultato.
  rimane l'impostazione predefinita per l'elaborazione; UI può scegliere un'alternativa TTML
  e mantenere la base LRC per il downgrade.

Provider di esempio con un aggiornamento TTML opzionale:

```python
from services.lyrics_types import InferredSong, LyricsAlternative, LyricsPayload


class LyricsProvider:
    name = "example"

    async def fetch(self, inferred_song: InferredSong, **kwargs):
        lrc = "[00:01.00]Original synced lyrics"
        ttml = "<tt>...valid timed TTML...</tt>"
        return LyricsPayload(
            lyrics=lrc,
            is_synced=True,
            provider=self.name,
            inferred_song=inferred_song,
            alternatives=(
                LyricsAlternative(
                    lyrics=ttml,
                    format="ttml",
                    provider=self.name,
                    is_synced=True,
                ),
            ),
        )
```

Il provider Musixmatch integrato utilizza questo contratto per il suo TTML opzionale
aggiornamento. Gli errori di aggiornamento sono trattati come un'alternativa mancante, quindi la base
Il risultato del testo rimane utilizzabile.

## Note di implementazione {#implementation-notes}

- Utilizzare `httpx` per le richieste web quando il provider parla con un HTTP API.
- Utilizzare `subprocess` se il provider si sposta su un altro programma.
- Un provider personalizzato può anche proxy per un server web di testi dedicato.

## Esempio HelloWorld {#helloworld-example}

```python
from services.lyrics_types import InferredSong, LyricsPayload
# import other libraries such as httpx, subprocess, etc. if needed

class LyricsProvider:
    name = "hello-world"

    async def fetch(self, inferred_song: InferredSong, **kwargs):
        # support both inferred_song and kwargs for title/artist hints
        title = inferred_song.title or kwargs.get("title")
        artist = inferred_song.artist or kwargs.get("artist")
        
        # Your custom logic goes here
        # lyrics = find_lyrics(title, artist)
        # is_synced = check_if_synced(lyrics)
        # score = calculate_provider_score(lyrics)

        return LyricsPayload(
            lyrics="""[00:00.00]Hello world
[00:02.00]From a custom provider
[00:04.00]This is just an example""",
            is_synced=is_synced,
            provider="hello-world",
            inferred_song=inferred_song,
            provider_score=250
        )
```

- è necessario importare i tipi da `services.lyrics_types` altrimenti il caricatore non riconoscerà il provider
- altre variabili come l'autenticazione, il proxy ecc. possono essere dichiarate nel costruttore della classe o negli attributi
- `lyrics` deve essere una stringa multi-linea, non un riferimento file/path/URL.
- `is_synced` di `False` indica che si tratta di un testo semplice, mentre `True` indica che ha timestamp.
- non restituire un `LyricsPayload` con testi vuoti; restituire invece `None`.
- non restituire un punteggio vuoto o nullo se si desidera che le app considerino il provider come un fallback; utilizzare un punteggio di 1 o superiore.
- puoi usare il valore più alto di 250 in modo che l'applicazione preferisca sempre il tuo provider rispetto ad altri

