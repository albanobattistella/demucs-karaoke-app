# Operazioni karaoke {#karaoke-tasks}

- [Aggiungere alla coda un video karaoke già pronto](#queue-a-premade-karaoke-video)
- [Aggiungere un brano per conto di un altro utente](#queue-a-song-as-another-user)
- [Controllare la coda da remoto](#control-queue-remotely)
- [Usare un proprio brano](#use-your-own-song)

## Aggiungere alla coda un video karaoke già pronto {#queue-a-premade-karaoke-video}

![Coda normale karaoke](../assets/images/tasks/queue-normal-karaoke.webp)

YouTube offre una vasta scelta di video karaoke, anche su canali come [Sing King](https://www.youtube.com/channel/UCwTRjvjVge51X-ILJ4i22ew). Per i brani più popolari è probabile che esista già un video karaoke. È il modo più semplice per iniziare e **non richiede il servizio Demucs**, ma offre meno possibilità di personalizzazione.

- Cerca un brano. In genere basta inserire il titolo e l'artista.
- Puoi anche incollare direttamente nella casella di ricerca un URL di YouTube.

!!! tip "[Ricerca parallela su YouTube](../configuration/downloads.md#parallel-youtube-search)"
    Attiva la ricerca parallela di YouTube per cercare contemporaneamente la query originale e una variante karaoke. Può aiutarti a trovare video karaoke già pronti.

- L'applicazione riconosce automaticamente i video karaoke, quindi non serve elaborarli. Aggiungi il video alla coda e attendi che venga scaricato.
- Al termine del download, il brano compare nella coda e sullo stage.

Se il download non riesce, consulta la [guida alla risoluzione dei problemi di yt-dlp](../troubleshooting/index.md).

## Aggiungere un brano per conto di un altro utente {#queue-a-song-as-another-user}

Di norma, gli ospiti usano i propri dispositivi e possono aggiungere brani alla coda solo a proprio nome. Su un tablet condiviso, un amministratore può accedere e aggiungere brani a nome di altri utenti, così più persone possono usare lo stesso dispositivo.

![Coda come un altro utente](../assets/images/tasks/queueas.webp)

- Attiva l'opzione **Richiedi un nome per la coda**.
- Cerca un brano come al solito.
- Nella schermata di configurazione prima dell'invio, seleziona un utente esistente o inserisci il nome da usare per la coda.

## Controllare la coda da remoto {#control-queue-remotely}

Potresti non avere la tastiera a portata di mano per controllare lo stage. Puoi usare il tuo dispositivo o quello condiviso per controllarlo in tempo reale. Sono disponibili queste opzioni:

![Controllo della fase](../assets/images/queue/control.webp)

!!! note "Controlli per ospiti e amministratori"
    Gli ospiti possono gestire i propri brani in coda; gli amministratori possono aggiungere brani a nome di altri utenti e gestire l'intera coda.

    Gli amministratori possono anche rimuovere o riordinare i brani aggiunti da altri utenti.


- **Pausa/Riproduci**: mette in pausa o riprende il brano corrente.
- **Salta**: passa al brano successivo.
- **Risincronizza**: corregge la desincronizzazione tra la voce e la base strumentale.
- **FF+5**: fa avanzare il brano corrente di cinque secondi.
- **Traccia vocale**: attiva o disattiva la traccia vocale guida (solo per i brani compatibili).
- **Volume vocale**: regola il volume della traccia vocale (solo per i brani compatibili).
- **Stile**: apre le opzioni avanzate di [personalizzazione dei testi](stage-and-branding.md#customize-the-stage-display) (solo per i brani compatibili).

## Usare un proprio brano {#use-your-own-song}

Se hai già scaricato un video karaoke o hai problemi a scaricare contenuti con l'applicazione, puoi caricare i tuoi file nella libreria.

L'applicazione supporta diversi formati multimediali comuni:

- Video: MP4, WEBM, MKV, MOV, AVI, M4V
- Audio: MP3, WAV, M4A, FLAC, AAC, OGG, OPUS, WEBM
- Altro: CDG (formato karaoke legacy), ZIP (pacchetto karaoke esportato dall'applicazione)

<div class="grid cards" markdown>

- __Utilizzando l'applicazione__

    - Apri la pagina Media e fai clic su **Carica**.
    - Seleziona un file multimediale e, se vuoi, inserisci il titolo e l'artista.
    - Per le opzioni avanzate di elaborazione, consulta la guida [Creare karaoke dai file caricati](create-ai-karaoke.md).

- __Esternamente__

    - Copia il file nella cartella `MEDIA_PATH` sull'host.
    - Se il server è remoto, puoi trasferire il file con `SCP/SFTP` (WinSCP/FileZilla) o usare una condivisione di rete `SMB/NFS`.
    - Nella pagina Media, fai clic su **Scansiona libreria** per rilevare i nuovi file.

</div>
