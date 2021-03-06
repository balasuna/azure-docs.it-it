---
title: Specifica per l&quot;inserimento live di un flusso MP4 frammentato con Servizi multimediali di Azure | Microsoft Docs
description: Questa specifica descrive il protocollo e il formato per l&quot;inserimento di un live streaming basato sul formato MP4 frammentato con Servizi multimediali di Microsoft Azure. Servizi multimediali di Microsoft Azure fornisce un servizio di live streaming che consente ai clienti di trasmettere in streaming eventi live e trasmettere contenuti in tempo reale usando Microsoft Azure come piattaforma cloud. Questo documento illustra inoltre le procedure consigliate per creare meccanismi di inserimento live affidabili e altamente ridondanti.
services: media-services
documentationcenter: 
author: cenkdin
manager: erikre
editor: 
ms.assetid: 43fac263-a5ea-44af-8dd5-cc88e423b4de
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: cenkd;juliako
translationtype: Human Translation
ms.sourcegitcommit: e126076717eac275914cb438ffe14667aad6f7c8
ms.openlocfilehash: 307c9a377fce32c056a54d35f173efd1bafc4df5


---
# <a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Specifica per l'inserimento live di un flusso MP4 frammentato con Servizi multimediali di Azure
Questa specifica descrive il protocollo e il formato per l'inserimento di un live streaming basato sul formato MP4 frammentato con Servizi multimediali di Microsoft Azure. Servizi multimediali di Microsoft Azure fornisce un servizio di live streaming che consente ai clienti di trasmettere in streaming eventi live e trasmettere contenuti in tempo reale usando Microsoft Azure come piattaforma cloud. Questo documento illustra inoltre le procedure consigliate per creare meccanismi di inserimento live affidabili e altamente ridondanti.

## <a name="1-conformance-notation"></a>1. Nota di conformità
Le parole chiave "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY" e "OPTIONAL" in questo documento vanno interpretate come descritto nella specifica RFC 2119.

## <a name="2-service-diagram"></a>2. Diagramma del servizio
Il diagramma seguente illustra l'architettura di alto livello del servizio di live streaming in Servizi multimediali di Microsoft Azure.

1. Il codificatore live invia feed live a canali che sono stati creati e di cui è stato eseguito il provisioning tramite l'SDK di Servizi multimediali di Microsoft Azure.
2. In Servizi multimediali la gestione di tutte le funzionalità di live streaming è affidata a entità Channel, Program e StreamingEndpoint, compresi inserimento, formattazione, DVR cloud, sicurezza, scalabilità e ridondanza.
3. Facoltativamente, i clienti possono scegliere di distribuire un livello di rete CDN tra l'endpoint di streaming e gli endpoint client.
4. Gli endpoint client trasmettono dall'endpoint di streaming tramite protocolli di flusso adattivo HTTP, ad esempio Smooth Streaming, DASH o HLS.

![Immagine1][image1]

## <a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3. Formato del flusso di bit: MP4 frammentato ISO 14496-12
Il formato di trasmissione per il processo di inserimento di un live streaming illustrato in questo documento si basa sullo standard [ISO&14496;-12]. Fare riferimento a [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx) per una spiegazione dettagliata del formato MP4 frammentato e delle estensioni disponibili sia per i file video on demand sia per l'inserimento di uno streaming live.

### <a name="live-ingest-format-definitions"></a>Definizioni del formato di inserimento live
Di seguito sono elencate alcune speciali definizioni di formato applicabili all'inserimento live in Servizi multimediali di Microsoft Azure.

