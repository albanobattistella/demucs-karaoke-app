## Dispositivo client del palco {#stage-client}
Il dispositivo usato come schermo dello stage mostra al pubblico i testi e i video karaoke.

Sono consigliati i computer con **sistemi operativi desktop** (Windows, Linux o macOS), soprattutto se collegati a un televisore o a un proiettore. La trasmissione dello schermo da un dispositivo mobile (AirPlay o Samsung DeX) potrebbe funzionare, ma non è stata verificata.

I dispositivi Android sono supportati, ma potrebbe essere necessario configurare il tema dello stage per visualizzarlo bene su uno schermo piccolo.

I dispositivi iOS e iPadOS non supportano tutte le funzioni dello stage. A causa delle limitazioni imposte da Apple ai browser, non possono riprodurre due flussi audio contemporaneamente. Su questi dispositivi non è quindi possibile attivare la traccia vocale: si sente solo la base strumentale.

Inoltre, i browser di alcuni dispositivi Apple meno recenti supportano solo i formati H.264 e AAC. Consulta la [guida per usare iPhone e iPad come schermo dello stage](../tasks/stage-and-branding.md#use-an-iphone-or-ipad-as-a-stage-display).

## Dispositivi client degli ospiti {#guest-clients}
Qualsiasi dispositivo con un browser moderno può essere usato per cercare brani, aggiungerli alla coda e controllare la riproduzione karaoke.

Il dispositivo dell'ospite deve poter raggiungere il server principale, tramite la rete locale o Internet. Se usi l'isolamento dei client Wi-Fi (o una rete ospiti), valuta la configurazione di un proxy inverso e l'attivazione dell'hairpin NAT sul router. La guida all'[amministrazione del server](../tasks/server-administration.md) spiega altre impostazioni di rete.

Per un dispositivo condiviso, va bene un tablet o un portatile. Puoi attivare la modalità kiosk per impedire agli ospiti di uscire dall'app.

[Modalità kiosk su iPad](https://support.apple.com/en-us/111795)

[App kiosk per Android](https://github.com/RushB-fr/freekiosk)
