---
title: Creare la prima applicazione di microservizi di Azure | Documentazione Microsoft
description: Creare, distribuire ed eseguire il debug di un&quot;applicazione Infrastruttura di servizi con Visual Studio
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/14/2016
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 7033955fa9c18b2fa1a28d488ad5268d598de287
ms.openlocfilehash: 23699d8d44ccd101519920e5f20e9b13cd15cc38


---
# <a name="create-your-first-azure-service-fabric-application"></a>Creare la prima applicazione di Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
> 
> 

Service Fabric SDK include un componente aggiuntivo per Visual Studio che fornisce i modelli e gli strumenti per creare, eseguire il debug e distribuire applicazioni di Service Fabric. Questo argomento descrive la procedura per creare la prima applicazione in Visual Studio.

## <a name="prerequisites"></a>Prerequisiti
Prima di iniziare, assicurarsi di avere [configurato l'ambiente di sviluppo](service-fabric-get-started.md).

## <a name="video-walkthrough"></a>Procedura dettagliata video
Il video seguente illustra i passaggi di questa esercitazione:

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Creating-your-first-Service-Fabric-application-in-Visual-Studio/player]
> 
> 

## <a name="create-the-application"></a>Creazione dell'applicazione
Un'applicazione Infrastruttura di servizi può contenere uno o più servizi, ognuno dei quali contribuisce alle funzionalità dell'applicazione con un ruolo specifico. Creare un progetto di applicazione, oltre al primo progetto di servizio, usando la procedura guidata Nuovo progetto. Sarà anche possibile aggiungere altri servizi in un secondo momento.

1. Avviare Visual Studio come amministratori.
2. Scegliere **File > Nuovo progetto > Cloud > Applicazione di Service Fabric**.
3. Assegnare un nome all'applicazione e fare clic su **OK**.
   
    ![Finestra di dialogo Nuovo progetto in Visual Studio][1]
