---
title: Uso di Application Insights in Visual Studio
description: Diagnostica e analisi delle prestazioni durante il debug e nell&quot;ambiente di produzione.
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 41ce9b0e323c0938b6db98b99d8d687d1ed0f0ef
ms.openlocfilehash: 2334a85402049eb8654039e98cf2c3131a43de62


---
# <a name="working-with-application-insights-in-visual-studio"></a>Uso di Application Insights in Visual Studio
In Visual Studio (2015 e versioni successive) è possibile analizzare le prestazioni e diagnosticare i problemi sia durante il debug che nell'ambiente di produzione con i dati di telemetria di [Azure Application Insights](app-insights-overview.md).

Se non si è ancora installato [Application Insights nell'app](app-insights-asp-net.md), è possibile farlo ora.

## <a name="a-nameruna-debug-your-project"></a><a name="run"></a> Eseguire il debug del progetto
Eseguire l'applicazione con F5 e provarla aprendo pagine diverse per generare alcuni dati di telemetria.

In Visual Studio verrà visualizzato il conteggio degli eventi che sono stati registrati.

![In Visual Studio il pulsante Application Insights viene visualizzato durante il debug.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Fare clic su questo pulsante per aprire la ricerca diagnostica. 

## <a name="diagnostic-search"></a>Ricerca diagnostica
La finestra di ricerca mostra gli eventi che sono stati registrati. Se è stato eseguito l'accesso ad Azure durante la configurazione di Application Insights, sarà possibile cercare gli stessi eventi nel portale.

![Fare clic con il pulsante destro del mouse sul progetto e scegliere Application Insights, Cerca.](./media/app-insights-visual-studio/34.png)

La ricerca di testo libero funziona in tutti i campi degli eventi. Ad esempio, è possibile cercare parte dell'URL di una pagina, il valore di una proprietà, come la città del client, o parole specifiche in un log di traccia.

Fare clic su qualsiasi evento per visualizzarne le proprietà dettagliate.

È anche possibile aprire la scheda Elementi correlati per poter diagnosticare le richieste non riuscite o le eccezioni.

![](./media/app-insights-visual-studio/41.png)

## <a name="diagnostics-hub"></a>Hub diagnostica
In Visual Studio 2015 o versione successiva Hub diagnostica mostra i dati di telemetria del server Application Insights man mano che vengono generati. Questo avviene anche se si è scelto di installare solo l'SDK, senza connetterlo a una risorsa nel portale di Azure.

![Aprire la finestra Strumenti di diagnostica e controllare gli eventi di Application Insights.](./media/app-insights-visual-studio/31.png)

## <a name="exceptions"></a>Eccezioni
Se è stato [configurato il monitoraggio delle eccezioni](app-insights-asp-net-exceptions.md), i report eccezioni verranno visualizzati nella finestra di ricerca. 

Fare clic su un'eccezione per ottenere un'analisi dello stack. Se il codice dell'app è aperto in Visual Studio, è possibile fare clic nell'analisi dello stack per visualizzare la relativa riga del codice.

![Analisi dello stack delle eccezioni](./media/app-insights-visual-studio/17.png)

Nella riga CodeLens sopra ogni metodo viene inoltre visualizzato un conteggio delle eccezioni registrate da Application Insights nelle ultime 24 ore.

![Analisi dello stack delle eccezioni](./media/app-insights-visual-studio/21.png)

## <a name="local-monitoring"></a>Monitoraggio locale
(Da Visual Studio 2015 Update 2) Se l'SDK non è stato configurato per l'invio della telemetria al portale di Application Insights (e quindi non è presente nessuna chiave di strumentazione in ApplicationInsights.config), la finestra di diagnostica visualizzerà la telemetria dalla sessione di debug più recente. 

Questo è consigliabile se è già stata pubblicata una versione precedente dell'app. Si vuole però evitare di combinare la telemetria delle sessioni di debug con la telemetria nel portale di Application Insights dell'app pubblicata.

È utile anche se si vuole eseguire il debug di alcuni [dati di telemetria personalizzati](app-insights-api-custom-events-metrics.md) prima di inviarli al portale.

* *Inizialmente, Application Insights è stato interamente configurato per inviare i dati di telemetria al portale. Ora però si vuole fare in modo che i dati di telemetria vengano visualizzati solo in Visual Studio.*
  
  * Nelle impostazioni della finestra di ricerca è disponibile un'opzione per cercare la diagnostica locale anche se l'app invia la telemetria al portale.
  * Per arrestare l'invio dei dati di telemetria al portale, impostare come commento la riga `<instrumentationkey>...` di ApplicationInsights.config. Quando si è pronti a inviare nuovamente i dati di telemetria al portale, rimuovere il commento.

## <a name="trends"></a>Tendenze
Tendenze è uno strumento che permette di visualizzare il comportamento dell'app nel tempo. 

Scegliere **Esplora tendenze di telemetria** usando il pulsante della barra degli strumenti di Application Insights o la finestra di ricerca di Application Insights. Scegliere una delle cinque query più comuni per iniziare. È possibile analizzare set di dati diversi in base ai tipi di dati di telemetria, agli intervalli di tempo e ad altre proprietà. 

Per trovare le anomalie nei dati, scegliere una delle opzioni relative alle anomalie nell'elenco a discesa del tipo di visualizzazione. Le opzioni di filtro nella parte inferiore della finestra permettono di trovare facilmente subset specifici dei dati di telemetria.

![Tendenze](./media/app-insights-visual-studio/51.png)

[Altre informazioni su Tendenze](app-insights-visual-studio-trends.md).

## <a name="whats-next"></a>Passaggi successivi
|  |  |
| --- | --- |
| **[Aggiungere altri dati](app-insights-asp-net-more.md)**<br/>Monitorare l'utilizzo, la disponibilità, le dipendenze e le eccezioni, integrare le tracce dei framework di registrazione e scrivere telemetria personalizzata. |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| **[Uso del portale Application Insights](app-insights-dashboards.md)**<br/>Dashboard, strumenti avanzati di diagnostica e di analisi, avvisi, mappa attiva delle dipendenze dell'applicazione ed esportazione dei dati di telemetria. |![Visual Studio](./media/app-insights-visual-studio/62.png) |




<!--HONumber=Nov16_HO3-->


