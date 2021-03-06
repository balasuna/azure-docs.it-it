---
title: Creare un servizio di Ricerca di Azure con il portale di Azure | Documentazione Microsoft
description: Informazioni sul provisioning del servizio di Ricerca di Azure nel portale di Azure.
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: c8c88922-69aa-4099-b817-60f7b54e62df
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 702ea254c19d1f9782f83d8445b7f440f11963b9
ms.openlocfilehash: 5046b3fc1444e37388505f2662a28486342c8822


---
# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Creare un servizio di Ricerca di Azure nel portale di Azure
Questo articolo illustra il processo di creazione (o provisioning) di un servizio Ricerca di Azure nel [portale di Azure](https://portal.azure.com/).

L'articolo presuppone che sia già stata eseguita la sottoscrizione di Azure e che sia possibile accedere al portale di Azure.

## <a name="find-azure-search-in-the-azure-portal"></a>Individuare la Ricerca di Azure nel portale di Azure
1. Aprire il [portale di Azure](https://portal.azure.com/) ed eseguire l’accesso.
2. Fare clic sul segno più ("+") nell'angolo superiore sinistro.
3. Selezionare **Web + Mobile**.
4. Selezionare **Ricerca di Azure**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Scegliere un nome e un endpoint dell'URL per il servizio
1. Il nome del servizio farà parte dell'URL dell'endpoint di servizio di Ricerca di Azure in cui si eseguiranno le chiamate API per gestire e utilizzare il servizio di ricerca.
2. Digitare il nome del servizio nel campo **URL** . Il nome del servizio:
   * deve contenere solo lettere minuscole, cifre o trattini ("-")
   * non può contenere un trattino ("-") nei primi 2 caratteri o nell'ultimo carattere
   * non può contenere trattini consecutivi ("--")
   * è di lunghezza compresa tra 2 e 60 caratteri

## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Selezionare una sottoscrizione in cui mantenere il servizio
Se sono disponibili più sottoscrizioni, è possibile selezionare la sottoscrizione che deve includere il servizio di Ricerca di Azure.

## <a name="select-a-resource-group-for-your-service"></a>Selezionare un gruppo di risorse per il servizio
Creare un nuovo gruppo di risorse o selezionarne uno esistente. Un gruppo di risorse è una raccolta di servizi e risorse di Azure utilizzate insieme. Se ad esempio si utilizza la Ricerca di Azure per l'indicizzazione di un database SQL, entrambi i servizi devono essere parte dello stesso gruppo di risorse.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Selezionare il percorso in cui verrà ospitato il servizio
Ricerca di Azure, in qualità di servizio di Azure, può essere ospitato nei data center di tutto il mondo. Tenere presente che i [prezzi possono variare](https://azure.microsoft.com/pricing/details/search/) in funzione dell'area geografica.

## <a name="select-your-pricing-tier"></a>Selezionare il piano tariffario
[Ricerca di Azure attualmente è disponibile con vari piani tariffari](https://azure.microsoft.com/pricing/details/search/): Gratuito, Basic o Standard. Ogni piano tariffario prevede una specifica [capacità e limiti](search-limits-quotas-capacity.md). Per indicazioni, vedere [Scegliere uno SKU o un piano tariffario per Ricerca di Azure](search-sku-tier.md) .

In questo caso per il servizio è stato scelto il piano tariffario Standard.

## <a name="select-the-create-button-to-provision-your-service"></a>Selezionare il pulsante "Crea" per il provisioning del servizio
![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>Ridimensionare il servizio
Al termine del provisioning del servizio, è possibile ridimensionare il servizio per adattarlo alle proprie esigenze. Poiché è stato scelto il piano tariffario Standard per il servizio Ricerca di Azure, è possibile ridimensionare il servizio in due dimensioni, ovvero partizioni e repliche. Se fosse stato scelto il piano Basic, sarebbe stato possibile aggiungere solo le repliche. Se è stato effettuato il provisioning del servizio Gratuito, la scalabilità non è disponibile.

Le ***partizioni*** consentono di archiviare e di eseguire ricerche in un numero maggiore di documenti nel servizio.

Le ***repliche*** consentono al servizio di gestire un carico superiore di query di ricerca. [Un servizio richiede 2 repliche per ottenere un contratto di servizio di sola lettura e 3 repliche per ottenere un contratto di servizio di lettura/scrittura](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Passare al pannello di gestione del servizio Ricerca di Azure nel portale di Azure.
2. Nel pannello **Impostazioni** selezionare **Ridimensiona**.
3. È possibile ridimensionare il servizio mediante l'aggiunta di repliche o partizioni.
   * Ogni livello di servizio presenta [limiti](search-limits-quotas-capacity.md) diversi per il numero totale di unità di ricerca consentite in un singolo servizio (repliche * partizioni = unità di ricerca totali).

![](./media/search-create-service-portal/scale-service.png)

## <a name="next-steps"></a>Passaggi successivi
Dopo il provisioning di un servizio di Ricerca di Azure, è possibile [definire un indice di Ricerca di Azure](search-what-is-an-index.md) per caricare e cercare i dati.

Per una breve esercitazione, vedere [Introduzione a Ricerca di Azure nel portale](search-get-started-portal.md) .




<!--HONumber=Jan17_HO2-->


