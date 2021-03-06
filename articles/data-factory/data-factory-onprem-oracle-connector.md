---
title: Spostare i dati da/verso Oracle con Data Factory | Documentazione Microsoft
description: Informazioni su come spostare i dati da e verso database Oracle in locale mediante Data factory di Azure.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: a4b067e732bccb01faa96f23dbfd2ed65b7711a0
ms.openlocfilehash: 62326da2e801a7c6e01d29e2298bd3552f331647


---
# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Spostare i dati da/verso Oracle locale con Azure Data Factory
Questo articolo illustra come usare l'attività di copia della data factory per spostare dati da/verso Oracle a un altro archivio dati. Questo articolo si basa sull'articolo [Attività di spostamento dei dati](data-factory-data-movement-activities.md) , che offre una panoramica generale dello spostamento dei dati con attività di copia e delle combinazioni di archivio dati supportate.

Data Factory supporta la connessione a origini Oracle locali mediante il gateway di gestione dati. Vedere l'articolo [Gateway di gestione dati](data-factory-data-management-gateway.md) per informazioni sul Gateway di gestione dati e l'articolo [Spostare dati tra origini locali e il cloud con Gateway di gestione dati](data-factory-move-data-between-onprem-and-cloud.md) per istruzioni dettagliate su come configurare un gateway e una pipeline di dati per spostare i dati.

> [!NOTE]
> Il gateway è necessario anche se il database Oracle è ospitato in una macchina virtuale IaaS di Azure. È possibile installare il gateway nella stessa VM IaaS dell'archivio dati o in una macchina virtuale diversa, purché il gateway possa connettersi al database.
>

## <a name="supported-versions-and-installation"></a>Versioni supportate e installazione
Il connettore Oracle supporta due versioni di driver:

- Il **driver Microsoft per Oracle** è incluso nel gateway di gestione dati a partire dalla versione 2.7. È **consigliabile** usare questo driver. È quindi necessario installare solo il gateway per connettersi a Oracle e con un'esperienza di copia migliore. È supportato Oracle Database 10g Release 2 o versioni successive.

    > [!NOTE]
    > Il driver Microsoft per Oracle supporta attualmente solo la copia dei dati da Oracle ma non la scrittura in Oracle. Si noti che la funzionalità di connessione di test nella scheda Diagnostica del Gateway di gestione dati supporta questo driver. In alternativa, è possibile usare la procedura guidata di copia per convalidare la connettività.
    >

