---
title: Debug delle applicazioni Azure in Eclipse
description: Informazioni sul debug delle applicazione Azure mediante Azure Toolkit per Eclipse.
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 3d1b34bf-399e-421e-bdcc-da2d422dbb4f
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: ff60ebaddd3a7888cee612f387bd0c50799496ac
ms.openlocfilehash: 04e6a43e478484d90478e4aa90d489ea4eb0cc68


---
# <a name="debugging-azure-applications-in-eclipse"></a>Debug delle applicazioni Azure in Eclipse
Azure Toolkit per Eclipse consente di eseguire il debug delle applicazioni, indipendentemente dal fatto che siano in esecuzione in Azure o nell'emulatore di calcolo se si usa Windows come sistema operativo. La figura seguente mostra la finestra di dialogo delle proprietà **Debugging** usata per abilitare il debug remoto:

![][ic719504]

In questa esercitazione si presuppone che sia già stata creata correttamente un'applicazione e che si abbia familiarità con l'emulatore di calcolo e con la distribuzione in Azure.

Come punto di partenza per questo argomento verrà usata l'applicazione creata nell'esercitazione relativa all'[uso della libreria di runtime del servizio Azure in JSP][Using the Azure Service Runtime Library in JSP]. Se non è stato ancora fatto, creare l'applicazione prima di continuare.

## <a name="to-debug-your-application-while-running-in-azure"></a>Per eseguire il debug dell'applicazione durante l'esecuzione in Azure
> [!WARNING]
> Il supporto attuale del toolkit per il debug remoto Java è destinato principalmente alle distribuzioni in esecuzione nell'emulatore di calcolo di Azure. Poiché la connessione di debug non è sicura, è consigliabile non abilitare il debug remoto in distribuzioni di produzione. Se è necessario eseguire il debug di un'applicazione in esecuzione sul cloud di Azure, usare una distribuzione di gestione temporanea. Occorre tuttavia ricordare che l'accesso non autorizzato alla sessione di debug è possibile se altri utenti conoscono l'indirizzo IP della distribuzione cloud, anche se si tratta di una distribuzione di gestione temporanea.
> 
> 

1. Compilare il progetto per il test nell'emulatore: in Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse su **MyAzureProject**, scegliere **Properties** (Proprietà), fare clic su **Azure** e impostare **Build for** (Compila per) su **Deployment to cloud** (Distribuzione nel cloud).
2. Ricompilare il progetto: scegliere **Project** (Progetto) dal menu di Eclipse, quindi fare cli su **Build All** (Compila tutto).
3. Distribuire l'applicazione in *gestione temporanea* in Azure.
    >[AZURE.IMPORTANT] Come indicato in precedenza, è consigliabile eseguire il debug nell'emulatore di calcolo nella maggior parte dei casi, quindi eseguire il debug nell'ambiente di gestione temporanea solo se è necessario debug aggiuntivo. È consigliabile non eseguire il debug nell'ambiente di produzione.
4. Quando la distribuzione è pronta in Azure, ottenerne il nome DNS dal [portale di gestione di Azure][Azure Management Portal]. Il nome DNS di una distribuzione di staging ha il formato http://*guid&lt;&gt;*.cloudapp.net, dove *&lt;guid&gt;* è un valore GUID assegnato da Azure.
5. In Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse su **WorkerRole1**, scegliere **Azure** e quindi fare clic su **Debugging** (Debug).
6. Nella finestra di dialogo **Properties for WorkerRole1 Debugging** :
   1. Selezionare **Enable remote debugging for this role**
   2. Per **Input endpoint to use** (Endpoint di input da usare) usare **Debugging (public:8090, private:8090)** (Debug (pubblico:&8090;, privato:&8090;)).
   3. Assicurarsi che l'opzione **Start JVM in suspended mode, waiting for a debugger connection** sia deselezionata.
       >[AZURE.IMPORTANT] L'opzione **Start JVM in suspended mode, waiting for a debugger connection** è destinata solo a scenari di debug avanzati nell'emulatore di calcolo (non alle distribuzioni cloud). Se viene usata l'opzione **Start JVM in suspended mode, waiting for a debugger connection** , il processo di avvio del server verrà sospeso fino alla connessione del debugger Eclipse alla rispettiva JVM. Anche se è possibile usare questa opzione per una sessione di debug tramite l'emulatore di calcolo, è consigliabile non usarla per una sessione di debug in una distribuzione cloud. L'inizializzazione di un server viene eseguita in un'attività di avvio di Azure e il cloud di Azure rende disponibili gli endpoint pubblici solo dopo il completamento dell'attività di avvio. Un processo di avvio non verrà quindi completato correttamente se questa opzione viene abilitata in una distribuzione cloud, poiché non sarà in grado di ricevere una connessione da un client di Eclipse esterno.
   4. Fare clic su **Create Debug Configurations**.
