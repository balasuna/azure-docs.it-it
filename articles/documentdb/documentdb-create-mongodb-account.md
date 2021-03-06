---
title: Creare un account DocumentDB da usare con le app MongoDB | Documentazione Microsoft
description: Esercitazione per imparare a creare un account DocumentDB con supporto del protocollo per MongoDB, ora disponibile in anteprima.
keywords: app MongoDB, app MongoDB
services: documentdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 413f3ae1-a68e-43be-b0d7-fa2987644f3e
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2016
ms.author: anhoh
translationtype: Human Translation
ms.sourcegitcommit: 9b83a0878f45bf3a79d00ad77965308d95f33d28
ms.openlocfilehash: 797ee3b1209e5abf545f0614c408d94391a29c8b


---

# <a name="create-a-documentdb-account-for-use-with-mongodb-apps"></a>Creare un account DocumentDB da usare con le app MongoDB
I database DocumentDB ora possono essere usati come archivio dati per le app scritte per MongoDB. Per usare questa funzionalità, sono necessari un account Azure e un account DocumentDB. Questa esercitazione illustra il processo di creazione di un account DocumentDB da usare con le app MongoDB. 

È possibile creare un account DocumentDB con supporto per MongoDB usando il portale di Azure o l'interfaccia della riga di comando di Azure con i modelli di Azure Resource Manager. Questo articolo illustra come creare un account DocumentDB con supporto per MongoDB usando il portale di Azure. Per creare un account tramite l'interfaccia della riga di comando con Azure Resource Manager, vedere [Automatizzare la creazione dell'account DocumentDB con supporto per MongoDB](documentdb-automation-resource-manager-cli.md#quick-create-documentdb-with-mongodb-api-account).

## <a name="prerequisite"></a>Prerequisito
Un account Azure. Se non si ha un account Azure, creare un [account Azure gratuito](https://azure.microsoft.com/free/) ora.
## <a name="create-a-documentdb-account"></a>Creare un account DocumentDB

1. In un browser Internet accedere al [portale di Azure](https://portal.azure.com).
2. Nel riquadro di spostamento a sinistra fare clic su **NoSQL (DocumentDB)**.

    ![Screenshot del riquadro di spostamento a sinistra del portale, in cui è evidenziata la voce NoSQL (DocumentDB)](./media/documentdb-create-mongodb-account/portalleftnav.png)

3. In alternativa, fare clic su **Altri servizi >**, digitare **DocumentDB** nella barra di ricerca in alto e fare clic su **NoSQL (DocumentDB)**.

    ![Screenshot del pannello Altri servizi, in cui viene eseguita la ricerca della voce NoSQL (DocumentDB)](./media/documentdb-create-mongodb-account/more-services-search.PNG)

4. Nella parte superiore del pannello **NoSQL (DocumentDB)** fare clic su **+ Aggiungi** nella barra delle azioni in alto.

    ![Screenshot del pulsante Aggiungi nel pannello della risorsa NoSQL (DocumentDB)](./media/documentdb-create-mongodb-account/add-documentdb-account.png)

5. Nel pannello **Account DocumentDB** specificare la configurazione per l'account.

   ![Screenshot del pannello Nuovo DocumentDB con supporto del protocollo per MongoDB](./media/documentdb-create-mongodb-account/create-documentdb-mongodb-account.PNG)

    - Nella casella **ID** immettere un nome per identificare l'account.  Quando l'**ID** viene convalidato, nella casella **ID** viene visualizzato un segno di spunta verde. Il valore **ID** diventa il nome host all'interno dell'URI. L' **ID** può contenere solo lettere minuscole, numeri e il carattere '-' e deve avere una lunghezza compresa tra 3 e 50 caratteri. Si noti che al nome endpoint scelto viene aggiunto *documents.azure.com* e il risultato finale sarà l'endpoint dell'account.

    - Per **API No SQL** selezionare **MongoDB**, che specifica l'API di comunicazione che si vuole usare per interagire con il database DocumentDB.

    - Per **Sottoscrizione**selezionare la sottoscrizione di Azure da usare per l'account. Se l'account ha solo una sottoscrizione, tale account viene selezionato per impostazione predefinita.

    - In **Gruppo di risorse**selezionare o creare un gruppo di risorse per l'account.  Per impostazione predefinita, verrà scelto un gruppo di risorse esistente nella sottoscrizione di Azure.  È tuttavia possibile scegliere di creare un nuovo gruppo di risorse a cui aggiungere l'account. Per altre informazioni, vedere [Uso del portale di Azure per gestire le risorse di Azure](../azure-portal/resource-group-portal.md).

    - Usare **Località** per specificare la posizione geografica in cui verrà ospitato l'account.

6. Dopo aver configurato le opzioni del nuovo account, fare clic su **Crea**.  La creazione dell'account può richiedere alcuni minuti.

   È possibile monitorare lo stato dall'Hub di notifica.  

   ![Schermata dell'hub Notifica, che mostra l'account DocumentDB creato](./media/documentdb-create-mongodb-account/create-documentdb-mongodb-deployment-status.png)  

7. Per accedere al nuovo account, fare clic su **DocumentDB (NoSQL)** dal menu di sinistra. Nell'elenco di account DocumentDB regolari e account DocumentDB con supporto del protocollo Mongo, fare clic sul nome del nuovo account.
8. Ora è possibile usare l'account DocumentDB con l'app MongoDB.

   ![Screenshot del pannello dell'account predefinito](./media/documentdb-create-mongodb-account/defaultaccountblade.png)

## <a name="next-steps"></a>Passaggi successivi
* Informazioni su come [connettersi](documentdb-connect-mongodb-account.md) a un account DocumentDB con supporto del protocollo per MongoDB.



<!--HONumber=Jan17_HO2-->