- **Provider di dati Oracle per .NET:** è possibile scegliere di usare il Provider di dati Oracle per copiare i dati da/in Oracle. Questo componente è incluso in [Oracle Data Access Components for Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Installare la versione appropriata (32/64 bit) nel computer in cui è installato il gateway. [Oracle Data Provider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) può accedere a Oracle Database 10g Release 2 o versione successiva.

    Se si sceglie di installare XCopy, eseguire la procedura indicata nel file readme.htm. È consigliabile scegliere il programma di installazione con l'interfaccia utente (non XCopy).

    Dopo avere installato il provider, **riavviare** il servizio host di Gateway di gestione dati nel computer mediante l'applet Services (o) Gestione configurazione di Gateway di gestione di dati.  

## <a name="copy-data-wizard"></a>Copia di dati guidata
Il modo più semplice per creare una pipeline che copia i dati da e in un database Oracle in uno degli archivi dati sink supportati consiste nell'usare la Copia dati guidata. Vedere [Esercitazione: Creare una pipeline usando la Copia guidata](data-factory-copy-data-wizard-tutorial.md) per la procedura dettagliata sulla creazione di una pipeline attenendosi alla procedura guidata per copiare i dati.

L'esempio seguente fornisce le definizioni JSON campione da usare per creare una pipeline con il [Portale di Azure](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) o [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Illustrano come copiare dati da un database Oracle in un archivio BLOB di Azure e viceversa. Tuttavia, i dati possono essere copiati in qualsiasi sink dichiarato [qui](data-factory-data-movement-activities.md#supported-data-stores-and-formats) usando l'attività di copia in Azure Data Factory.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Esempio: Copiare i dati da Oracle a BLOB di Azure
Questo esempio illustra come copiare dati da un database Oracle locale a un archivio BLOB di Azure. Tuttavia, i dati possono essere copiati **direttamente** in qualsiasi sink dichiarato [qui](data-factory-data-movement-activities.md#supported-data-stores-and-formats) usando l'attività di copia in Azure Data Factory.  

L'esempio include le entità di Data factory seguenti:

1. Un servizio collegato di tipo [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2. Un servizio collegato di tipo [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service).
3. Un [set di dati](data-factory-create-datasets.md) di input di tipo [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties).
4. Un [set di dati](data-factory-create-datasets.md) di output di tipo [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5. Una [pipeline](data-factory-create-pipelines.md) con attività di copia che usa [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) come origine e [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) come sink.

L'esempio copia i dati da una tabella di un database Oracle locale a un BLOB ogni ora. Per altre informazioni sulle varie proprietà usate nell'esempio, vedere la documentazione nelle sezioni che seguono gli esempi.

**Servizio collegato Oracle:**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Servizio collegato di archiviazione BLOB di Azure:**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Set di dati di input Oracle:**

L'esempio presuppone che sia stata creata una tabella "MyTable" in Oracle e che contenga una colonna denominata "timestampcolumn" per i dati di una serie temporale.

Impostando "external" su "true" si comunica al servizio Data Factory che il set di dati è esterno alla data factory e non è prodotto da un'attività al suo interno.

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {     
            "externalData": {        
                "retryInterval": "00:01:00",    
                "retryTimeout": "00:10:00",       
                "maximumRetry": 3       
            }     
        }
    }
}
```

**Set di dati di output del BLOB di Azure:**

I dati vengono scritti in un nuovo BLOB ogni ora (frequenza: ora, intervallo: 1). Il percorso della cartella e il nome del file per il BLOB vengono valutati dinamicamente in base all'ora di inizio della sezione in fase di elaborazione. Il percorso della cartella usa le parti anno, mese, giorno e ora dell'ora di inizio.

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t",
                "rowDelimiter": "\n"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Pipeline con attività di copia:**

La pipeline contiene un'attività di copia configurata per usare i set di dati di input e output ed è programmata per essere eseguita ogni ora. Nella definizione JSON della pipeline il tipo di **origine** è impostato su **OracleSource** e il tipo di **sink** è impostato su **BlobSink**.  La query SQL specificata con la proprietà **oracleReaderQuery** consente di selezionare i dati da copiare nell'ultima ora.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Esempio: Copiare i dati dal BLOB di Azure in Oracle
Questo esempio illustra come copiare dati da un archivio BLOB di Azure a un database Oracle locale. I dati possono tuttavia essere copiati **direttamente** da qualsiasi origine dichiarata [qui](data-factory-data-movement-activities.md#supported-data-stores-and-formats) usando l'attività di copia in Azure Data Factory.  

L'esempio include le entità di Data factory seguenti:

1. Un servizio collegato di tipo [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2. Un servizio collegato di tipo [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service).
3. Un [set di dati](data-factory-create-datasets.md) di input di tipo [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Un [set di dati](data-factory-create-datasets.md) di output di tipo [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties).
5. Una [pipeline](data-factory-create-pipelines.md) con attività di copia che usa [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) come origine e [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) come sink.

L'esempio copia i dati da un BLOB a una tabella nel database Oracle locale ogni ora. Per altre informazioni sulle varie proprietà usate nell'esempio, vedere la documentazione nelle sezioni che seguono gli esempi.

**Servizio collegato Oracle:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Servizio collegato di archiviazione BLOB di Azure:**
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Set di dati di input del BLOB di Azure**

I dati vengono prelevati da un nuovo BLOB ogni ora (frequenza: ora, intervallo: 1). Il percorso della cartella e il nome del file per il BLOB vengono valutati dinamicamente in base all'ora di inizio della sezione in fase di elaborazione. Il percorso della cartella usa le parti di anno, mese e giorno della data/ora di inizio e il nome file usa la parte relativa all'ora. L'impostazione di "external" su "true" comunica al servizio Data Factory che la tabella è esterna alla data factory e non è prodotta da un'attività al suo interno.

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**Set di dati di output Oracle:**

L'esempio presuppone che sia stata creata una tabella "MyTable" in Oracle. È necessario creare la tabella in Oracle con lo stesso numero di colonne contenute nel file con estensione csv del BLOB. Alla tabella vengono aggiunte nuove righe ogni ora.

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

**Pipeline con attività di copia:**

La pipeline contiene un'attività di copia configurata per usare i set di dati di input e output ed è programmata per essere eseguita ogni ora. Nella definizione JSON della pipeline il tipo di **origine** è impostato su **BlobSource** e il tipo di **sink** è impostato su **OracleSink**.  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

## <a name="oracle-linked-service-properties"></a>Proprietà del servizio collegato Oracle
La tabella seguente contiene le descrizioni degli elementi JSON specifici del servizio collegato Oracle.

| Proprietà | Descrizione | Obbligatorio |
| --- | --- | --- |
| type |La proprietà type deve essere impostata su: **OnPremisesOracle** |Sì |
| driverType | Specificare il driver da usare per copiare i dati da/verso il database Oracle. I valori consentiti sono **Microsoft** o **ODP** (impostazione predefinita). Per informazioni dettagliate sui driver, vedere la sezione [Versione e installazione supportate](#supported-versions-and-installation). | No |
| connectionString | Specificare le informazioni necessarie per connettersi all'istanza del database Oracle per la proprietà connectionString. Vedere gli esempi seguenti. | Sì |
| gatewayName | Nome del gateway usato per connettersi al server Oracle locale |Sì |

Vedere [Spostare dati tra origini locali e il cloud con Gateway di gestione dati](data-factory-move-data-between-onprem-and-cloud.md) per informazioni dettagliate sull'impostazione delle credenziali per un'origine dati Oracle locale.

**Esempio: mediante il driver Microsoft**
```JSON
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Esempio: mediante il driver ODP**

Per informazioni su altri formati consentiti, vedere [questo sito](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/).
```JSON
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="oracle-dataset-type-properties"></a>Proprietà del tipo di set di dati Oracle
Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione di set di dati, vedere l'articolo sulla [creazione di set di dati](data-factory-create-datasets.md). Le sezioni come struttura, disponibilità e criteri di un set di dati JSON sono simili per tutti i tipi di set di dati (Oracle, BLOB di Azure, tabelle di Azure e così via).

La sezione typeProperties è diversa per ogni tipo di set di dati e contiene informazioni sulla posizione dei dati nell'archivio dati. La sezione typeProperties per il set di dati di tipo OracleTable presenta le proprietà seguenti:

| Proprietà | Descrizione | Obbligatorio |
| --- | --- | --- |
| tableName |Nome della tabella nell'istanza del database Oracle a cui fa riferimento il servizio collegato. |No, se **oracleReaderQuery** di **OracleSource** è specificato |

## <a name="oracle-copy-activity-type-properties"></a>Proprietà del tipo di attività di copia Oracle
Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione delle attività, fare riferimento all'articolo [Creazione di pipeline](data-factory-create-pipelines.md). Per tutti i tipi di attività sono disponibili proprietà come nome, descrizione, tabelle di input e output e criteri.

> [!NOTE]
> L'attività di copia accetta solo un input e produce solo un output.
>
>

Le proprietà disponibili nella sezione typeProperties dell'attività variano invece in base al tipo di attività. Per l'attività di copia variano in base ai tipi di origine e sink.

### <a name="oraclesource"></a>OracleSource
Nell'attività di copia con origine di tipo **OracleSource** sono disponibili le proprietà seguenti nella sezione **typeProperties**:

| Proprietà | Descrizione | Valori consentiti | Obbligatorio |
| --- | --- | --- | --- |
| oracleReaderQuery |Usare la query personalizzata per leggere i dati. |Stringa di query SQL. Ad esempio: select * Se non è specificata, corrisponde all'istruzione SQL eseguita: select * Se non è specificata, corrisponde all'istruzione SQL eseguita: select * from MyTable <br/><br/> * Se non è specificata, corrisponde all'istruzione SQL eseguita: select * Se non è specificata, corrisponde all'istruzione SQL eseguita: select * from MyTable |No (se **tableName** di **set di dati** è specificato) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** supporta le proprietà seguenti:

| Proprietà | Descrizione | Valori consentiti | Obbligatorio |
| --- | --- | --- | --- |
| writeBatchTimeout |Tempo di attesa per l'operazione di inserimento batch da completare prima del timeout. |Intervallo di tempo<br/><br/> Ad esempio: "00:30:00" (30 minuti). |No |
| writeBatchSize |Inserisce dati nella tabella SQL quando la dimensione del buffer raggiunge writeBatchSize. |Numero intero (numero di righe) |No (valore predefinito: 100) |
| sqlWriterCleanupScript |Specificare una query da eseguire nell'attività di copia per pulire i dati di una sezione specifica. |Istruzione di query. |No |
| sliceIdentifierColumnName |Specificare il nome della colonna per l'attività di copia da riempire con l'identificatore di sezione generato automaticamente, che viene usato per eliminare i dati di una sezione specifica quando viene nuovamente eseguita. |Nome di colonna di una colonna con tipo di dati binario (32). |No |

## <a name="troubleshooting-tips"></a>Suggerimenti per la risoluzione dei problemi
### <a name="problem-1-net-framework-data-provider"></a>Problema 1: provider di dati .NET Framework

Viene visualizzato il **messaggio di errore** seguente:

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .Net Framework Data Provider. It may not be installed”.  

**Possibili cause:**

1. Non è stato installato il Provider di dati Framework .NET per Oracle.
2. Il Provider di dati Framework .NET per Oracle è stato installato nel Framework .NET 2.0 e non è disponibile nelle cartelle del Framework.NET 4.0.

**Risoluzione/Soluzione alternativa:**

1. Se non è stato installato il provider .NET per Oracle, [installarlo](http://www.oracle.com/technetwork/topics/dotnet/downloads/) e ripetere lo scenario.
2. Se viene visualizzato il messaggio di errore anche dopo l'installazione del provider, eseguire questa procedura:
   1. Aprire la configurazione del computer di .NET 2.0 dalla cartella: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
   2. Cercare **Provider di dati Oracle per .NET** per trovare una voce, come illustrato nell'esempio seguente in **system.data** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Provider di dati Oracle per .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />"
3. Copiare questa voce nel file .config del computer nella seguente cartella v4.0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config e modificare la versione a 4.xxx.x.x.
4. Installare "<percorso di installazione di ODP.NET>\11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" nella Global Assembly Cache eseguendo `gacutil /i [provider path]`.## Suggerimenti per la risoluzione dei problemi

### <a name="problem-2-datetime-formatting"></a>Problema 2: formattazione di DateTime

Viene visualizzato il **messaggio di errore** seguente:

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Risoluzione/Soluzione alternativa:**

Potrebbe essere necessario modificare la stringa di query nell'attività di copia in base al modo in cui le date sono configurate nel database Oracle, come illustrato nell'esempio seguente (usando la funzione to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Mapping dei tipi per Oracle
Come accennato nell'articolo sulle [attività di spostamento dei dati](data-factory-data-movement-activities.md) , l'attività di copia esegue conversioni automatiche da tipi di origine a tipi di sink con l'approccio seguente in 2 passaggi:

1. Conversione dai tipi di origine nativi al tipo .NET
2. Conversione dal tipo .NET al tipo di sink nativo

Quando si spostano dati da Oracle, vengono usati i mapping seguenti dal tipo di dati Oracle al tipo .NET e viceversa.

| Tipo di dati Oracle | Tipo di dati .NET Framework |
| --- | --- |
| BFILE |Byte[] |
| BLOB |Byte[] |
| CHAR |String |
| CLOB |String |
| DATE |DateTime |
| FLOAT |Decimal |
| INTEGER |Decimal |
| INTERVAL YEAR TO MONTH |Int32 |
| INTERVAL DAY TO SECOND |Intervallo di tempo |
| LONG |String |
| LONG RAW |Byte[] |
| NCHAR |String |
| NCLOB |String |
| NUMBER |Decimal |
| NVARCHAR2 |String |
| RAW |Byte[] |
| ROWID |String |
| TIMESTAMP |DateTime |
| TIMESTAMP WITH LOCAL TIME ZONE |DateTime |
| TIMESTAMP WITH TIME ZONE |DateTime |
| UNSIGNED INTEGER |NUMBER |
| VARCHAR2 |String |
| XML |string |

> [!NOTE]
> I tipi di dati **INTERVAL YEAR TO MONTH** e **INTERVAL DAY TO SECOND** non sono supportati quando si usa il driver Microsoft.
>

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Ottimizzazione delle prestazioni
Per informazioni sui fattori chiave che influiscono sulle prestazioni dello spostamento dei dati, ovvero dell'attività di copia, in Azure Data Factory e sui vari modi per ottimizzare tali prestazioni, vedere la [Guida alle prestazioni delle attività di copia e all'ottimizzazione](data-factory-copy-activity-performance.md).



<!--HONumber=Feb17_HO1-->


