---
title: Utilizzare il portale dei servizi Web di Azure Machine Learning | Documentazione Microsoft
description: Gestione dell&quot;accesso alle aree di lavoro di Azure Machine Learning e distribuzione e gestione dei servizi Web API ML
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: jhubbard
editor: cgronlun
ms.assetid: b62cf2ca-dd2a-4a83-bb54-469f948fb026
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-donglo
translationtype: Human Translation
ms.sourcegitcommit: 66fb3dc316ce25aea4dff4add5c25b7f0f56ad7a
ms.openlocfilehash: ef24162006c508134db3f385e791306495ac4614


---
# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Gestire un servizio Web usando il portale dei servizi Web di Azure Machine Learning
Il portale dei servizi Web di Microsoft Azure Machine Learning consente di gestire i servizi Web classici e nuovi di Machine Learning. Poiché i servizi Web classici e nuovi sono basati su tecnologie diverse, sono disponibili funzionalità di gestione leggermente diverse.

Nel portale dei servizi Web di Azure Machine Learning è possibile:

* Monitorare la modalità d'uso del servizio Web.
* Configurare la descrizione, aggiornare le chiavi per il servizio Web (solo servizi nuovi), aggiornare la chiave dell'account di archiviazione (solo servizi nuovi), abilitare la registrazione e abilitare o disabilitare i dati di esempio.
* Eliminare il servizio Web.
* Creare, eliminare o aggiornare i piani di fatturazione (solo servizi nuovi).
* Aggiungere ed eliminare gli endpoint (solo servizi classici)

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Gestire i nuovi servizi Web
Per gestire i nuovi servizi Web:

