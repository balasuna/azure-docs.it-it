---
title: "Impostazione di proprietà e metadati durante il processo di Importazione/Esportazione di Azure | Documentazione Microsoft"
description: "Informazioni su come specificare le proprietà e i metadati da impostare nei BLOB di destinazione quando si esegue lo strumento Importazione/Esportazione di Azure per preparare le unità."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 41bc5585f3d9dea2a08dc8a6bc1e4fdf9f0c8fc4
ms.openlocfilehash: 2f597da2c056a2b4ea2af751e955fafdd7016744


---

# <a name="setting-properties-and-metadata-during-the-import-process"></a>Impostazione di proprietà e metadati durante il processo di importazione

Quando si esegue lo strumento Importazione/Esportazione di Microsoft Azure per preparare le unità, è possibile specificare le proprietà e i metadati da impostare nei BLOB di destinazione. A tale scopo, seguire questa procedura:

1.  Per impostare le proprietà dei BLOB, creare nel computer locale un file di testo che specifichi i nomi e i valori delle proprietà.
2.  Per impostare i metadati dei BLOB, creare nel computer locale un file di testo che specifichi i nomi e i valori dei metadati.
3.  Passare il percorso completo di uno di questi file o di entrambi allo strumento Importazione/Esportazione di Azure durante l'operazione `PrepImport`.

> [!NOTE]
>  Quando si specifica un file di proprietà o di metadati durante una sessione di copia, tali proprietà o metadati vengono impostati per ogni BLOB importato durante tale sessione di copia. Per specificare un set diverso di proprietà o metadati per alcuni dei BLOB da importare, sarà necessario creare una sessione di copia separata con file di proprietà o metadati diversi.

## <a name="specify-blob-properties-in-a-text-file"></a>Specificare le proprietà dei BLOB in un file di testo

Per specificare le proprietà dei BLOB, creare un file di testo locale e includere un codice XML che specifichi i nomi delle proprietà come elementi e i valori delle proprietà come valori. Ecco un esempio che specifica alcuni valori delle proprietà:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

Salvare il file in un percorso locale, ad esempio `C:\WAImportExport\ImportProperties.txt`.

## <a name="specify-blob-metadata-in-a-text-file"></a>Specificare i metadati dei BLOB in un file di testo

Allo stesso modo, per specificare i metadati dei BLOB, creare un file di testo locale che specifichi i nomi dei metadati come elementi e i valori dei metadati come valori. Ecco un esempio che specifica alcuni valori dei metadati:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export Service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

Salvare il file in un percorso locale, ad esempio `C:\WAImportExport\ImportMetadata.txt`.

## <a name="add-the-path-to-properties-and-metadata-files-in-datasetcsv"></a>Aggiungere il percorso dei file di proprietà e di metadati in dataset.csv

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="next-steps"></a>Passaggi successivi

[Import-Export Service Metadata and Properties File format](storage-import-export-file-format-metadata-and-properties.md) (Formato dei file di proprietà e metadati del servizio Importazione/Esportazione)



<!--HONumber=Dec16_HO3-->


