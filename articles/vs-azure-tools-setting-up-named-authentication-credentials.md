---
title: Configurazione delle credenziali per l&quot;autenticazione denominate | Documentazione Microsoft
description: "Informazioni su come fornire le credenziali che Visual Studio può usare per l&quot;autenticazione delle richieste inviate a Azure per pubblicare un&quot;applicazione in Azure da Visual Studio o monitorare un servizio cloud esistente. "
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 01623fa76175091439d5a571fb8b8f96aee01c4c
ms.openlocfilehash: 03875b4215943a8bbabeb15610776a221cbf7b71


---
# <a name="setting-up-named-authentication-credentials"></a>Configurazione delle credenziali per l'autenticazione denominate
Per pubblicare un'applicazione in Azure da Visual Studio o monitorare un servizio cloud esistente, è necessario fornire le credenziali che Visual Studio può usare per l'autenticazione delle richieste inviate a Azure. In Visual Studio è possibile fornire queste credenziali accedendo a più posizioni. Ad esempio, da Esplora server è possibile aprire il menu di scelta rapida per il nodo di **Azure** e scegliere **Connetti ad Azure**. Dopo l'accesso, le informazioni sulla sottoscrizione associate all'account Azure sono disponibili in Visual Studio. Non è necessario eseguire altre operazioni.

Gli strumenti di Azure supportano anche un modo meno recente di fornire le credenziali, attraverso il file di sottoscrizione, con estensione publishsettings. Questo argomento illustra tale metodo, ancora supportato in Azure SDK 2.2.

Per l'autenticazione in Azure sono richiesti i seguenti elementi.

* ID sottoscrizione
* Un certificato X.509 v3 valido

> [!NOTE]
> La lunghezza della chiave del certificato X.509 v3 deve essere di almeno 2048 bit. Azure rifiuterà qualsiasi certificato che non soddisfa questo requisito o che sia non valido.
>
>

Visual Studio usa l'ID sottoscrizione e i dati del certificato come credenziali. Nel file di sottoscrizione, con estensione publishsettings, che contiene una chiave pubblica per il certificato, viene fatto riferimento alle credenziali appropriate. Il file di sottoscrizione può contenere le credenziali per più di una sottoscrizione.

Per modificare le informazioni sulla sottoscrizione è possibile usare le finestre di dialogo **Nuova sottoscrizione e Modifica sottoscrizione** , come illustrato più avanti in questo argomento.

Se si vuole creare un certificato, vedere le istruzioni contenute in [Creare e caricare un certificato di gestione per Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) e quindi caricare manualmente il certificato nel [portale di Azure classico](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!NOTE]
> Le credenziali richieste da Visual Studio per gestire i servizi cloud non sono le stesse necessarie per autenticare una richiesta per i servizi di archiviazione di Azure.
>
>

## <a name="modify-or-export-authentication-credentials-in-visual-studio"></a>Modificare o esportare le credenziali di autenticazione in Visual Studio
È anche possibile impostare, modificare o esportare le credenziali di autenticazione nella finestra di dialogo **Nuova sottoscrizione** , visualizzata dopo una delle azioni seguenti:

* In **Esplora server** aprire il menu di scelta rapida per il nodo di **Azure**, scegliere **Gestisci sottoscrizioni**, selezionare la scheda **Certificati** e infine fare clic sul pulsante **Nuovo** o **Modifica**.
* Quando si pubblica un servizio cloud di Azure dalla procedura guidata **Pubblica applicazione Azure**, scegliere **Gestisci** dall'elenco **Scegliere la sottoscrizione**, selezionare la scheda Certificati e quindi fare clic sul pulsante **Nuovo** o **Modifica**.

La procedura riportata di seguito presuppone che la finestra di dialogo **Nuova sottoscrizione** sia visualizzata.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>Per configurare le credenziali di autenticazione in Visual Studio
1. Scegliere un certificato dall'elenco **Selezionare un certificato esistente per l'autenticazione** .
2. Fare clic sul pulsante **Copia il percorso completo**. Il percorso del certificato, ovvero il file con estensione cer, viene copiato negli Appunti.

   > [!IMPORTANT]
   > Per pubblicare l'applicazione Azure da Visual Studio, è necessario caricare questo certificato nel [portale di Azure classico](http://go.microsoft.com/fwlink/?LinkID=213885).
   >
   >
3. Per caricare il certificato nel [portale di Azure classico](http://go.microsoft.com/fwlink/?LinkID=213885):

   1. Scegliere il collegamento Portale di Azure.

        Verrà visualizzato il [portale di Azure classico](http://go.microsoft.com/fwlink/?LinkID=213885) .
   2. Accedere al [portale di Azure classico](http://go.microsoft.com/fwlink/?LinkID=213885)e quindi fare clic su **Servizi cloud** .
   3. Scegliere il servizio cloud desiderato.

       Viene visualizzata la pagina del servizio.
   4. Nella scheda **Certificati** fare clic su **Carica**.
   5. Incollare il percorso completo del file con estensione cer appena creato e digitare la password specificata.



<!--HONumber=Dec16_HO2-->