4. Nella pagina successiva scegliere **Con stato** come primo tipo di servizio da includere nell'applicazione. Assegnare un nome al servizio e fare clic su **OK**.
   
    ![Finestra di dialogo Nuovo servizio in Visual Studio][2]
   
   > [!NOTE]
   > Per altre informazioni sulle opzioni, vedere [Panoramica dei modelli di programmazione di Service Fabric](service-fabric-choose-framework.md).
   > 
   > 
   
    Visual studio crea il progetto di applicazione e il progetto di servizio con stato e li visualizza in Esplora soluzioni.
   
    ![Esplora soluzioni dopo la creazione dell'applicazione con servizio con stato][3]
   
    Il progetto di applicazione non contiene codice direttamente, ma fa riferimento a un set di progetti di servizio. Include inoltre altri tre tipi di contenuto:
   
   * **Profili di pubblicazione**: usati per gestire le preferenze relative agli strumenti per diversi ambienti.
   * **Script**: include uno script di PowerShell per distribuire o aggiornare l'applicazione. Visual Studio usa lo script in background. può essere richiamato direttamente dalla riga di comando.
   * **Definizione di applicazione**: include il manifesto dell'applicazione in *ApplicationPackageRoot*. I file dei parametri dell'applicazione associati sono disponibili in *ApplicationParameters*, definiscono l'applicazione e consentono di configurarla appositamente per un ambiente specifico.
     
     Per una panoramica del contenuto del progetto di servizio, vedere la [Guida introduttiva a Reliable Services](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Distribuire ed eseguire il debug dell'applicazione
A questo punto, è possibile provare a eseguire l'applicazione creata.

1. Premere F5 in Visual Studio per compilare l'applicazione per il debug.
   
   > [!NOTE]
   > La distribuzione richiederà alcuni minuti, perché Visual Studio crea un cluster locale per lo sviluppo. Un cluster locale esegue lo stesso codice della piattaforma che viene creato in un cluster con più macchine virtuali, ma in una singola macchina virtuale. Lo stato della creazione del cluster verrà visualizzato nella finestra di output di Visual Studio.
   > 
   > 
   
    Quando il cluster è pronto, si riceverà una notifica dall'applicazione di gestione della barra delle applicazioni inclusa nell'SDK.
   
    ![Notifica della barra delle applicazioni per il cluster locale][4]
2. All'avvio dell'applicazione Visual Studio apre automaticamente il visualizzatore eventi di diagnostica in cui è possibile visualizzare l'output di traccia del servizio.
   
    ![Visualizzatore eventi di diagnostica][5]
   
    Nel caso del modello di servizio con stato, i messaggi indicano semplicemente l'incremento del valore del contatore nel metodo `RunAsync` del file MyStatefulService.cs.
3. Espandere uno degli eventi per visualizzare altri dettagli, incluso il nodo in cui viene eseguito il codice. In questo caso, è il _Node_2, anche se nel computer locale potrebbe essere diverso.
   
    ![Dettaglio del visualizzatore eventi di diagnostica][6]
   
    Il cluster locale include cinque nodi ospitati in un singolo computer. Simula un cluster di cinque nodi, ognuno dei quali risiede in un computer distinto. Per simulare la perdita di un computer durante l'esecuzione del debugger di Visual Studio, portare offline uno dei nodi del cluster locale.
   
   > [!NOTE]
   > Gli eventi di diagnostica dell'applicazione inviati dal modello di progetto usano la classe `ServiceEventSource` inclusa. Per altre informazioni, vedere [Monitorare e diagnosticare servizi in una configurazione di sviluppo con computer locale](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).
   > 
   > 
4. Nel progetto di servizio trovare la classe che deriva da StatefulService, ad esempio MyStatefulService, e impostare un punto di interruzione sulla prima riga del metodo `RunAsync` .
   
    ![Punto di interruzione nel metodo RunAsync del servizio con stato][7]
5. Per avviare Service Fabric Explorer, fare clic con il pulsante destro del mouse sull'app dell'area di notifica Local Cluster Manager (Gestione cluster locale) e scegliere **Manage Local Cluster** (Gestisci cluster locale).
   
    ![Avvio di Service Fabric Explorer da Local Cluster Manager][systray-launch-sfx]
   
    Service Fabric Explorer offre una rappresentazione visiva del cluster, incluso il set di applicazioni distribuite al suo interno e il set di nodi fisici che lo costituiscono. Per altre informazioni su Service Fabric Explorer, vedere [Visualizzare il cluster con Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
6. Nel riquadro sinistro espandere **Cluster > Nodi** e individuare il nodo in cui è in esecuzione il codice.
7. Fare clic su **Azioni > Disattiva (riavvio)** per simulare il riavvio di una macchina virtuale. In alternativa, disattivare il nodo dalla vista elenco dei nodi nel riquadro a sinistra.
   
    ![Arresto di un nodo in Service Fabric Explorer][sfx-stop-node]
   
    Verrà visualizzato il raggiungimento del punto di interruzione in Visual Studio mentre viene effettuato il failover del calcolo eseguito su un nodo in un altro nodo.
8. Tornare al visualizzatore eventi di diagnostica e osservare i messaggi. Il contatore ha continuato ad aumentare anche se gli eventi provengono in effetti da un nodo diverso.
   
    ![Visualizzatore eventi di diagnostica dopo il failover][diagnostic-events-viewer-detail-post-failover]

## <a name="switch-cluster-mode"></a>Cambiare la modalità cluster
Per impostazione predefinita, il cluster di sviluppo locale è configurato per essere eseguito come cluster a cinque nodi. Questo è utile per il debug dei servizi distribuiti in più nodi. La distribuzione di un'applicazione nel cluster di sviluppo a cinque nodi, tuttavia, può richiedere tempo. Per eseguire l'iterazione delle modifiche al codice rapidamente e senza eseguire l'app in cinque nodi, cambiare il cluster di sviluppo con la modalità a un nodo. Per eseguire il codice in un cluster con un nodo, fare clic con il pulsante destro del mouse su Local Cluster Manager (Gestione cluster locale) nell'area di notifica e selezionare **Switch Cluster Mode -> 1 Node** (Cambia modalità cluster -> 1 nodo).  

![Cambiare la modalità cluster][switch-cluster-mode]

Quando si cambia la modalità cluster, il cluster di sviluppo viene reimpostato e tutte le applicazioni di cui è stato effettuato il provisioning o che sono in esecuzione in tale cluster vengono rimosse.

## <a name="cleaning-up"></a>+ Cleaning up
Prima di concludere, è importante ricordare che il cluster locale è reale. L'arresto del debugger rimuove l'istanza dell'applicazione e annulla la registrazione del tipo di applicazione. L'esecuzione del cluster tuttavia continua in background. A questo scopo, sono disponibili diverse opzioni per gestire il cluster:

1. Per arrestare il cluster mantenendo i dati applicazione e le tracce, fare clic su **Stop Local Cluster** (Arresta cluster locale) nell'app dell'area di notifica.
2. Per eliminare completamente il cluster, fare clic su **Remove Local Cluster** (Rimuovi cluster locale) nell'app dell'area di notifica. Questa opzione comporterà un'altra distribuzione lenta la prossima volta che si preme F5 in Visual Studio. Eliminare il cluster se non si prevede di usare il cluster locale per un certo periodo o se è necessario recuperare risorse.

## <a name="next-steps"></a>Passaggi successivi
* Informazioni su come creare un [cluster in Azure](service-fabric-cluster-creation-via-portal.md) o un [cluster autonomo in Windows](service-fabric-cluster-creation-for-windows-server.md).
* Provare a creare un servizio con il modello di programmazione[Reliable Services](service-fabric-reliable-services-quick-start.md) o [Reliable Actors](service-fabric-reliable-actors-get-started.md).
* Provare a distribuire un [contenitore Windows](service-fabric-deploy-container.md) o un'app esistente come [eseguibile guest](service-fabric-deploy-existing-app.md).
* Informazioni su come esporre i servizi a Internet con un [front-end di servizio Web](service-fabric-add-a-web-frontend.md).
* Usare un' [esercitazione pratica](https://msdnshared.blob.core.windows.net/media/2016/07/SF-Lab-Part-I.docx) per creare un servizio senza stato, configurare i report di integrità e di monitoraggio ed eseguire un aggiornamento dell'applicazione.
* Informazioni sulle [opzioni di supporto di Service Fabric](service-fabric-support.md)

<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png



<!--HONumber=Jan17_HO4-->


