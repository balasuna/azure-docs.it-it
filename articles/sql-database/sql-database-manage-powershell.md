---
title: Gestire il database SQL di Azure con PowerShell | Documentazione Microsoft
description: Gestione del database SQL di Azure con PowerShell.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 3f21ad5e-ba99-4010-b244-5e5815074d31
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: ae230c012a17eb73c8993a32197c844c6abaa2a4
ms.openlocfilehash: 32d8c5f13d95c3de8b06782f4f6541866389be5b


---
# <a name="managing-azure-sql-database-using-powershell"></a>Gestire il database SQL di Azure con PowerShell

Questo argomento illustra i cmdlet PowerShell che consentono di eseguire molte attività del database SQL di Azure. Per un elenco più completo, vedere [Cmdlet del database SQL di Azure](https://msdn.microsoft.com/library/mt574084\(v=azure.300\).aspx).

> [!TIP]
> Per un'esercitazione che illustra come creare un server, creare un firewall basato su server, visualizzare le proprietà del server, connettersi ed eseguire query nel database master, creare un database di esempio e un database vuoto, eseguire query relative alle proprietà del database, connettersi ed eseguire query nel database di esempio, vedere l'[Esercitazione introduttiva](sql-database-get-started-powershell.md).
>

## <a name="how-do-i-create-a-resource-group"></a>Come si crea un gruppo di risorse?
Per creare un gruppo di risorse per il database SQL e le relative risorse di Azure, usare il cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837\(v=azure.300\).aspx).

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Per altre informazioni, vedere [Uso di Azure PowerShell con Gestione risorse di Azure](../powershell-azure-resource-manager.md).
Per un'esercitazione completa, vedere [Introduzione ai server, ai database e alle regole del firewall del database SQL di Azure con Azure PowerShell](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database-server"></a>Come si crea un server di database SQL?
Per creare un server di database SQL, usare il cmdlet [New-AzureRmSqlServer](/powershell/resourcemanager/azurerm.sql/v2.5.0/new-azurermsqlserver). Sostituire *server1* con il nome del server. I nomi dei server devono essere univoci nell'ambito di tutti i server di database SQL di Azure. Se il nome del server è già in uso, viene visualizzato un errore. Il completamento di questo comando può richiedere alcuni minuti. Il gruppo di risorse deve già esistere nella sottoscrizione.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString -String $serverPassword -AsPlainText -Force
$creds = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $serverAdmin, $securePassword


$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Per altre informazioni sui server, vedere [SQL Database features](sql-database-features.md) (Funzionalità del database SQL). Per un'esercitazione completa, vedere [Introduzione ai server, ai database e alle regole del firewall del database SQL di Azure con Azure PowerShell](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database-server-firewall-rule"></a>Come si crea una regola del firewall per il server di database SQL?
Per creare una regola del firewall per accedere al server, usare il cmdlet [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx). Eseguire il comando seguente sostituendo gli indirizzi IP di inizio e fine con i valori validi per il client. Il gruppo di risorse e il server devono già esistere nella sottoscrizione.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Per consentire al server l'accesso ad altri servizi di Azure, creare una regola del firewall e impostare sia `-StartIpAddress` che `-EndIpAddress` su **0.0.0.0**. Questa regola del firewall speciale consente a tutto il traffico di Azure di accedere al server.

Per ulteriori informazioni, vedere [Firewall di database SQL di Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx). Per un'esercitazione completa, vedere [Introduzione ai server, ai database e alle regole del firewall del database SQL di Azure con Azure PowerShell](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database"></a>Come si crea un database SQL?
Per creare un database SQL, usare il cmdlet [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339\(v=azure.300\).aspx). Il gruppo di risorse e il server devono già esistere nella sottoscrizione. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Per altre informazioni, vedere [Informazioni sul database SQL](sql-database-technical-overview.md). Per un'esercitazione completa, vedere [Introduzione ai server, ai database e alle regole del firewall del database SQL di Azure con Azure PowerShell](sql-database-get-started-powershell.md).

## <a name="how-do-i-change-the-performance-level-of-a-sql-database"></a>Come si modifica il livello di prestazioni di un database SQL?
Per modificare il livello di prestazioni, ridimensionare il database verso l'alto o verso il basso con il cmdlet [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433\(v=azure.300\).aspx). Il gruppo di risorse, il server e il database devono già esistere nella sottoscrizione. Impostare il `-RequestedServiceObjectiveName` su uno spazio singolo (come nel seguente frammento) per il livello di base. Impostarlo su *S0*, *S1*, *P1*, *P6* e così via, come nell'esempio precedente, per gli altri livelli.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Per altre informazioni, vedere [Opzioni e prestazioni disponibili in ogni livello di servizio del database SQL](sql-database-service-tiers.md). Per uno script di esempio, vedere [Script di esempio di PowerShell per modificare il livello di servizio e il livello delle prestazioni del database SQL](sql-database-manage-single-databases-powershell.md#change-the-service-tier-and-performance-level-of-a-single-database).

## <a name="how-do-i-copy-a-sql-database-to-the-same-server"></a>Come si copia un database SQL nello stesso server?
Per copiare un database SQL nello stesso server, usare il cmdlet [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644\(v=azure.300\).aspx). Impostare `-CopyServerName` e `-CopyResourceGroupName` sugli stessi valori del server di database e del gruppo di risorse di origine.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Per altre informazioni, vedere [Copiare un database SQL di Azure](sql-database-copy.md). Per uno script di esempio, vedere [Copiare uno script di PowerShell per database SQL](sql-database-copy-powershell.md#example-powershell-script).

## <a name="how-do-i-delete-a-sql-database"></a>Come si elimina un database SQL?
Per eliminare un database SQL, usare il cmdlet [Remove-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368\(v=azure.300\).aspx). Il gruppo di risorse, il server e il database devono già esistere nella sottoscrizione.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="how-do-i-delete-a-sql-database-server"></a>Come si elimina un server di database SQL?
Per eliminare un server di database SQL, usare il cmdlet [Remove-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488\(v=azure.300\).aspx).

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="how-do-i-create-and-manage-elastic-pools-using-powershell"></a>Come si creano e si gestiscono i pool elastici tramite PowerShell?
Per informazioni sulla creazione dei pool elastici mediante PowerShell, vedere [Creare un nuovo pool elastico con PowerShell](sql-database-elastic-pool-manage-powershell.md).

Per informazioni dettagliate sulla gestione dei pool elastici con PowerShell, vedere [Monitorare e gestire un pool elastico con PowerShell](sql-database-elastic-pool-manage-powershell.md).

## <a name="related-information"></a>Informazioni correlate
* [Cmdlet del database SQL di Azure.](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx)
* [Informazioni di riferimento sui cmdlet di Azure](https://msdn.microsoft.com/library/azure/dn708514\(v=azure.300\).aspx)




<!--HONumber=Feb17_HO3-->


