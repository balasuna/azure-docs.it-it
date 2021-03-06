---
title: "Inoltro automatico di entità di messaggistica del bus di servizio | Documentazione Microsoft"
description: Come concatenare una coda o una sottoscrizione a un&quot;altra coda o argomento.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/10/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 994a379129bffd7457912bc349f240a970aed253
ms.openlocfilehash: cbbd416a065b3284e85957cc024955d11524d3da


---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Concatenamento di entità del bus di servizio con l'inoltro automatico
La funzionalità di *inoltro automatico* consente di concatenare una coda o una sottoscrizione a un'altra coda o a un altro argomento che fa parte dello stesso spazio dei nomi. Quando l'inoltro automatico è abilitato, il bus di servizio rimuove automaticamente i messaggi presenti nella prima coda o sottoscrizione (origine) e li inserisce nella seconda coda o argomento (destinazione). Si noti che è comunque possibile inviare un messaggio direttamente all'entità di destinazione. Tenere presente che non è possibile concatenare una coda secondaria, ad esempio una coda di messaggi non recapitabili, a una coda o a un argomento differente.

## <a name="using-auto-forwarding"></a>Utilizzo dell'inoltro automatico
Per abilitare l'inoltro automatico, è possibile impostare la proprietà [QueueDescription.ForwardTo][QueueDescription.ForwardTo] o [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo] nell'oggetto [QueueDescription][QueueDescription] o [SubscriptionDescription][SubscriptionDescription] per l'origine, come illustrato nell'esempio seguente.

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

L'entità di destinazione deve essere presente al momento della creazione dell'entità di origine. In caso contrario, il bus di servizio restituisce un'eccezione quando gli viene chiesto di creare l'entità di origine. 

È possibile usare l'inoltro automatico per aumentare il numero di istanze di un singolo argomento. Il bus di servizio limita il [numero delle sottoscrizioni per un determinato argomento](service-bus-quotas.md) a 2000. Creando argomenti di secondo livello, è possibile aggiungere altre sottoscrizioni. Si noti che, pur non essendo vincolati dalla limitazione del bus di servizio relativa al numero delle sottoscrizioni, l'aggiunta di un secondo livello di argomenti consente complessivamente di migliorare la velocità effettiva dell'argomento.

![Scenario di inoltro automatico][0]

È possibile usare l'inoltro automatico anche per disaccoppiare i mittenti dei messaggi dai destinatari. Si consideri ad esempio un sistema ERP costituito da tre moduli: Elaborazione ordini, Gestione magazzino e Gestione rapporti con i clienti. Ognuno di questi moduli genera messaggi che vengono accodati in un argomento corrispondente. Due dei rappresentanti di vendita sono interessati a tutti i messaggi correlati ai propri clienti. Per ricevere questi messaggi, i due rappresentanti creano una coda personale e una sottoscrizione per ognuno degli argomenti ERP che inoltrano automaticamente tutti i messaggi alla rispettiva coda.

![Scenario di inoltro automatico][1]

Se uno dei rappresentanti si assenta, viene riempita la coda personale, ma non l'argomento ERP. In questo scenario, poiché un rappresentante di vendita non ha ricevuto alcun messaggio, nessuno degli argomenti ERP raggiunge la quota.

## <a name="auto-forwarding-considerations"></a>Considerazioni sulla funzionalità di inoltro automatico
Se l'entità di destinazione ha accumulato troppi messaggi e supera la quota oppure è disabilitata, l'entità di origine aggiunge i messaggi alla relativa [coda dei messaggi non recapitabili](service-bus-dead-letter-queues.md) finché non si libera spazio nella destinazione o l'entità non viene riabilitata. I messaggi resteranno nella coda dei messaggi non recapitabili, pertanto sarà necessario riceverli ed elaborarli in modo esplicito da tale coda.

Se si concatenano singoli argomenti per ottenere un argomento composito con diverse sottoscrizioni, è consigliabile avere un certo numero di sottoscrizioni nell'argomento di primo livello e un numero più elevato di sottoscrizioni negli argomenti di secondo livello. Ad esempio, un argomento di primo livello con 20 sottoscrizioni, ognuna delle quali concatenata a un argomento di secondo livello con 200 sottoscrizioni, consente una velocità effettiva più alta rispetto a un argomento di primo livello con 200 sottoscrizioni, ognuna delle quali concatenata a un argomento di secondo livello con 20 sottoscrizioni.

Il bus di servizio addebita un'operazione per ogni messaggio inoltrato. Ad esempio, se viene inviato un messaggio a un argomento con 20 sottoscrizioni, ognuna delle quali configurata per l'inoltro automatico dei messaggi a una coda o un argomento differente, vengono addebitate 21 operazioni se tutte le sottoscrizioni di primo livello ricevono una copia del messaggio.

Per creare una sottoscrizione concatenata a una coda o a un argomento diverso, l'autore deve avere le autorizzazioni di **gestione** per l'entità di origine e per quella di destinazione. Per l'invio di messaggi solo all'argomento di origine sono necessarie solo le autorizzazioni di **invio** per l'argomento di origine.

## <a name="next-steps"></a>Passaggi successivi
Per informazioni dettagliate sull'inoltro automatico, vedere gli argomenti di riferimento seguenti:

* [SubscriptionDescription.ForwardTo][SubscriptionDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

Per altre informazioni sui miglioramenti delle prestazioni del bus di servizio, vedere [Code e argomenti partizionati][Partitioned messaging entities].

[QueueDescription.ForwardTo]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md



<!--HONumber=Jan17_HO2-->