1. "ftyp", LiveServerManifestBox e la casella "moov" devono (MUST) essere inviati con ogni richiesta (HTTP POST).  DEVONO (MUST) essere inviati all'inizio del flusso e ogni qualvolta il codificatore deve riconnettersi per riprendere il processo di inserimento del flusso.  Per altri dettagli, fare riferimento alla sezione 6 di [1].
2. La sezione 3.3.2 di [1] definisce una finestra facoltativa denominata StreamManifestBox per l'inserimento live. A causa della logica di routing del bilanciamento del carico di Microsoft Azure, questa finestra è stata deprecata e non dovrebbe (SHOULD NOT) quindi essere presente durante l'inserimento in Servizi multimediali di Microsoft Azure. Se questa finestra è presente, Servizi multimediali di Azure automaticamente la ignora.
3. L'elemento TrackFragmentExtendedHeaderBox definito nella sezione 3.2.3.2 di [1] deve (MUST) essere presente per ogni segmento.
4. La versione 2 dell'elemento TrackFragmentExtendedHeaderBox dovrebbe (SHOULD) essere usata per generare segmenti multimediali con URL identici in più data center. Il campo di indice del frammento è obbligatorio (REQUIRED) per il failover tra data center di formati di streaming basati su indice, quali Apple HTTP Live Streaming (HLS) e MPEG-DASH basato su indice.  Per abilitare il failover tra data center, l'indice del frammento deve (MUST) essere sincronizzato tra più codificatori e aumentare di un'unità per ogni frammento multimediale successivo, anche in caso di riavvii o errori del codificatore.
5. La sezione 3.3.6 di [1] definisce la casella denominata MovieFragmentRandomAccessBox ("mfra"), che può (MAY) essere inviata al termine dell'operazione di inserimento live per indicare al canale la fine del flusso. A causa della logica di inserimento di Servizi multimediali di Azure, la fine del flusso è deprecata e per l'inserimento live non dovrebbe (SHOULD NOT) essere inviata la casella "mfra". Se viene inviata, Servizi multimediali di Azure automaticamente la ignora. È consigliabile usare la funzione di [reimpostazione del canale](https://docs.microsoft.com/rest/api/media/operations/channel#reset_channels) per reimpostare lo stato del punto di inserimento e l'opzione di [interruzione del programma](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) per terminare una presentazione e un flusso.
6. La durata del frammento MP4 dovrebbe (SHOULD) essere costante, in modo da ridurre le dimensioni dei manifesti client e migliorare l'approccio euristico di download del client tramite l'utilizzo di tag di ripetizione.  La durata può (MAY) variare per compensare frequenze di fotogrammi costituite da valori non interi.
7. La durata del frammento MP4 dovrebbe (SHOULD) essere compresa tra 2 e 6 secondi circa.
8. I timestamp e gli indici (TrackFragmentExtendedHeaderBox fragment_absolute_time e fragment_index) del frammento MP4 dovrebbero (SHOULD) arrivare in ordine crescente.  Sebbene Servizi multimediali di Azure sia resiliente alla duplicazione di frammenti, presenta una capacità molto limitata di riordinare i frammenti in base alla sequenza temporale dei contenuti.

## <a name="4-protocol-format--http"></a>4. Formato del protocollo: HTTP
L'inserimento live basato sul formato ISO MP4 frammentato per Servizi multimediali di Microsoft Azure usa una richiesta HTTP POST standard con esecuzione prolungata per trasmettere al servizio dati multimediali codificati in formato MP4 frammentato. Ogni HTTP POST invia un flusso in bit MP4 frammentato completo ("flusso") iniziando con le caselle di intestazione ("ftyp", "Live Server Manifest Box" e casella "moov") e continuando con una sequenza di frammenti (caselle "moof" e "mdat"). Fare riferimento alla sezione 9.2 di [1] per la sintassi dell'URL relativo alla richiesta HTTP POST. Un esempio di URL POST è: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

### <a name="requirements"></a>Requisiti
Di seguito sono elencati i requisiti dettagliati:

1. Il codificatore dovrebbe (SHOULD) avviare la trasmissione inviando una richiesta HTTP POST con un "corpo" vuoto (lunghezza del contenuto pari a zero) mediante lo stesso URL di inserimento. In questo modo, dovrebbe essere possibile capire più rapidamente se l'endpoint di inserimento live è valido e se è richiesta l'autenticazione o se sono necessarie altre condizioni. Per ogni protocollo HTTP, il server sarà in grado di restituire la risposta HTTP fino a quando non viene ricevuta la richiesta intera, incluso il corpo del POST. Data la natura prolungata dell'evento live, senza questo passaggio il codificatore potrebbe non essere in grado di rilevare eventuali errori fino a quando non viene completato l'invio di tutti i dati.
2. Il codificatore deve (MUST) gestire eventuali errori o problemi di autenticazione in base a quanto emerso in (1). Se in (1) è stata restituita una risposta 200, continua.
3. Il codificatore deve (MUST) avviare una nuova richiesta HTTP POST con il flusso MP4 frammentato.  Il payload deve (MUST) iniziare con le caselle di intestazione, seguite dai frammenti.  Tenere presente che con ogni richiesta devono (MUST) essere inviati "ftyp", “Live Server Manifest Box” e la casella "moov" (in questo ordine), anche nel caso in cui il codificatore debba riconnettersi perché la richiesta precedente è stata interrotta prima della fine del flusso. 
4. Il codificatore deve (MUST) usare la codifica di trasferimento in blocchi per il caricamento poiché non è possibile prevedere la lunghezza dell'intero contenuto dell'evento live.
5. Al termine dell'evento, dopo aver inviato l'ultimo frammento, il codificatore deve (MUST) terminare normalmente la sequenza di messaggi della codifica di trasferimento in blocchi (la maggior parte degli stack client HTTP gestiscono questo aspetto in modo automatico). Il codificatore deve (MUST) attendere che il servizio restituisca il codice di risposta finale e quindi termina la connessione. 
6. Il codificatore non deve (MUST NOT) usare il nome Events() come descritto nella sezione 9.2 di [1] per l'inserimento live in Servizi multimediali di Microsoft Azure.
7. Se la richiesta HTTP POST termina o scade prima della fine del flusso con un errore TCP, il codificatore deve (MUST) inviare una nuova richiesta POST usando una nuova connessione e attenendosi ai requisiti sopra riportati, nonché al requisito aggiuntivo che il codificatore deve (MUST) nuovamente inviare i due precedenti frammenti MP4 per ogni traccia del flusso e riprendere l'attività senza introdurre discontinuità nella sequenza temporale dei contenuti.  L'invio degli ultimi due frammenti MP4 per ogni traccia impedisce eventuali perdite di dati.  In altre parole, se un flusso contiene una traccia audio e una video e la richiesta POST corrente ha esito negativo, il codificatore deve riconnettersi e inviare nuovamente sia gli ultimi due frammenti per la traccia audio, già correttamente inviati in precedenza, sia gli ultimi due frammenti per la traccia video, anch'essi già correttamente inviati in precedenza, in modo da garantire che non vi siano perdite di dati.  Il codificatore deve (MUST) mantenere un buffer "anticipato" di frammenti di contenuti, che invia nuovamente quando si riconnette.

## <a name="5-timescale"></a>5. Scala cronologica
[[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) descrive l'uso della scala cronologica per SmoothStreamingMedia (sezione 2.2.2.1), StreamElement (sezione 2.2.2.3), StreamFragmentElement (2.2.2.6) e LiveSMIL (sezione 2.2.7.3.1). Se non è presente un valore di scala cronologica, il valore predefinito usato è 10.000.000 (10 MHz). Sebbene le specifiche del formato Smooth Streaming non impediscano l'uso di altri valori di scala cronologica, la maggior parte delle implementazioni di codificatori usa questo valore predefinito (10 MHz) per generare dati di inserimento Smooth Streaming. In presenza della funzione di [creazione dinamica dei pacchetti di Azure Media](media-services-dynamic-packaging-overview.md) , è consigliabile usare un valore di scala cronologica di 90 kHz per i flussi video e di 44,1 o 48,1 kHz per i flussi audio. Se vengono usati valori di scala cronologica differenti per flussi diversi, è necessario (MUST) inviare il valore di scala cronologica del livello di flusso. Fare riferimento a [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

## <a name="6-definition-of-stream"></a>6. Definizione di "flusso"
Il "flusso" è l'unità di base delle operazioni nel processo di inserimento live per la composizione di presentazioni live e la gestione di scenari di ridondanza e failover di streaming. Il "flusso" è definito come un singolo flusso in bit MP4 frammentato che può contenere una singola traccia o più tracce. Una presentazione live completa può contenere uno o più flussi a seconda della configurazione del codificatore live. Gli esempi seguenti illustrano varie modalità d'uso dei flussi per comporre una presentazione live completa.

**Esempio:** 

Un cliente desidera creare una presentazione in live streaming che include le seguenti velocità in bit audio/video:

Video: 3000 kbps, 1500 kbps, 750 kbps

Audio: 128 kbps

### <a name="option-1-all-tracks-in-one-stream"></a>Opzione 1: tutte le tracce in un flusso
In questo caso, un unico codificatore genera tutte le tracce audio/video e le raggruppa in un flusso in bit MP4 frammentato, che viene quindi inviato tramite una singola connessione HTTP POST. In questo esempio, è presente un solo flusso per la presentazione live:

![Immagine2][image2]

### <a name="option-2-each-track-in-a-separate-stream"></a>Opzione 2: ogni traccia in un flusso separato
In questo caso, i codificatori si limitano a inserire una traccia in ogni flusso in bit MP4 frammentato e a registrare tutti i flussi con più connessioni HTTP separate. Questa operazione può essere eseguita con uno o più codificatori. Dal punto di vista dell'inserimento live, questa presentazione live è composta da quattro flussi.

![Immagine3][image3]

### <a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Opzione 3: creare un bundle con una traccia audio e la traccia video a velocità in bit più bassa in un flusso
In questo caso, il cliente sceglie di creare un bundle con la traccia audio e la traccia video con la velocità in bit più bassa in un flusso in bit MP4 frammentato e lascia che le altre due tracce video costituiscano un flusso indipendente. 

![Immagine4][image4]

### <a name="summary"></a>Riepilogo
Quanto sopra riportato NON costituisce un elenco completo di tutte le possibili opzioni di inserimento applicabili a questo esempio. In realtà, qualsiasi forma di raggruppamento di tracce in flussi è supportata dal processo di inserimento live. Clienti e fornitori di codificatori possono scegliere le proprie implementazioni in base alla complessità di progettazione, alla capacità del codificatore e a considerazioni sui livelli di ridondanza e failover. È bene notare, tuttavia, che nella maggior parte dei casi è presente una sola traccia audio per l'intera presentazione live ed è quindi molto importante verificare l'integrità del flusso di inserimento che contiene la traccia audio. Questa considerazione determina spesso la scelta di inserire la traccia audio in un flusso indipendente (come nell'opzione 2) o di aggregarla in bundle con la traccia video con velocità in bit più bassa (come nell'opzione 3). Anche per ottenere un miglior grado di ridondanza e tolleranza di errore, l'invio della stessa traccia audio in due flussi diversi (opzione 2 con tracce audio ridondanti) o l'aggregazione in bundle della traccia audio con almeno due delle tracce video a velocità in bit più bassa (opzione 3 con audio aggregato in bundle in almeno due flussi video) è fortemente consigliato quando si esegue l'inserimento live in Servizi multimediali di Microsoft Azure.

## <a name="7-service-failover"></a>7. Failover del servizio
Data la particolare natura del live streaming, un buon livello di supporto per il failover è essenziale per garantire la disponibilità del servizio. Servizi multimediali di Microsoft Azure è progettato per gestire vari tipi di errori, tra cui errori di rete, errori del server, problemi di archiviazione e così via. Se usato contestualmente a una logica di failover adeguata dal lato del codificatore live, il cliente può ottenere dal cloud un servizio di live streaming estremamente affidabile.

Questa sezione illustra gli scenari di failover del servizio. In questo caso, l'errore si verifica in un punto qualsiasi all'interno del servizio e si manifesta come errore di rete. Di seguito sono riportati alcuni consigli per implementare il codificatore in modo che sia in grado di gestire il failover del servizio:

1. Usare un timeout di 10 secondi per stabilire la connessione TCP.  Se un tentativo di stabilire la connessione richiede più di 10 secondi, interrompere l'operazione e riprovare. 
2. Usare un timeout breve per inviare i blocchi del messaggio di richiesta HTTP.  Se la durata del frammento MP4 di destinazione è di N secondi, usare un timeout di invio compreso tra N e 2N secondi. Ad esempio: usare un timeout compreso tra 6 e 12 secondi se la durata del frammento MP4 è di 6 secondi.  Se si verifica un timeout, reimpostare la connessione, aprire una nuova connessione e riprendere l'inserimento del flusso con la nuova connessione. 
3. Gestire un buffer in sequenza contenente, per ogni traccia, gli ultimi due frammenti completamente inviati al servizio.  Se la richiesta HTTP POST per un flusso viene terminata o scade prima della fine del flusso, aprire una nuova connessione e iniziare un'altra richiesta HTTP POST, quindi inviare nuovamente le intestazioni del flusso e gli ultimi due frammenti per ogni traccia e riprendere il flusso senza introdurre una discontinuità nella sequenza temporale dei contenuti.  In questo modo, il rischio di perdita di dati viene ridotto al minimo.
4. È consigliabile che il codificatore NON ponga un limite al numero di tentativi per ristabilire una connessione o riprendere il flusso dopo che si verifica un errore TCP.
5. Dopo un errore TCP:
   1. È necessario (MUST) chiudere la connessione corrente e creare una nuova connessione per una nuova richiesta HTTP POST.
   2. Il nuovo URL per HTTP POST deve (MUST) corrispondere all'URL relativo al POST iniziale.
   3. Il nuovo HTTP POST deve (MUST) includere intestazioni di flusso ("ftyp", “Live Server Manifest Box” e la casella "moov") identiche alle intestazioni di flusso del POST iniziale.
   4. Gli ultimi due frammenti inviati per ogni traccia devono (MUST) essere nuovamente inviati e lo streaming deve essere ripristinato senza introdurre una discontinuità nella sequenza temporale dei contenuti.  I timestamp del frammento MP4 devono aumentare costantemente, anche con richieste HTTP POST diverse.
6. Se i dati non vengono inviati a una velocità adeguata alla durata del frammento MP4, il codificatore dovrebbe (SHOULD) terminare la richiesta HTTP POST.  Una richiesta HTTP POST che non invia dati può impedire a Servizi multimediali di Azure di disconnettersi rapidamente dal codificatore in caso di aggiornamento del servizio.  Per questo motivo, la richiesta HTTP POST per tracce di tipo sparse (segnale dell'annuncio) dovrebbe (SHOULD) avere una durata breve e terminare non appena viene inviato il frammento di tipo sparse.

## <a name="8-encoder-failover"></a>8. Failover del codificatore
Il failover del codificatore è il secondo tipo di scenario di failover che deve essere affrontato per la distribuzione di un live streaming end-to-end. In questo scenario, la condizione di errore si è verificata sul lato del codificatore. 

![Immagine5][image5]

Di seguito è descritto il comportamento previsto in corrispondenza dell'endpoint di inserimento live quando si verifica il failover del codificatore:

1. Deve (MUST) essere creata una nuova istanza del codificatore per poter continuare lo streaming, come illustrato nel diagramma precedente (flusso per video a 3000 K con linea tratteggiata).
2. Il nuovo codificatore deve (MUST) usare per le richieste HTTP POST lo stesso URL dell'istanza non riuscita.
3. La richiesta POST del nuovo codificatore deve (MUST) includere le stesse caselle di intestazione del flusso MP4 frammentato presenti nell'istanza non riuscita.
4. Il nuovo codificatore deve (MUST) essere correttamente sincronizzato con tutti gli altri codificatori in esecuzione per la stessa presentazione live, in modo da generare esempi audio/video sincronizzati con i limiti di frammento allineati.
5. Il nuovo flusso deve (MUST) essere semanticamente equivalente al flusso precedente e intercambiabile a livello di intestazione e di frammento.
6. Il nuovo codificatore deve (MUST) tentare di ridurre al minimo la perdita di dati.  I valori fragment_absolute_time e fragment_index dei frammenti multimediali dovrebbero (SHOULD) aumentare progressivamente dal punto in cui si è interrotto il codificatore.  I valori fragment_absolute_time e fragment_index dovrebbero (SHOULD) aumentare in modo costante ma, se necessario, è possibile introdurre una discontinuità.  Poiché Servizi multimediali di Azure ignora i frammenti già ricevuti ed elaborati, è preferibile sbagliare inviando nuovamente frammenti già ricevuti piuttosto che introdurre discontinuità nella sequenza temporale dei contenuti. 

## <a name="9-encoder-redundancy"></a>9. Ridondanza del codificatore
In caso di eventi live critici per i quali sono richiesti livelli molto elevati di disponibilità e qualità di esperienza, è consigliabile usare codificatori ridondanti di tipo attivo-attivo in modo da conseguire un failover efficiente senza perdita di dati.

![Immagine6][image6]

Come illustrato nel diagramma precedente, due gruppi di codificatori inviano contemporaneamente al servizio live due copie di ogni flusso. Questa impostazione è supportata grazie alla capacità di Servizi multimediali di Microsoft Azure di escludere i frammenti duplicati in base all'ID del flusso e al timestamp del frammento. L'archivio e il flusso live risultanti saranno costituiti da una singola copia di tutti i flussi, ovvero dalla migliore aggregazione possibile a partire dalle due origini. Ad esempio, in un ipotetico caso estremo, fino a quando per ogni flusso sarà sempre presente un codificatore (non deve essere necessariamente lo stesso) in esecuzione, il flusso live risultante dal servizio sarà continuo, senza perdita di dati. 

I requisiti per questo scenario sono molto simili ai requisiti descritti per il failover del codificatore, tranne per il fatto che il secondo set di codificatori viene eseguito contemporaneamente ai codificatori primari.

## <a name="10-service-redundancy"></a>10. Ridondanza del servizio
Per una distribuzione globale altamente ridondante, può essere necessario eseguire un backup pertinente a più aree geografiche per poter gestire situazioni di emergenza nelle aree interessate. Estendendo la topologia descritta in "Ridondanza del codificatore", i clienti possono scegliere di distribuire il servizio ridondante in un'area diversa, correlata al secondo set di codificatori. I clienti possono anche usare un provider di rete CDN per distribuire un GTM (Global Traffic Manager) davanti alle due distribuzioni del servizio, in modo da indirizzare correttamente il traffico del client. I requisiti per i codificatori sono identici a quelli descritti nella sezione "Ridondanza del codificatore", con la sola eccezione che il secondo set di codificatori deve puntare a un diverso endpoint dell'inserimento live. Il diagramma seguente illustra questa configurazione:

![Immagine7][image7]

## <a name="11-special-types-of-ingestion-formats"></a>11. Tipi speciali di formati di inserimento
Questa sezione illustra alcuni tipi speciali di formati di inserimento live appositamente sviluppati per gestire scenari specifici.

### <a name="sparse-track"></a>Traccia di tipo sparse
Quando si trasmette in live streaming una presentazione con un forte coinvolgimento del cliente, è spesso necessario trasmettere eventi sincronizzati o segnali di tipo in-band con i principali dati multimediali. Un esempio di questo scenario è l'inserimento dinamico di annunci live. La segnalazione per questo tipo di eventi è diversa dal normale streaming di flussi audio/video a causa della particolare natura di tipo sparse. In altre parole, i dati di segnalazione non presentano in genere un andamento costante e può essere molto difficile determinare l'intervallo. Il concetto di traccia di tipo sparse è stato sviluppato per consentire l'inserimento e la trasmissione di dati di segnalazione in-band.

Di seguito è illustrata la procedura consigliata per l'inserimento di una traccia di tipo sparse:

1. Creare un flusso in bit MP4 frammentato contenente solo le tracce di tipo sparse, senza tracce audio e video.
2. Nella finestra "Live Server Manifest Box", come definita nella sezione 6 di [1], usare il parametro "parentTrackName" per specificare il nome della traccia padre. Per altri dettagli, fare riferimento alla sezione 4.2.1.2.1.2 di [1].
3. Nella finestra "Live Server Manifest Box", l'opzione manifestOutput deve essere impostata su "true".
4. Data la natura sparse dell'evento di segnalazione, è consigliabile quanto segue:
   1. All'inizio dell'evento live, il codificatore invia le caselle di intestazione iniziali al servizio, in modo che questo possa registrare la traccia di tipo sparse nel manifesto del client.
   2. Il codificatore dovrebbe (SHOULD) terminare la richiesta HTTP POST quando i dati non sono ancora stati inviati.  Una richiesta HTTP POST con esecuzione prolungata che non invia dati può impedire a Servizi multimediali di Azure di disconnettersi rapidamente dal codificatore in caso di aggiornamento del servizio o riavvio del server, poiché il server multimediale risulterà temporaneamente bloccato in un'operazione di ricezione sul socket. 
   3. Durante il periodo in cui i dati di segnalazione non sono disponibili, il codificatore dovrebbe (SHOULD) chiudere la richiesta HTTP POST  e inviare i dati mentre la richiesta POST è ancora attiva. 
   4. Quando si inviano frammenti di tipo sparse, il codificatore può impostare esplicitamente l'intestazione Content-Length, se disponibile.
   5. Quando si inviano frammenti di tipo sparse con una nuova connessione, il codificatore dovrebbe (SHOULD) inviare prima le caselle di intestazione, quindi i nuovi frammenti. Questo consente di gestire casi in cui il failover si verifica tra una connessione e l'altra e la nuova connessione di tipo sparse viene stabilita con un nuovo server che non ha mai visto prima la traccia di tipo sparse.
   6. Il frammento della traccia di tipo sparse viene reso disponibile al client nel momento in cui il frammento della traccia padre corrispondente, uguale o maggiore rispetto al valore timestamp, viene reso disponibile al client. Ad esempio, se il frammento di tipo sparse presenta un timestamp di t = 1000, dopo che il client rileva il timestamp del frammento video (presupponendo che il nome della traccia padre sia "video"), uguale o superiore a 1000, è possibile scaricare il frammento di tipo sparse t = 1000. Tenere presente che il segnale effettivo può essere usato anche per una posizione diversa nella sequenza temporale della presentazione rispetto a quella designata. Nell'esempio precedente, è possibile che il frammento di tipo sparse con t =&1000; disponga di un payload XML che consente di inserire un annuncio in una posizione di pochi secondi successiva.
   7. Il payload del frammento della traccia di tipo sparse può essere di vari formati (ad esempio, XML, testo o binario), a seconda dello scenario. 

### <a name="redundant-audio-track"></a>Traccia audio ridondante
In un tipico scenario di streaming adattivo HTTP (ad esempio Smooth Streaming o DASH), è spesso presente una sola traccia audio nell'intera presentazione. A differenza delle tracce video, in cui il client può scegliere tra più livelli di qualità in condizioni di errore, la traccia audio può costituire un singolo punto di errore se il processo di inserimento del flusso contenente la traccia audio viene interrotto. 

Per risolvere questo problema, Servizi multimediali di Microsoft Azure supporta l'inserimento live di tracce audio ridondanti. L'idea è quella di poter inviare più volte la stessa traccia audio in flussi diversi. Se il servizio da un lato registra la traccia audio una sola volta nel manifesto client, dall'altro è in grado di usare tracce audio ridondanti come backup, in modo da poter recuperare i frammenti audio necessari in caso di problemi nella traccia audio principale. Per poter inserire tracce audio ridondanti, il codificatore deve:

1. Creare la stessa traccia audio in più flussi in bit MP4 frammentati. Le tracce audio ridondanti devono (MUST) essere semanticamente equivalenti ai timestamp degli stessi frammenti e intercambiabili a livello di intestazione e di frammento.
2. Assicurarsi che la voce "audio" nella finestra Live Server Manifest (sezione 6 di [1]) sia uguale per tutte le tracce audio ridondanti.

Di seguito è illustrata la procedura consigliata per l'inserimento di tracce audio ridondanti:

1. Inviare ogni singola traccia audio in un flusso separato.  Inviare inoltre un flusso ridondante per ciascuno di questi flussi di traccia audio, in cui il secondo flusso si differenzi dal primo solo per l'identificatore dell'URL HTTP POST: {protocol}://{server address}/{publishing point path}/Streams({identifier}).
2. Usare flussi distinti per inviare le due velocità in bit video più basse. Ciascuno di questi flussi dovrebbe (SHOULD) inoltre contenere una copia di ogni singola traccia audio.  Se, ad esempio, sono supportate più lingue, i flussi dovrebbero (SHOULD) contenere tracce audio per ogni lingua.
3. Usare istanze del server (codificatore) distinte per codificare e inviare i flussi ridondanti citati in (1) e (2). 

## <a name="media-services-learning-paths"></a>Percorsi di apprendimento di Servizi multimediali
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Fornire commenti e suggerimenti
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png




<!--HONumber=Feb17_HO3-->


