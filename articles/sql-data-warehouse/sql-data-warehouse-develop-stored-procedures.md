---
title: Stored procedure in SQL Data Warehouse | Documentazione Microsoft
description: Suggerimenti per l&quot;implementazione delle stored procedure in Azure SQL Data Warehouse per lo sviluppo di soluzioni.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 103b3ad93c6bb99f1781e9b3c485caa6042ae0a3


---
# <a name="stored-procedures-in-sql-data-warehouse"></a>Stored procedure in SQL Data Warehouse
SQL Data Warehouse supporta molte delle funzionalità Transact-SQL disponibili in SQL Server. Ancora più importanti sono le funzionalità di scalabilità orizzontale specifiche, che si useranno per massimizzare le prestazioni della soluzione.

Tuttavia, per mantenere la scalabilità e le prestazioni di SQL Data Warehouse sono disponibili anche funzionalità e caratteristiche con differenze di comportamento e altra che non sono supportate.

Questo articolo illustra come implementare stored procedure in SQL Data Warehouse.

## <a name="introducing-stored-procedures"></a>Introduzione alle stored procedure
Le stored procedure sono un ottimo modo per incapsulare il codice SQL, archiviandolo vicino i dati nel data warehouse. Incapsulando il codice in unità gestibili, stored procedure consentono agli sviluppatori di modularizzare le soluzioni, rendendo possibile un maggiore riutilizzo del codice. Ogni stored procedure può anche accettare parametri per essere ancora più flessibile.

SQL Data Warehouse fornisce un'implementazione semplificata e ottimizzata delle stored procedure. La differenza principale rispetto a SQL Server è che la stored procedure non è codice precompilato. Nei data warehouse si è in genere meno preoccupati del tempo di compilazione. È più importante che il codice della stored procedure sia ottimizzato nel modo corretto quando si lavora con grandi volumi di dati. L'obiettivo consiste nel risparmiare ore, minuti e secondi, non millisecondi. È quindi più utile pensare alle stored procedure come contenitori per la logica di SQL.     

Quando SQL Data Warehouse esegue la stored procedure, le istruzioni SQL vengono analizzate, convertite e ottimizzate in fase di esecuzione. Durante questo processo, ogni istruzione viene convertita in query distribuite. Il codice SQL effettivamente eseguito sui dati è diverso dalla query inviata.

## <a name="nesting-stored-procedures"></a>Annidamento di stored procedure
Quando la stored procedure chiamano altre stored procedure o eseguono istruzioni SQL dinamiche, la stored procedure interna o la chiamata al codice è detta annidata.

SQL Data Warehouse supporta un massimo di 8 livelli di annidamento. Questo comportamento è leggermente diverso rispetto a SQL Server. In SQL Server i livelli di annidamento sono 32.

La chiamata alla stored procedure di livello superiore equivale al livello di annidamento 1.

```sql
EXEC prc_nesting
```
Se la stored procedure effettua anche un'altra chiamata a EXEC, il livello di annidamento passerà a 2.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Se la seconda procedura esegue quindi alcune istruzioni SQL dinamiche, il livello di annidamento passerà a 3.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Si noti che SQL Data Warehouse attualmente non supporta @@NESTLEVEL. È necessario tenere traccia manualmente del livello di annidamento. È improbabile che venga raggiunto il limite del livello di annidamento 8, ma in questo caso sarà necessario usare di nuovo il codice e renderlo "flat" in modo che rientri in tale limite.

## <a name="insertexecute"></a>INSERT..EXECUTE
SQL Data Warehouse non consente di usare il set di risultati di una stored procedure con un'istruzione INSERT. Si può tuttavia un approccio alternativo.

Per un esempio di come eseguire questa operazione, fare riferimento all'articolo seguente sulle [tabelle temporanee] .

## <a name="limitations"></a>Limitazioni
Esistono alcuni aspetti delle stored procedure Transact-SQL che non sono implementate in SQL Data Warehouse.

Sono:

* Stored procedure temporanee
* Stored procedure numerate
* Stored procedure estese
* Stored procedure CLR
* Opzione di crittografia
* Opzione di replica
* Parametri con valori di tabella
* Parametri di sola lettura
* Parametri predefiniti
* Contesti di esecuzione
* Istruzione return

## <a name="next-steps"></a>Passaggi successivi
Per altri suggerimenti sullo sviluppo, vedere la [panoramica sullo sviluppo][panoramica sullo sviluppo].

<!--Image references-->

<!--Article references-->
[tabelle temporanee]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[panoramica sullo sviluppo]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[livello di annidamento]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->



<!--HONumber=Nov16_HO3-->