7. Nella finestra di dialogo **Azure Debug Configuration** :
   1. Per **Java project to debug** (Progetto Java di cui eseguire il debug) selezionare il progetto **MyHelloWorld**.
   2. Per **Configure debugging for** (Configura debug per) selezionare **Azure cloud (staging)** (Cloud di Azure (staging)).
   3. Assicurarsi che l'opzione **Azure compute emulator** sia deselezionata.
   4. Per **Host** immettere il nome DNS della pre-distribuzione, senza il prefisso **http://**. Ad esempio (usare il GUID specifico invece del GUID mostrato qui): **4e616d65-6f6e-6d65-6973-526f62657274.cloudapp.net**
8. Fare clic su **OK** per chiudere la finestra di dialogo **Azure Debug Configuration** (Configurazione di debug di Azure).
9. Fare clic su **OK** per chiudere la finestra di dialogo **Properties for WorkerRole1 Debugging** (Proprietà per il debug di WorkerRole1).
10. Se non è stato ancora impostato un punto di interruzione in index.jsp, impostarlo:
    1. In Project Explorer (Esplora progetti) di Eclipse espandere **MyHelloWorld**, quindi **WebContent** e infine fare doppio clic su **index.jsp**.
    2. In index.jsp fare clic con il pulsante destro del mouse sulla barra blu a destra del codice Java e quindi scegliere **Toggle Breakpoints** (Attiva/disattiva punti di interruzione), come illustrato di seguito:  ![][ic551537]
11. In Eclipse fare clic su **Run** (Esegui) e quindi su **Debug Configurations** (Configurazioni di debug).
12. Nella finestra di dialogo **Debug Configurations** (Configurazioni di debug) espandere **Remote Java Application** (Applicazione Java remota) nel riquadro di sinistra, selezionare **Azure Cloud (WorkerRole1)** (Cloud di Azure (WorkerRole1)) e infine fare clic su **Debug**.
13. Nel browser eseguire l'applicazione di staging, **http://***&lt;guid&gt;***.cloudapp.net/MyHelloWorld**, sostituendo il GUID dal nome DNS per *&lt;guid&gt;*. Se richiesto da una finestra di dialogo **Confirm Perspective Switch** (Conferma il cambio di prospettiva), fare clic su **Yes** (Sì). La sessione di debug dovrebbe ora essere eseguita fino alla riga di codice in cui è stato impostato il punto di interruzione.

> [!NOTE]
> Se si tenta di avviare una connessione di debug remoto in una distribuzione in cui sono in esecuzione più istanze del ruolo, non sarà possibile controllare l'istanza a cui il debugger verrà connesso inizialmente, poiché il bilanciamento del carico di Azure sceglierà un'istanza in modo casuale. Dopo la connessione all'istanza, si continuerà a eseguire il debug della stessa istanza. Si noti anche che in caso di un periodo di inattività superiore a 4 minuti (ad esempio in caso di arresto in corrispondenza di un punto di interruzione per troppo tempo) è possibile che Azure chiuda la connessione.
> 
> 

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Debug di un'istanza del ruolo specifica in una distribuzione a più istanze
Quando la distribuzione è in esecuzione nel cloud, è probabile che sia eseguita in più istanze di calcolo o del ruolo. Questo permette di sfruttare la garanzia di disponibilità di Azure per il 99,95% del tempo e di scalare orizzontalmente l'applicazione.

