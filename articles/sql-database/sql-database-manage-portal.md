---
title: Gestire un database SQL di Azure tramite il portale di Azure |Documentazione Microsoft
description: Informazioni di riferimento rapide su come usare il portale di Azure per gestire un database relazionale nel cloud usando il portale di Azure.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3a56e9de-c21a-40ba-9a35-958172cb4e5b
ms.service: sql-database
ms.custom: overview
ms.devlang: NA
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.date: 01/10/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 420b2153f6115dd712d3033e30f11f79b18cd80f
ms.openlocfilehash: be89a2799af3bdc2938f73e3d54f00f81d9ab9cd


---
# <a name="manage-azure-sql-databases-using-the-azure-portal"></a>Gestire database SQL di Azure mediante il portale di Azure
> [!div class="op_single_selector"]
> * [Portale di Azure](sql-database-manage-portal.md)
> * [SSMS](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

Il [portale di Azure](https://portal.azure.com/) consente di creare, monitorare e gestire server e database SQL di Azure. Questo articolo offre una descrizione rapida e collegamenti per i dettagli delle attività più comuni.

> [!TIP]
> Per un'esercitazione che illustra come creare un server, creare un firewall basato su server, visualizzare le proprietà del server, connettersi usando SQL Server Management Studio, eseguire query nel database master, creare un database di esempio e un database vuoto, eseguire query relative alle proprietà del database, connettersi usando SQL Server Management Studio ed eseguire query nel database di esempio, vedere l'[Esercitazione introduttiva](sql-database-get-started.md).

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Visualizzare database, server e pool SQL di Azure
Per visualizzare i servizi di database SQL disponibili, fare clic su **Altri servizi** e digitare **SQL** nella casella di ricerca:

![Database SQL](./media/sql-database-manage-portal/sql-services.png)

## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Come si creano o visualizzano i database SQL di Azure?
Per aprire il pannello **Database SQL**, fare clic su **Database SQL** e quindi sul database da usare oppure fare clic su **+Aggiungi** per creare un database SQL. Per informazioni dettagliate, vedere [Esercitazione sul database SQL: Creare un database SQL in pochi minuti usando il portale di Azure](sql-database-get-started.md).

![Database SQL](./media/sql-database-manage-portal/sql-databases.png)

## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Come si creano o visualizzano i server SQL di Azure?
Per aprire il pannello **SQL Server**, fare clic su **SQL Server** e quindi sul server da usare oppure fare clic su **+Aggiungi** per creare un server SQL. Per informazioni dettagliate, vedere [Esercitazione sul database SQL: Creare un database SQL in pochi minuti usando il portale di Azure](sql-database-get-started.md).

![Server di SQL](./media/sql-database-manage-portal/sql-servers.png)

## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Come si creano o visualizzano i pool elastici SQL di Azure?
Per aprire il pannello **Pool elastici SQL**, fare clic su **Pool elastici SQL** e quindi sul pool da usare oppure fare clic su **+Aggiungi** per creare un pool. Per informazioni dettagliate, vedere [Creare un nuovo pool elastico con il portale di Azure](sql-database-elastic-pool-create-portal.md).

![Pool elastici SQL](./media/sql-database-manage-portal/elastic-pools.png)

## <a name="how-do-i-update-or-view-sql-database-settings"></a>Come si aggiornano o visualizzano le impostazioni del database SQL?
Per visualizzare o aggiornare le impostazioni del database, fare clic sull'impostazione desiderata nel pannello Database SQL:

![Impostazioni del database SQL](./media/sql-database-manage-portal/settings.png)

## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Come si trova il nome completo di un server di database SQL?
Per trovare il nome del server di database, fare clic su **Panoramica** nel pannello **Database SQL** per visualizzare il nome del server:

![Impostazioni del database SQL](./media/sql-database-manage-portal/server-name.png)

## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Come è possibile gestire le regole del firewall per controllare l'accesso a server e database SQL?
Per visualizzare, creare o aggiornare le regole del firewall, fare clic su **Imposta firewall server** nel pannello **Database SQL**. Per informazioni dettagliate, vedere [Configurare una regola firewall a livello di server per il database SQL di Azure tramite il portale di Azure](sql-database-configure-firewall-settings.md).

![Regole del firewall](./media/sql-database-manage-portal/sql-database-firewall.png)

## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Come è possibile modificare il livello di servizio o il livello di prestazioni del database SQL?
Per aggiornare il livello di servizio o il livello di prestazioni di un database SQL, fare clic su **Piano tariffario (piano DTU)** nel pannello **Database SQL**. Per informazioni dettagliate, vedere [Modificare il livello di servizio e il livello di prestazioni (piano tariffario) di un database SQL](sql-database-scale-up.md).

![Piani tariffari](./media/sql-database-manage-portal/pricing-tier.png)

## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Come si configurano il controllo e il rilevamento delle minacce per un database SQL?
Per configurare il controllo e il rilevamento delle minacce per un database SQL, fare clic su **Controllo e rilevamento minacce** nel pannello **Database SQL**. Per informazioni dettagliate, vedere [Introduzione al controllo del database SQL](sql-database-auditing-get-started.md) e [Introduzione al rilevamento delle minacce nel database SQL](sql-database-threat-detection-get-started.md).

## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Come si configura la maschera dati dinamica di un database SQL?
Per configurare la maschera dati dinamica per un database SQL, fare clic su **Maschera dati dinamica** nel pannello **Database SQL**. Per informazioni dettagliate, vedere [Introduzione alla maschera dati dinamica del database SQL (portale di Azure)](sql-database-dynamic-data-masking-get-started.md).

## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Come si configura Transparent Data Encryption (TDE) per un database SQL?
Per configurare Transparent Data Encryption per un database SQL, fare clic su **Transparent Data Encryption** nel pannello **Database SQL**. Per informazioni dettagliate, vedere [Abilitare TDE in un database con il portale](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Come si visualizzano o modificano le dimensioni massime di un database SQL?
Per visualizzare o modificare le dimensioni di un database SQL, fare clic su **Dimensioni database** nel pannello **Database SQL**. Aggiornare le dimensioni massime di un database modificando il livello di servizio o il livello di prestazioni. Per informazioni dettagliate, vedere [Modificare il livello di servizio e il livello di prestazioni (piano tariffario) di un database SQL](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Come è possibile monitorare e migliorare le prestazioni di un database SQL?
Per monitorare e migliorare le prestazioni di un database SQL, fare clic su **Informazioni generali sulle prestazioni** nel pannello **Database SQL**. Per altre informazioni, vedere [Informazioni dettagliate sulle prestazioni del database SQL](sql-database-performance.md).

## <a name="how-do-i-configure-geo-replication"></a>Come si configura la replica geografica?
Per configurare la replica geografica per un database SQL, fare clic su **Replica geografica** nel pannello **Database SQL**. Per informazioni dettagliate, vedere [Configurare la replica geografica per il database SQL di Azure con il portale di Azure](sql-database-geo-replication-portal.md).

## <a name="how-do-i-fail-over-to-a-geo-replicated-sql-database"></a>Come si esegue il failover a un database SQL con replica geografica?
Per eseguire il failover a un database secondario con replica geografica, fare clic su **Replica geografica** nel pannello **Database SQL** e quindi fare clic su **Failover**. Per informazioni dettagliate, vedere [Avviare un failover pianificato o non pianificato per il database SQL di Azure con il portale di Azure](sql-database-geo-replication-failover-portal.md).

## <a name="how-do-i-copy-a-sql-database"></a>Come si copia un database SQL?
Per copiare un database SQL, fare clic su **Copia** nel pannello **Database SQL**. Per informazioni dettagliate, vedere [Copiare un database SQL di Azure tramite il portale di Azure](sql-database-copy-portal.md).

![Impostazioni del database SQL](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Come si archivia un database SQL di Azure in un file BACPAC?
Per creare un file BACPAC di un database SQL, fare clic su **Esporta** nel pannello **Database SQL**. Per informazioni dettagliate, vedere [Archiviare un database SQL di Azure in un file BACPAC tramite il portale di Azure](sql-database-export.md).

![Esportazione di un database SQL](./media/sql-database-manage-portal/sql-database-export.png)

## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Come si ripristina un database SQL a un momento precedente?
Per ripristinare un database SQL, fare clic su **Ripristina** nel pannello **Database SQL**. Per informazioni dettagliate, vedere [Ripristinare un database SQL di Azure a un momento precedente con il portale di Azure](sql-database-point-in-time-restore.md).

![Impostazioni del database SQL](./media/sql-database-manage-portal/sql-database-restore.png)

## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Come si crea un database SQL di Azure da un file BACPAC?
Per creare un database SQL da un file BACPAC, fare clic su **Importa database** nel pannello **SQL Server**. Per informazioni dettagliate, vedere [Importare un file BACPAC per creare un nuovo database SQL di Azure](sql-database-import.md).

![SQL Server](./media/sql-database-manage-portal/server-commands.png)

## <a name="how-do-i-restore-a-deleted-sql-database"></a>Come si ripristina un database SQL eliminato?
Per ripristinare un database SQL eliminato, fare clic su **Database eliminati** nel pannello **SQL Server**. Si tratta del server SQL contenente il database che è stato eliminato. Per informazioni dettagliate, vedere [Ripristinare un database SQL di Azure con il portale di Azure](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Come si elimina un database SQL?
Per eliminare un database SQL, fare clic su **Elimina** nel pannello **Database SQL**. 

![Impostazioni del database SQL](./media/sql-database-manage-portal/sql-database-delete.png)

## <a name="additional-resources"></a>Risorse aggiuntive
* [Database SQL](sql-database-technical-overview.md)
* [Monitorare e gestire un pool elastico con il portale di Azure](sql-database-elastic-pool-manage-portal.md)




<!--HONumber=Dec16_HO3-->


