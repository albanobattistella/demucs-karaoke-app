# Docker {#docker}

### 1. Scaricare il file Compose e `.env.example` {#1-download-the-compose-file-and-envexample}

```bash
wget https://raw.githubusercontent.com/vttc08/demucs-karaoke-app/refs/heads/main/compose.yml
wget https://raw.githubusercontent.com/vttc08/demucs-karaoke-app/refs/heads/main/.env.example
```

### 2. Preparare l'ambiente e le cartelle {#2-prepare-the-environment-and-folders}

```bash
mkdir -p data
mv .env.example .env
```

- L'applicazione viene eseguita come utente non root per impostazione predefinita, quindi la cartella `data` deve essere creata in anticipo.
- È possibile modificare `user: uid:gid` in base alle autorizzazioni dell'host.

### 3. Configurare l'ambiente {#3-configure-the-environment}

- Utilizzare un editor di testo come `vim` o `nano` per esaminare sia la sezione `environment` in `compose.yml` che `.env`. La configurazione predefinita dovrebbe essere sufficiente per la maggior parte dei casi d'uso.

Sono disponibili due immagini:

- `vttc08/demucs-karaoke-app` è l'immagine predefinita utilizzata da `compose.yml`.
- `vttc08/demucs-karaoke-app:vocal-sync` è un'immagine più grande che include `numpy` e `scipy`, necessari per il flusso di lavoro di sincronizzazione vocale.

!!! note "Nota"

    I token di Musixmatch e Last.fm migliorano la ricerca dei testi. Senza, alcune funzioni di ricerca non saranno disponibili.

- [Token Last.fm](https://www.last.fm/api/authentication)
- token Musixmatch (è richiesta l'app desktop): [segui questa guida](https://spicetify.app/docs/faq#sometimes-popup-lyrics-andor-lyrics-plus-seem-to-not-work)

### 4. Avviare l'applicazione {#4-start-the-application}

```bash
docker compose up -d
docker compose logs -f
```

### 5. Configurare l'amministratore e i preset predefiniti {#5-configure-the-admin-user-and-default-presets}

```bash
docker compose exec -it karaoke python scripts/admin_user.py create --username admin
docker compose exec -it karaoke python scripts/default_presets.py
```

Una volta che l'applicazione è in esecuzione, apri `http://<your-server-ip>:8000/login` e accedi con l'utente amministratore appena creato.

## Passaggi successivi {#next-steps}

- [Backup, ripristino e aggiornamento](backup-and-restore.md)
- [Distribuire il servizio Demucs (opzionale)](demucs-service.md)
- [Connetti i clienti](clients.md)
- [Pagina in coda](../features/queue-page.md)
- [Creare karaoke con l'IA](../tasks/create-ai-karaoke.md)
