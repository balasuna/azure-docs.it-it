---
title: Importare i dati in Analytics in Azure Application Insights | Documentazione Microsoft
description: Importare i dati statici per creare un join con la telemetria dell&quot;app o importare un flusso di dati separato per eseguire query con Analytics.
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 47c3491b067d5e112db589672b68e7cfc7cbe921
ms.openlocfilehash: eb89c6f485f2321f729dcfe650af4355de84a9ac


---
# <a name="import-data-into-analytics"></a>Importazione di dati in Analytics

Importare i dati tabulari in [Analytics](app-insights-analytics.md), ad esempio per creare un join con [Application Insights](app-insights-overview.md) Telemetry dall'app o altro in modo che sia possibile eseguire l'analisi come flusso separato. Analytics è un linguaggio di query avanzato ideale per l'analisi dei flussi di timestamp a volume elevato di telemetria.

È possibile importare dati in Analytics usando uno schema personalizzato. Non è necessario usare gli schemi di Application Insights standard, come la richiesta o la traccia.

Attualmente, è possibile importare file CSV (valori separati da virgola) o formati simili con valori delimitati da tabulazione o punto e virgola.

L'importazione in Analytics è utile in tre situazioni:

* **Creare un join con la telemetria dell'app.** Ad esempio, è possibile importare una tabella che associa gli URL dal sito Web a titoli di pagine più leggibili. In Analytics è possibile creare un report grafico del dashboard che mostra le dieci pagine più visitate nel sito Web. Consente ora di visualizzare i titoli delle pagine anziché gli URL.
* **Correlare la telemetria dell'applicazione** con altre origini, ad esempio il traffico di rete, i dati del server o i file di log della rete CDN.
* **Applicare Analytics a un flusso di dati separato.** Application Insights Analytics è uno strumento potente, che funziona bene con flussi di timestamp frammentati, spesso in modo più efficace di SQL. Se si dispone di tale flusso da un'altra origine, è possibile analizzarlo con Analytics.

L'invio di dati all'origine dati è semplice. 

1. (Una volta) Definire lo schema dei dati in un'origine dati.
2. (Periodicamente) Caricare i dati nell'Archiviazione di Azure e chiamare l'API REST per segnalare che nuovi dati sono in attesa per l'inserimento. Entro pochi minuti i dati sono disponibili per le query di Analytics.

La frequenza del processo di caricamento viene definita dall'utente e dalla velocità con cui si vuole rendere disponibili i dati per le query. È più efficiente caricare i dati in blocchi più grandi, ma non maggiori di 1 GB.

