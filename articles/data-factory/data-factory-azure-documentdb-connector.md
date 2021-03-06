---
title: Spostare dati in/da DocumentDB | Documentazione Microsoft
description: Informazioni su come spostare i dati da e verso la raccolta di Azure DocumentDB mediante Data factory di Azure
services: data-factory, documentdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: f0592824bc5296a4c6e5781d43746c09d80609f9
ms.openlocfilehash: 622f5547dee171d1b3f0a0cb65cba375d5478476


---
# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Spostare dati da e verso DocumentDB mediante Data factory di Azure
Questo articolo illustra come usare l'attività di copia in una data factory di Azure per spostare i dati in Azure DocumentDB da un altro archivio dati e spostare i dati da Azure DocumentDB in un altro archivio dati. Questo articolo si basa sull'articolo [Attività di spostamento dei dati](data-factory-data-movement-activities.md) , che offre una panoramica generale dello spostamento dei dati con attività di copia e delle combinazioni di archivio dati supportate.

Gli esempi seguenti mostrano come copiare dati da e in Azure DocumentDB e nell'archivio BLOB di Azure. I dati possono tuttavia essere copiati **direttamente** da qualsiasi origine a qualsiasi sink supportato. Per altre informazioni, vedere la sezione "Archivi dati e formati supportati" in [Spostare dati con l'attività di copia](data-factory-data-movement-activities.md).  

> [!NOTE]
> La copia dei dati dagli archivi dati IaaS di Azure/locali in Azure DocumentDB e viceversa è supportata con Gateway di gestione dati 2.1 e versioni successive.
>
>

## <a name="supported-versions"></a>Versioni supportate
Questo connettore DocumentDB supporta la copia dei dati da e verso raccolte DocumentDB a singola partizione e raccolte partizionate. [DocDB per MongoDB](../documentdb/documentdb-protocol-mongodb.md) non è supportata.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Esempio: Copiare i dati da DocumentDB a BLOB di Azure
L'esempio seguente mostra:

1. Un servizio collegato di tipo [DocumentDB](#azure-documentdb-linked-service-properties).
2. Un servizio collegato di tipo [AzureStorage](data-factory-azure-blob-connector.md).
3. Un [set di dati](data-factory-create-datasets.md) di input di tipo [DocumentDbCollection](#azure-documentdb-dataset-type-properties).
4. Un [set di dati](data-factory-create-datasets.md) di output di tipo [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5. Una [pipeline](data-factory-create-pipelines.md) con attività di copia che usa [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) e [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Nell'esempio vengono copiati dati da Azure DocumentDB a BLOB di Azure. Le proprietà JSON usate in questi esempi sono descritte nelle sezioni riportate dopo gli esempi.

**Servizio collegato di Azure DocumentDB:**

```JSON
{
  "name": "DocumentDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Servizio collegato di archiviazione BLOB di Azure:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Set di dati di input di Azure DocumentDB:**

L'esempio presuppone di avere una raccolta denominata **Person** in un database di Azure DocumentDB.

Impostando "external" su "true" e specificando i criteri externalData si comunica al servizio Data factory di Azure che la tabella è esterna e non è prodotta da un'attività al suo interno.

```JSON
{
  "name": "PersonDocumentDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "DocumentDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Set di dati di output del BLOB di Azure:**

I dati vengono copiati in un nuovo BLOB ogni ora e il percorso del BLOB riflette la data e l'ora specifiche con granularità oraria.

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
Documento JSON di esempio nella raccolta Person in un database di DocumentDB:

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
DocumentDB supporta l’esecuzione di query di documenti utilizzando una sintassi come SQL su documenti JSON gerarchici.

Esempio: 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

La pipeline seguente copia i dati dalla raccolta Person nel database di DocumentDB a un BLOB di Azure. Come parte dell'attività di copia, i set di dati di input e output sono stati specificati.  

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonDocumentDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Esempio: Copiare i dati dal BLOB di Azure ad Azure DocumentDB
L'esempio seguente mostra:

1. Un servizio collegato di tipo [DocumentDB](#azure-documentdb-linked-service-properties).
2. Un servizio collegato di tipo [AzureStorage](data-factory-azure-blob-connector.md).
3. Un [set di dati](data-factory-create-datasets.md) di input di tipo [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Un [set di dati](data-factory-create-datasets.md) di output di tipo [DocumentDbCollection](#azure-documentdb-dataset-type-properties).
5. Una [pipeline](data-factory-create-pipelines.md) con attività di copia che usa [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) e [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).

Nell’esempio vengono copiati dati dal BLOB di Azure ad Azure DocumentDB. Le proprietà JSON usate in questi esempi sono descritte nelle sezioni riportate dopo gli esempi.

**Servizio collegato di archiviazione BLOB di Azure:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Servizio collegato di Azure DocumentDB:**

```JSON
{
  "name": "DocumentDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Set di dati di input del BLOB di Azure:**

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
**Set di dati di output di Azure DocumentDB:**

Nell'esempio vengono copiati dati a una raccolta denominata "Person".

```JSON
{
  "name": "PersonDocumentDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "DocumentDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
La pipeline seguente copia i dati dal BLOB di Azure alla raccolta Person in DocumentDB. Come parte dell'attività di copia, i set di dati di input e output sono stati specificati.

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          }
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonDocumentDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
Se l’input BLOB d’esempio è come

```
1,John,,Doe
```
Quindi l'output JSON in DocumentDB sarà come:

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
DocumentDB è un archivio NoSQL per i documenti JSON, dove sono consentite strutture nidificate. Azure Data Factory consente di indicare una gerarchia tramite **nestingSeparator**, ovvero "." in questo esempio. Con il separatore, l'attività copia genererà l'oggetto "Name" con tre elementi figlio First, Middle e Last, in base a "Name.First", "Name.Middle" e "Name.Last" nella definizione della tabella.

## <a name="azure-documentdb-linked-service-properties"></a>Proprietà del servizio collegato di Azure DocumentDB
La tabella seguente fornisce la descrizione degli elementi JSON specifici del servizio collegato di Azure DocumentDB.

| **Proprietà** | **Descrizione** | **Obbligatorio** |
| --- | --- | --- |
| type |La proprietà del tipo deve essere impostata su: **DocumentDb** |Sì |
| connectionString |Specificare le informazioni necessarie per connettersi al database di Azure DocumentDB. |Sì |

## <a name="azure-documentdb-dataset-type-properties"></a>Proprietà del tipo del set di dati di Azure DocumentDB
Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione di set di dati, fare riferimento all'articolo [Creazione di set di dati](data-factory-create-datasets.md). Le sezioni come struttura, disponibilità e criteri di un set di dati JSON sono simili per tutti i tipi di set di dati, ad esempio Azure SQL, BLOB di Azure, tabelle di Azure e così via.

La sezione typeProperties è diversa per ogni tipo di set di dati e contiene informazioni sulla posizione dei dati nell'archivio dati. La sezione typeProperties per il set di dati di tipo **DocumentDbCollection** presenta le proprietà seguenti.

| **Proprietà** | **Descrizione** | **Obbligatorio** |
| --- | --- | --- |
| collectionName |Nome della raccolta documenti di DocumentDB. |Sì |

Esempio:

```JSON
{
  "name": "PersonDocumentDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "DocumentDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a>Schema da Data Factory
Per gli archivi di dati privi di schema, ad esempio DocumentDB, il servizio Data Factory deduce lo schema in uno dei modi seguenti:  

1. Se si specifica la struttura dei dati tramite la proprietà **structure** nella definizione del set di dati, il servizio Data Factory considera la struttura come schema. In questo caso, se una riga non contiene un valore per una colonna, verrà inserito un valore null.
2. Se non si specifica la struttura dei dati usando la proprietà **structure** nella definizione del set di dati, il servizio Data Factory deduce lo schema usando la prima riga di dati. In questo caso, se la prima riga non contiene lo schema completo, alcune colonne non saranno presenti nel risultato dell'operazione di copia.

Di conseguenza, per le origini dati prive di schema, la procedura consigliata consiste nello specificare la struttura dei dati usando la proprietà **structure** .

## <a name="azure-documentdb-copy-activity-type-properties"></a>Proprietà del tipo di attività di copia di Azure DocumentDB
Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione delle attività, fare riferimento all'articolo sulla [creazione di pipeline](data-factory-create-pipelines.md). Per tutti i tipi di attività sono disponibili proprietà come nome, descrizione, tabelle di input e output e criteri.

> [!NOTE]
> L'attività di copia accetta solo un input e produce solo un output.

Le proprietà disponibili nella sezione typeProperties dell'attività variano invece per ogni tipo di attività e in caso di attività di copia variano in base ai tipi di origini e ai sink.

In caso di attività di copia con origine di tipo **DocumentDbCollectionSource**, sono disponibili le proprietà seguenti nella sezione **typeProperties**:

| **Proprietà** | **Descrizione** | **Valori consentiti** | **Obbligatorio** |
| --- | --- | --- | --- |
| query |Specificare la query per leggere i dati. |Stringa di query supportata da DocumentDB. <br/><br/>Esempio: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |No <br/><br/>Se non specificato, l'istruzione SQL eseguita: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Carattere speciale per indicare che il documento è nidificato |Qualsiasi carattere. <br/><br/>DocumentDB è un archivio NoSQL per i documenti JSON, dove sono consentite strutture nidificate. Azure Data Factory consente di indicare una gerarchia tramite nestingSeparator, ovvero "." negli esempi precedenti. Con il separatore, l'attività copia genererà l'oggetto "Name" con tre elementi figlio First, Middle e Last, in base a "Name.First", "Name.Middle" e "Name.Last" nella definizione della tabella. |No |

**DocumentDbCollectionSink** supporta le proprietà seguenti:

| **Proprietà** | **Descrizione** | **Valori consentiti** | **Obbligatorio** |
| --- | --- | --- | --- |
| nestingSeparator |È necessario un carattere speciale nel nome della colonna di origine per indicare tale documento nidificato. <br/><br/>Per l'esempio sopra: `Name.First` nella tabella di output produce la struttura JSON seguente nel documento di DocumentDB:<br/><br/>"Name": {<br/>    "First": "John"<br/>}, |Carattere utilizzato per separare i livelli di nidificazione.<br/><br/>Il valore predefinito è `.` (punto). |Carattere utilizzato per separare i livelli di nidificazione. <br/><br/>Il valore predefinito è `.` (punto). |
| writeBatchSize |Numero di richieste in parallelo per il servizio DocumentDB per creare documenti.<br/><br/>È possibile ottimizzare le prestazioni quando si copiano dati da e verso DocumentDB usando questa proprietà. È possibile prevedere prestazioni migliori quando si aumenta writeBatchSize, poiché vengono inviate più richieste in parallelo a DocumentDB. Tuttavia è necessario evitare la limitazione che può generare il messaggio di errore: "La frequenza delle richieste è troppo elevata".<br/><br/>La limitazione è dovuta a diversi fattori, inclusi la dimensione dei documenti, il numero di termini nei documenti, i criteri di indicizzazione della raccolta di destinazione, ecc. Per le operazioni di copia, è possibile usare una raccolta migliore, ad esempio S3, per disporre della massima velocità effettiva disponibile, ovvero 2500 unità di richiesta al secondo. |Integer |No (valore predefinito: 5) |
| writeBatchTimeout |Tempo di attesa per il completamento dell’operazione prima del timeout. |Intervallo di tempo<br/><br/>  Ad esempio: "00:30:00" (30 minuti). |No |

## <a name="importexport-json-documents"></a>Importare/esportare documenti JSON
Usando questo connettore DocumentDB, è possibile:

* Importare i documenti JSON da diverse origini in DocumentDB, tra cui BLOB di Azure, Azure Data Lake, file system locale o altri archivi di file supportati da Azure Data Factory.
* Esportare documenti JSON da raccolte DocumentDB in diversi archivi basati su file
* Eseguire la migrazione dei dati tra due raccolte DocumentDB così come sono

Per ottenere questa copia senza schema, non specificare la sezione "structure" nel set di dati di input o nella proprietà "nestingSeparator" nell'origine/sink di DocumentDB in attività di copia. Vedere la sezione "Specifica formato" nell'argomento relativo al connettore basato su file all'interno dei dettagli sulla configurazione del formato JSON.

## <a name="appendix"></a>Appendice
1. **Domanda:**
    c'è l'aggiornamento del supporto di attività di copia dei record esistenti?

    **Risposta:**
    no.
2. **Domanda:**
    come fa un nuovo tentativo di una copia del DocumentDB ad affrontare i record copiati?

    **Risposta:**
    se i record dispongono di un campo "ID" e l'operazione di copia tenta di inserire un record con lo stesso ID, l'operazione di copia genera un errore.  
3. **Domanda:**
    il Data Factory supporta [il partizionamento dei dati basato su hash o sull'intervallo](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)?

    **Risposta:**
    no.
4. **Domanda:**
    è possibile specificare più di una raccolta di DocumentDB per una tabella?

    **Risposta:**
    no. In questo momento, è possibile specificare solo una raccolta.

## <a name="performance-and-tuning"></a>Ottimizzazione delle prestazioni
Per informazioni sui fattori chiave che influiscono sulle prestazioni dello spostamento dei dati, ovvero dell'attività di copia, in Azure Data Factory e sui vari modi per ottimizzare tali prestazioni, vedere la [Guida alle prestazioni delle attività di copia e all'ottimizzazione](data-factory-copy-activity-performance.md).



<!--HONumber=Dec16_HO3-->


