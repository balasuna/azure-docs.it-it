---
title: Creare cluster Hadoop, HBase, Storm o Spark in Linux in HDInsight con il portale | Documentazione Microsoft
description: Informazioni su come creare cluster Hadoop, HBase, Storm o Spark in Linux per HDInsight usando un browser Web e il portale di anteprima di Azure.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 697278cf-0032-4f7c-b9b2-a84c4347659e
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/05/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fe4577516a3cd34cce8fd78e22074065209035ad


---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Creare cluster basati su Linux in HDInsight tramite il portale di Azure
[!INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Il portale di Azure è uno strumento di gestione basato sul Web per servizi e risorse ospitati nel cloud di Microsoft Azure. In questo articolo si apprenderà come creare cluster HDInsight basati su Linux tramite il portale.

## <a name="prerequisites"></a>Prerequisiti
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Una sottoscrizione di Azure**. Vedere [Ottenere una versione di valutazione gratuita di Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Un Web browser moderno**. Il portale di Azure usa HTML5 e Javascript e potrebbe non funzionare correttamente nei Web browser meno recenti.

### <a name="access-control-requirements"></a>Requisiti di controllo di accesso
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Creare i cluster
Il portale di Azure espone la maggior parte delle proprietà del cluster. Con il modello di Azure Resource Manager è possibile nascondere molti dettagli. Per altre informazioni, vedere [Creare cluster Hadoop basati su Linux in HDInsight tramite modelli di Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

1. Accedere al [Portale di Azure](https://portal.azure.com).
2. Fare clic su **NUOVO**, **Analisi dei dati** e quindi su **HDInsight**.
   
    ![Creazione di un nuovo cluster nel portale di Azure](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.1.png "Creating a new cluster in the Azure portal")
3. Inserire il **Nome cluster**: il nome deve essere univoco a livello globale.
4. Fare clic su **Selezionare il tipo di cluster**, quindi selezionare:
   
   * **Tipo di cluster**: in caso di dubbi su questa opzione, scegliere **Hadoop**. È il tipo di cluster più diffuso.
     
     > [!IMPORTANT]
     > Sono disponibili molti tipi di cluster HDInsight, che corrispondono al carico di lavoro o alla tecnologia per cui è ottimizzato il cluster. Non è disponibile alcun metodo supportato per creare un cluster che combini più tipi, ad esempio Storm e HBase in un cluster. 
     > 
     > 
   * **Sistema operativo**: selezionare **Linux**.
   * **Versione**: utilizzare la versione predefinita, in caso di dubbi. Per altre informazioni, vedere [Versioni del cluster HDInsight](hdinsight-component-versioning.md).
   * **Livello del cluster**: HDInsight di Azure fornisce offerte cloud per i Big Data in due categorie: livello Standard e livello Premium. Per ulteriori informazioni, vedere [Livelli di cluster](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
     
     ![Configurazione del livello Premium HDInsight](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)
5. Fare clic su **Sottoscrizione** per selezionare la sottoscrizione di Azure utilizzata per il cluster.
6. Fare clic su **Gruppo di risorse** per selezionare un gruppo di risorse esistente o fare clic su **Nuovo** per crearne uno nuovo.
   
   > [!NOTE]
   > Questa voce sarà impostata su uno dei gruppi di risorse esistenti, se disponibili.
   > 
   > 
7. Fare clic su **Credenziali** e quindi immettere una password per l'utente amministratore. È anche necessario immettere un valore nel campo **Nome utente SSH** e un valore in **PASSWORD** o **CHIAVE PUBBLICA**, che verranno usati per autenticare l'utente SSH. Si consiglia di utilizzare una chiave pubblica. Fare clic su **Seleziona** nella parte inferiore per salvare la configurazione delle credenziali.
   
    ![Fornire le credenziali del cluster](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.3.png "Provide cluster credentials")
   
    Per altre informazioni sull'uso di SSH con HDInsight, vedere gli articoli seguenti:
   
   * [Usare SSH con Hadoop basato su Linux in HDInsight da Linux, Unix o OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
   * [Usare SSH con Hadoop basato su Linux in HDInsight da Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
8. Fare clic su **Origine dati** per scegliere un'origine dati esistente per il cluster o crearne una nuova.
   
    ![Pannello di origine dati](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.4.png "Provide data source configuration")
   
    Attualmente è possibile selezionare un account di archiviazione di Azure come origine dati per un cluster HDInsight. Usare le informazioni seguenti per comprendere le voci nel pannello **Origine dati** .
   
   * **Metodo di selezione**: impostare questo valore su **Da tutte le sottoscrizioni** per consentire l'esplorazione degli account di archiviazione da tutte le sottoscrizioni. Impostare questa voce su **Chiave di accesso** se si vuole immettere un valore nei campi **Nome archiviazione** e **Chiave di accesso** per un account di archiviazione esistente.
   * **Selezionare l'account di archiviazione/Nuovo**: fare clic su **Selezionare l'account di archiviazione** per cercare e selezionare un account di archiviazione esistente da associare al cluster. In alternativa, fare clic su **Nuovo** per creare un nuovo account di archiviazione. Usare il campo che viene visualizzato per immettere il nome dell'account di archiviazione. Se il nome è disponibile, verrà visualizzato un segno di spunta verde.
   * **Scegli contenitore predefinito**: utilizzare questa opzione per immettere il nome del contenitore predefinito da utilizzare per il cluster. È possibile immettere qualsiasi nome, è consigliabile usare lo stesso nome del cluster in modo che sia facilmente intuibile che il contenitore viene usato per tale cluster specifico.
   * **Percorso**: l'area geografica dove si trova o dove verrà creato l'account di archiviazione.
     
     > [!IMPORTANT]
     > La selezione del percorso per l'origine dati predefinito imposterà anche il percorso del cluster HDInsight. L'origine dati del cluster e l'origine dati predefinita devono trovarsi nella stessa area.
     > 
     > 
   * **Identità AAD del cluster**: configurandolo si rende accessibile il cluster agli archivi Azure Data Lake in base alla configurazione di AAD.
     
     Fare clic su **Seleziona** per salvare la configurazione dell'origine dati.
9. Fare clic su **Piani tariffari per il nodo** per visualizzare informazioni sui nodi che verranno creati per questo cluster. Impostare il numero di nodi del ruolo di lavoro necessari per il cluster. Verrà visualizzato il costo stimato del cluster all'interno del pannello.
   
    ![Pannello livelli dei prezzi di nodo](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.5.png "Specify number of cluster nodes")
   
   > [!IMPORTANT]
   > Se si prevedono più di 32 nodi di lavoro, al momento della creazione del cluster o con il ridimensionamento del cluster dopo la creazione, è necessario selezionare una dimensione del nodo head con almeno 8 core e 14 GB di RAM.
   > 
   > Per altre informazioni sulle dimensioni di nodo e i costi associati, vedere [Prezzi di HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).
   > 
   > 
   
    Fare clic su **Seleziona** per salvare la configurazione del prezzo del nodo.
10. Fare clic su **Configurazione facoltativa** per selezionare la versione del cluster, nonché configurare altre impostazioni facoltative, ad esempio l'aggiunta a una **Rete virtuale** e la configurazione di un **Metastore esterno** per contenere i dati per Hive e Oozie, usare azioni di Script per personalizzare un cluster per installare i componenti personalizzati o usare altri account di archiviazione con il cluster.
    
    * **Rete virtuale**: selezionare una rete virtuale di Azure e la subnet, se si desidera posizionare il cluster in una rete virtuale.  
      
        ![Pannello della rete virtuale](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.6.png "Specify virtual network details")
      
        Per informazioni sull'uso di HDInsight con una rete virtuale, inclusi i requisiti di configurazione specifici per la rete virtuale, vedere [Estendere le funzionalità di HDInsight usando Rete virtuale di Azure](hdinsight-extend-hadoop-virtual-network.md).
    * Fare clic su **Metastore esterni** per specificare il database SQL che si vuole usare per salvare i metadati Hive e Oozie associati al cluster.
      
      > [!NOTE]
      > La configurazione Metastore non è disponibile per i tipi di cluster HBase.
      > 
      > 
      
        ![Pannello metastore personalizzati](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.7.png "Specify external metastores")
      
        Per **Usare un database SQL esistente per i metadati Hive?** fare clic su **Sì**, selezionare un database SQL e quindi specificare il nome utente e la password per il database. Ripetere questa procedura se si desidera **Utilizzare un database SQL esistente per i metadati Oozie**. Fare clic su **Seleziona** fino a quando non si visualizza di nuovo il pannello **Configurazione facoltativa**.
      
      > [!NOTE]
      > Il database SQL di Azure usato per il metastore deve consentire la connettività ad altri servizi di Azure, incluso Azure HDInsight. Sul lato destro del dashboard del database SQL di Azure fare clic sul nome del server, cioè il server in cui è in esecuzione l'istanza di database SQL. Nella visualizzazione server fare clic su **Configura**, quindi per **Servizi di Microsoft Azure** fare clic su **Sì** e infine su **Salva**.
      > 
      > 
      
        &nbsp;
      
      > [!IMPORTANT]
      > Quando si crea un metastore personalizzato, non utilizzare un nome di database che contiene trattini o segni meno, poiché il processo di creazione del cluster non andrebbe a buon fine.
      > 
      > 
    * **Azioni script** : se si desidera usare uno script personalizzato per personalizzare un cluster durante la creazione. Per altre informazioni sulle azioni di script, vedere [Personalizzare i cluster HDInsight mediante le azioni script](hdinsight-hadoop-customize-cluster-linux.md). Nel pannello Azioni di Script fornire i dettagli, come illustrato nella schermata.
      
        ![Pannello azione di script](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.8.png "Specify script action")
    * Fare clic su **Account di archiviazione collegati** per specificare gli account di archiviazione aggiuntivi da associare al cluster. Nel pannello **Chiavi di archiviazione di Azure** fare clic su **Aggiungi una chiave di archiviazione**, quindi selezionare un account di archiviazione esistente o crearne uno nuovo.
      
        ![Pannello risorse di archiviazione aggiuntive](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.9.png "Specify additional storage accounts")
      
        Dopo aver creato un cluster, è anche possibile aggiungere altri account di archiviazione.  Vedere [Personalizzare cluster HDInsight basati su Linux tramite Azione script](hdinsight-hadoop-customize-cluster-linux.md).
      
        Fare clic su **Seleziona** fino a quando non verrà visualizzato il pannello **Nuovo cluster HDInsight**.
      
        Oltre a un account di archiviazione BLOB, è anche possibile collegare archivi Azure Data Lake. La configurazione può essere eseguita configurando AAD dall'origine dati in cui è stato configurato l'account di archiviazione predefinito e il contenitore predefinito.
11. Nel pannello **Nuovo cluster HDInsight** assicurarsi che l'opzione **Aggiungi alla Schermata iniziale** sia selezionata, quindi fare clic su **Crea**. Questo creerà il cluster e aggiungerà una sezione apposita nella schermata iniziale del portale di Azure. L'icona indica che il cluster sta eseguendo il provisioning e verrà visualizzata l'icona di HDInsight, una volta completato il provisioning.
    
    | Durante il provisioning | Provisioning completato |
    | --- | --- |
    | ![Indicatore del provisioning nella Schermata iniziale](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioning.png) |![Sezione del cluster su cui è stato effettuato il provisioning](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioned.png) |
    
    > [!NOTE]
    > La creazione del cluster richiederà del tempo, in genere circa 15 minuti. Usare il riquadro sulla schermata iniziale, o la voce **Notifiche** a sinistra della pagina per controllare il processo di provisioning.
    > 
    > 
12. Al termine della procedura di creazione, fare clic sul riquadro per il cluster dalla Schermata iniziale per avviare il pannello del cluster. Il pannello del cluster fornisce informazioni essenziali sui cluster, ad esempio nome, gruppo di risorse che a cui appartiene, percorso, sistema operativo, URL per il dashboard del cluster e così via.
    
    ![Pannello del cluster](./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.Cluster.Blade.png "Cluster properties")
    
    Usare le informazioni seguenti per comprendere le icone nella parte superiore di questo pannello e nella sezione **Informazioni di base** :
    
    * **Impostazioni** e **Tutte le impostazioni**: consentono di visualizzare il pannello **Impostazioni** per il cluster, che consente di accedere a informazioni dettagliate sulla configurazione del cluster.
    * **Dashboard**, **Dashboard cluster** e **URL**: sono tutti modi per accedere al dashboard del cluster, ovvero un portale Web per eseguire processi nel cluster.
    * **Secure Shell**: informazioni necessarie per accedere al cluster tramite SSH.
    * **Elimina**: consente di eliminare il cluster HDInsight.
    * **Avvio rapido** (![icona nuvola e lampo = avvio rapido](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)): visualizza le informazioni che consentiranno di iniziare a usare HDInsight.
    * **Utenti** (![icona utenti](./media/hdinsight-hadoop-create-linux-cluster-portal/users.png)): consente di impostare le autorizzazioni per la *gestione del portale* di questo cluster per altri utenti nella sottoscrizione di Azure.
      
      > [!IMPORTANT]
      > Questo influisce *solo* sull'accesso e sulle autorizzazioni per tale cluster nel portale di Azure e non ha alcun effetto su chi può connettersi o inviare processi al cluster HDInsight.
      > 
      > 
    * **Tag** (![icona tag](./media/hdinsight-hadoop-create-linux-cluster-portal/tags.png)): consente di impostare coppie chiave/valore per definire una tassonomia dei servizi cloud personalizzata. Ad esempio, è possibile creare una chiave denominata **progetto**e usare un valore comune per tutti i servizi associati a un progetto specifico.

## <a name="customize-clusters"></a>Personalizzare i cluster
* Vedere [Personalizzare cluster HDInsight tramite Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Vedere [Personalizzare cluster HDInsight basati su Linux tramite Azione script](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Eliminazione del cluster
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Passaggi successivi
Dopo aver creato un cluster HDInsight, usare le informazioni seguenti per acquisire familiarità con il cluster:

### <a name="hadoop-clusters"></a>Cluster Hadoop
* [Usare Hive con HDInsight](hdinsight-use-hive.md)
* [Usare Pig con HDInsight](hdinsight-use-pig.md)
* [Usare MapReduce con HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>Cluster HBase
* [Introduzione a HBase in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Sviluppare applicazioni Java per HBase in HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Cluster Storm
* [Sviluppare topologie Java per Storm in HDInsight](hdinsight-storm-develop-java-topology.md)
* [Usare i componenti di Python in Storm in HDInsight](hdinsight-storm-develop-python-topology.md)
* [Distribuire e monitorare le topologie con Storm in HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Cluster Spark
* [Creare un'applicazione autonoma con Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Eseguire i processi in modalità remota in un cluster Spark utilizzando Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark con Business Intelligence: eseguire l'analisi interattiva dei dati con strumenti di Business Intelligence mediante Spark in HDInsight](hdinsight-apache-spark-use-bi-tools.md)
* [Spark con Machine Learning: utilizzare Spark in HDInsight per stimare i risultati dell'ispezione cibo](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Streaming Spark: usare Spark in HDInsight per la creazione di applicazioni di streaming in tempo reale](hdinsight-apache-spark-eventhub-streaming.md)




<!--HONumber=Nov16_HO3-->