> [!NOTE]
> *Il numero di origini dati da analizzare è elevato?* [*Si consiglia di usare* logstash *per fornire i dati in Application Insights.*](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a>Prima di iniziare

Sono necessari:

1. Una risorsa di Application Insights in Microsoft Azure.

 * Se si vuole analizzare i dati separatamente da altra telemetria, [creare una nuova risorsa di Application Insights](app-insights-create-new-resource.md).
 * Se si sta eseguendo il join o il confronto dei dati con la telemetria di un'app già configurata con Application Insights, è possibile usare la risorsa per questa app.
 * Accesso del collaboratore o del proprietario a questa risorsa.
 
2. nell'archiviazione di Azure. Si caricano i dati in Archiviazione di Azure e Analytics recupera i dati da questa posizione. 

 * È consigliabile creare un account di archiviazione dedicato per i BLOB. Se i BLOB sono condivisi con altri processi, la lettura dei BLOB da parte dei processi richiede più tempo.

2. Quando questa funzionalità è disponibile in anteprima, è necessario chiedere l'accesso.

 * Aprire Analytics dalla risorsa di Application Insights nel [portale di Azure](https://portal.azure.com). 
 * Nella parte inferiore del riquadro dello schema fare clic sul collegamento "Contattaci" in **Altre origini dati** 
 * Se viene visualizzato "Aggiungi origine dati", l'accesso è stato eseguito.


## <a name="define-your-schema"></a>Definire lo schema

Prima di poter importare dati, è necessario definire un'*origine dati* che specifica lo schema dei dati.

1. Avviare la Creazione guidata origine dati

    ![Aggiungere la nuova origine dati](./media/app-insights-analytics-import/add-new-data-source.png)

2. Seguire le istruzioni per caricare un file di dati di esempio.

 * La prima riga dell'esempio può essere rappresentata dalle intestazioni di colonna. È possibile modificare i nomi dei campi nel passaggio successivo.
 * L'esempio deve includere almeno 10 righe di dati.

3. Esaminare lo schema che la procedura guidata ha dedotto dall'esempio. È possibile modificare i tipi derivati delle colonne, se necessario.

4. Selezionare un timestamp. Tutti i dati in Analytics devono avere un campo di timestamp. Il tipo deve essere `datetime`, ma non deve essere denominato "timestamp". Se i dati includono una colonna contenente una data e ora in formato ISO, scegliere questa opzione come colonna di timestamp. In caso contrario, scegliere "as data arrived" e il processo di importazione aggiungerà un campo di timestamp.

    ![Esaminare le schema](./media/app-insights-analytics-import/data-source-review-schema.png)

5. Creare l'origine dati.


## <a name="import-data"></a>Importa dati

Per importare i dati, caricarli in Archiviazione di Azure, creare una chiave di accesso corrispondente ed eseguire una chiamata API REST.

![Aggiungere la nuova origine dati](./media/app-insights-analytics-import/analytics-upload-process.png)

È possibile eseguire manualmente il processo seguente o configurare un sistema automatizzato per eseguire questa operazione a intervalli regolari. È necessario seguire questi passaggi per ogni blocco di dati da importare.

1. Caricare dati nell'[Archiviazione BLOB di Azure](../storage/storage-dotnet-how-to-use-blobs.md). 

 * I BLOB possono essere di qualsiasi dimensione, fino a 1 GB non compressi. I BLOB di centinaia di MB sono ideali dal punto di vista delle prestazioni.
 * È possibile comprimerli con Gzip per migliorare i tempi di caricamento e la latenza per i dati che devono essere disponibili per la query. Usare l'estensione del nome file `.gz`.
 
2. [Creare una chiave di firma di accesso condiviso per il BLOB](../storage/storage-dotnet-shared-access-signature-part-2.md). La chiave deve avere un periodo di scadenza di un giorno e fornire l'accesso in lettura.
3. Eseguire una chiamata REST per notificare ad Application Insights che i dati sono in attesa.

 * Endpoint: `https://dc.services.visualstudio.com/v2/track`
 * Metodo HTTP: POST
 * Payload:

```JSON

    {
       "data": {
            "baseType":"OpenSchemaData",
            "baseData":{
               "ver":"2",
               "blobSasUri":"<Blob URI with Shared Access Key>",
               "sourceName":"<Schema ID>",
               "sourceVersion":"1.0"
             }
       },
       "ver":1,
       "name":"Microsoft.ApplicationInsights.OpenSchema",
       "time":"<DateTime>",
       "iKey":"<instrumentation key>"
    }
```

I segnaposto sono:

* `Blob URI with Shared Access Key`: valore ottenuto dalla procedura per la creazione di una chiave. È specifico per il BLOB.
* `Schema ID`: l'ID di schema generato per lo schema definito. I dati in questo BLOB devono essere conformi allo schema.
* `DateTime`: ora in cui viene inviata la richiesta, UTC. Vengono accettati i seguenti formati: ISO8601 (ad esempio "2016-01-01 13:45:01"); RFC822 ("mer, 14 dic 16 14:57:01 +0000"); RFC850 ("mercoledì, 14-dic-16 14:57:00 UTC"); RFC1123 ("mer, 14 dic 2016 14:57:00 +0000").
* `Instrumentation key` della risorsa di Application Insights.

I dati sono disponibili in Analytics dopo alcuni minuti.

## <a name="error-responses"></a>Risposte agli errori

* **400 richiesta non valida**: indica che il payload della richiesta non è valido. Controllare:
 * Chiave di strumentazione corretta.
 * Valore dell'ora valido. L'ora dovrebbe essere in formato UTC.
 * Dati conformi allo schema.
* **403 - Accesso negato**: non è possibile accedere al BLOB inviato. Assicurarsi che la chiave di accesso condiviso sia valida e non scaduta.
* **404 - Pagina non trovata**:
 * Il BLOB non esiste.
 * Il nome dell'origine dati non è corretto.

Informazioni più dettagliate sono disponibili nel messaggio di errore della risposta.

## <a name="sample-code"></a>Codice di esempio

Questo codice usa il pacchetto NuGet [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1).

### <a name="classes"></a>Classi

```C#


namespace IngestionClient 
{ 
    using System; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceIngestionRequest 
    { 
        #region Members 
        private const string BaseDataRequiredVersion = "2"; 
        private const string RequestName = "Microsoft.ApplicationInsights.OpenSchema"; 
        #endregion Members 

        public AnalyticsDataSourceIngestionRequest(string ikey, string schemaId, string blobSasUri, int version = 1) 
        { 
            Ver = version; 
            IKey = ikey; 
            Data = new Data 
            { 
                BaseData = new BaseData 
                { 
                    Ver = BaseDataRequiredVersion, 
                    BlobSasUri = blobSasUri, 
                    SourceName = schemaId, 
                    SourceVersion = version.ToString() 
                } 
            }; 
        } 


        [JsonProperty("data")] 
        public Data Data { get; set; } 

        [JsonProperty("ver")] 
        public int Ver { get; set; } 

        [JsonProperty("name")] 
        public string Name { get { return RequestName; } } 

        [JsonProperty("time")] 
        public DateTime Time { get { return DateTime.UtcNow; } } 

        [JsonProperty("iKey")] 
        public string IKey { get; set; } 
    } 

    #region Internal Classes 

    public class Data 
    { 
        private const string DataBaseType = "OpenSchemaData"; 

        [JsonProperty("baseType")] 
        public string BaseType 
        { 
            get { return DataBaseType; } 
        } 

        [JsonProperty("baseData")] 
        public BaseData BaseData { get; set; } 
    } 


    public class BaseData 
    { 
        [JsonProperty("ver")] 
        public string Ver { get; set; } 

        [JsonProperty("blobSasUri")] 
        public string BlobSasUri { get; set; } 

        [JsonProperty("sourceName")] 
        public string SourceName { get; set; } 

        [JsonProperty("sourceVersion")] 
        public string SourceVersion { get; set; } 
    } 

    #endregion Internal Classes 
} 


namespace IngestionClient 
{ 
    using System; 
    using System.IO; 
    using System.Net; 
    using System.Text; 
    using System.Threading.Tasks; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceClient 
    { 
        #region Members 
        private readonly Uri endpoint = new Uri("https://dc.services.visualstudio.com/v2/track"); 
        private const string RequestContentType = "application/json; charset=UTF-8"; 
        private const string RequestAccess = "application/json"; 
        #endregion Members 

        #region Public 

        public async Task<bool> RequestBlobIngestion(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            HttpWebRequest request = WebRequest.CreateHttp(endpoint); 
            request.Method = WebRequestMethods.Http.Post; 
            request.ContentType = RequestContentType; 
            request.Accept = RequestAccess; 

            string notificationJson = Serialize(ingestionRequest); 
            byte[] notificationBytes = GetContentBytes(notificationJson); 
            request.ContentLength = notificationBytes.Length; 

            Stream requestStream = request.GetRequestStream(); 
            requestStream.Write(notificationBytes, 0, notificationBytes.Length); 
            requestStream.Close(); 

            HttpWebResponse response; 
            try 
            { 
                using (var response = (HttpWebResponse)await request.GetResponseAsync())
                {
                    return response.StatusCode == HttpStatusCode.OK;
                }
            } 
            catch (WebException e) 
            { 
                HttpWebResponse httpResponse = e.Response as HttpWebResponse; 
                if (httpResponse != null) 
                { 
                    Console.WriteLine( 
                        "Ingestion request failed with status code: {0}. Error: {1}", 
                        httpResponse.StatusCode, 
                        httpResponse.StatusDescription); 
                    return false; 
                }
                throw; 
            } 
        } 
        #endregion Public 

        #region Private 
        private byte[] GetContentBytes(string content) 
        { 
            return Encoding.UTF8.GetBytes(content);
        } 


        private string Serialize(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            return JsonConvert.SerializeObject(ingestionRequest); 
        } 
        #endregion Private 
    } 
} 

```

### <a name="ingest-data"></a>Inserire dati

Usare questo codice per ogni BLOB. 

```C#


   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "tableId/sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);

```

## <a name="next-steps"></a>Passaggi successivi

* [Presentazione del linguaggio di query di Analytics](app-insights-analytics-tour.md)
* [Usare *Logstash* per inviare i dati ad Application Insights](https://github.com/Microsoft/logstash-output-application-insights)



<!--HONumber=Jan17_HO3-->