In questi scenari potrebbe essere necessario eseguire il debug remoto dell'applicazione Java in un'istanza del ruolo specifica. Se tuttavia si abilita solo un endpoint di input normale per il debug, il bilanciamento del carico di Azure renderà praticamente impossibile la connessione del debugger a un'istanza del ruolo specifica. Effettuerà invece la connessione a un'istanza del ruolo scelta casualmente.

In questo tipo di scenario l'uso degli endpoint di input dell'istanza semplificherà l'esecuzione del debug di un'istanza del ruolo specifica.

Si supponga di pianificare l'esecuzione di un massimo di 5 istanze del ruolo della distribuzione. Usando la pagina delle proprietà **Endpoints** nella finestra di dialogo delle proprietà del ruolo, creare un endpoint di input dell'istanza e assegnare all'endpoint un intervallo di porte pubbliche, invece di un singolo numero di porta. Ad esempio, nella casella di input **Porta pubblica** specificare **81-85**.

Dopo la distribuzione dell'applicazione con questo endpoint dell'istanza, Azure assegnerà un numero di porta univoco da questo intervallo a ogni istanza del ruolo. Per scoprire a quale istanza è stato assegnato un numero di porta specifico, è quindi possibile usare la variabile di ambiente *InstanceEndpointName***_PUBLICPORT**, dove *InstanceEndpointName* è il nome assegnato durante la creazione dell'endpoint dell'istanza, configurata automaticamente dal toolkit nella distribuzione, ad esempio restituendo il rispettivo valore nel piè di pagina di una pagina Web, in modo che sia possibile leggerlo quando si passa alla pagina.

Quando si conosce il numero di porta pubblica assegnato all'istanza, sarà possibile farvi riferimento nella configurazione di debug in Eclipse, aggiungendolo alla fine del nome host del servizio. Questo permetterà al debugger di Eclipse di connettersi a tale istanza specifica e non alle altre istanze.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Solo Windows: per eseguire il debug dell'applicazione durante l'esecuzione nell'emulatore di calcolo
> [!NOTE]
> L'emulatore di Azure è disponibile solo per Windows. Ignorare questa sezione se si usa un sistema operativo diverso da Windows. 
> 
> 

1. Compilare il progetto per il test nell'emulatore: in Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse su **MyAzureProject**, scegliere **Properties** (Proprietà), fare clic su **Azure** e impostare **Build for** (Compila per) su **Testing in emulator** (Test nell'emulatore).
2. Ricompilare il progetto: scegliere **Project** (Progetto) dal menu di Eclipse, quindi fare cli su **Build All** (Compila tutto).
3. In Project Explorer (Esplora progetti) di Eclipse fare clic con il pulsante destro del mouse su **WorkerRole1**, scegliere **Azure** e quindi fare clic su **Debugging** (Debug).
4. Nella finestra di dialogo **Properties for WorkerRole1 Debugging** :
   1. Selezionare **Enable remote debugging for this role**
   2. Per **Input endpoint to use** (Endpoint di input da usare), usare l'endpoint predefinito generato automaticamente dal toolkit, elencato come **Debugging (public:8090, private:8090)** (Debug (pubblico:&8090;, privato:&8090;)).
   3. Assicurarsi che l'opzione **Start JVM in suspended mode, waiting for a debugger connection** sia deselezionata.
       >[AZURE.IMPORTANT] L'opzione **Start JVM in suspended mode, waiting for a debugger connection** è destinata solo a scenari di debug avanzati nell'emulatore di calcolo (non alle distribuzioni cloud). Se viene usata l'opzione **Start JVM in suspended mode, waiting for a debugger connection** , il processo di avvio del server verrà sospeso fino alla connessione del debugger Eclipse alla rispettiva JVM. Anche se è possibile usare questa opzione per una sessione di debug tramite l'emulatore di calcolo, è consigliabile non usarla per una sessione di debug in una distribuzione cloud. L'inizializzazione di un server viene eseguita in un'attività di avvio di Azure e il cloud di Azure rende disponibili gli endpoint pubblici solo dopo il completamento dell'attività di avvio. Un processo di avvio non verrà quindi completato correttamente se questa opzione viene abilitata in una distribuzione cloud, poiché non sarà in grado di ricevere una connessione da un client di Eclipse esterno.
   4. Fare clic su **Create Debug Configurations**.
