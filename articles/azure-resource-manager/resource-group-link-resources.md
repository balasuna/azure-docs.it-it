---
title: Collegare risorse correlate nelle soluzioni di Azure | Documentazione Microsoft
description: Creare un collegamento tra risorse correlate in gruppi di risorse diversi di Azure Resource Manager.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0738d072-c093-4cf1-a790-de13025d9d60
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 2a9075f4c9f10d05df3b275a39b3629d4ffd095f
ms.openlocfilehash: 2750e971ae281e498e9c505e1cd2790cefbfbc78


---
# <a name="link-related-resources-from-different-resource-groups"></a>Collegare risorse correlate di diversi gruppi di risorse
Durante la distribuzione, è possibile contrassegnare una risorsa come dipendente da un'altra risorsa, ma tale ciclo di vita termina con la distribuzione. Dopo la distribuzione non esistono relazioni identificate tra le risorse dipendenti. Azure Resource Manager offre una funzionalità denominata collegamento delle risorse che consente di stabilire relazioni persistenti tra le risorse.

Con il collegamento delle risorse è possibile documentare relazioni che interessano più gruppi di risorse. Ad esempio, è uno scenario comune che un database con un proprio ciclo di vita si trovi in un gruppo di risorse, mentre un'applicazione con un ciclo di vita diverso si trovi in un gruppo di risorse diverso. L'app si connette al database, perciò è utile definire un collegamento tra l'applicazione e il database. 

Tutte le risorse collegate devono appartenere alla stessa sottoscrizione. Ogni risorsa può essere collegata a 50 altre risorse. L'unico metodo per eseguire query sulle risorse correlate è l'uso dell'API REST. Se una qualsiasi delle risorse collegate viene eliminata o spostata, il proprietario del collegamento deve eliminare il corrispondente collegamento. **Non** si riceve un avviso quando viene eliminata una risorsa collegata ad altre risorse.

## <a name="linking-in-templates"></a>Collegamento nei modelli
Per definire un collegamento in un modello si include un tipo di risorsa che combina lo spazio dei nomi del provider di risorse e il tipo della risorsa di origine con **/providers/links**. Il nome deve includere il nome della risorsa di origine. L'ID risorsa della risorsa di destinazione viene specificato dall'utente. L'esempio seguente stabilisce un collegamento tra un sito Web e un account di archiviazione.

    {
      "type": "Microsoft.Web/sites/providers/links",
      "apiVersion": "2015-01-01",
      "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
      "dependsOn": [ "[variables('siteName')]" ],
      "properties": {
        "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
        "notes": "This web site uses the storage account to store user information."
      }
    }


Per la descrizione completa del formato del modello vedere [Collegamenti alle risorse - schema del modello](resource-manager-template-links.md).

## <a name="linking-with-rest-api"></a>Collegamento con l'API REST
Per definire un collegamento tra risorse distribuite, eseguire:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

Sostituire {subscription-id} con l'identificatore della sottoscrizione. Sostituire {resource-group}, {provider-namespace}, {resource-type} e {resource-name} con i valori che identificano la prima risorsa nel collegamento. Sostituire {link-name} con il nome del collegamento da creare. Usare 2015-01-01 per {api-version}.

Nella richiesta includere un oggetto che definisce la seconda risorsa nel collegamento:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

L'elemento properties contiene l'identificatore della seconda risorsa.

È possibile eseguire query su collegamenti nella propria sottoscrizione con:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Per altri esempi, tra cui come recuperare le informazioni sui collegamenti, vedere l'articolo sulle [risorse collegate](https://docs.microsoft.com/rest/api/resources/resourcelinks).

## <a name="next-steps"></a>Passaggi successivi
* È inoltre possibile organizzare le risorse con i tag. Per altre informazioni sull'applicazione di tag alle risorse, vedere [Uso dei tag per organizzare le risorse](resource-group-using-tags.md).
* Per una descrizione su come creare modelli e definire le risorse da distribuire, vedere [Creazione di modelli](resource-group-authoring-templates.md).




<!--HONumber=Jan17_HO4-->


