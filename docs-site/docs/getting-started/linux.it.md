# Linux {#linux}

La distribuzione Linux utilizza systemd per gestire l'applicazione principale. Questa configurazione è adatta per installazioni bare-metal e LXC.

## Installazione {#installation}

### 1. Preparare l'applicazione e le dipendenze {#1-prepare-the-application-and-dependencies}

Clona il repository dell'applicazione:

```bash
git clone https://github.com/vttc08/demucs-karaoke-app.git /opt/karaoke/app
```

Installa [uv](https://docs.astral.sh/uv/getting-started/installation/), poi installa le dipendenze di sistema e dell'applicazione:

```bash
sudo apt-get install ffmpeg
cd /opt/karaoke/app
uv venv
uv pip install -e .
```

!!! note "Nota"

    L'installazione predefinita non include `numpy` e `scipy`, necessari per la [sincronizzazione vocale](../tasks/create-ai-karaoke.md). Se ti serve questa funzione, installa l'extra `vocal-sync`:

```bash
uv pip install -e ".[vocal-sync]"
```

### 2. Installare Deno se necessario {#2-install-deno-if-needed}

Alcuni video richiedono il runtime JavaScript esterno di yt-dlp. Installa [Deno](https://docs.deno.com/runtime/getting_started/installation/) solo se necessario. Se npm è già installato sull'host, puoi usare il pacchetto npm:

```bash
npm install -g deno
which deno
```

### 3. Generare la documentazione {#3-build-the-documentation}

Nelle installazioni senza Docker, la documentazione statica non è inclusa per impostazione predefinita. Puoi usare l'applicazione senza generarla e consultare la [documentazione online](https://vttc08.github.io/demucs-karaoke-app/). Per generarla localmente, installa le dipendenze della documentazione ed esegui lo script di compilazione:

```bash
uv pip install mkdocs mkdocs-material mkdocs-minify-plugin mkdocs-static-i18n pymdown-extensions
uv run scripts/build_docs.py
```

### 4. Configurare l'ambiente {#4-configure-the-environment}

Salva le variabili d'ambiente in `/etc/karaoke.env`:

La configurazione seguente dovrebbe essere adatta alla maggior parte dei casi. Sostituisci i percorsi di esempio e l'URL del servizio con i valori del tuo host. Per i dettagli, consulta la [sezione di configurazione](../configuration/settings.md).

```env
HOST=0.0.0.0
PORT=8000
DATABASE_URL=sqlite:////opt/karaoke/data/karaoke.db
MEDIA_PATH=/your/media/path
CACHE_PATH=/opt/karaoke/data/cache
LOG_DIR=/opt/karaoke/data/logs
YTDLP_PATH=yt-dlp
YTDLP_DENO_PATH=/usr/local/bin/deno
DEMUCS_API_URL=http://demucs-host:8001
```

!!! note "Nota"

    La cartella indicata da `MEDIA_PATH` deve essere scrivibile dall'utente che esegue l'applicazione e avere spazio sufficiente per i file multimediali. Sono supportate le condivisioni SMB/NFS.

### 5. Creare il servizio systemd {#5-create-the-systemd-service}

Crea `/etc/systemd/system/karaoke.service` con i seguenti contenuti:

```ini
[Unit]
Description=Karaoke main app
After=network-online.target
Wants=network-online.target

[Service]
User=<your-user>
Group=<your-group>
WorkingDirectory=/opt/karaoke/app
EnvironmentFile=/etc/karaoke.env
ExecStart=/usr/bin/env uv run uvicorn main:app --host 0.0.0.0 --port 8000
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### 6. Abilitare e avviare il servizio {#6-enable-and-start-the-service}

```bash
sudo systemctl daemon-reload
sudo systemctl enable karaoke.service
sudo systemctl start karaoke.service
sudo systemctl status karaoke.service
```

Dopo aver modificato in `/settings` i percorsi degli strumenti o dei file, riavvia il servizio se la modifica riguarda le cartelle montate all'avvio o l'ambiente del processo.

### 7. Configurare l'amministratore e i preset predefiniti {#7-configure-the-admin-user-and-default-presets}

Dalla cartella dell'applicazione, crea il primo utente amministratore e installa i preset predefiniti dello stage:

```bash
cd /opt/karaoke/app
uv run python scripts/admin_user.py create --username admin
uv run python scripts/default_presets.py
```

Quando l'applicazione è in esecuzione, apri `http://<your-server-ip>:8000/login` e accedi con l'account amministratore appena creato.

## Passaggi successivi {#next-steps}

- [Backup, ripristino e aggiornamento](backup-and-restore.md)
- [Distribuire il servizio Demucs (opzionale)](demucs-service.md)
- [Connetti i clienti](clients.md)
- [Pagina in coda](../features/queue-page.md)
- [Creare karaoke con l'IA](../tasks/create-ai-karaoke.md)
