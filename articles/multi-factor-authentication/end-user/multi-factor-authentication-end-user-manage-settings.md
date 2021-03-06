---
title: Gestire le impostazioni della verifica in due passaggi | Documentazione Microsoft
description: Gestire l&quot;utilizzo di Azure Multi-Factor Authentication, inclusa la modifica delle informazioni di contatto e la configurazione dei dispositivi.
services: multi-factor-authentication
keywords: client di multi-factor authentication, problema di autenticazione, ID di correlazione
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 4ff028f88e984f28bc0f4a228aabed1fabc90560
ms.openlocfilehash: b5c031d1e36508e7622ba516d7bb8b2cfb3d7a89


---
# <a name="manage-your-settings-for-two-step-verification"></a>Gestire le impostazioni per la verifica in due passaggi
Questo articolo risponde a domande su come aggiornare le impostazioni della verifica in due passaggi o della Multi-Factor Authentication. Se si sono verificati problemi di accesso all'account, vedere le informazioni relative ai [problemi con la verifica in due passaggi](multi-factor-authentication-end-user-troubleshoot.md) che potrebbero essere utili.

## <a name="where-to-find-the-settings-page"></a>Dove trovare la pagina delle impostazioni
In base al modo in cui l'azienda ha configurato Azure Multi-Factor Authentication, le impostazioni come il numero di telefono possono essere modificate in diverse posizioni.

Se l'amministratore IT ha fornito un URL o passaggi specifici per la gestione della verifica in due passaggi, seguire tali istruzioni. In caso contrario, le istruzioni seguenti dovrebbero funzionare per tutti gli utenti. Se si esegue questa procedura ma non vengono visualizzate le stesse opzioni, significa che l'azienda o l'istituto di istruzione ha personalizzato il proprio portale. Chiedere all'amministratore il collegamento al portale di Azure Multi-Factor Authentication.

1. Effettuare l'accesso ad [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Nella parte superiore selezionare il **profilo**.  
3. Selezionare **Verifica aggiuntiva di sicurezza**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. La pagina Verifica aggiuntiva di sicurezza viene caricata e mostra le impostazioni attive.

    ![Verifica](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Si desidera modificare il numero di telefono o aggiungere un numero secondario
È importante configurare un numero di telefono di autenticazione secondario.  Poiché il numero di telefono primario e l'app per dispositivi mobili si trovano probabilmente sullo stesso telefono, il numero di telefono secondario è l'unico modo che consente di accedere di nuovo all'account in caso di perdita o furto del telefono.

> [!NOTE]
> Se non si ha accesso al proprio numero di telefono principale e si ha bisogno di assistenza per l'accesso al proprio account, vedere le informazioni relative ai [problemi con la verifica in due passaggi](multi-factor-authentication-end-user-troubleshoot.md).  

**Per modificare il numero di telefono principale:**  

1. Nella pagina Verifica aggiuntiva di sicurezza selezionare la casella di testo con il numero di telefono attuale e modificarla con il nuovo numero di telefono.  
2. Selezionare **Salva**.  
3. Se questo è il numero che si usa per l'opzione di verifica preferita, è necessario verificare il nuovo numero prima di poterlo salvare.  

**Per aggiungere un numero di telefono secondario:**  

1. Nella pagina Verifica aggiuntiva di sicurezza selezionare la casella di controllo accanto a **Telefono per l'autenticazione alternativo**.  
2. Immettere il numero di telefono secondario nella casella di testo.  
3. Selezionare **Salva** e le modifiche sono terminate.  

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Come si rimuove Microsoft Authenticator dal dispositivo precedente per passare a un nuovo dispositivo?
Quando si disinstalla l'app dal dispositivo o si ripristina il dispositivo, l'attivazione sul back-end non verrà rimossa. Per altre informazioni, vedere [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

## <a name="next-steps"></a>Passaggi successivi
* Leggere le informazioni di guida relative ai [problemi con la verifica in due passaggi](multi-factor-authentication-end-user-troubleshoot.md)
* Impostare [password di app](multi-factor-authentication-end-user-app-passwords.md) per tutte le applicazioni che non supportano la verifica in due passaggi.



<!--HONumber=Dec16_HO4-->


