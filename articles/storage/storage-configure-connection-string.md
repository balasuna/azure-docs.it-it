---
title: Configurare una stringa di connessione ad Archiviazione di Azure | Microsoft Docs
description: Configurare una stringa di connessione a un account di archiviazione di Azure. Una stringa di connessione include le informazioni necessarie per autenticare l&quot;accesso a un account di archiviazione dall&quot;applicazione in fase di esecuzione.
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: ecb0acb5-90a9-4eb2-93e6-e9860eda5e53
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 1a3c754bf2f2b73d0bf72cbf48b906d8085eaef1
ms.openlocfilehash: f4410c10ce66d50b64307e364e64a3367b9397f9


---
# <a name="configure-azure-storage-connection-strings"></a>Configurare le stringhe di connessione di archiviazione di Azure
## <a name="overview"></a>Panoramica
Una stringa di connessione include le informazioni necessarie per accedere ai dati in un account di archiviazione di Azure dall'applicazione in fase di esecuzione. Le stringhe di connessione possono essere configurate per:

* Connettersi all'emulatore di archiviazione di Azure.
* Accedere a un account di archiviazione in Azure.
* Accedere alle risorse specificate in Azure tramite una firma di accesso condiviso (SAS).

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Recupero della stringa di connessione
L'applicazione dovrà accedere alla stringa di connessione in fase di esecuzione per autenticare le richieste inviate al servizio Archiviazione di Azure. Sono disponibili diverse opzioni per l'archiviazione della stringa di connessione:

* Per un'applicazione in esecuzione sul desktop o in un dispositivo, è possibile archiviare la stringa di connessione in un file **app.config** o in un file **web.config**. Aggiungere la stringa di connessione alla sezione **AppSettings** .
* Se l'applicazione è un servizio cloud in esecuzione in Azure, in genere la stringa di connessione viene salvata nello [schema di configurazione dei servizi di Azure (file .cscfg)](https://msdn.microsoft.com/library/ee758710.aspx). Aggiungere la stringa di connessione alla sezione **ConfigurationSettings** del file di configurazione del servizio.
* È anche possibile includere la stringa di connessione direttamente nel codice. Per la maggior parte degli scenari, è tuttavia consigliabile archiviare la stringa di connessione in un file di configurazione.

Archiviare la stringa di connessione all'interno di un file di configurazione consente di aggiornare la stringa di connessione per alternare l'emulatore di archiviazione e un account di archiviazione di Azure nel cloud. È sufficiente modificare la stringa di connessione in modo che faccia riferimento all'ambiente di destinazione.

È possibile usare la classe [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) per accedere alla stringa di connessione in fase di esecuzione indipendentemente dall'ambiente in cui viene eseguita l'applicazione.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Creare una stringa di connessione nell'emulatore di archiviazione
[!INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Vedere [utilizzare l'emulatore di archiviazione di Azure per lo sviluppo e Testing](storage-use-emulator.md) per ulteriori informazioni sull'emulatore di archiviazione.

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Creare una stringa di connessione in un account di archiviazione di Azure
Per creare una stringa di connessione nell'account di archiviazione di Azure, usare il formato della stringa di connessione riportato di seguito. Indicare se si vuole eseguire la connessione all'account di archiviazione tramite HTTPS (opzione consigliata) o HTTP, sostituire *myAccountName* con il nome dell'account di archiviazione e sostituire *myAccountKey* con la chiave di accesso del proprio account:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Ad esempio, la stringa di connessione dovrebbe somigliare all'esempio seguente:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

> [!NOTE]
> L'archiviazione di Azure supporta sia HTTP che HTTPS in una stringa di connessione. Tuttavia, è consigliabile usare HTTPS.
>
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Creare una stringa di connessione usando una firma di accesso condiviso
[!INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Creazione di una stringa di connessione in un endpoint di archiviazione esplicito
Nella stringa di connessione è possibile specificare in modo esplicito gli endpoint di servizio anziché gli endpoint predefiniti. Per creare una stringa di connessione che specifica un endpoint esplicito, specificare l'endpoint di servizio completo per ogni servizio, inclusa la specifica del protocollo, ad esempio HTTPS (scelta consigliata) o HTTP, usando il formato seguente:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Si specificherà un endpoint esplicito se l'endpoint dell'archiviazione BLOB è stato associato a un dominio personalizzato. In questo caso, è possibile specificare l'endpoint personalizzato per l'archivio BLOB nella stringa di connessione e, facoltativamente, specificare gli endpoint predefiniti per gli altri servizi, se l'applicazione li usa.

Ecco alcuni esempi di stringhe di connessione valide che specificano un endpoint esplicito per il servizio BLOB:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=www.mydomain.com;
AccountName=storagesample;
AccountKey=account-key

# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=www.mydomain.com;
FileEndpoint=myaccount.file.core.windows.net;
QueueEndpoint=myaccount.queue.core.windows.net;
TableEndpoint=myaccount;
AccountName=storagesample;
AccountKey=account-key
```

Il valore dell'endpoint elencato nella stringa di connessione, viene usato per costruire gli URI delle richieste per il servizio BLOB, e determina la forma di qualsiasi URI restituito al codice.

Se si sceglie di omettere un endpoint di servizio dalla stringa di connessione, non sarà possibile usare la stringa di connessione per accedere ai dati in quel servizio dal codice.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Creazione di una stringa di connessione con un suffisso dell'endpoint
Per creare una stringa di connessione per il servizio di archiviazione in aree o istanze con suffissi dell'endpoint diversi, ad esempio per Azure China o Azure Governance, usare il seguente formato per la stringa di connessione. Indicare se si vuole eseguire la connessione all'account di archiviazione tramite HTTP o HTTPS, sostituire *myAccountName* con il nome dell'account di archiviazione, sostituire *myAccountKey* con la chiave di accesso del proprio account e infine sostituire *mySuffix* con il suffisso URI:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Ad esempio, la stringa di connessione avrà un aspetto simile al seguente:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Analisi di una stringa di connessione
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Passaggi successivi
* [Usare l'emulatore di archiviazione di Azure per sviluppo e test](storage-use-emulator.md)
* [Strumenti di esplorazione degli archivi di Azure](storage-explorers.md)
* [Uso delle firme di accesso condiviso](storage-dotnet-shared-access-signature-part-1.md)




<!--HONumber=Dec16_HO2-->


