---
title: Monitorare un cluster Azure Kubernetes con DataDog | Documentazione Microsoft
description: Monitoraggio di cluster Kubernetes nel servizio contenitore di Azure con DataDog
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: what-goes-here?
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
translationtype: Human Translation
ms.sourcegitcommit: 0aa9b3ae14f586fc79e6ebee898e794d526c19bd
ms.openlocfilehash: 2984a3811eba670b54fd253a8d7dc84acd8c8ec4


---

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Monitorare un cluster del servizio contenitore di Azure con Datadog

## <a name="prerequisites"></a>Prerequisiti
Si presume che questa procedura dettagliata abbia [creato un cluster Kubernetes mediante il servizio contenitore di Azure](container-service-kubernetes-walkthrough.md).

Si presume anche che gli strumenti dell'interfaccia della riga di comando di Azure `az` e `kubectl` siano installati.

È possibile verificare se lo strumento `az` è installato eseguendo:

```console
$ az --version
```

Se lo strumento `az` non è installato, le istruzioni sono disponibili [qui](https://github.com/azure/azure-cli#installation).

È possibile verificare se lo strumento `kubectl` è installato eseguendo:

```console
$ kubectl version
```

Se `kubectl` non è installato, è possibile eseguire:

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a>DataDog
Datadog è un servizio che raccoglie dati di monitoraggio dai contenitori all'interno del cluster del servizio contenitore di Azure. Datadog è dotato di un dashboard di integrazione Docker in cui è possibile visualizzare metriche specifiche all'interno dei propri contenitori. Le metriche raccolte dai contenitori sono organizzate per CPU, memoria, rete e I/O. Datadog suddivide le metriche in contenitori e immagini.

Prima è necessario [creare un account](https://www.datadoghq.com/lpg/)

## <a name="installing-the-datadog-agent-with-a-daemonset"></a>Installazione dell'agente Datadog con DaemonSet
Gli elementi DaemonSet vengono usati da Kubernetes per eseguire una singola istanza di un contenitore in ogni host del cluster.
Sono ideali per l'esecuzione di agenti di monitoraggio.

Dopo l'accesso a DataDog, è possibile seguire le [istruzioni di DataDog](https://app.datadoghq.com/account/settings#agent/kubernetes) per installare gli agenti DataDog nel cluster usando DaemonSet.

## <a name="conclusion"></a>Conclusioni
L'operazione è terminata. Con gli agenti operativi, i dati verranno visualizzati nella console entro alcuni minuti. È possibile visitare il [dashboard di Kubernetes](https://app.datadoghq.com/screen/integration/kubernetes) integrato per visualizzare un riepilogo del cluster.


<!--HONumber=Jan17_HO4-->


