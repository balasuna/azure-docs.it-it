---
title: Come disinstallare lo strumento processi di database elastico
description: Come disinstallare lo strumento processi di database elastico
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: bfc9d820-edbd-4fca-bfbf-1f339cfcc448
ms.service: sql-database
ms.workload: sql-database
ms.custom: multiple databases
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: aa438680348748a771f9016ecdcc68372c6c5e86


---
# <a name="uninstall-elastic-database-jobs-components"></a>Disinstallare i componenti dei processi di database elastici
**Processi di database elastici** possono essere disinstallati utilizzando il portale o PowerShell.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Disinstallare i componenti dei processi di database elastici utilizzando il portale di Azure
1. Aprire il [portale di Azure](https://portal.azure.com/).
2. Passare alla sottoscrizione che contiene i componenti dei **processi di database elastici** , vale a dire la sottoscrizione in cui i componenti dei processi di database elastici sono stati installati.
3. Fare clic su **Sfoglia** e quindi su **Gruppi di risorse**.
4. Selezionare il gruppo di risorse denominato "__ElasticDatabaseJob".
5. Eliminare il gruppo di risorse.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Disinstallare i componenti dei processi dei database elastici tramite PowerShell
1. Avviare una finestra di comando di Microsoft Azure PowerShell e passare alla sottodirectory strumenti nella cartella Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: digitare **cd strumenti**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools
2. Eseguire lo script .\UninstallElasticDatabaseJobs.ps1 di PowerShell.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1

O semplicemente, eseguire lo script seguente, supponendo che i valori predefiniti se utilizzati nell'installazione dei componenti:

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Passaggi successivi
Per reinstallare i processi di database elastici, vedere [Installazione del servizio processo di database elastico](sql-database-elastic-jobs-service-installation.md)

Per ulteriori informazioni sui processi dei database elastici, vedere [Panoramica dei processi di database elastici](sql-database-elastic-jobs-overview.md).

<!--Image references-->





<!--HONumber=Nov16_HO3-->


