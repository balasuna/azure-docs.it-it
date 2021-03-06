---
title: Analisi dell&quot;utilizzo con Azure Application Insights | Documentazione Microsoft
description: Panoramica dell&quot;analisi dell&quot;uso con Application Insights
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: b633e0d5-a6e5-4ab7-9c72-499e5d7571ce
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 12/02/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 75b651bd3e77ac19e22dcc3442870469fe2aaca1
ms.openlocfilehash: 94b619cfce15f93bc50f10266c6dc21da78e3481


---
# <a name="usage-analysis-with-application-insights"></a>Uso dell'analisi con Application Insights
La conoscenza della modalità d'uso dell'applicazione consente di concentrare l'attenzione sugli scenari più importanti e ottenere informazioni sugli obiettivi più facili o più difficili da raggiungere per gli utenti.

Application Insights può offrire una visione chiara dell'uso dell'applicazione, consentendo di migliorare l'esperienza degli utenti e soddisfare gli obiettivi aziendali.

Application Insights funziona per entrambe le applicazioni autonome (in iOS, Android e Windows) e per le applicazioni web (ospitate su .NET o J2EE). 

## <a name="add-application-insights-to-your-project"></a>Aggiunta di Application Insights al progetto
Per iniziare, ottenere un account in [Microsoft Azure](https://azure.com). Si paga solo per le risorse usate ed entro certi limiti non sono previsti addebiti per l'uso sperimentale.

Nel [portale di Azure](https://portal.azure.com)creare una risorsa di Application Insights. che mostra i dati sulle prestazioni e l'utilizzo dell'app.

![In Azure, fare clic su Nuovo, Servizi per gli sviluppatori, Application Insights](./media/app-insights-overview-usage/01-create.png)

**Se l'app è un'app per dispositivo,** aggiungere Application Insights SDK al progetto. L'esatta procedura varia a seconda del [IDE e piattaforma](app-insights-platforms.md). Per le app di Windows, è sufficiente fare clic sul progetto in Visual Studio e scegliere "Aggiungi Application Insights".

**In caso di un'app web,** aprire il pannello di Avvio Rapido e ottenere il frammento di codice da aggiungere alle pagine web. Pubblicarli con questo frammento di codice.

![Scegliere Avvio rapido, quindi ottenere il codice per monitorare le pagine Web. Copiare lo script nell'intestazione della pagina master web.](./media/app-insights-overview-usage/02-monitor-web-page.png)

È inoltre possibile aggiungere Application Insights per il codice server [ASP.NET](app-insights-asp-net.md) o [J2EE](app-insights-java-get-started.md) in modo da combinare dati di telemetria sia dal client che dal server.

### <a name="run-your-project-and-see-first-results"></a>Eseguire il progetto e vedere i primi risultati
Eseguire il progetto in modalità di debug per alcuni minuti e quindi eseguire il [portale Azure](https://portal.azure.com) e passare alla risorsa di progetto in Application Insights.

![In Azure, fare clic su Sfoglia, Application Insights e selezionare il progetto](./media/app-insights-overview-usage/00-start.png)

Pubblicare l'app per ottenere ulteriori dati di telemetria e scoprire cosa fanno gli utenti con l'applicazione.

## <a name="analytics-out-of-the-box"></a>Analisi predefinita
Fare clic sul riquadro di visualizzazioni pagina per visualizzare i dettagli di utilizzo.

![In Azure, Sfoglia > Application Insights > il nome del progetto e scorrere verso il basso fino al riquadro delle Visualizzazioni Pagina.](./media/app-insights-overview-usage/01-overview.png)

Passare il mouse nella parte vuota su un grafico per visualizzare i conteggi in un momento specifico. In caso contrario, i numeri indicano il valore aggregato, ad esempio una media, un totale o un numero di utenti definito durante il periodo.

Nelle applicazioni Web gli utenti vengono conteggiati usando i cookie. Una persona che usa diversi browser, cancella i cookie o usa la funzionalità di privacy verrà conteggiata più volte.

Una sessione Web viene conteggiata dopo 30 minuti di inattività. Una sessione su un telefono o un altro dispositivo viene conteggiata quando l'app viene sospesa per più di pochi secondi.

Fare clic su qualsiasi grafico per visualizzare altri dettagli. ad esempio:

![Nel pannello Panoramica fare clic sul grafico Sessioni](./media/app-insights-overview-usage/02-sessions.png)

Questo esempio è tratto da un sito Web, ma i grafici sono simili per le app in esecuzione sui dispositivi.

Confrontare con la settimana precedente per verificare se vi sono cambiamenti:

![Selezionare un grafico che mostra una sola metrica, passare a Settimana precedente](./media/app-insights-overview-usage/021-prior.png)

Confrontare due metriche, ad esempio utenti e nuovi utenti:

![Selezionare un grafico, cercare e selezionare o deselezionare le metriche.](./media/app-insights-overview-usage/031-dual.png)

Raggruppare (segmentare) i dati in base a una proprietà come browser, sistema operativo o città:

![Selezionare un grafico che mostra una sola metrica, passare su Raggruppamento e scegliere una proprietà](./media/app-insights-overview-usage/03-browsers.png)

## <a name="page-usage"></a>Uso delle pagine
Fare clic su visualizzazioni di pagina affiancata per ottenere un una suddivisione delle pagine più diffuse:

![Nel pannello Panoramica fare clic sul grafico delle visualizzazioni Pagina](./media/app-insights-overview-usage/05-games.png)

L'esempio precedente è tratto da un sito Web di giochi. Da quest'ultimo è immediatamente evidente quanto segue:

* L'utilizzo non è migliorato nell'ultima settimana. Potrebbe essere utile ottimizzare il motore di ricerca.
* Le pagine di giochi sono visualizzate da molte meno persone rispetto alla home page. Occorre capire perché la home page non invoglia gli utenti a giocare.
* "Crossword" è il gioco più popolare. È consigliabile dare la priorità a nuove idee e a miglioramenti in questo ambito.

## <a name="custom-tracking"></a>Rilevamento personalizzato
Si supponga che, invece di implementare ogni gioco in una pagina Web separata, si decida di eseguire il refactoring di tutti i giochi nella stessa app a singola pagina con la maggior parte delle funzionalità codificate come Javascript nella pagina Web. Ciò consente all'utente di passare rapidamente da un gioco all'altro o di avere a disposizione diversi giochi in un'unica pagina.

Tuttavia, si vuole comunque che Application Insights registri il numero di volte in cui ogni gioco viene aperto, esattamente come se fossero in pagine Web separate. La risposta è semplice: è sufficiente inserire una chiamata al modulo di telemetria nel codice JavaScript in cui si vuole registrare l'apertura di una nuova 'pagina':

    telemetryClient.trackPageView(game.Name);

## <a name="custom-events"></a>Eventi personalizzati
È possibile usare i dati di telemetria in molti modi per comprendere l'uso dell'applicazione. Ma non sempre si desidera combinare i messaggi con le visualizzazioni pagina. Usare invece gli eventi personalizzati. È possibile inviarli da app per dispositivi, pagine Web o un server Web:

(JavaScript)

    telemetryClient.trackEvent("GameEnd");

(C#)

    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("GameEnd");

(VB)

    Dim tc = New Microsoft.ApplicationInsights.TelemetryClient()
    tc.TrackEvent("GameEnd")


Gli eventi personalizzati più frequenti sono elencati nel pannello Panoramica.

![Nel pannello Panoramica scorrere verso il basso e fare clic su Eventi personalizzati.](./media/app-insights-overview-usage/04-events.png)

Fare clic sull'intestazione della tabella per visualizzare il numero totale di eventi. È possibile segmentare il grafico in base a vari attributi, ad esempio il nome dell'evento:

![Selezionare un grafico che mostra solo una metrica. Attivare il Raggruppamento. Scegliere una proprietà. Non tutte le proprietà sono disponibili.](./media/app-insights-overview-usage/06-eventsSegment.png)

Le sequenze temporali, una funzionalità particolarmente utile, consentono di correlare le modifiche con altre metriche ed eventi. Ad esempio, nei momenti in cui vengono usati più giochi, ci si aspetterebbe anche un incremento dei giochi abbandonati. Ma se l'aumento dei giochi abbandonati è sproporzionato, è necessario verificare se il carico elevato causa problemi ritenuti inaccettabili dagli utenti.

## <a name="drill-into-specific-events"></a>Esaminare eventi specifici
Per una migliore comprensione dell'andamento di una sessione tipica, è possibile concentrarsi su una sessione utente specifica che contiene un particolare tipo di evento.

In questo esempio viene codificato un evento personalizzato "NoGame" che viene chiamato se l'utente si disconnette senza effettivamente iniziare un gioco. Occorre capire perché un utente si comporta in questo modo. Forse se si esaminano alcune occorrenze specifiche, si otterranno delle indicazioni.

Gli eventi personalizzati ricevuti dall'app sono elencati per nome nel pannello Panoramica:

![Nel pannello Panoramica fare clic su uno dei tipi di evento personalizzato.](./media/app-insights-overview-usage/07-clickEvent.png)

Fare clic nell'evento di interesse e quindi selezionare un'occorrenza specifica recente:

![Nell'elenco sotto il grafico di riepilogo fare clic su un evento](./media/app-insights-overview-usage/08-searchEvents.png)

Esaminare i dati di telemetria relativi alla sessione in cui si è verificato l'evento NoGame specifico.

![Fare clic su 'tutti i dati di telemetria per sessione'](./media/app-insights-overview-usage/09-relatedTelemetry.png)

Non si sono verificate eccezioni, quindi non sono stati eventuali errori a impedire all'utente di giocare.

È possibile escludere tramite filtro tutti i tipi di dati di telemetria, ad eccezione delle visualizzazioni pagina per la sessione corrente:

![](./media/app-insights-overview-usage/10-filter.png)

Come si può vedere ora, questo utente si è connesso semplicemente per controllare i punteggi più recenti. Potrebbe essere utile sviluppare una storia utente che semplifichi questa operazione. È anche consigliabile implementare un evento personalizzato per segnalare quando si verifica questa storia specifica.

## <a name="filter-search-and-segment-your-data-with-properties"></a>Filtrare, cercare e segmentare i dati con proprietà
È possibile allegare tag arbitrari e valori numerici agli eventi.

JavaScript nel client

    appInsights.trackEvent("WinGame",
        // String properties:
        {Game: currentGame.name, Difficulty: currentGame.difficulty},
        // Numeric measurements:
        {Score: currentGame.score, Opponents: currentGame.opponentCount}
    );

C# nel server

    // Set up some properties:
    var properties = new Dictionary <string, string>
        {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var measurements = new Dictionary <string, double>
        {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send the event:
    telemetry.TrackEvent("WinGame", properties, measurements);

VB nel server

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim measurements = New Dictionary (Of String, Double)
    measurements.Add("Score", currentGame.Score)
    measurements.Add("Opponents", currentGame.OpponentCount)

    ' Send the event:
    telemetry.TrackEvent("WinGame", properties, measurements)

Allegare le proprietà alle visualizzazioni pagine nello stesso modo:

JavaScript nel client

    appInsights.trackPageView("Win",
        {Game: currentGame.Name},
        {Score: currentGame.Score});

Nella Ricerca diagnostica è possibile visualizzare le proprietà facendo clic su una singola occorrenza di un evento.

![Nell'elenco di eventi aprire un evento e quindi fare clic su '...' per visualizzare altre proprietà](./media/app-insights-overview-usage/11-details.png)

Usare il campo Ricerca per visualizzare le occorrenze di eventi con un valore della proprietà particolare.

![Digitare un valore nel campo Ricerca](./media/app-insights-overview-usage/12-searchEvents.png)

## <a name="a--b-testing"></a>Test A | B
Se non si sa quale variante di una funzionalità sarà più efficace, rilasciarle entrambe, rendendo ognuna accessibile a utenti diversi. Valutare la riuscita di ognuna e quindi passare a una versione unificata.

Per questa tecnica è possibile collegare tag differenti per tutti i dati di telemetria inviati da ogni versione dell'app. A questo scopo, definire le proprietà nel TelemetryContext attivo. Queste proprietà predefinite vengono aggiunte a ogni messaggio di telemetria inviato dall'applicazione, non solo ai messaggi personalizzati, ma anche ai dati di telemetria standard.

Nel portale Application Insights sarà quindi possibile filtrare e raggruppare (segmentare) i dati sui tag, in modo da confrontare versioni diverse.

C# nel server

    using Microsoft.ApplicationInsights.DataContracts;

    var context = new TelemetryContext();
    context.Properties["Game"] = currentGame.Name;
    var telemetry = new TelemetryClient(context);
    // Now all telemetry will automatically be sent with the context property:
    telemetry.TrackEvent("WinGame");

VB nel server

    Dim context = New TelemetryContext
    context.Properties("Game") = currentGame.Name
    Dim telemetry = New TelemetryClient(context)
    ' Now all telemetry will automatically be sent with the context property:
    telemetry.TrackEvent("WinGame")

I singoli dati di telemetria possono sostituire i valori predefiniti.

È possibile impostare un inizializzatore universale in modo che tutti i nuovi TelemetryClient usino automaticamente il contesto.

    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }

Nell'inizializzatore di app, ad esempio Global.asax.cs, procedere come segue:

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }


## <a name="build---measure---learn"></a>Compilare - Misurare - Acquisire informazioni
Quando si usa l'analisi, questa diventa parte integrante del ciclo di sviluppo, non solo un'attività da prendere in considerazione per la risoluzione dei problemi. Di seguito sono riportati alcuni suggerimenti:

* Determinare la metrica chiave dell'applicazione. Stabilire se si vogliono prendere in considerazione tutti gli utenti possibili o è preferibile un piccolo gruppo di utenti molto soddisfatti oppure se si vogliono ottimizzare le visite o le vendite.
* Pianificare la misurazione di ogni storia. Quando si definisce una nuova storia o funzionalità utente oppure si pianifica l'aggiornamento di una storia o una funzionalità esistente, pensare sempre a come si misurerà la riuscita della modifica. Prima di iniziare a scrivere codice, chiedersi quale effetto avrà sulle metriche, se funziona e se si dovrà tenere traccia dei nuovi eventi.
  Naturalmente, quando la funzionalità è live, assicurarsi di esaminare l'analisi e agire in base ai risultati.
* Correlare altre metriche alla metrica chiave. Ad esempio, se si aggiunge una funzionalità 'Preferiti', si vuole sapere con quale frequenza gli utenti aggiungono i preferiti. Tuttavia, forse è più interessante sapere con quale frequenza tornano a consultare i preferiti. E, ancora più importante, è utile sapere se i clienti che usano i preferiti in definitiva effettuano più acquisti relativi al prodotto.
* Effettuare un test. Impostare uno switch di funzionalità che consente di visualizzare una nuova funzionalità solo ad alcuni utenti. Usare Application Insights per verificare se la nuova funzionalità viene usata nel modo previsto. Apportare modifiche, quindi rilasciarla per un pubblico più ampio.
* È essenziale comunicare con gli utenti. L'analisi non è sufficiente da sola, ma è complementare per mantenere una buona relazione con i clienti.

## <a name="learn-more"></a>Altre informazioni
* [Rilevare, valutare e diagnosticare gli arresti anomali e i problemi di prestazione nell'applicazione](app-insights-detect-triage-diagnose.md)
* [Iniziare a utilizzare Application Insights su molte piattaforme](app-insights-detect-triage-diagnose.md)

## <a name="video"></a>Video
> [!VIDEO https://channel9.msdn.com/Series/ConnectOn-Demand/231/player]
> 
> 




<!--HONumber=Dec16_HO1-->


