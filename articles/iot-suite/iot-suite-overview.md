---
title: Panoramica di Microsoft Azure IoT Suite | Documentazione Microsoft
description: Panoramica di come Azure IoT Suite offra soluzioni preconfigurate Internet delle cose per raccogliere, analizzare e archiviare dati, fornire visualizzazioni ed eseguire l&quot;integrazione con altri sistemi.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7b0df1fc79e82a99ea65296a4e09bdb48fde9451


---
# <a name="what-is-azure-iot-suite"></a>Che cos'è Azure IoT Suite?
I servizi di Azure IoT (Internet delle cose) offrono una vasta gamma di funzionalità. Questi servizi di livello aziendale consentono di:

* Raccogliere dati dai dispositivi
* Analizzare i flussi dei dati in movimento
* Archiviare ed eseguire query su set di dati di grandi dimensioni
* Visualizzare i dati in tempo reale e cronologici
* Eseguire l'integrazione con i sistemi back-office

Per offrire queste funzionalità, Azure IoT Suite include più servizi di Azure con estensioni personalizzate come *soluzioni preconfigurate*. Queste soluzioni preconfigurate sono implementazioni di base dei modelli di soluzione IoT comuni che contribuiscono a ridurre i tempi di distribuzione delle soluzioni IoT. I [Software Development Kit IoT][lnk-sdks] consentono di personalizzare ed estendere queste soluzioni in base alle proprie esigenze. È anche possibile usare queste soluzioni come esempi o modelli durante lo sviluppo di nuove soluzioni IoT.

Nel video seguente viene fornita un'introduzione ad Azure IoT Suite:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a>Servizi di Azure IoT in Azure IoT Suite
Le soluzioni preconfigurate usano in genere i servizi seguenti:

* Il servizio principale di Azure IoT Suite è [Hub IoT di Azure][lnk-iot-hub]. Questo servizio fornisce funzionalità di messaggistica da dispositivo a cloud e da cloud a dispositivo e funge da gateway per il cloud e gli altri servizi chiave di IoT Suite. Il servizio consente di ricevere messaggi dai dispositivi su vasta scala e inviare i comandi ai dispositivi.
* [Analisi di flusso di Azure][lnk-asa] fornisce l'analisi dei dati in movimento. IoT Suite si basa su questo servizio per elaborare i dati di telemetria in entrata, eseguire operazioni di aggregazione e rilevare gli eventi. Le soluzioni preconfigurate usando anche l'analisi di flusso per elaborare i messaggi informativi che contengono dati, ad esempio i metadati o le risposte ai comandi dai dispositivi. Le soluzioni usano Analisi di flusso per elaborare i messaggi dei dispositivi e inviare i messaggi ad altri servizi.
* [Archiviazione di Azure][lnk-azure-storage] e [Azure DocumentDB][lnk-document-db] forniscono le funzionalità di archiviazione dei dati. Le soluzioni preconfigurate usano l'archivio BLOB per archiviare i dati di telemetria e renderli disponibili per l'analisi. Le soluzioni usano DocumentDB per archiviare i metadati e abilitare le funzionalità di gestione di dispositivi delle soluzioni.
* [App Web di Azure][lnk-web-apps] e [Microsoft Power BI][lnk-power-bi] forniscono le funzionalità di visualizzazione dei dati. La flessibilità di Power BI consente di creare rapidamente i propri dashboard interattivi che usano i dati di IoT Suite.

Per una panoramica dell'architettura di una tipica soluzione IoT, vedere [Microsoft Azure e Internet delle cose (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>soluzioni preconfigurate
IoT Suite include soluzioni preconfigurate che consentono di iniziare rapidamente e di esplorare gli scenari comuni di IoT resi possibili da Azure IoT Suite, come il *monitoraggio remoto* e la *manutenzione predittiva*. È possibile distribuire queste soluzioni nella sottoscrizione di Azure e quindi eseguire uno scenario IoT end-to-end completo.

## <a name="next-steps"></a>Passaggi successivi
Dopo aver acquisito una panoramica di IoT Suite e dei suoi componenti, è possibile ottenere altre informazioni sulle soluzioni preconfigurate di IoT Suite nell'articolo [Informazioni sulle soluzioni preconfigurate di Azure IoT Suite][lnk-what-are-preconfig].

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md



<!--HONumber=Nov16_HO2-->


