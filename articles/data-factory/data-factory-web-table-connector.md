---
title: Spostare dati da una tabella Web usando Azure Data Factory | Documentazione Microsoft
description: Informazioni su come spostare dati da una tabella in una pagina Web con Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f54a26a4-baa4-4255-9791-5a8f935898e2
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: dd8a68029449ad013c4df9a46c558efaefd20e96
ms.openlocfilehash: 88f746a4802e13e062f4414e5d2032dfcee565b7


---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Spostare i dati da un'origine tabella Web con Azure Data Factory
Questo articolo illustra come usare l'attività di copia in una data factory di Azure per copiare dati da una tabella in una pagina Web a un altro archivio dati. Questo articolo si basa sull'articolo [Attività di spostamento dei dati](data-factory-data-movement-activities.md) , che offre una panoramica generale dello spostamento dei dati con attività di copia e delle combinazioni di archivio dati supportate.

Data Factory supporta attualmente solo lo spostamento di dati da una tabella Web ad altri archivi dati, non da altri archivi dati a una tabella Web.

> [!NOTE]
> Questo connettore Web attualmente supporta soltanto l'estrazione del contenuto della tabella da una pagina HTML.
>
>

## <a name="sample-copy-data-from-web-table-to-azure-blob"></a>Esempio: Copiare i dati da una tabella Web al BLOB di Azure
L'esempio seguente mostra:

1. Un servizio collegato di tipo [Web](#web-linked-service-properties).
2. Un servizio collegato di tipo [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service).
3. Un [set di dati](data-factory-create-datasets.md) di input di tipo [WebTable](#WebTable-dataset-properties).
4. Un [set di dati](data-factory-create-datasets.md) di output di tipo [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5. Una [pipeline](data-factory-create-pipelines.md) con attività di copia che usa [WebSource](#websource-copy-activity-type-properties) e [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Nell'esempio i dati vengono copiati da una tabella Web a un BLOB di Azure ogni ora. Le proprietà JSON usate in questi esempi sono descritte nelle sezioni riportate dopo gli esempi.

Questo esempio illustra come copiare dati da una tabella Web a un BLOB di Azure. Tuttavia, i dati possono essere copiati direttamente in uno qualsiasi dei sink indicati nell'articolo [Attività di spostamento dei dati](data-factory-data-movement-activities.md) , usando l'attività di copia in Azure Data Factory.

**Servizio collegato Web** : questo esempio usa il servizio collegato Web con l'autenticazione anonima. Per i diversi tipi di autenticazione disponibili, vedere la sezione [Servizio collegato Web](#web-linked-service-properties) .

```JSON
{
    "name": "WebLinkedService",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

**Servizio collegato Archiviazione di Azure**

```JSON
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**Set di dati di input WebTable** Impostando **external** su **true**, si comunica al servizio Data Factory che il set di dati è esterno alla data factory e non è prodotto da un'attività al suo interno.

> [!NOTE]
> Per i passaggi per ottenere l'indice di una tabella in una pagina HTML, vedere la sezione [Ottenere l'indice di una tabella in una pagina HTML](#get-index-of-a-table-in-an-html-page) .  
>
>

```JSON
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```


**Set di dati di output del BLOB di Azure**

I dati vengono scritti in un nuovo BLOB ogni ora (frequenza: ora, intervallo: 1).

```JSON
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/Movies"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```



**Pipeline con attività di copia**

La pipeline contiene un'attività di copia configurata per usare i set di dati di input e output precedenti. È programmata per essere eseguita ogni ora. Nella definizione JSON della pipeline, il tipo **source** è impostato su **WebSource** e il tipo **sink** è impostato su **BlobSink**.

Per l'elenco delle proprietà supportate da WebSource, vedere le [proprietà del tipo WebSource](#websource-copy-activity-type-properties) .

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "WebTableToAzureBlob",
        "description": "Copy from a Web table to an Azure blob",
        "type": "Copy",
        "inputs": [
          {
            "name": "WebTableInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "WebSource"
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

## <a name="web-linked-service-properties"></a>Proprietà del servizio collegato Web
La tabella seguente contiene le descrizioni degli elementi JSON specifici del servizio collegato Web.

| Proprietà | Descrizione | Obbligatorio |
| --- | --- | --- |
| type |La proprietà type deve essere impostata su: **Web** |Sì |
| Url |URL dell'origine Web |Sì |
| authenticationType |Anonima o di base. |Sì |
| userName |Nome utente dell'autenticazione di base |Sì (per l'autenticazione di base) |
| password |Password dell'autenticazione di base |Sì (per l'autenticazione di base) |

### <a name="using-anonymous-authentication"></a>Uso dell'autenticazione anonima

```JSON
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "Anonymous",
            "url" : "https://en.wikipedia.org/wiki/"
        }
    }
}
```

### <a name="using-basic-authentication"></a>Uso dell'autenticazione di base

```JSON
{
    "name": "web",
    "properties":
    {
        "type": "Web",
        "typeProperties":
        {
            "authenticationType": "basic",
            "url" : "http://myit.mycompany.com/",
            "userName": "Administrator",
            "password": "password"
        }
    }
}
```

## <a name="webtable-dataset-properties"></a>Proprietà set di dati WebTable
Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione di set di dati, vedere l'articolo sulla [creazione di set di dati](data-factory-create-datasets.md). Le sezioni come struttura, disponibilità e criteri di un set di dati JSON sono simili per tutti i tipi di set di dati, ad esempio Azure SQL, BLOB di Azure, tabelle di Azure e così via.

La sezione **typeProperties** è diversa per ogni tipo di set di dati e contiene informazioni sulla posizione dei dati nell'archivio dati. La sezione typeProperties per il set di dati di tipo **WebTable** presenta le proprietà seguenti

| Proprietà | Descrizione | Obbligatorio |
|:--- |:--- |:--- |
| type |Tipo del set di dati. Deve essere impostato su **WebTable** |Sì |
| path |URL relativo della risorsa che contiene la tabella. |No. Quando non è specificato alcun percorso, viene usato solo l'URL specificato nella definizione del servizio collegato. |
| index |Indice della tabella nella risorsa. Per i passaggi per ottenere l'indice di una tabella in una pagina HTML, vedere la sezione [Ottenere l'indice di una tabella in una pagina HTML](#get-index-of-a-table-in-an-html-page) . |Sì |

**Esempio:**

```JSON
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval":  1
        }
    }
}
```

## <a name="websource---copy-activity-type-properties"></a>Proprietà del tipo di attività di copia WebSource
Per un elenco completo delle sezioni e delle proprietà disponibili per la definizione delle attività, fare riferimento all'articolo [Creazione di pipeline](data-factory-create-pipelines.md). Per tutti i tipi di attività sono disponibili proprietà come nome, descrizione, tabelle di input e output e criteri.

Le proprietà disponibili nella sezione typeProperties dell'attività variano invece in base al tipo di attività. Per l'attività di copia variano in base ai tipi di origine e sink.

Quando l'origine nell'attività di copia è di tipo **WebSource**non sono attualmente supportate altre proprietà.

## <a name="get-index-of-a-table-in-an-html-page"></a>Ottenere l'indice di una tabella in una pagina HTML
1. Avviare **Excel 2016** e passare alla scheda **Dati**.  
2. Fare clic su **Nuova query** sulla barra degli strumenti, scegliere **Da altre origini** e fare clic su **Da Web**.

    ![Menu di Power Query](./media/data-factory-web-table-connector/PowerQuery-Menu.png)
3. Nella finestra di dialogo **Da Web** immettere l'**URL** che si intende usare nel servizio collegato JSON, ad esempio https://en.wikipedia.org/wiki/, insieme al percorso specificato per il set di dati, ad esempio AFI%27s_100_Years...100_Movies, e fare clic su **OK**.

    ![Finestra di dialogo Da Web](./media/data-factory-web-table-connector/FromWeb-DialogBox.png)

    URL usato in questo esempio: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies
4. Se viene visualizzata la finestra di dialogo **Accedi a contenuto Web**, selezionare l'**URL** corretto, l'**autenticazione** e fare clic su **Connetti**.

   ![Finestra di dialogo Accedi a contenuto Web](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. Fare clic su un elemento della **tabella** nella visualizzazione ad albero per visualizzare il contenuto dalla tabella e quindi fare clic su **Modifica** nella parte inferiore.  

   ![Finestra di dialogo Strumento di spostamento](./media/data-factory-web-table-connector/Navigator-DialogBox.png)
6. Nella finestra **Editor di query** fare clic sul pulsante **Editor avanzato** sulla barra degli strumenti.

    ![Pulsante Editor avanzato](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. Nella finestra di dialogo Editor avanzato il numero accanto a "Source" è l'indice.

    ![Editor avanzato - Indice](./media/data-factory-web-table-connector/AdvancedEditor-Index.png)

Se si usa Excel 2013, per ottenere l'indice usare [Microsoft Power Query per Excel](https://www.microsoft.com/download/details.aspx?id=39379) . Per informazioni dettagliate, vedere l'articolo [Connettersi a una pagina Web (Power Query)](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) . I passaggi sono simili se si usa [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/).

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Ottimizzazione delle prestazioni
Per informazioni sui fattori chiave che influiscono sulle prestazioni dello spostamento dei dati, ovvero dell'attività di copia, in Azure Data Factory e sui vari modi per ottimizzare tali prestazioni, vedere la [Guida alle prestazioni delle attività di copia e all'ottimizzazione](data-factory-copy-activity-performance.md).



<!--HONumber=Jan17_HO4-->


