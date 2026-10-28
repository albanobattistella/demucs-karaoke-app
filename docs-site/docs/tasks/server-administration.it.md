# Amministrazione del server {#server-administration}

Questa pagina descrive come limitare l'accesso alla rete, collegarsi a un servizio Demucs remoto, instradare i download tramite proxy e monitorare l'applicazione.

## Contenuto {#contents}

- [Limitare l'accesso agli utenti della rete Wi-Fi ospiti](#restrict-access-to-guest-wi-fi-users)
- [Rendere raggiungibile un servizio Demucs remoto](#expose-a-remote-demucs-service)
- [Utilizzare un server proxy per i download](#use-a-proxy-server-for-downloads)
- [Monitoraggio del server](#server-monitoring)

## Limitare l'accesso agli utenti Wi-Fi degli ospiti {#restrict-access-to-guest-wi-fi-users}

L'applicazione è pensata per l'uso su una rete locale e, per impostazione predefinita, chiunque sia connesso alla stessa rete può accedervi. Invece di collegare gli ospiti alla rete dei computer personali, molti router domestici, compresi quelli forniti dagli operatori Internet, permettono di creare una rete Wi-Fi ospiti isolata.

La rete Wi-Fi ospiti può anche contribuire a limitare l'accesso all'applicazione karaoke: gli ospiti vi accedono tramite l'indirizzo pubblico, mentre il proxy inverso accetta solo le richieste provenienti dalla rete domestica.

??? note "Sono necessari un indirizzo IP pubblico e l'hairpin NAT"

    Se il router non supporta l'hairpin NAT o l'operatore usa CGNAT, scegli una delle [alternative di accesso remoto](#expose-a-remote-demucs-service).

### Come funziona questa configurazione {#how-this-setup-works}

1. Punta un record DNS all'indirizzo IP pubblico della rete domestica.
2. Configura il proxy inverso in modo che consenta le richieste dalla sottorete domestica e neghi quelle dagli altri indirizzi IP.
3. Collega i dispositivi degli ospiti alla rete Wi-Fi ospiti isolata.

Con l'hairpin NAT, una richiesta inviata dalla rete Wi-Fi ospiti all'indirizzo DNS pubblico passa dal router o dal gateway e rientra nella rete domestica. Per il proxy inverso, la richiesta proviene dal router, incluso nell'elenco degli indirizzi consentiti. Le richieste degli altri utenti Internet vengono invece respinte.

Questa configurazione offre un buon controllo degli accessi per un karaoke domestico: per raggiungere l'applicazione, gli ospiti devono trovarsi fisicamente sulla rete autorizzata.

### Configurare la pagina con accesso limitato {#configure-the-access-restricted-page}

L'applicazione include una pagina `/access-restricted` che può visualizzare un messaggio agli utenti negati. È anche possibile configurare il proxy inverso per mostrare una pagina personalizzata.

![Pagina di accesso negato](../assets/images/sysadmin/access-denied.webp)

La configurazione generale del reverse proxy dovrebbe:

- Consentire la sottorete domestica e negare tutti gli altri indirizzi IP.
- Inoltrare il traffico consentito all'indirizzo IP e alla porta dell'applicazione karaoke.
- Consentire esplicitamente tutte le richieste a `/access-restricted` e inoltrarle all'applicazione.
- Reindirizzare a `/access-restricted` le risposte `403` generate dal proxy inverso.

Ad esempio, in Nginx Proxy Manager crea un elenco di accesso, aggiungi la sottorete domestica (per esempio `192.168.0.0/24`), consentila, nega gli altri indirizzi e applica l'elenco all'host proxy.

??? note "Configurazione Nginx personalizzata per Nginx Proxy Manager"

    ```nginx
    location /access-restricted {
        allow all;
        proxy_pass http://<your-application-ip>:<your-application-port>;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    error_page 403 = @handle_403;

    location @handle_403 {
        # Replace with your destination URL or path.
        return 302 https://<your-karaoke-domain>/access-restricted;
    }
    ```

### Indirizzo del client dietro un reverse proxy {#client-address-behind-a-reverse-proxy}

Uvicorn usa `X-Forwarded-For` e `X-Forwarded-Proto` solo se il peer che si connette direttamente è attendibile. Imposta `FORWARDED_ALLOW_IPS` sull'indirizzo IP del proxy visto dall'applicazione, oppure su un intervallo CIDR che includa solo proxy attendibili. Il valore predefinito è `127.0.0.1`, che in genere non corrisponde a un proxy Docker o remoto. Uvicorn non usa `X-Real-IP`. In Docker Compose, aggiungi l'impostazione al file `.env` dell'app; per l'avvio locale, esportala oppure passa `--forwarded-allow-ips` a Uvicorn. Consulta il [riferimento alle variabili d'ambiente](../configuration/environments.md#server-and-routing). Mantieni privata la porta dell'app o limita l'accesso al proxy; non usare mai `*` se client non attendibili possono raggiungere direttamente l'app.

??? note "Configurazione di Caddy"

    Sostituisci il nome host, l'indirizzo dell'applicazione e la sottorete domestica con i tuoi valori. L'esempio consente a tutti di raggiungere `/access-restricted`, limita il resto dell'app alla sottorete domestica e reindirizza gli altri client alla pagina di accesso limitato.

    ```caddyfile
    your-karaoke-domain.example {
        route {
            @access_restricted path /access-restricted*
            handle @access_restricted {
                reverse_proxy http://<your-application-ip>:<your-application-port>
            }

            @outside_home not remote_ip 192.168.0.0/24
            handle @outside_home {
                redir /access-restricted 302
            }

            handle {
                reverse_proxy http://<your-application-ip>:<your-application-port>
            }
        }
    }
    ```

    Il matcher `remote_ip` controlla l'indirizzo sorgente visto da Caddy. Se davanti a Caddy c'è un altro proxy o tunnel, configura i proxy attendibili e verifica che l'indirizzo del client venga conservato prima di affidarti a questo elenco di accesso.

Questa configurazione blocca tutti gli indirizzi IP esterni alla sottorete domestica. Le richieste negate ricevono una risposta `403` e vengono reindirizzate a `/access-restricted`, accessibile a tutti.

### CGNAT e altre alternative di rete {#cgnat-and-other-network-alternatives}

La configurazione descritta sopra richiede sia l'hairpin NAT sia un indirizzo IP pubblico. Se l'operatore usa CGNAT, puoi usare un VPS gratuito, [Tailscale Funnel o Cloudflare Tunnel](#expose-a-remote-demucs-service) per gestire il proxy inverso e il controllo degli accessi. Potrebbe inoltre essere necessario aggiornare l'elenco del proxy inverso quando cambia l'indirizzo IP WAN.

## Esporre un servizio Demucs remoto {#expose-a-remote-demucs-service}

Puoi usare il computer di un amico con una GPU per eseguire Demucs, oppure condividere la tua GPU con lui. L'applicazione karaoke deve poter raggiungere il servizio Demucs remoto; Cloudflare Tunnel e Tailscale sono due possibili soluzioni.

Le prestazioni dipendono molto dalla qualità di entrambe le connessioni Internet. L'elaborazione può rallentare se Tailscale inoltra il traffico tramite un relay. Usa la pagina di test `/transfer` di Demucs per controllare la velocità di upload e download, quindi regola di conseguenza il [limite per la separazione diretta dei file multimediali](../configuration/karaoke-processing.md#separation-direct-media-cutoff-mb).

![Test di velocità Demucs](../assets/images/sysadmin/demucs-speedtest.webp)

### Cloudflare Tunnel {#cloudflare-tunnel}

!!! warning "Proteggi i servizi Demucs esposti pubblicamente"

    Si consiglia vivamente di impostare una chiave API ogni volta che il servizio Demucs è esposto su Internet. Configura la stessa chiave nel [servizio Demucs](../configuration/environments.md#service-paths-and-access) e nell'[applicazione Karaoke](../configuration/karaoke-processing.md#separation-service-api-key), quindi verifica la connessione con [Controlla Demucs](../configuration/tools.md#check-demucs).

Cloudflare Tunnel può essere utilizzato senza registrare un dominio quando è sufficiente un nome host casuale temporaneo. L'utilizzo del proprio dominio richiede un dominio gestito da Cloudflare e una configurazione aggiuntiva del tunnel, vedere [Cloudflare Tunnel con Docker da Homelab Haven](https://homelabhaven.com/posts/remote-access-2-cloudflare-tunnel-docker/).

### Installare `cloudflared` {#install-cloudflared}

Per scaricare i pacchetti e i file binari più recenti, usa la [pagina ufficiale dei download di Cloudflare](https://developers.cloudflare.com/tunnel/downloads/). Gli esempi seguenti installano il connettore; dovrai comunque creare un tunnel con nome e configurarne host e origine in Cloudflare.

=== "Windows"

    1. Scarica il 64-bit MSI o eseguibile dal download di [Pagina dei download di Cloudflare](https://developers.cloudflare.com/tunnel/downloads/).
    2. Se hai scaricato l'eseguibile, rinominalo in `cloudflared.exe` e posizionalo in una directory sul tuo `PATH`, ad esempio `C:\Cloudflared\bin`.
    3. Aprire PowerShell e verificare l'installazione:

        ```powershell
        cloudflared.exe --version
        ```

    Le installazioni di Windows non si aggiornano automaticamente. Scaricare una versione più recente manualmente quando necessario.

=== "macOS"

    Installare con [Homebrew](https://brew.sh/):

    ```bash
    brew install cloudflared
    cloudflared --version
    ```

    Per installare il connettore come servizio macOS dopo aver creato un tunnel denominato, utilizzare `cloudflared service install` per un agente di accesso o `sudo cloudflared service install` per un demone di lancio.

=== "Linux"

    Su Debian o Ubuntu, installa dal repository dei pacchetti di Cloudflare:

    ```bash
    sudo mkdir -p --mode=0755 /usr/share/keyrings
    curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg \
      | sudo tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null
    echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared any main" \
      | sudo tee /etc/apt/sources.list.d/cloudflared.list
    sudo apt-get update
    sudo apt-get install cloudflared
    cloudflared --version
    ```

    Per le distribuzioni basate su RPM, Arch Linux o i download binari diretti, utilizzare le istruzioni di download [Istruzioni per il download ufficiale](https://developers.cloudflare.com/tunnel/downloads/). Dopo aver creato un tunnel denominato, installare il servizio systemd con `sudo cloudflared service install`.

=== "Docker"

    Tirare l'immagine ufficiale e verificarlo:

    ```bash
    docker pull cloudflare/cloudflared:latest
    docker run --rm cloudflare/cloudflared:latest version
    ```

    Per un tunnel gestito in remoto, copiare il comando di installazione dalla dashboard di Cloudflare. Un esempio di composizione basato su token è:

    ```yaml
    services:
      cloudflared:
        image: cloudflare/cloudflared:latest
        restart: unless-stopped
        command: tunnel --no-autoupdate run --token ${TUNNEL_TOKEN}
    ```

### Creare ed eseguire il tunnel {#create-and-run-the-tunnel}

1. Nella dashboard di Cloudflare, apri **Networking  Tunnels**, crea un tunnel e scegli `cloudflared`.
2. Aggiungi un nome host pubblico e puntalo all'origine locale, ad esempio `http://<your-application-ip>:8000` per l'applicazione karaoke o `http://<your-demucs-ip>:8001` per Demucs.
3. Eseguire il connettore, il comando è:

    ```bash
    cloudflared tunnel --no-autoupdate run --token <TUNNEL_TOKEN>
    ```

4. Proteggere l'endpoint Demucs con una chiave API e configurare la stessa chiave in entrambi i servizi.

??? note "I Quick Tunnel sono solo per i test"

    Un tunnel veloce crea un hostname `trycloudflare.com` casuale senza richiedere un account Cloudflare. È utile per un breve test di connettività, ma non è adatto per la produzione karaoke: Cloudflare documenta un limite di concorrenza di 200 richieste e nessun supporto per eventi inviati dal server.

    ```bash
    cloudflared tunnel --url http://localhost:<port>
    ```

!!! warning "Limite di caricamento del CDN Cloudflare"

    Il CDN di Cloudflare limita a **100 MB** la dimensione dei file caricati. Imposta il [limite per la separazione diretta dei file multimediali](../configuration/karaoke-processing.md#separation-direct-media-cutoff-mb) a `100` MB o meno; altrimenti l'elaborazione dei video più grandi non riuscirà.

### Tailscale {#tailscale}

Tailscale crea una rete mesh tra l'applicazione karaoke e il servizio Demucs remoto. Se il computer che ospita Demucs appartiene a un'altra persona, puoi [condividere la tua macchina](https://tailscale.com/docs/features/sharing) con quell'utente.

1. Installare [Tailscale](https://tailscale.com/download) su entrambe le macchine.
2. Accedi allo stesso account o unisciti alla stessa rete.
3. Trova l'indirizzo IP di Tailscale del servizio Demucs nella console di amministrazione o con `tailscale status`.
4. Impostare il servizio di separazione [URL del servizio di separazione](../configuration/karaoke-processing.md#separation-service-url) sull'indirizzo IP di Tailscale e sulla porta del servizio Demucs.
5. Prova la connessione ed esegui il test di velocità Demucs `/transfer`.

Tailscale funziona al meglio con una connessione diretta. Consulta la guida ai [tipi di connessione](https://tailscale.com/docs/reference/connection-types#home-and-small-office-networks) e la [guida al firewall](https://tailscale.com/docs/integrations/firewalls#firewall-compatibility-and-workarounds) per migliorare la connettività diretta.

## Utilizzare un server proxy per i download {#use-a-proxy-server-for-downloads}

L'applicazione supporta un server [server proxy](../configuration/downloads.md#yt-dlp-proxy-url) per il download di video da YouTube. Un proxy può aiutare quando una rete domestica è temporaneamente bloccata o limitata da YouTube.

Per un routing più avanzato, utilizzare una dashboard come [Mihomo con metacubexd](https://github.com/metacubex/metacubexd) per aggiungere, gestire e passare tra più server proxy.

![dashboard Mihomo](../assets/images/sysadmin/mihomo-dashboard.webp)

### Semplice proxy SOCKS5 {#simple-socks5-proxy}

Cloudflare WARP può essere usato per creare un proxy. L'[immagine Docker di warproxy](https://github.com/kingcc/warproxy) trasforma WARP in un proxy SOCKS5 per il traffico in uscita.

Testare un proxy SOCKS5 locale con:

```bash
curl -4 -x socks5://localhost:1080 https://ifconfig.me
# A Cloudflare WARP proxy should return a 104.x.x.x IPv4 address.
```

Per instradare un endpoint VPN WireGuard o OpenVPN tramite un proxy SOCKS5, consulta [Gluetun](https://github.com/passteque/gluetun) e la relativa [configurazione del proxy HTTP](https://github.com/qdm12/gluetun-wiki/blob/main/setup/options/http-proxy.md).

### Mihomo e metacubexd {#mihomo-and-metacubexd}

L'applicazione attualmente supporta un server proxy configurato e non fornisce la commutazione del proxy o il bilanciamento del carico. Mihomo può fornire tali funzionalità senza richiedere l'impostazione del proxy dell'applicazione karaoke per cambiare.

Configurare Mihomo con un proxy HTTP in entrata, quindi aggiungere e gestire più server proxy upstream attraverso la sua dashboard.

??? note "Configurazione di base di Mihomo e metacubexd"

    Esempio di configurazione Docker Compose:

    ```yaml
    services:
      metacubexd:
        image: ghcr.io/metacubex/metacubexd-server:latest
        restart: unless-stopped
        environment:
          CONTROL_TOKEN: change-me-control
          CLASH_SECRET: change-me-clash
          CONTROL_PORT: "8080"
          CLASH_API_PORT: "9090"
          MIXED_PORT: "7890"
          TZ: Asia/Shanghai
        ports:
          - "8080:8080" # Dashboard UI and control agent API.
          - "9090:9090" # Mihomo Clash API and WebSocket.
          - "7890:7890" # Mixed proxy port.
        volumes:
          - metacubexd-data:/data

    volumes:
      metacubexd-data: {}
    ```

    Consulta la [documentazione di configurazione di Mihomo](https://wiki.metacubex.one/en/config/) e aggiungi i server proxy. Ecco un esempio di base:

    ```yaml
    external-controller: 0.0.0.0:9090
    secret: change-me-clash
    mixed-port: 7890
    allow-lan: true
    mode: rule
    log-level: info
    ipv6: false
    proxies:
      - name: Proxy1
    ```

    Apri `http://<your-server-ip>:8080` e usa `change-me-clash` per accedere alla dashboard. Prima di usare questa configurazione, sostituisci tutti i segreti di esempio.

Dopo aver configurato il proxy, passare a un altro server proxy dalla dashboard metacubexd se un download yt-dlp non riesce. L'impostazione proxy dell'applicazione karaoke non ha bisogno di cambiare.

## Monitoraggio del server {#server-monitoring}

Per individuare problemi o errori di configurazione nell'applicazione principale, controlla i log.

Se si esegue l'applicazione con `uvicorn` in primo piano o in una sessione tmux, i suoi registri vengono visualizzati nel terminale.

Per Linux con systemd, utilizzare:

```bash
sudo journalctl -u your-karaoke-service-name -f
```

Per Docker, utilizzare:

```bash
docker logs -f your-karaoke-container-name
# Or: docker compose logs -f
```

L'applicazione registra anche i comandi `yt-dlp` usati per i download, utili per individuarne le cause di errore.

Per il servizio Demucs, nel terminale vengono visualizzati i log Demucs e WhisperX pertinenti. Ad esempio:

```text
2026-09-12 14:43:34 - whisperx.asr - INFO - No language specified, language will be detected for each audio file (increases inference time)
2026-09-12 14:43:34 - whisperx.vads.pyannote - INFO - Performing voice activity detection using Pyannote..
2026-09-12 14:43:39 - whisperx.asr - INFO - Detected language: en (0.88) in first 30s of audio
started inference
Inference time for segment 0: 0.78 seconds
```

Il servizio Demucs espone anche un endpoint `/metrics` che è possibile monitorare con uno strumento esterno.

```json
{"service":"demucs","snapshot_at":"2026-09-12T21:58:47.346862+00:00","active_job_count":0,"running_job_count":0,"active_job_counts_by_status":{},"active_job_counts_by_kind":{},"free_vram_bytes":11013193728,"total_vram_bytes":12878086144,"last_gc_at":"2026-09-12T21:48:58.140879+00:00","last_gc_mode":"full","last_gc_detail":"Released WhisperX caches and CUDA memory","active_jobs":[]}
```

Su Windows, il modo migliore per monitorare l'applicazione o eseguire il debug dei lavori bloccati è utilizzare Task Manager e controllare il grafico GPU per l'utilizzo di VRAM e GPU.

![Task Manager di Windows che mostra l'utilizzo della GPU](../assets/images/sysadmin/task-manager.webp)

Task Manager non è disponibile su un telefono. In questo caso, utilizzare [RustDesk](https://rustdesk.com/) per accedere in remoto al desktop di Windows.
