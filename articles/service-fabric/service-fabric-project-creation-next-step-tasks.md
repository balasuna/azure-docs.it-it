---
title: Passaggi successivi per la creazione del progetto di Service Fabric | Documentazione Microsoft
description: "In questo articolo vengono forniti collegamenti a un set di attività di sviluppo principali per Infrastruttura di servizi"
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b4b2424e5efe3392b08e58ceb05ec63f15c7ad32


---
# <a name="your-service-fabric-application-and-next-steps"></a>Applicazione dell'infrastruttura di servizi e fasi successive
L'applicazione Service Fabric di Azure è stata creata. In questo articolo si descrive la costruzione del progetto e alcuni potenziali passaggi successivi.

## <a name="your-application"></a>L'applicazione
Ogni nuova applicazione include un progetto di applicazione. Potrebbero essere presenti uno o due progetti aggiuntivi in base al tipo di servizio scelto.

### <a name="the-application-project"></a>Il progetto dell'applicazione
Il progetto dell'applicazione è composto da:

* Un set di riferimenti ai servizi che costituiscono l'applicazione.
* Tre profili di pubblicazione (locale a 1 nodo, locale a 5 nodi e cloud) che consentono di gestire le preferenze per l'uso in ambienti diversi, ad esempio relative a un endpoint del cluster e alla scelta di eseguire o meno distribuzioni di aggiornamento per impostazione predefinita.
* Tre file di parametri dell'applicazione (identici a quelli riportati in precedenza), che consentono di gestire configurazioni dell'applicazione specifiche per ogni ambiente, ad esempio il numero di partizioni da creare per un servizio.
* Uno script di distribuzione che consente di distribuire l'applicazione dalla riga di comando o nell'ambito di una pipeline di integrazione e distribuzione continua automatizzata.
* Il manifesto dell'applicazione, che descrive l'applicazione. Il manifesto è disponibile nella cartella ApplicationPackageRoot.

### <a name="stateless-service"></a>Servizio senza stato
Quando si aggiunge un nuovo servizio senza stato, Visual Studio aggiunge alla soluzione un progetto del servizio, che include un tipo proveniente da `StatelessService`. Il servizio incrementa una variabile locale in un contatore.

### <a name="stateful-service"></a>Servizio con stato
Quando si aggiunge un nuovo servizio con stato, Visual Studio aggiunge alla soluzione un progetto del servizio, che include un tipo proveniente da `StatefulService`. Il servizio incrementa un contatore nel relativo metodo `RunAsync` e archivia il risultato in un `ReliableDictionary`.

### <a name="actor-service"></a>Servizio Actor
Quando si aggiunge un nuovo Reliable Actor, Visual Studio aggiunge due progetti alla soluzione: un progetto attore e un progetto interfaccia.

Il progetto attore fornisce metodi per l'impostazione e il recupero del valore di un contatore che viene mantenuto in modo affidabile all'interno dello stato dell'attore. Il progetto di interfaccia fornisce un'interfaccia che può essere utilizzata da altri servizi per richiamare l'attore.

### <a name="stateless-web-api"></a>API Web senza stato
Il progetto API Web senza stato fornisce un servizio Web di base che è possibile usare per aprire l'applicazione ai client esterni. Per altre informazioni su come è strutturato il progetto, vedere [Introduzione ai servizi API Web di Service Fabric con self-hosting OWIN](service-fabric-reliable-services-communication-webapi.md).

### <a name="aspnet-core"></a>ASP.NET Core
Service Fabric SDK offre lo stesso set di modelli ASP.NET Core disponibile per progetti ASP.NET Core autonomi: modello vuoto, [API Web][aspnet-webapi] e [applicazione Web][aspnet-webapp].

## <a name="next-steps"></a>Passaggi successivi
### <a name="create-an-azure-cluster"></a>Creare un cluster di Azure
LSDK di Infrastruttura di servizi fornisce un cluster locale per lo sviluppo e il test. Per creare un cluster in Azure, vedere [Setting up a Service Fabric cluster from the Azure portal][create-cluster-in-portal] (Configurare un cluster di Service Fabric dal portale di Azure).

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Provare a distribuire in Azure gratuitamente i cluster di terze parti
Se si vuole provare a distribuire e gestire applicazioni in Azure senza configurare i propri cluster, è possibile usare il [servizio gratuito per l'uso di cluster di terze parti](http://aka.ms/tryservicefabric).

### <a name="publish-your-application-to-azure"></a>Pubblicazione dell'applicazione in Azure
È possibile pubblicare l'applicazione direttamente da Visual Studio in un cluster di Azure. Per informazioni, vedere [Publishing your application to Azure][publish-app-to-azure] (Pubblicare l'applicazione in Azure).

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Visualizzare il cluster con Service Fabric Explorer
Service Fabric Explorer offre un modo semplice per la visualizzazione del cluster, tra cui le applicazioni distribuite e il layout fisico. Per altre informazioni, vedere [Visualizzare il cluster con Service Fabric Explorer][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Effettuare il versioning e aggiornare i servizi
Service Fabric consente il controllo indipendente delle versioni e l'aggiornamento di servizi indipendenti in un'applicazione. Per altre informazioni, vedere [Versioning and upgrading your services][app-upgrade-tutorial] (Esecuzione del versioning e dell'aggiornamento dei servizi).

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Configurare l'integrazione continua con Visual Studio Team Services
Per informazioni su come impostare un processo di integrazione continua per l'applicazione di Service Fabric, vedere [Configurare l'integrazione continua per un'applicazione Service Fabric con Visual Studio Team Services][ci-with-vso].

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html



<!--HONumber=Nov16_HO3-->


