---
title: Usare Archivio Azure Data Lake con Apache Storm in HDInsight
description: Informazioni su come scrivere dati nell&quot;Archivio Azure Data Lake da una topologia Apache Storm in HDInsight. Questo documento e l&quot;esempio associato illustrano come usare il componente HdfsBolt per scrivere nell&quot;Archivio Data Lake.
services: hdinsight
documentationcenter: na
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 1df98653-a6c8-4662-a8c6-5d288fc4f3a6
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 279990a67ae260b09d056fd84a12160150eb4539
ms.openlocfilehash: 0342c13e48d3f3605dcc169523d7d8d2d7aedba8


---
# <a name="use-azure-data-lake-store-with-apache-storm-with-hdinsight-java"></a>Usare Azure Data Lake Store con Apache Storm in HDInsight (Java)

Archivio Azure Data Lake è un servizio di archiviazione cloud compatibile con HDFS che garantisce una velocità effettiva elevata, disponibilità, durata e affidabilità per i dati. Questo documento illustra come usare una topologia Storm basata su Java per scrivere dati in un Archivio Azure Data Lake con il componente [HdfsBolt](http://storm.apache.org/releases/1.0.2/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) , fornito con Apache Storm.

> [!IMPORTANT]
> La topologia di esempio usata in questo documento si basa su componenti inclusi nei cluster Storm in HDInsight. Se usata con altri cluster Apache Storm, potrebbe essere necessario modificarla per il funzionamento con Archivio Azure Data Lake.

## <a name="how-to-work-with-azure-data-lake-store"></a>Usare Azure Data Lake Store

Per HDInsight, Data Lake Store è un file system compatibile con HDFS e di conseguenza è possibile usare il bolt Storm-HDFS per scrivervi. Il codice seguente illustra come è possibile usare il bolt Storm-HDFS per scrivere dati in una directory denominata `/stormdata` su un account Data Lake Store denominato `MYDATALAKE`.

```java
// 1. Create sync and rotation policies to control when data is synched
//    (written) to the file system and when to roll over into a new file.
SyncPolicy syncPolicy = new CountSyncPolicy(1000);
FileRotationPolicy rotationPolicy = new FileSizeRotationPolicy(0.5f, Units.KB);
// 2. Set the format. In this case, comma delimited
RecordFormat recordFormat = new DelimitedRecordFormat().withFieldDelimiter(",");
// 3. Set the directory name. In this case, '/stormdata/'
FileNameFormat fileNameFormat = new DefaultFileNameFormat().withPath("/stormdata/");
// 4. Create the bolt using the previously created settings,
//    and also tell it the base URL to your Data Lake Store.
// NOTE! Replace 'MYDATALAKE' below with the name of your data lake store.
HdfsBolt adlsBolt = new HdfsBolt()
    .withFsUrl("adl://MYDATALAKE.azuredatalakestore.net/")
        .withRecordFormat(recordFormat)
        .withFileNameFormat(fileNameFormat)
        .withRotationPolicy(rotationPolicy)
        .withSyncPolicy(syncPolicy);
// 4. Give it a name and wire it up to the bolt it accepts data
//    from. NOTE: The name used here is also used as part of the
//    file name for the files written to Data Lake Store.
builder.setBolt("ADLStoreBolt", adlsBolt, 1)
    .globalGrouping("finalcount");
```

> [!IMPORTANT]
> Quando si usa Data Lake Store in HDInsight, usare lo schema URI `adl://`.

## <a name="prerequisites"></a>Prerequisiti

* [Java JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) o versione successiva.

* [Maven 3.x](https://maven.apache.org/download.cgi)

* Un cluster Storm nella versione 3.2 di HDInsight. Per creare un nuovo cluster Storm in HDInsight, seguire la procedura descritta nel documento [Usare HDInsight con Archivio Data Lake tramite Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) . Questo documento illustra nel dettaglio i passaggi necessari per la creazione di un nuovo cluster HDInsight e di un Archivio Azure Data Lake.  
  
  > [!IMPORTANT]
  > Quando si crea il cluster HDInsight, è necessario selezionare **Storm** come tipo di cluster e **3.2** come versione. Il sistema operativo può essere Windows o Linux.

### <a name="configure-environment-variables"></a>Configurare le variabili di ambiente

Le variabili di ambiente seguenti possono essere impostate quando si installa Java e l'JDK nella workstation di sviluppo. È tuttavia necessario verificare che esistano e che contengano i valori corretti per il sistema in uso.

* **JAVA_HOME**: deve puntare alla directory in cui è installato Java Runtime Environment (JRE). In una distribuzione Unix o Linux, ad esempio, deve avere un valore simile a `/usr/lib/jvm/java-7-oracle`. In Windows, avrebbe un valore simile a `c:\Program Files (x86)\Java\jre1.7`.
* **PATH** : deve contenere i percorsi seguenti:
  
  * **JAVA\_HOME** (o percorso equivalente)
  * **JAVA\_HOME\bin** (o percorso equivalente)
  * Directory in cui è installato Maven

## <a name="topology-implementation"></a>Implementare la topologia

L'esempio usato in questo documento è scritto in Java e fa uso dei componenti seguenti:

* **TickSpout**: genera i dati usati da altri componenti nella topologia.
* **PartialCount**: conta gli eventi generati da TickSpout.
* **FinalCount**: aggrega i dati conteggiati da PartialCount.
* **ADLStoreBolt**: scrive i dati in Azure Data Lake Store usando il componente [HdfsBolt](http://storm.apache.org/releases/1.0.2/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html).

Il progetto contenente questa topologia è disponibile per il download da [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).

### <a name="understanding-adlstorebolt"></a>Informazioni su ADLStoreBolt

ADLStoreBolt è il nome usato per l'istanza di HdfsBolt nella topologia che scrive in Azure Data Lake. Non si tratta di una versione speciale di HdfsBolt creata da Microsoft, ma si basa effettivamente su valori di configurazione del sito principale, nonché su componenti Hadoop inclusi in Azure HDInsight per la comunicazione con Data Lake.

In particolare, quando si crea un cluster HDInsight, è possibile associarlo a un Archivio Azure Data Lake. Le voci vengono scritte nel sito principale per l'Archivio Azure Data Lake selezionato e vengono usate da componenti quali hadoop-client e hadoop-hdfs per consentire le comunicazioni con l'Archivio Data Lake.

> [!NOTE]
> Il contributo di codice fornito da Microsoft ai progetti Apache Hadoop e Storm consente la comunicazione con l'Archivio Azure Data Lake e con l'archivio BLOB di Azure, ma questa funzionalità potrebbe non essere inclusa per impostazione predefinita in altre distribuzioni di Hadoop e Storm.

La configurazione per HdfsBolt nella topologia è riportata di seguito:

    // 1. Create sync and rotation policies to control when data is synched
    //    (written) to the file system and when to roll over into a new file.
    SyncPolicy syncPolicy = new CountSyncPolicy(1000);
    FileRotationPolicy rotationPolicy = new FileSizeRotationPolicy(0.5f, Units.KB);
    // 2. Set the format. In this case, comma delimited
    RecordFormat recordFormat = new DelimitedRecordFormat().withFieldDelimiter(",");
    // 3. Set the directory name. In this case, '/stormdata/'
    FileNameFormat fileNameFormat = new DefaultFileNameFormat().withPath("/stormdata/");
    // 4. Create the bolt using the previously created settings,
    //    and also tell it the base URL to your Data Lake Store.
    // NOTE! Replace 'MYDATALAKE' below with the name of your data lake store.
    HdfsBolt adlsBolt = new HdfsBolt()
        .withFsUrl("adl://MYDATALAKE.azuredatalakestore.net/")
          .withRecordFormat(recordFormat)
          .withFileNameFormat(fileNameFormat)
          .withRotationPolicy(rotationPolicy)
          .withSyncPolicy(syncPolicy);
    // 4. Give it a name and wire it up to the bolt it accepts data
    //    from. NOTE: The name used here is also used as part of the
    //    file name for the files written to Data Lake Store.
    builder.setBolt("ADLStoreBolt", adlsBolt, 1)
      .globalGrouping("finalcount");

Se si ha familiarità con HdfsBolt si noterà che si tratta di una configurazione piuttosto standard, ad eccezione dell'URL, che fornisce il percorso della radice dell'Archivio Azure Data Lake.

Dal momento che la scrittura nell'Archivio Azure Data Lake usa HdfsBolt e si tratta di una semplice modifica dell'URL, dovrebbe essere possibile prendere una qualsiasi topologia esistente che scrive in HDFS o WASB usando HdfsBolt e modificarla per l'uso dell'Archivio Azure Data Lake.

## <a name="build-and-package-the-topology"></a>Compilare e creare il pacchetto della topologia

1. Scaricare il progetto di esempio da [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) nell'ambiente di sviluppo.

2. Aprire il file `StormToDataLake\src\main\java\com\microsoft\example\StormToDataLakeStore.java` in un editor e trovare la riga contenente `.withFsUrl("adl://MYDATALAKE.azuredatalakestore.net/")`. Sostituire **MYDATALAKE** con il nome dell'istanza di Azure Data Lake Store usata durante la creazione del server HDInsight.

3. Da un prompt dei comandi, un terminale o una sessione della shell, passare alla directory radice del progetto scaricato ed eseguire questi comandi per compilare e creare il pacchetto della topologia.
   
        mvn compile
        mvn package
   
    Al termine della compilazione e della creazione del pacchetto sarà presente una nuova directory denominata `target`, che contiene un file denominato `StormToDataLakeStore-1.0-SNAPSHOT.jar`, contenente a sua volta la topologia compilata.

## <a name="deploy-and-run-on-linux-based-hdinsight"></a>Distribuire ed eseguire in HDInsight basato su Linux

Se è stato creato un cluster Storm in HDInsight basato su Linux, usare la procedura seguente per distribuire ed eseguire la topologia.

1. Usare il comando seguente per copiare la topologia nel cluster HDInsight. Sostituire **USER** con il nome utente SSH usato durante la creazione del cluster. Sostituire **CLUSTERNAME** con il nome del cluster.
   
        scp target\StormToDataLakeStore-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToDataLakeStore-1.0-SNAPSHOT.jar
   
    Quando richiesto, immettere la password usata durante la creazione dell'utente SSH per il cluster. Se è stata usata una chiave pubblica anziché una password, può essere necessario usare il parametro `-i` per specificare il percorso della chiave privata corrispondente.
   
   > [!NOTE]
   > Se si sta usando un client Windows per lo sviluppo, potrebbe non essere disponibile un comando `scp`. In tal caso, è possibile usare `pscp`, disponibile nella pagina [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

2. Al termine del caricamento, usare il comando seguente per connettersi al cluster HDInsight tramite SSH. Sostituire **USER** con il nome utente SSH usato durante la creazione del cluster. Sostituire **CLUSTERNAME** con il nome del cluster.
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    Quando richiesto, immettere la password usata durante la creazione dell'utente SSH per il cluster. Se è stata usata una chiave pubblica anziché una password, può essere necessario usare il parametro `-i` per specificare il percorso della chiave privata corrispondente.
   
   > [!NOTE]
   > Se si sta usando un client Windows per lo sviluppo, seguire quanto indicato in [Connect to Linux-based HDInsight with SSH from Windows](hdinsight-hadoop-linux-use-ssh-windows.md) (Connettersi a HDInsight basato su Linux con SSH da Windows) per informazioni sull'uso del client PuTTY per connettersi al cluster.

3. Dopo la connessione, usare il comando seguente per avviare la topologia:
   
        storm jar StormToDataLakeStore-1.0-SNAPSHOT.jar com.microsoft.example.StormToDataLakeStore datalakewriter
   
    Verrà avviata la topologia con il nome descrittivo `datalakewriter`.

## <a name="deploy-and-run-on-windows-based-hdinsight"></a>Distribuire ed eseguire in HDInsight basato su Windows

1. Aprire un Web browser e passare a https://NOMECLUSTER.azurehdinsight.net, dove **NOMECLUSTER** è il nome del cluster HDInsight. Quando richiesto, specificare il nome utente amministratore (`admin`) e la password usati per questo account durante la creazione del cluster.

2. Nel Dashboard di Storm selezionare **Sfoglia** nell'elenco a discesa **File JAR** e quindi il file StormToDataLakeStore-1.0-SNAPSHOT.jar nella directory `target`. Usare i valori seguenti per le altre voci del modulo:
   
   * Nome classe: com.microsoft.example.StormToDataLakeStore
   * Parametri aggiuntivi: datalakewriter
     
  ![immagine del dashboard di storm](./media/hdinsight-storm-write-data-lake-store/submit.png)

3. Selezionare il pulsante **Invia** per caricare e avviare la topologia. Dopo l'avvio della topologia, il campo del risultato sotto il pulsante **Invia** dovrebbe riportare informazioni simili alle seguenti:
   
        Process exit code: 0
        Currently running topologies:
        Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
        -------------------------------------------------------------------
        datalakewriter       ACTIVE     68         8            10        

## <a name="view-output-data"></a>Visualizzare i dati di output

È possibile visualizzare i dati in diversi modi. In questa sezione vengono usati il portale di Azure e il comando `hdfs` per visualizzare i dati.

> [!NOTE]
> Consentire l'esecuzione delle topologie per alcuni minuti prima di controllare i dati di output, in modo che i dati vengano sincronizzati con diversi file nell'Archivio Azure Data Lake.


* **Dal [portale di Azure](https://portal.azure.com)**: selezionare nel portale l'archivio Azure Data Lake usato con HDInsight.
  
  > [!NOTE]
  > Se Data Lake Store non è stato aggiunto al dashboard del portale di Azure, è possibile trovarlo facendo clic su **Esplora** nella parte inferiore dell'elenco sulla sinistra e quindi su **Data Lake Store** e infine selezionando l'archivio.
  
    Selezionare l'icona **Esplora dati** tra quelle disponibili nella parte superiore di Data Lake Store.
  
    ![icona di esplorazione dati](./media/hdinsight-storm-write-data-lake-store/dataexplorer.png)
  
    Selezionare quindi la cartella **stormdata**. Verrà visualizzato un elenco di file di testo.
  
    ![file di testo](./media/hdinsight-storm-write-data-lake-store/stormoutput.png)
  
    Selezionare uno dei file per visualizzarne il contenuto.

* **Dal cluster**: se si è connessi al cluster HDInsight con SSH (cluster Linux) o Desktop remoto (cluster Windows), è possibile seguire questa procedura per visualizzare i dati. Sostituire **DATALAKE** con il nome dell'istanza di Data Lake Store.
  
        hdfs dfs -cat adl://DATALAKE.azuredatalakestore.net/stormdata/*.txt
  
    I file di testo archiviati nella directory verranno concatenati e verranno visualizzate informazioni simili alle seguenti:
  
        406000000
        407000000
        408000000
        409000000
        410000000
        411000000
        412000000
        413000000
        414000000
        415000000

## <a name="stop-the-topology"></a>Arrestare la topologia

Le topologie Storm vengono eseguite fino a quando non vengono arrestate o non viene eliminato il cluster. Per arrestare le topologie, usare le informazioni riportate di seguito:

**Per HDInsight basato su Linux**:

Da una sessione SSH al cluster, usare il comando seguente:

    storm kill datalakewriter

**Per HDInsight basato su Windows**:

1. Nel Dashboard di Storm (https://NOMECLUSTER.azurehdinsight.net) selezionare il collegamento relativo all'**interfaccia utente di Storm** nella parte superiore della pagina.

2. Al termine del caricamento dell'interfaccia utente di Storm, selezionare il collegamento **datalakewriter**.
   
    ![collegamento a datalakewriter](./media/hdinsight-storm-write-data-lake-store/selecttopology.png)

3. Nella sezione **Topology Actions** (Azioni di topologia) selezionare **Kill** (Termina) e quindi fare clic su OK nella finestra di dialogo visualizzata.
   
    ![azioni della topologia](./media/hdinsight-storm-write-data-lake-store/topologyactions.png)

## <a name="delete-your-cluster"></a>Eliminare il cluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Passaggi successivi

Ora che si è appreso come usare Storm per scrivere nell'Archivio Azure Data Lake, è possibile vedere altri [esempi di Storm per HDInsight](hdinsight-storm-example-topology.md).




<!--HONumber=Jan17_HO3-->


