---
title: 'Risoluzione dei problemi: Creare e connettersi a un&quot;area di lavoro di Machine Learning | Documentazione Microsoft'
description: Soluzioni per problemi comuni di creazione e connessione a un&quot;area di lavoro di Azure Machine Learning
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 5e773168480bcb2b011e5539555391c06e970047


---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Guida per la risoluzione dei problemi: Creazione e connessione a un'area di lavoro di Machine Learning
Questa guida offre soluzioni per alcuni problemi frequenti relativi alla configurazione di aree di lavoro di Azure Machine Learning.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Proprietario dell'area di lavoro
Quando si crea una nuova area di lavoro di Machine Learning, l'ID immesso nel campo WORKSPACE OWNER deve essere un account Microsoft valido (in precedenza Windows Live ID), ad esempio, john-contoso@live.com o john-contoso@hotmail.com.. Non può essere un account non Microsoft, come ad esempio l'account di posta elettronica aziendale. Per creare un account Microsoft gratuito, passare a [www.live.com](http://www.live.com).

Tenere presente che l'account Microsoft usato per accedere al portale di Azure per creare un'area di lavoro non è automaticamente autorizzato ad *aprire* quell'area di lavoro, a meno di specificare quell'account come proprietario. Per aprire un'area di lavoro in Machine Learning Studio, è necessario essere connessi con l'account Microsoft definito come proprietario dell'area di lavoro oppure ricevere un invito dal proprietario per partecipare all'area di lavoro. Dal portale di Azure classico è tuttavia possibile *gestire* l'area di lavoro, con la possibilità, ad esempio, di modificare il proprietario e configurare l'accesso.

Per altre informazioni sulla gestione di un'area di lavoro, vedere [Gestire un'area di lavoro di Azure Machine Learning].

[Gestire un'area di lavoro di Azure Machine Learning]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Aree geografiche disponibili
Machine Learning è attualmente disponibile in un numero limitato di aree. Se la sottoscrizione non include una di queste aree, è possibile che venga visualizzato l'errore "Non sono presenti sottoscrizioni nelle aree consentite".

Per richiedere l'aggiunta di un'area per la sottoscrizione, selezionare **Contattare il supporto tecnico Microsoft** dal portale di Azure classico, scegliere **Fatturazione** come tipo di problema, quindi seguire le istruzioni per inviare la richiesta.

![Contattare il supporto Microsoft][screen1]

## <a name="storage-account"></a>Account di archiviazione
Il servizio Machine Learning necessita di un account di archiviazione per archiviare i dati. È possibile usare un account di archiviazione esistente oppure creare un nuovo account di archiviazione quando si crea la nuova area di lavoro di Machine Learning, se la quota disponibile è sufficiente per la creazione di un nuovo account di archiviazione.

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Creare un'area di lavoro][screen2]

Dopo la creazione di una nuova area di lavoro di Machine Learning, sarà possibile accedere a Machine Learning Studio usando l'account Microsoft specificato come proprietario dell'area di lavoro. Se viene visualizzato il messaggio di errore relativo all'area di lavoro non trovata, analogo a quanto mostrato nella schermata sottostante, usare la procedura seguente per eliminare i cookie dal browser.

![Area di lavoro non trovata][screen3]

**Per eliminare i cookie dal browser**

Se si usa Internet Explorer, fare clic sul pulsante **Strumenti** nell'angolo superiore destro, quindi selezionare **Opzioni Internet**.  

![Opzioni Internet][screen4]

Nella scheda **Generale** fare clic su **Elimina**.

![Generale][screen5]

Nella finestra di dialogo **Elimina cronologia esplorazioni** assicurarsi che l'opzione **Cookie e dati di siti Web** sia selezionata, quindi fare clic su **Elimina**.

![Eliminare i cookie][screen6]

Dopo l'eliminazione dei cookie, riavviare il browser e quindi passare alla pagina [Microsoft Azure Machine Learning](https://studio.azureml.net) . Quando sono richiesti nome utente e password, immettere lo stesso account Microsoft specificato come proprietario dell'area di lavoro.

L'obiettivo consiste nel semplificare al massimo l'esperienza di Machine Learning. Inserire eventuali commenti e problemi nel [forum su Azure Machine Learning](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) per contribuire al miglioramento del prodotto.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png



<!--HONumber=Dec16_HO2-->