5. Nella finestra di dialogo **Azure Debug Configuration** :
   1. Per **Java project to debug** (Progetto Java di cui eseguire il debug) selezionare il progetto **MyHelloWorld**.
   2. Per **Configure debugging for** (Configura debug per) selezionare **Azure compute emulator** (Emulatore di calcolo di Azure).
6. Fare clic su **OK** per chiudere la finestra di dialogo **Azure Debug Configuration** (Configurazione di debug di Azure).
7. Fare clic su **OK** per chiudere la finestra di dialogo **Properties for WorkerRole1 Debugging** (Proprietà per il debug di WorkerRole1).
8. Impostare un punto di interruzione in index.jsp:
   
   1. In Project Explorer (Esplora progetti) di Eclipse espandere **MyHelloWorld**, quindi **WebContent** e infine fare doppio clic su **index.jsp**.
   2. In index.jsp fare clic con il pulsante destro del mouse sulla barra blu a destra del codice Java e quindi scegliere **Toggle Breakpoints** (Attiva/disattiva punti di interruzione), come illustrato di seguito:  ![][ic551537]
      
      Il punto di interruzione è impostato se viene visualizzata un'icona di interruzione nella barra blu a sinistra del codice Java.
9. Avviare l'applicazione nell'emulatore di calcolo facendo clic sul pulsante **Run in Azure Emulator** sulla barra degli strumenti di Azure.
10. In Eclipse fare clic su **Run** (Esegui) e quindi su **Debug Configurations** (Configurazioni di debug).
11. Nella finestra di dialogo **Debug Configurations** (Configurazioni di debug) espandere **Remote Java Application** (Applicazione Java remota) nel riquadro di sinistra, selezionare **Azure Emulator (WorkerRole1)** (Emulatore di Azure (WorkerRole1)) e infine fare clic su **Debug**.
12. Quando l'emulatore di calcolo indica che l'applicazione è in esecuzione, nel browser eseguire **http://localhost:8080/MyHelloWorld**.
     Se richiesto da una finestra di dialogo **Confirm Perspective Switch** (Conferma il cambio di prospettiva), fare clic su **Yes** (Sì).
     La sessione di debug dovrebbe ora essere eseguita fino alla riga di codice in cui è stato impostato il punto di interruzione.

Questa sezione ha illustrato come eseguire il debug nell'emulatore di calcolo. La sezione successiva illustra come eseguire il debug di un'applicazione distribuita in Azure.

## <a name="debugging-notes"></a>Note sul debug
* Dopo il debug, è possibile passare da **Debug** a **Java** tramite il menu di Eclipse, facendo clic su **Window** (Finestra), **Open Perspective** (Apri prospettiva) e selezionando la prospettiva da usare.
* Per abilitare il debug remoto in GlassFish, non usare la funzionalità di configurazione del debug remoto di Azure Toolkit per Eclipse, ma configurare GlassFish manualmente. A causa del modo in cui GlassFish gestisce le opzioni Java predefinite nelle variabili di ambiente, la funzionalità di configurazione del debug remoto del toolkit non funziona correttamente con GlassFish. Se la funzionalità di configurazione del debug remoto del toolkit è abilitata, potrebbe impedire l'avvio di GlassFish.

## <a name="see-also"></a>Vedere anche
[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]

[Creare un'applicazione Hello World per Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Installazione di Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse] 

Per altre informazioni su come usare Azure con Java, vedere il [Centro per sviluppatori Java di Azure][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Using the Azure Service Runtime Library in JSP]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->



<!--HONumber=Jan17_HO1-->


