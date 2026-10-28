# Per iniziare {#getting-started}

DMKaraoke è composto da due servizi.

- [Applicazione principale](#ways-to-deploy): un server web che gestisce la coda, i controlli dello stage e la libreria multimediale.
- [Servizio Demucs](demucs-service.md): un'applicazione separata che esegue WhisperX e Demucs per separare le tracce vocali e generare i testi sincronizzati.

Questa architettura permette di eseguire l'applicazione principale su un server domestico poco potente e il servizio Demucs su un computer più prestante, anche quello di un amico raggiungibile via Internet. I servizi comunicano tramite HTTP, quindi un singolo servizio Demucs può elaborare brani per più server karaoke.

![Architettura di DMKaraoke](../assets/images/architecture.webp)

Per l'applicazione principale si consiglia Docker su un server Linux. Sono supportate anche le installazioni senza Docker, direttamente sull'host o in un container LXC, così come le installazioni su Windows.

## Modalità di installazione {#ways-to-deploy}

L'applicazione principale è un server web FastAPI leggero con un database SQLite. Può funzionare su un server Linux x64 o ARM64, compresi un Raspberry Pi 4 o un vecchio computer da ufficio.

<div class="grid cards" markdown>

-   :material-docker: **Docker**

    Consigliato per i server Linux.

    [:octicons-arrow-right-24: Apri la guida Docker](docker.md)

-   :material-linux: **Linux**

    Eseguire l'applicazione principale senza Docker.

[:octicons-arrow-right-24: Apri la guida per Linux](linux.md)

-   :material-microsoft-windows: **Windows**

    Installare l'applicazione principale o il servizio Demucs su Windows.

[:octicons-arrow-right-24: Apri la guida per Windows](windows.md)

</div>

### Servizio Demucs {#demucs-service}

Il servizio Demucs offre le prestazioni migliori con una GPU NVIDIA compatibile con CUDA. Può funzionare anche senza GPU, ma l'elaborazione sarà più lenta. È possibile eseguirlo su un altro computer.

- [Servizio Demucs](demucs-service.md)

## Considerazioni per l'uso in produzione {#production-considerations}

Esegui periodicamente il backup dei dati dell'applicazione e dei file multimediali e aggiorna il software quando escono nuove versioni. Consulta la guida [Backup, ripristino e aggiornamento](backup-and-restore.md).

La guida all'[amministrazione del server](../tasks/server-administration.md) spiega come usare DMKaraoke in produzione, anche con proxy, reverse proxy, strumenti di monitoraggio e controlli di accesso.