1. Accedere al [portale dei servizi Web di Microsoft Azure Machine Learning](https://services.azureml.net/quickstart) usando l'account di Microsoft Azure associato alla sottoscrizione di Azure.
2. Nel menu fare clic su **Servizi Web**.

Verrà visualizzato un elenco di servizi Web distribuiti per la sottoscrizione. 

Per gestire un servizio Web, fare clic su Web Services (Servizi Web). Nella pagina Servizi Web è possibile:

* Fare clic sul servizio Web per gestirlo.
* Fare clic sul piano di fatturazione per il servizio Web per aggiornarlo.
* Eliminare un servizio Web.
* Copiare un servizio Web e distribuirlo in un'altra area.

Quando si fa clic su un servizio Web, viene visualizzata la pagina di avvio rapido del servizio Web. Questa pagina contiene due opzioni di menu che consentono di gestire il servizio Web:

* **DASHBOARD**: consente di visualizzare l'utilizzo del servizio Web.
* **CONFIGURE** (CONFIGURA): consente di aggiungere testo descrittivo, aggiornare la chiave dell'account di archiviazione associato al servizio Web e abilitare o disabilitare i dati di esempio.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Monitoraggio della modalità d'uso del servizio Web
Fare clic sulla scheda **DASHBOARD** .

Nel dashboard è possibile visualizzare l'utilizzo complessivo del servizio Web in un periodo di tempo. È possibile selezionare il periodo da visualizzare dal menu a discesa Periodo in alto a destra dei grafici di utilizzo. Il dashboard visualizza le informazioni seguenti:

* **Requests Over Time** (Richieste nel tempo) visualizza un grafico con il numero di richieste nel periodo di tempo selezionato. Può aiutare a identificare se si verificano picchi di utilizzo.
* **Request-Response Requests** (Richieste richiesta-risposta) visualizza il numero totale di chiamate di richiesta-risposta ricevute dal servizio nel periodo di tempo selezionato e il numero di richieste con errore.
* **Average Request-Response Compute Time** (Tempo medio di calcolo richiesta-risposta) visualizza una media del tempo necessario per eseguire le richieste ricevute.
* **Batch Requests** (Richieste batch) visualizza il numero totale di richieste batch ricevute dal servizio nel periodo di tempo selezionato e il numero di richieste con errore.
* **Average Job Latency** (Latenza processo media) visualizza una media del tempo necessario per eseguire le richieste ricevute.
* **Errors** (Errori) visualizza il numero complessivo di errori che si sono verificati nelle chiamate al servizio Web.
* **Services Costs** (Costi servizi) visualizza le spese per il piano di fatturazione associato al servizio.

### <a name="configuring-the-web-service"></a>Configurazione del servizio Web
Fare clic sull'opzione di menu **CONFIGURA** .

È possibile aggiornare le proprietà seguenti:

* **Description** (Descrizione) consente di immettere una descrizione per il servizio Web.
* **Title** (Titolo) consente di immettere un titolo per il servizio Web.
* **Chiavi** consente di ruotare le chiavi API primarie e secondarie.
* **Storage account key** (Chiave dell'account di archiviazione) consente di aggiornare la chiave per l'account di archiviazione associato alle modifiche del servizio Web. 
* **Enable Sample data** (Abilita dati di esempio) consente di fornire dati di esempio che è possibile usare per testare il servizio di richiesta-risposta. Se il servizio Web è stato creato in Machine Learning Studio, i dati di esempio vengono prelevati dai dati usati per il training del modello. Se il servizio è stato creato a livello di codice, i dati vengono ricavati dai dati di esempio forniti come parte del pacchetto JSON.

### <a name="managing-billing-plans"></a>Gestione dei piani di fatturazione
Fare clic sull'opzione di menu **Piani** della pagina Avvio rapido dei servizi Web. È anche possibile fare clic sul piano associato al servizio Web specifico per gestire tale piano.

* **Nuovo** consente di creare un nuovo piano.
* **Add/Remove Plan instance** (Aggiungi/Rimuovi istanza di piano) consente di aumentare un piano esistente per aggiungere capacità.
* **Upgrade/DownGrade** (Esegui aggiornamento/downgrade) consente di aumentare un piano esistente per aggiungere capacità.
* **Elimina** consente di eliminare un piano.

Fare clic su un piano per visualizzare il relativo dashboard. Il dashboard indica l'uso dello snapshot o del piano in un periodo di tempo selezionato. Per selezionare il periodo di tempo da visualizzare, fare clic sull'elenco a discesa **Period** (Periodo) nella parte superiore destra del dashboard. 

Il dashboard del piano contiene le informazioni seguenti:

* **Descrizione piano** visualizza le informazioni sui costi e le capacità associate al piano.
* **Plan Usage** (Uso piano) visualizza il numero di transazioni e di ore di calcolo addebitate in base al piano.
* **Web Services** (Servizi Web) visualizza il numero di servizi Web che usano il piano.
* **Top Web Service By Calls** (Primi servizi Web per chiamate) visualizza i primi quattro servizi Web che stanno effettuando chiamate per cui viene applicato un addebito in base al piano.
* **Top Web Services by Compute Hrs** (Primi servizi Web per ore di calcolo) visualizza i primi quattro servizi Web che stanno usando le risorse di calcolo per cui viene applicato un addebito in base al piano.

## <a name="manage-classic-web-services"></a>Gestire i servizi Web classici
> [!NOTE]
> Le procedure di questa sezione si riferiscono alla gestione dei servizi Web classici nel portale dei servizi Web di Azure Machine Learning. Per informazioni sulla gestione dei servizi Web classici tramite Machine Learning Studio e il portale di Azure classico, vedere [Gestire un'area di lavoro di Azure Machine Learning](machine-learning-manage-workspace.md).
> 
> 

Per gestire i servizi Web classici:

1. Accedere al [portale dei servizi Web di Microsoft Azure Machine Learning](https://services.azureml.net/quickstart) usando l'account di Microsoft Azure associato alla sottoscrizione di Azure.
2. Fare clic su **Classic Web Services** (Servizi Web classici).

Per gestire un servizio Web classico, fare clic su **Classic Web Services**(Servizi Web classici). Nella pagina relativa ai servizi Web classici è possibile:

* Fare clic sul servizio Web per visualizzare gli endpoint associati.
* Eliminare un servizio Web.

Quando si gestisce un servizio Web classico, si gestisce ognuno degli endpoint separatamente. Quando si fa clic su un servizio Web nella pagina dei servizi Web, viene aperto l'elenco degli endpoint associati al servizio. 

Nella pagina relativa agli endpoint dei servizi Web classici è possibile aggiungere ed eliminare gli endpoint del servizio. Per altre informazioni sull'aggiunta di endpoint, vedere [Creazione di endpoint](machine-learning-create-endpoint.md).

Fare clic su uno degli endpoint per aprire la pagina di avvio rapido del servizio Web. Questa pagina contiene due opzioni di menu che consentono di gestire il servizio Web:

* **DASHBOARD**: consente di visualizzare l'utilizzo del servizio Web.
* **CONFIGURE** (CONFIGURA): consente di aggiungere testo descrittivo, attivare e disattivare la registrazione degli errori, aggiornare la chiave dell'account di archiviazione associato al servizio Web e abilitare e disabilitare i dati di esempio.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Monitoraggio della modalità d'uso del servizio Web
Fare clic sulla scheda **DASHBOARD** .

Nel dashboard è possibile visualizzare l'utilizzo complessivo del servizio Web in un periodo di tempo. È possibile selezionare il periodo da visualizzare dal menu a discesa Periodo in alto a destra dei grafici di utilizzo. Il dashboard visualizza le informazioni seguenti:

* **Requests Over Time** (Richieste nel tempo) visualizza un grafico con il numero di richieste nel periodo di tempo selezionato. Può aiutare a identificare se si verificano picchi di utilizzo.
* **Request-Response Requests** (Richieste richiesta-risposta) visualizza il numero totale di chiamate di richiesta-risposta ricevute dal servizio nel periodo di tempo selezionato e il numero di richieste con errore.
* **Average Request-Response Compute Time** (Tempo medio di calcolo richiesta-risposta) visualizza una media del tempo necessario per eseguire le richieste ricevute.
* **Batch Requests** (Richieste batch) visualizza il numero totale di richieste batch ricevute dal servizio nel periodo di tempo selezionato e il numero di richieste con errore.
* **Average Job Latency** (Latenza processo media) visualizza una media del tempo necessario per eseguire le richieste ricevute.
* **Errors** (Errori) visualizza il numero complessivo di errori che si sono verificati nelle chiamate al servizio Web.
* **Services Costs** (Costi servizi) visualizza le spese per il piano di fatturazione associato al servizio.

### <a name="configuring-the-web-service"></a>Configurazione del servizio Web
Fare clic sull'opzione di menu **CONFIGURA** .

È possibile aggiornare le proprietà seguenti:

* **Description** (Descrizione) consente di immettere una descrizione per il servizio Web. La descrizione è un campo obbligatorio.
* **Logging** (Registrazione) consente di abilitare o disabilitare la registrazione nell'endpoint. Per altre informazioni sulla registrazione, vedere [Abilitare la registrazione per i servizi Web di Machine Learning](machine-learning-web-services-logging.md).
* **Enable Sample data** (Abilita dati di esempio) consente di fornire dati di esempio che è possibile usare per testare il servizio di richiesta-risposta. Se il servizio Web è stato creato in Machine Learning Studio, i dati di esempio vengono prelevati dai dati usati per il training del modello. Se il servizio è stato creato a livello di codice, i dati vengono ricavati dai dati di esempio forniti come parte del pacchetto JSON.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Concedere o sospendere l'accesso ai servizi Web per gli utenti nel portale
Usando il portale di Azure classico, è possibile consentire o negare l'accesso a utenti specifici.

### <a name="access-for-users-of-new-web-services"></a>Accesso per gli utenti dei nuovi servizi Web
Per consentire ad altri utenti di usare i servizi Web nel portale dei servizi Web di Azure Machine Learning, è necessario aggiungere gli utenti come coamministratori nella sottoscrizione di Azure.

Accedere al [portale di Azure classico](https://manage.windowsazure.com/) usando l'account di Microsoft Azure associato alla sottoscrizione di Azure.

1. Nel riquadro di spostamento sinistro fare clic su **Impostazioni** e quindi su **Amministratori**.
2. Nella parte inferiore della finestra fare clic su **Aggiungi**. 
3. Nella finestra di dialogo AGGIUNGI COAMMINISTRATORE digitare l'indirizzo di posta elettronica della persona che si vuole aggiungere come coamministratore e quindi selezionare la sottoscrizione a cui il coamministratore potrà accedere.
4. Fare clic su **Save**.

### <a name="access-for-users-of-classic-web-services"></a>Accesso per gli utenti dei servizi Web classici
Per gestire un'area di lavoro:

Accedere al [portale di Azure classico](https://manage.windowsazure.com/) usando l'account di Microsoft Azure associato alla sottoscrizione di Azure.

1. Nel riquadro dei servizi di Microsoft Azure fare clic su **MACHINE LEARNING**.
2. Fare clic sull'area di lavoro da gestire.
3. Fare clic sulla scheda **CONFIGURE** .

Nella scheda relativa alla configurazione sospendere l'accesso all'area di lavoro di Machine Learning facendo clic su **NEGA**. Gli utenti non saranno più in grado di aprire l'area di lavoro in Machine Learning Studio. Per ripristinare l'accesso, fare clic su **CONSENTI**.

Per utenti specifici:

Per gestire account aggiuntivi che hanno accesso all'area di lavoro in Machine Learning Studio, fare clic su **Accedi a ML Studio** nella scheda **DASHBOARD**. Verrà aperta l'area di lavoro in Machine Learning Studio. A questo punto, fare clic sulla scheda **SETTINGS** (IMPOSTAZIONI) e quindi su **USERS** (UTENTI). È possibile fare clic su **INVITE MORE USERS** (INVITA ALTRI UTENTI) per concedere agli utenti l'accesso all'area di lavoro oppure selezionare un utente e fare clic su **REMOVE** (RIMUOVI).

> [!NOTE]
> Il link all’ **Accesso a ML Studio** consente di aprire Machine Learning Studio usando l'account Microsoft con cui è stato eseguito l'accesso. L'account Microsoft usato per accedere al portale di Azure classico per creare un'area di lavoro non è automaticamente autorizzato ad aprire tale area di lavoro. Per aprire un'area di lavoro, è necessario essere connessi con l'account Microsoft definito come proprietario dell'area di lavoro oppure ricevere un invito dal proprietario per partecipare all'area di lavoro.
> 
> 




<!--HONumber=Jan17_HO5-->


