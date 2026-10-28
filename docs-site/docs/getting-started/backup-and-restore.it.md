## Backup e ripristino {#backup-and-restore}
Per l'applicazione principale è sufficiente salvare il database `karaoke.db`, che si trova nella cartella `/data`. L'applicazione manterrà la configurazione anche se viene spostata su un altro host.
### Docker {#docker}
Esegui il comando di backup sul computer host.
```bash
tar -czvf backup.tar.gz -C /path/to/data/ .
```
> Questo comando presuppone che il database, i file karaoke e la cache si trovino tutti in `/data`. Se i file multimediali sono salvati altrove, includi nel backup anche le relative cartelle.

Copia `compose.yml`, `.env` e `backup.tar.gz` sul nuovo host, quindi ripristina i dati con questi comandi:
```bash
tar -xzvf backup.tar.gz -C /path/to/new/data/
docker compose up -d
```

Facoltativo: esegui una nuova scansione della libreria multimediale.

### Linux/Windows {#linuxwindows}

Segui la stessa procedura usata con Docker: salva la cartella `/data` e ripristinala sul nuovo host. Se cambia il percorso della cartella, aggiorna `MEDIA_PATH` e `CACHE_PATH` nel file `.env` in modo che puntino alle nuove posizioni.

Su Windows puoi usare `robocopy`:
```powershell
robocopy C:\path\to\data D:\path\to\new\data /E
```

### Demucs {#demucs}

Non è necessario salvare file di database o di configurazione. Per spostare il servizio basta aggiornare il file `.env`.

I modelli vengono scaricati dall'applicazione. Se vuoi includerli nel backup, individua la cartella della cache Hugging Face:

- SherpaONNX: `<demucs_svc_root>/model_data/sherpa_spleeter`
- Demucs e WhisperX: `$env:HF_HOME` o `%USERPROFILE%\.cache\huggingface` su Windows, `~/.cache/huggingface` su Linux

Se i modelli non vengono trovati, saranno scaricati di nuovo; perciò potrebbe non essere necessario includerli nel backup.

## Aggiornamento {#upgrade}
### Docker {#docker_1}
Scarica l'immagine più recente e riavvia il servizio:
```bash
docker compose pull
docker compose up -d
docker image prune -f
```
### Linux/Windows/Demucs {#linuxwindowsdemucs}
Scarica il codice più recente:
```bash
git pull
uv pip install -e . --upgrade
```
Se necessario, riavvia il servizio tramite `systemd` o `services.msc`.
