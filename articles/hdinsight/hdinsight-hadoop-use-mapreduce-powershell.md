---
title: Usare MapReduce e PowerShell con Hadoop | Documentazione Microsoft
description: "Informazioni su come usare PowerShell per eseguire in modalità remota processi MapReduce con Hadoop in HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: f3be777497d842f019c1904ec1990bd1f1213ba2
ms.openlocfilehash: c2bed4f1fddf99183faa0730f052ee79cf77f9f8


---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>Esecuzione di processi MapReduce con Hadoop in HDInsight mediante PowerShell

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Questo documento fornisce un esempio di come usare Azure PowerShell per eseguire un processo MapReduce in un cluster Hadoop in HDInsight.

## <a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Prerequisiti

Per seguire la procedura descritta in questo articolo, è necessario quanto segue:

* **Un cluster Azure HDInsight (Hadoop in HDInsight)**

  > [!IMPORTANT]
  > Linux è l'unico sistema operativo usato in HDInsight versione 3.4 o successiva. Per altre informazioni, vedere [HDInsight deprecato in Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* **Workstation con Azure PowerShell**.

## <a name="a-idpowershellarun-a-mapreduce-job-using-azure-powershell"></a><a id="powershell"></a>Eseguire un processo MapReduce con Azure PowerShell

Azure PowerShell fornisce *cmdlet* che consentono di eseguire in modalità remota processi MapReduce in HDInsight. Questo risultato si ottiene internamente usando chiamate REST a [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (chiamato in precedenza Templeton) in esecuzione nel cluster HDInsight.

Durante l'esecuzione di processi MapReduce in un cluster HDInsight remoto, vengono usati i seguenti cmdlet.

* **Login-AzureRmAccount**: autentica Azure PowerShell nella sottoscrizione di Azure.

* **New-AzureRmHDInsightMapReduceJobDefinition**: crea una nuova *definizione di processo* usando le informazioni MapReduce specificate.

* **Start-AzureRmHDInsightJob**: invia la definizione del processo a HDInsight, avvia il processo e restituisce un oggetto *job* che può essere usato per verificare lo stato del processo.

* **Wait-AzureRmHDInsightJob**: usa l'oggetto job per verificare lo stato del processo. Attende che il processo venga completato o che scada il periodo di attesa previsto.

* **Get-AzureRmHDInsightJobOutput**: viene usato per recuperare l'output del processo.

La seguente procedura illustra come usare questi cmdlet per eseguire un processo nel cluster HDInsight.

1. Usando un editor, salvare il seguente codice come **mapreducejob.ps1**.
    
    ```powershell
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount
    }

    # Get cluster info
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    $creds=Get-Credential -Message "Enter the login for the cluster"

    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
    -ResourceGroupName $resourceGroup)[0].Value

    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey

    #Define the MapReduce job
    #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
    # -JarFile = the JAR containing the MapReduce application
    # -ClassName = the class of the application
    # -Arguments = The input file, and the output directory
    $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
        -ClassName "wordcount" `
        -Arguments `
            "wasb:///example/data/gutenberg/davinci.txt", `
            "wasb:///example/data/WordCountOutput"

    #Submit the job to the cluster
    Write-Host "Start the MapReduce job..." -ForegroundColor Green
    $wordCountJob = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $wordCountJobDefinition `
        -HttpCredential $creds

    #Wait for the job to complete
    Write-Host "Wait for the job to complete..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
        -Blob 'example/data/WordCountOutput/part-r-00000' `
        -Container $container `
        -Destination output.txt `
        -Context $context
    # Print the output of the job.
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds
    ```

2. Quindi, aprire un nuovo prompt dei comandi di **Azure PowerShell** . Passare al percorso del file **mapreducejob.ps1** , quindi usare il seguente comando per eseguire lo script.
   
        .\mapreducejob.ps1
   
    Quando si esegue lo script viene chiesto il nome del cluster HDInsight e il nome dell'account Admin/HTTPS e la password per il cluster. Potrebbe anche essere richiesta l'autenticazione alla sottoscrizione di Azure.

3. Al termine del processo, viene visualizzato un output simile al seguente:
    
        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071
    
    Questo output indica che il processo è stato completato correttamente.
    
    > [!NOTE]
    > Se **ExitCode** è un valore diverso da 0, vedere [Risoluzione dei problemi](#troubleshooting).
    
    Questo esempio consente anche di archiviare i file scaricati in un file **output.txt** nella directory da cui viene eseguito lo script.

### <a name="view-output"></a>Visualizzare l’output

Per vedere le parole e i numeri generati dal processo, aprire il file **output.txt** in un editor di testo.

> [!NOTE]
> I file di output di un processo MapReduce non sono modificabili. Se pertanto si esegue di nuovo l'esempio, è necessario cambiare il nome del file di output.

## <a name="a-idtroubleshootingatroubleshooting"></a><a id="troubleshooting"></a>Risoluzione dei problemi

Se al termine del processo non vengono restituite informazioni, potrebbe essersi verificato un errore durante l'elaborazione. Per visualizzare informazioni relative all'errore per questo processo, aggiungere il seguente comando alla fine del file **mapreducejob.ps1** , salvare il file, quindi eseguirlo nuovamente.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Vengono restituite le informazioni scritte in STDERR nel server durante l'esecuzione del processo. Tali informazioni possono essere utili per determinare la causa del problema.

## <a name="a-idsummaryasummary"></a><a id="summary"></a>Riepilogo

Come è possibile notare, Azure PowerShell fornisce un modo semplice per eseguire processi MapReduce in un cluster HDInsight, monitorare lo stato del processo e recuperare l'output.

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Passaggi successivi

Per informazioni generali sui processi MapReduce in HDInsight:

* [Usare MapReduce in HDInsight Hadoop](hdinsight-use-mapreduce.md)

Per informazioni su altre modalità d'uso di Hadoop in HDInsight:

* [Usare Hive con Hadoop in HDInsight](hdinsight-use-hive.md)
* [Usare Pig con Hadoop in HDInsight](hdinsight-use-pig.md)




<!--HONumber=Jan17_HO3-->


