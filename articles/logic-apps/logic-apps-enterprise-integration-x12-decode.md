---
title: Messaggi Decode X12 nelle app per la logica di Azure | Documentazione Microsoft
description: Utilizzo del decodificatore X12 in Enterprise Integration Pack nelle app per la logica
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 4fd48d2d-2008-4080-b6a1-8ae183b48131
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 2f407a428aa176cc5c2a3b6bb236b522bda5ab64
ms.openlocfilehash: 2fa435d5a52ee7a9c18926912a7212ee862b3832


---
# <a name="get-started-with-decode-x12-messages"></a>Introduzione ai messaggi Decode X12
Convalida le proprietà EDI e specifiche del partner, genera un documento XML per ogni set di transazioni e un riconoscimento per ogni transazione elaborata.

## <a name="prereqs"></a>Prerequisiti
* Un account Azure, che è possibile [creare gratuitamente](https://azure.microsoft.com/free)
* Per usare il connettore Decode X12 Message, è necessario un account di integrazione. Vedere i dettagli su come creare un [account di integrazione](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), [partner](../logic-apps/logic-apps-enterprise-integration-partners.md) e un [contratto X12](../logic-apps/logic-apps-enterprise-integration-x12.md)

## <a name="decode-x12-messages"></a>Messaggi Decode X12

1. [Creare un'app per la logica](../logic-apps/logic-apps-create-a-logic-app.md) illustra un esempio
2. Questo connettore non include trigger. Usare altri trigger per avviare l'app per la logica, ad esempio un trigger di richiesta.  Nella finestra di progettazione dell'app per la logica aggiungere un trigger e un'azione.  Selezionare Mostra API gestite da Microsoft nell'elenco a discesa, quindi immettere "x12" nella casella di ricerca.  Selezionare X12 - Decode X12 Message:
   
    ![ricerca x12](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  
3. Se in precedenza non sono state create connessioni all'account di archiviazione, vengono richiesti i dettagli della connessione:
   
    ![connessione all'account di integrazione](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)     
4. Immettere i dettagli dell'account di integrazione.  Le proprietà con l'asterisco sono obbligatorie:
   
   | Proprietà | Dettagli |
   | --- | --- |
   | Nome connessione* |Immettere un nome per la connessione. |
   | Account di integrazione * |Immettere il nome dell'account di integrazione. Assicurarsi che l'account di integrazione e l'app per la logica si trovino nella stessa località di Azure. |
   
    Al termine i dettagli della connessione saranno simili ai seguenti:
   
    ![connessione all'account di integrazione creata](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 
5. Selezionare **Crea**.
6. La connessione è stata creata:
   
    ![dettagli della connessione all'account di integrazione](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 
7. Selezionare il messaggio con il file flat X12 da decodificare:
   
    ![specificare i campi obbligatori](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

## <a name="x12-decode-details"></a>Dettagli Decode X12

Il connettore Decode X12 effettua le seguenti operazioni:

* Convalida la busta in base all'accordo tra partner commerciali.
* Genera un documento XML per ogni set di transazioni.
* Convalida le proprietà EDI e specifiche del partner.
  * Convalida strutturale EDI e convalida estesa dello schema.
  * Convalida della struttura della busta dell'interscambio.
  * Convalida dello schema della busta in base allo schema di controllo.
  * Convalida dello schema degli elementi dati del set di transazioni rispetto allo schema del messaggio.
  * Convalida EDI eseguita sugli elementi dati del set di transazioni. 
* Verifica che i numeri di controllo di un set di interscambio, gruppo e di transazioni non siano duplicati.
  * Controlla il numero di controllo dell'interscambio rispetto agli interscambi ricevuti in precedenza.
  * Controlla il numero di controllo del gruppo con gli altri numeri di controllo del gruppo dell'interscambio.
  * Controlla il numero di controllo del set di transazioni con gli altri numeri di controllo del set transazioni in tale gruppo.
* Converte l'intero interscambio in XML. 
  * Suddivide l'interscambio in set di transazioni - Sospende i set di transazioni in caso di errore: analizza ogni set di transazioni di un interscambio in un documento XML separato. Se la convalida di uno o più set di transazioni dell'interscambio non riesce, X12 Decode sospende solo i set di transazioni interessati.
  * Suddivide l'interscambio in set di transazioni - Sospende l'interscambio in caso di errore: analizza ogni set di transazioni di un interscambio in un documento XML separato.  Se la convalida di uno o più set di transazioni dell'interscambio non riesce, X12 Decode sospende l'intero interscambio.
  * Mantiene l'interscambio - Sospende i set transazioni in caso di errore: crea un documento XML per l'intero interscambio in batch. X12 Decode sospende solo i set di transazioni che non superano la convalida, pur continuando a elaborare tutti gli altri set di transazioni.
  * Mantiene l'interscambio - Sospende l'interscambio in caso di errore: crea un documento XML per l'intero interscambio in batch. Se la convalida di uno o più set di transazioni dell'interscambio non riesce, X12 Decode sospende l'intero interscambio. 
* Genera un riconoscimento tecnico e/o funzionale (se configurata).
  * Un riconoscimento tecnico viene generato in seguito alla convalida dell'intestazione. Il riconoscimento tecnico segnala lo stato dell'elaborazione di un'intestazione e finale di interscambio in base all'indirizzo del ricevitore.
  * Un riconoscimento funzionale viene generato in seguito alla convalida del corpo. Il riconoscimento funzionale segnala ogni errore rilevato durante l'elaborazione del documento ricevuto.

## <a name="next-steps"></a>Passaggi successivi
[Altre informazioni su Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Informazioni su Enterprise Integration Pack") 




<!--HONumber=Jan17_HO5-->


