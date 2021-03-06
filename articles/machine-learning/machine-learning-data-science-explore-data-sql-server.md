---
title: Esplorare i dati in una macchina virtuale di SQL Server in Azure| Microsoft Docs
description: Come esplorare i dati archiviati in una macchina virtuale SQL Server in Azure.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: bradsev
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1654c100c1c57b04a61cf2b9d2cf37d58cec1870


---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Esplorazione dei dati nella macchina virtuale di SQL Server in Azure
In questo documento viene illustrato come esplorare i dati archiviati in una macchina virtuale SQL Server in Azure. Questa operazione può essere eseguita gestendo i dati tramite SQL o utilizzando un linguaggio di programmazione come Python.

Il **menu** seguente collega ad argomenti che descrivono come usare gli strumenti per esplorare i dati da vari ambienti di archiviazione. Questa attività è un passaggio nel Cortana Analytics Process (CAP).

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> Le istruzioni SQL di esempio fornite nel documento presuppongono che i dati si trovino in SQL Server. In caso contrario, fare riferimento al processo di analisi scientifica dei dati cloud per visualizzare informazioni su come spostare i dati in SQL Server.
> 
> 

## <a name="a-namesql-dataexplorationaexplore-sql-data-with-sql-scripts"></a><a name="sql-dataexploration"></a>Esplorare i dati SQL mediante gli script SQL
Di seguito, sono riportati alcuni script SQL di esempio da utilizzare per esplorare gli archivi dati in SQL Server.

1. Visualizzare il numero di osservazioni per giorno
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Visualizzare i livelli in una colonna di categoria
   
    `select  distinct <column_name> from <databasename>`
3. Visualizzare il numero di livelli in una combinazione di due colonne di categoria 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Visualizzare la distribuzione per colonne numeriche 
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> Per un esempio pratico, è possibile usare il [set di dati dei taxi di NYC](http://www.andresmh.com/nyctaxitrips/) e vedere l'IPNB intitolato [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) (Gestione dei dati di NYC tramite IPython Notebook e SQL Server) per una procedura dettagliata end-to-end.
> 
> 

## <a name="a-namepythonaexplore-sql-data-with-python"></a><a name="python"></a>Esplorare i dati SQL mediante Python
L'uso di Python per esplorare i dati e creare funzionalità, se i dati si trovano in SQL Server, funziona in modo analogo all'elaborazione dei dati BLOB di Azure tramite Python, come illustrato in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md) (Elaborare i dati BLOB di Azure in un ambiente di analisi scientifica dei dati). I dati devono essere caricati dal database nei frame di dati Panda. A questo punto, è possibile elaborarli ulteriormente. In questa sezione, è stato descritto il processo di connessione al database per caricare dati all'interno di un frame di dati.

Il seguente formato della stringa di connessione può essere usato per connettersi a un database di SQL Server da Pyhton usando pyodbc (sostituire il nome del server, quello del database, il nome utente e la password con i valori personalizzati):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

La [libreria Pandas](http://pandas.pydata.org/) in Python fornisce una vasta gamma di strutture di dati e strumenti di analisi dei dati per la manipolazione dei dati nella programmazione in Python. Il codice seguente consente di leggere i risultati restituiti da un database di SQL Server all'interno di un frame di dati di Pandas:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

A questo punto, è possibile usare il frame di dati di Pandas, come descritto nell'argomento [Elaborazione dei dati BLOB di Azure nell'ambiente di analisi scientifica dei dati](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Il Cortana Analytics Process nell’esempio di azione
Per un esempio della procedura dettagliata end-to-end del Cortana Analytics Process tramite un set di dati pubblico, vedere [Processo di analisi scientifica dei dati per i team in azione: uso di SQL Sever](machine-learning-data-science-process-sql-walkthrough.md).




<!--HONumber=Nov16_HO3-->


