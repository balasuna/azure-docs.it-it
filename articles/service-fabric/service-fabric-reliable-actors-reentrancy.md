---
title: Reentrancy nei microservizi di Azure basati su attori | Documentazione Microsoft
description: Introduzione alla rientranza per Reliable Actors di Service Fabric
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: vturecek
translationtype: Human Translation
ms.sourcegitcommit: 7033955fa9c18b2fa1a28d488ad5268d598de287
ms.openlocfilehash: 7f6ac80472d3e4fab390b9aa69105e19dca7b947


---
# <a name="reliable-actors-reentrancy"></a>Rientranza di Reliable Actors
Per impostazione predefinita, il runtime di Reliable Actors consente la reentrancy basata sul contesto di chiamata logico. Ciò consente agli attori di essere rientranti se si trovano nella stessa catena del contesto di chiamata. Ad esempio, l'attore A invia un messaggio all'attore B che invia un messaggio all'attore C. Durante l'elaborazione del messaggio, se l'attore C chiama l'attore A, il messaggio è rientrante e sarà quindi consentito. Tutti gli altri messaggi che fanno parte di un contesto di chiamata diverso verranno bloccati sull'attore A fino al completamento dell'elaborazione.

Per la reentrancy degli attori sono disponibili due opzioni, definite nell'enumerazione `ActorReentrancyMode` :

* `LogicalCallContext` (comportamento predefinito)
* `Disallowed` : disabilita la reentrancy

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```

La reentrancy può essere configurata nelle impostazioni di `ActorService`durante la registrazione. L'impostazione si applica a tutte le istanze degli attori create nel servizio Actor.

L'esempio seguente illustra un servizio Actor che imposta la modalità di reentrancy su `ActorReentrancyMode.Disallowed`. In questo caso, se un attore invia un messaggio rientrante a un altro attore verrà generata un'eccezione di tipo `FabricException` .

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context, 
                    actorType, () => new Actor1(), 
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}
```

## <a name="next-steps"></a>Passaggi successivi
* [Diagnostica e monitoraggio delle prestazioni per Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
* [Documentazione di riferimento delle API di Actors](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Codice di esempio](https://github.com/Azure/servicefabric-samples)




<!--HONumber=Jan17_HO4-->


