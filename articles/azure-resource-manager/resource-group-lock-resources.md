---
title: Impedire modifiche nelle risorse strategiche di Azure | Documentazione Microsoft
description: Impedire agli utenti di aggiornare o eliminare alcune risorse applicando una restrizione a utenti e ruoli.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 2a9075f4c9f10d05df3b275a39b3629d4ffd095f
ms.openlocfilehash: de8137a69ccc2028a7dcbff491573f36640bdc50


---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Bloccare le risorse per impedire modifiche impreviste 
L'amministratore può avere la necessità di bloccare una sottoscrizione, una risorsa o un gruppo di risorse per impedire che altri utenti nell'organizzazione modifichino o eliminino accidentalmente risorse strategiche. È possibile impostare il livello di blocco **CanNotDelete** o **ReadOnly**. 

* **CanNotDelete** significa che gli utenti autorizzati possono leggere e modificare una risorsa, ma non eliminarla. 
* **ReadOnly** significa che gli utenti autorizzati possono leggere una risorsa, ma non eliminarla o aggiornarla. L'applicazione di questo blocco è simile alla concessione a tutti gli utenti autorizzati solo le autorizzazioni concesse dal ruolo **Lettore**. 

I blocchi di Resource Manager si applicano solo alle operazioni che si verificano nel piano di gestione, costituito da operazioni inviate a `https://management.azure.com`. I blocchi non limitano il modo in cui le risorse eseguono le proprie funzioni. Vengono limitate le modifiche alle risorse, ma non le operazioni delle risorse. Un blocco di sola lettura in un database SQL impedisce ad esempio l'eliminazione o la modifica del database, ma non impedisce la creazione, l'aggiornamento o l'eliminazione dei dati nel database. Le transazioni di dati sono consentite in quanto tali operazioni non vengono inviate a `https://management.azure.com`.

L'applicazione di **ReadOnly** può causare risultati imprevisti, perché alcune operazioni che sembrano operazioni di lettura richiedono in effetti azioni aggiuntive. Ad esempio, l'inserimento di un blocco **ReadOnly** in un account di archiviazione impedisce a tutti gli utenti di ottenere un elenco delle chiavi. L'operazione di elenco delle chiavi viene gestita tramite una richiesta POST, perché le chiavi restituite sono disponibili per operazioni di scrittura. Per fare un altro esempio, l'inserimento di un blocco **ReadOnly** in una risorsa del servizio app impedisce a Esplora Server di Visual Studio di visualizzare i file relativi alla risorsa, perché tale interazione richiede l'accesso in scrittura.

Diversamente dal controllo degli accessi in base al ruolo, i blocchi di gestione consentono di applicare una restrizione a tutti gli utenti e i ruoli. Per informazioni sull'impostazione delle autorizzazioni per utenti e ruoli, vedere [Controllo degli accessi in base al ruolo nel Portale di Azure](../active-directory/role-based-access-control-configure.md).

Quando si applica un blocco in un ambito padre, tutte le risorse in tale ambito ereditano lo stesso blocco. Anche le risorse aggiunte successivamente ereditano il blocco dal padre. Il blocco più restrittivo nell'ereditarietà ha la precedenza.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Chi può creare o eliminare i blocchi nell'organizzazione
Per creare o eliminare i blocchi di gestione, è necessario avere accesso alle azioni `Microsoft.Authorization/*` o `Microsoft.Authorization/locks/*`. Dei ruoli predefiniti, solo **Proprietario** e **Amministratore Accesso utenti** garantiscono tali azioni.

## <a name="creating-a-lock-through-the-portal"></a>Creazione di un blocco tramite il portale
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Creazione di un blocco in un modello
L'esempio seguente mostra un modello che crea un blocco in un account di archiviazione. L'account di archiviazione a cui applicare il blocco viene specificato come parametro. Il nome del blocco viene creato concatenando il nome della risorsa con **/Microsoft.Authorization/** e il nome del blocco stesso, in questo caso **myLock**.

Il tipo fornito è specifico del tipo di risorsa. Per l'archiviazione, impostare il tipo su "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Creazione di un blocco con l'API REST
È possibile bloccare le risorse distribuite tramite l'[API REST per i blocchi di gestione](https://docs.microsoft.com/rest/api/resources/managementlocks). L'API REST consente di creare ed eliminare i blocchi e recuperare informazioni sui blocchi esistenti.

Per creare un blocco, eseguire:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

L'ambito può essere una sottoscrizione, un gruppo di risorse o una risorsa. Lock-name indica il nome che si desidera assegnare al blocco. Per api-version, usare **2015-01-01**.

Nella richiesta includere un oggetto JSON che specifica le proprietà per il blocco.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 


## <a name="creating-a-lock-with-azure-powershell"></a>Creazione di un blocco con Azure PowerShell
È possibile bloccare le risorse distribuite con Azure PowerShell tramite **New-AzureRmResourceLock** , come mostra l'esempio seguente.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure Powershell fornisce altri comandi per la gestione dei blocchi, ad esempio **Set-AzureRmResourceLock** per aggiornare un blocco e **Remove-AzureRmResourceLock** per eliminarlo.

## <a name="next-steps"></a>Passaggi successivi
* Per altre informazioni sull'uso dei blocchi di risorse, vedere [Blocco delle risorse di Azure](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
* Per informazioni sull'organizzazione logica delle risorse, vedere [Uso dei tag per organizzare le risorse](resource-group-using-tags.md)
* Per modificare il gruppo di risorse in cui si trova una risorsa, vedere [Spostamento delle risorse in un nuovo gruppo di risorse](resource-group-move-resources.md)
* È possibile applicare restrizioni e convenzioni all’interno della sottoscrizione con criteri personalizzati. Per altre informazioni, vedere [Usare i criteri per gestire le risorse e controllare l'accesso](resource-manager-policy.md).
* Per indicazioni su come le aziende possono usare Resource Manager per gestire efficacemente le sottoscrizioni, vedere [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Scaffolding aziendale Azure - Governance prescrittiva per le sottoscrizioni).




<!--HONumber=Jan17_HO4-->


