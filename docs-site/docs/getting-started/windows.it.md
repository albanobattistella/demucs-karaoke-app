## Windows {#windows}

Questa guida spiega come installare l'applicazione principale su Windows. Per installare il servizio Demucs, consulta la guida al [servizio Demucs](demucs-service.md).

### 1. Scaricare Python/UV e le dipendenze {#1-download-pythonuv-and-dependencies}

- [Python](https://www.python.org/downloads/windows/) 3.11 o versioni successive. Durante l'installazione, seleziona "Add Python to PATH".
- [uv](https://docs.astral.sh/uv/getting-started/installation/)
- [ffmpeg](https://ffmpeg.org/download.html) 6.0 o versioni successive; aggiungilo a `PATH`.
- [Deno](https://docs.deno.com/runtime/getting_started/installation/), da aggiungere a `PATH` solo se necessario.

### 2. Aggiungere gli eseguibili a PATH {#2-add-binary-to-path}

Apri "Modifica le variabili di ambiente di sistema" e aggiungi a `PATH`:

- `C:\Tools\ffmpeg\bin` (oppure la cartella in cui hai installato ffmpeg, ffprobe e ffplay.exe)
- Se necessario, il percorso di Deno, per esempio `C:\Tools\deno`

Verificare sulla riga di comando:

```powershell
ffmpeg -version
uv --version
deno --version  # only if needed
python --version
```

### 3. Scaricare e configurare l'applicazione {#3-download-and-setup-the-application}

```powershell
git clone https://github.com/vttc08/demucs-karaoke-app.git
cd demucs-karaoke-app
uv venv
uv pip install -e .
```

!!! tip "Suggerimento"
    Se Git non è installato, puoi scaricare l'archivio ZIP da GitHub ed estrarlo.

!!! note "Nota"
    L'installazione predefinita non include `numpy` e `scipy`, che sono necessari per [Sincronizzazione vocale](../tasks/create-ai-karaoke.md). Se hai bisogno di sincronizzazione vocale, installa l'`vocal-sync` extra:

    ```powershell
    uv pip install -e .[vocal-sync]
    ```

### 4. Generare la documentazione {#4-build-documentation}

Nelle installazioni senza Docker la documentazione statica non è inclusa. Puoi usare l'applicazione senza generarla e consultare la [documentazione online](https://vttc08.github.io/demucs-karaoke-app/). Per generarla localmente, installa `mkdocs` e `mkdocs-material`:

```bash
uv pip install mkdocs mkdocs-material mkdocs-minify-plugin mkdocs-static-i18n
uv run scripts/build_docs.py
```

### 5. Configurare l'ambiente {#5-configure-the-environment}

```powershell
cp .env.example .env
```

La configurazione predefinita dovrebbe essere adatta alla maggior parte dei casi. Per i dettagli, consulta la [pagina delle impostazioni](../configuration/settings.md).

### 6. Avviare l'applicazione {#6-running-the-application}

```powershell
uv run uvicorn main:app --host 0.0.0.0 --port 8000
```

Per avviare l'applicazione senza effettuare l'accesso, usa Utilità di pianificazione o un wrapper di servizio come NSSM. Salva i file multimediali, la cache, i log e il database SQLite in una cartella leggibile e scrivibile dall'account del servizio.

### 7. Configurare l'amministratore e i preset predefiniti {#7-configure-the-admin-user-and-default-presets}

Mentre l'applicazione è in esecuzione, apri un'altra finestra di PowerShell nella cartella dell'applicazione. Crea il primo utente amministratore e installa i preset predefiniti dello stage:

```powershell
uv run python scripts/admin_user.py create --username admin
uv run python scripts/default_presets.py
```

Quando l'applicazione è in esecuzione, apri `http://<your-server-ip>:8000/login` e accedi con l'account amministratore appena creato.

## Passaggi successivi {#next-steps}

- [Configurare l'applicazione](../configuration/settings.md)
- [Distribuire il servizio Demucs (facoltativo)](demucs-service.md)
- [Dispositivi client](clients.md)
- [Esplorare la pagina della coda](../features/queue-page.md)
