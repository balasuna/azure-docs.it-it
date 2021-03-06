---
title: "Che cos&quot;è un pool elastico di Azure? | Microsoft Docs"
description: "È possibile gestire centinaia o migliaia di database con un pool. Un unico prezzo per un set di unità di prestazioni può essere distribuito nel pool. I database possono essere inseriti o rimossi in base alle esigenze."
keywords: pool elastici, database sql
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 02/01/2017
ms.author: ddove
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 65b7e72d2cf0afd57aebd2e5938b540b2a2b4f20
ms.openlocfilehash: a38e1bf40bde45d0122e07b50c748d57757635a5


---
# <a name="what-is-an-azure-sql-elastic-pool"></a>Informazioni sul pool elastico di Azure SQL
A un pool elastico SQL viene assegnato un certo numero di eDTU a un determinato prezzo, in un server logico di Azure, condivise da un pool di database in tale server. I pool elastici del database SQL offrono una soluzione semplice e conveniente per gestire gli obiettivi di prestazioni per più database con modelli di utilizzo estremamente mutevoli e imprevedibili.

> [!NOTE]
> I pool elastici sono disponibili a livello generale in tutte le aree di Azure tranne India occidentale, dove sono attualmente in anteprima.  I pool elastici verranno resi disponibili a livello generale in quest'area non appena possibile.
>
>

## <a name="how-it-works"></a>Funzionamento
Un modello di applicazione SaaS molto diffuso è il modello di database a singolo tenant, in cui a ogni cliente viene assegnato un proprio database. Ciascun cliente (database) ha requisiti di risorse imprevedibili per memoria, IO e CPU. Con questi picchi e stagnazioni della domanda, come fare ad allocare le risorse in modo efficiente e conveniente? In genere, è possibile procedere in due modi: (1) effettuare il provisioning eccessivo delle risorse in base all'utilizzo massimo e pagare di più oppure (2) effettuare un provisioning insufficiente per risparmiare sui costi, a scapito delle prestazioni e del livello di soddisfazione del cliente nei momenti di picco. I pool elastici risolvono il problema assicurando ai database l'ottenimento delle risorse di prestazioni necessarie, quando richieste. Forniscono un meccanismo semplice di allocazione delle risorse che rientra in un budget prevedibile. Per altre informazioni sui modelli di progettazione per applicazioni SaaS con pool elastici, vedere l'articolo relativo ai [modelli di progettazione per applicazioni SaaS multi-tenant con database SQL di Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Nel database SQL, la misura relativa della capacità del database di gestire le richieste di risorse è espressa in unità di transazione di database (DTU) per i singoli database e in DTU elastiche (eDTU) per i database in un pool elastico. Per altre informazioni su DTU ed eDTU, vedere [Introduzione al database SQL](sql-database-technical-overview.md).

A un pool viene assegnato un numero definito di eDTU per un prezzo prestabilito. All'interno del pool i singoli database sono sufficientemente flessibili da assicurare una scalabilità automatica nell'ambito di parametri prefissati. Se il carico di lavoro è importante, un database può utilizzare più eDTU per soddisfare la domanda. Se invece il carico di lavoro è più leggero, i database in assenza di carico non utilizzano gli eDTU. La possibilità di effettuare il provisioning delle risorse per l'intero pool e non per i singoli database semplifica le attività di gestione. Il budget del pool, inoltre, è facilmente prevedibile.

È possibile aggiungere altre eDTU a un pool esistente senza causare tempi di inattività del database, tranne per il fatto che potrebbe essere necessario spostare i database per fornire risorse di calcolo aggiuntive per le nuove eDTU riservate. Analogamente, se gli eDTU aggiuntivi non sono più necessari, è possibile rimuoverli da un pool esistente in qualsiasi momento.

È possibile aggiungere e rimuovere database dal pool. Se si prevede che un database sottoutilizzerà le proprie risorse, è possibile rimuoverlo.

## <a name="which-databases-go-in-a-pool"></a>Scelta dei database da inserire in un pool
![Database SQL che condividono eDTU in un pool elastico.][1]

I database che sono ottimi candidati per i pool elastici sono in genere caratterizzati da periodi di attività e da periodi di inattività. Nell'esempio sopra riportato è possibile visualizzare l'attività di un database singolo, di 4 database e infine di un pool elastico con 20 database. I database con livelli di attività variabili nel corso del tempo sono ottimi candidati per i pool elastici in quanto non sono tutti attivi nello stesso momento e possono condividere gli eDTU. Non tutti i database seguono questo modello. I database che hanno una richiesta di risorse più costante sono più indicati per i livelli di servizio Basic, Standard e Premium, nei quali le risorse vengono assegnate singolarmente.

[Considerazioni di prezzo e prestazioni per un pool elastico](sql-database-elastic-pool-guidance.md).

## <a name="edtu-and-storage-limits-for-elastic-pools"></a>Limiti di archiviazione e di eDTU per i pool elastici

La tabella seguente descrive le caratteristiche dei pool elastici Basic, Standard e Premium.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Se vengono utilizzate tutte le DTU di un pool elastico, ogni database del pool riceve un'uguale quantità di risorse per l'elaborazione di query.  Il servizio di database SQL suddivide equamente le risorse fra i database con intervalli equivalenti per i tempi di calcolo. La condivisione equa delle risorse del pool elastico accompagna ogni quantità di risorse altrimenti garantita per ogni database quando il numero minimo di DTU per ogni database è impostato su un valore diverso da zero.

## <a name="elastic-pool-properties"></a>Proprietà dei pool elastici

Le tabelle seguenti descrivono i limiti per i pool elastici e i database in pool.

