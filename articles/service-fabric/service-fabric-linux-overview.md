---
title: Azure Service Fabric in Linux | Documentazione Microsoft
description: I cluster di Service Fabric supportano Linux e Java, per cui le applicazioni Service Fabric scritte in Java e C# possono essere distribuite e ospitate in Linux.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 459afade-145d-4ee6-b72b-ddf380ccd1bf
ms.service: service-fabric
ms.devlang: Java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2016
ms.author: SubramaR
translationtype: Human Translation
ms.sourcegitcommit: c0e2324a2b2e6294df6e502f2e7a0ae36ff94158
ms.openlocfilehash: 175edd2c45319f197d1df65ae22853ca0dc3d310


---
# <a name="service-fabric-on-linux"></a>Service Fabric in Linux
L'anteprima di Service Fabric su Linux, consente di compilare, distribuire e gestire applicazioni a disponibilità e scalabilità elevata in tale ambiente proprio come in Windows. I framework di Service Fabric (Reliable Services e Reliable Actors) sono disponibili in Java su Linux in aggiunta a C# (.NET Core).  È anche possibile compilare [servizi eseguibili guest](service-fabric-deploy-existing-app.md) con qualsiasi linguaggio o framework. L'anteprima, inoltre, supporta contenitori Docker di orchestrazione, che possono eseguire eseguibili guest o servizi nativi di Service Fabric, che usano i framework di Service Fabric.

Concettualmente, Service Fabric in Linux è equivalente a ServiceFabric in Windows (tranne per le specifiche del sistema operativo e il supporto del linguaggio di programmazione). Di conseguenza, la maggior parte della [documentazione esistente](http://aka.ms/servicefabricdocs) è valida e consente di acquisire familiarità con la tecnologia.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
> 
> 

## <a name="supported-operating-systems-and-programming-languages"></a>Sistemi operativi e linguaggi di programmazione supportati
L'anteprima limitata supporta la creazione di cluster di sviluppo con un solo computer, oltre ai cluster con più computer in Azure che eseguono Ubuntu Server 16.04. L'anteprima supporta i framework Reliable Actors e Reliable Services senza stato in Java e C#, oltre a eseguibili guest e contenitori Docker di orchestrazione.  

> [!NOTE]
> Le raccolte Reliable Collections non sono ancora supportate in Linux. Non sono supportati anche i cluster autonomi. Nell'anteprima sono supportati solo cluster con un solo computer e più computer Linux di Azure.
> 
> 


## <a name="supported-tooling"></a>Strumenti supportati
L'anteprima supporta l'interazione con il cluster tramite l'interfaccia della riga di comando di Azure. Per gli sviluppatori Java viene offerta l'integrazione con Eclipse e Yeoman, con supporto di Eclipse in Linux e OSX. Per l'integrazione in OSX viene usata in background una VM Linux tramite Vagrant. Per gli sviluppatori C# viene offerta l'integrazione don Yeoman per la generazione di modelli di applicazione.

## <a name="next-steps"></a>Passaggi successivi
1. Acquisire familiarità con i framework di programmazione [Reliable Actors](service-fabric-reliable-actors-introduction.md) e [Reliable Services](service-fabric-reliable-services-introduction.md).
2. [Preparare l'ambiente di sviluppo in Linux](service-fabric-get-started-linux.md)
3. [Preparare l'ambiente di sviluppo in OSX](service-fabric-get-started-mac.md)
4. [Creare la prima applicazione Java di Service Fabric in Linux](service-fabric-create-your-first-linux-application-with-java.md)




<!--HONumber=Jan17_HO5-->