### <a name="limits-for-elastic-pools"></a>Limiti per i pool elastici
| Proprietà | Descrizione |
|:--- |:--- |
| Livello di servizio |Basic, Standard o Premium. Il livello di servizio determina l'intervallo relativo ai limiti di archiviazione e di prestazioni configurabili nonché le opzioni di continuità aziendale. Ogni database all'interno di un pool ha lo stesso livello di servizio del pool. Il "livello di servizio" è detto anche "edizione". |
| eDTU per pool |Numero massimo di eDTU condivisibile dai database nel pool. In numero totale di eDTU usate dai database nel pool nello stesso momento non può superare questo limite. |
| Spazio di archiviazione massimo per pool (GB) |Quantità massima di spazio di archiviazione in GB condivisibile dai database nel pool. La quantità totale di spazio di archiviazione usato da tutti i database nel pool non può superare questo limite, che è determinato dal numero di eDTU per pool. Se il limite viene superato, tutti i database diventano di sola lettura. Lo spazio di archiviazione massimo per pool indica lo spazio di archiviazione massimo per i file di dati nel pool e non include lo spazio usato dai file di log. |
| Numero massimo di database per pool |Il numero massimo di database consentiti per pool. |
| Numero massimo di ruoli di lavoro simultanei per pool |Il numero massimo di ruoli di lavoro simultanei (richieste) disponibile per tutti i database nel pool. |
| Numero massimo di accessi simultanei per pool |Il numero massimo di accessi simultanei per tutti i database nel pool. |
| Numero massimo di sessioni simultanee per pool |Il numero massimo di sessioni disponibili per tutti i database nel pool. |

### <a name="limits-for-pooled-databases"></a>Limiti per i database in pool
| Proprietà | Descrizione |
|:--- |:--- |
| Numero massimo di eDTU per database |Il numero massimo di eDTU di cui un database può usufruire nel pool se disponibili sulla base dell'uso da parte di altri database nel pool.  Il numero massimo di eDTU per database non è una garanzia di risorse per un database.  Si tratta di un'impostazione globale che si applica a tutti i database nel pool. Impostare il numero massimo di eDTU per database sufficiente per gestire i picchi di utilizzo dei database. È previsto un certo grado di overcommit perché il pool in genere presuppone modelli di utilizzo dei database a freddo e a caldo in cui i database non raggiungono il picco contemporaneamente. Si pensi al caso in cui il picco di utilizzo per ogni database sia di 20 eDTU e solo il 20% dei 100 database nel pool raggiunga il picco nello stesso momento.  Se il numero massimo di eDTU per ogni database è impostato su 20 eDTU, è ragionevole eseguire l'overcommit del pool moltiplicando per 5 e impostare il numero di eDTU su 400. |
| Numero minimo di eDTU per database |Il numero minimo di eDTU garantito a ogni database nel pool.  Si tratta di un'impostazione globale che si applica a tutti i database nel pool. Il numero minimo di eDTU per database può essere impostato su 0, che corrisponde anche al valore predefinito. Questa proprietà è impostata su un valore compreso tra 0 e l'utilizzo medio di eDTU per ogni database. Il prodotto tra il numero di database nel pool e il numero minimo di eDTU per database non può superare il numero di eDTU per pool.  Ad esempio, se un pool dispone di 20 database e di un numero minimo di eDTU per database impostato su 10 eDTU, il numero di eDTU per pool deve essere almeno pari a 200. |
| Spazio di archiviazione massimo per database (GB) |Lo spazio di archiviazione massimo per un database in un pool. I database in pool condividono lo spazio di archiviazione del pool, quindi lo spazio di archiviazione del database è limitato allo spazio di archiviazione del pool rimanente e allo spazio massimo per database. Lo spazio di archiviazione massimo per database indica le dimensioni massime dei file di dati e non include lo spazio usato dai file di log. |

## <a name="elastic-jobs"></a>Processi elastici
L'uso di un pool permette di semplificare le attività di gestione con l'esecuzione di script in **[processi elastici](sql-database-elastic-jobs-overview.md)**. Un processo elastico elimina la maggior parte delle attività ripetitive associate a un elevato numero di database. Per iniziare, vedere l'[introduzione ai processi elastici](sql-database-elastic-jobs-getting-started.md).

Per altre informazioni sugli altri strumenti di database per usare più database, vedere [Aumentare il numero di istanze con il database SQL di Azure](sql-database-elastic-scale-introduction.md).

## <a name="business-continuity-features-for-databases-in-a-pool"></a>Funzionalità di continuità aziendale per i database in un pool
I database in pool supportano in genere le stesse [funzionalità di continuità aziendale](sql-database-business-continuity.md) disponibili per i singoli database.

### <a name="point-in-time-restore"></a>Ripristino temporizzato
Il ripristino temporizzato usa i backup automatici del database per ripristinare un database in un pool a un punto specifico nel tempo. Vedere [Ripristino temporizzato](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="geo-restore"></a>Ripristino geografico
Il ripristino geografico fornisce un'opzione predefinita di ripristino quando un database non è disponibile a causa di un evento imprevisto nell'area in cui è ospitato. Vedere [Ripristinare un database SQL di Azure o eseguire il failover in un database secondario](sql-database-disaster-recovery.md)

### <a name="active-geo-replication"></a>Replica geografica attiva
Per le applicazioni che hanno requisiti di ripristino più rigorosi rispetto alle possibilità del ripristino geografico, configurare la replica geografica attiva usando il [portale di Azure](sql-database-geo-replication-portal.md), [PowerShell](sql-database-geo-replication-powershell.md) o [Transact-SQL](sql-database-geo-replication-transact-sql.md).

## <a name="additional-resources"></a>Risorse aggiuntive
* [Esercitazione video di Microsoft Virtual Academy sulle funzionalità di elasticità del database SQL di Azure](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)

<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png



<!--HONumber=Feb17_HO1-->


