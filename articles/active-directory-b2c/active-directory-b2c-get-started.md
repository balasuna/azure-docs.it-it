---
title: 'Azure Active Directory B2C: creare un tenant di Azure Active Directory B2C | Microsoft Docs'
description: Un argomento su come creare un tenant di Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 01/26/2017
ms.author: swkrish
translationtype: Human Translation
ms.sourcegitcommit: 351149296a6d7dfa801b295ec21fc04215c7b051
ms.openlocfilehash: 77b8e707b5743ce2d0b3eb52f75828c9ab0337d9


---
# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory B2C: creare un tenant di Azure AD B2C
Per iniziare a usare Microsoft Azure Active Directory (Azure AD) B2C, seguire i tre passaggi descritti in questo articolo.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Passaggio 1: Iscriversi per ottenere una sottoscrizione di Azure
Se si ha già una sottoscrizione di Azure ignorare questo passaggio. In caso contrario, iscriversi per ottenere una [sottoscrizione di Azure](../active-directory/sign-up-organization.md) e accedere ad Azure AD B2C.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Passaggio 2: Creare un Tenant AD B2C di Azure
Utilizzare la procedura seguente per creare un nuovo tenant AD B2C di Azure. Attualmente le funzionalità B2C non possono essere attivate nei tenant esistenti.

1. Accedere al [portale di Azure classico](https://manage.windowsazure.com/) come amministratore della sottoscrizione. Si tratta dello stesso account aziendale o dell'istituto d'istruzione o dello stesso account Microsoft usato per l'iscrizione ad Azure.
2. Fare clic su **Nuovo** > **Servizi app** > **Active Directory** > **Directory** > **Creazione personalizzata**.
   
    ![Schermata di inizio della creazione di un tenant](./media/active-directory-b2c-get-started/new-directory.png)
3. Specificare **Nome**, **Nome di dominio** e **Paese o area geografica** per il tenant.
4. Selezionare l'opzione " **Questa è una directory B2C**".
5. Fare clic sul segno di spunta per completare l’operazione.
   
    ![Schermata dell'opzione per la creazione di una directory B2C](./media/active-directory-b2c-get-started/create-b2c-directory.png)
6. Il tenant è stato creato e verrà visualizzato nell'estensione Active Directory. Si ha anche il ruolo di amministratore generale del tenant. È possibile aggiungere altri amministratori generali in base alle esigenze.
   
   > [!IMPORTANT]
   > Se si prevede di usare un tenant B2C per un'app di produzione, vedere l'articolo sui [tenant B2C a livello di produzione e di anteprima](active-directory-b2c-reference-tenant-type.md). Si noti che si verificano problemi noti quando si elimina un tenant B2C esistente e lo si crea nuovamente con lo stesso nome di dominio. È necessario creare un tenant B2C con un nome di dominio diverso.
   > 
   > 

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Passaggio 3: Passare al pannello delle funzionalità B2C nel portale di Azure
1. Passare all'estensione Active Directory sulla barra di spostamento sul lato sinistro.
2. Trovare il tenant nella scheda **Directory** e selezionarlo.
3. Fare clic sulla scheda **Configure** .
4. Fare clic sul collegamento **Gestione impostazioni B2C** nella sezione **Amministrazione B2C**.
   
    ![Schermata della configurazione della directory per B2C](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)
5. Il portale di Azure con il pannello delle funzionalità B2C si aprirà in una nuova scheda o finestra del browser.
   
   > [!IMPORTANT]
   > Perché il tenant sia accessibile nel portale di Azure possono essere necessari fino a 2-3 minuti. Per risolvere questo problema, ripetere la procedura dopo qualche minuto. Altrimenti, contattare il team di supporto.
   > 
   > 
6. Aggiungere il pannello alla schermata iniziale per un accesso rapido. Lo strumento di aggiunta appare in rosso nell'angolo superiore destro del pannello delle funzionalità.
   
    ![Schermata del pannello delle funzioni B2C](./media/active-directory-b2c-get-started/b2c-features-blade.png)
   
   > [!NOTE]
   > È possibile gestire utenti e gruppi, la configurazione della reimpostazione autonoma della password e le funzionalità di personalizzazione della società per il tenant nel [portale di Azure classico](https://manage.windowsazure.com/).
   > 
   > 

## <a name="step-4-link-your-azure-ad-b2c-tenant-to-your-azure-subscription"></a>Passaggio 4: Collegare il tenant di Azure AD B2C alla sottoscrizione di Azure
Se si intende utilizzare tenant B2C per le applicazioni di produzione, è necessario collegare il tenant di Azure AD B2C alla sottoscrizione di Azure per pagare gli addebiti per l'utilizzo. Per ulteriori informazioni, vedere [questo articolo](active-directory-b2c-how-to-enable-billing.md).

   > [!IMPORTANT]
   > Se si non collega il tenant di Azure AD B2C alla sottoscrizione di Azure, verrà visualizzato un messaggio di avviso ("Nessuna sottoscrizione collegata a questo tenant B2C o La sottoscrizione richiede attenzione.") nel pannello delle caratteristiche B2C nel portale di Azure. È importante eseguire questo passaggio prima dell'invio delle applicazioni alla produzione.
   > 
   > 

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Accesso semplificato al pannello delle funzionalità B2C nel portale di Azure
Per migliorare l'individuazione, nel portale di Azure è stato aggiunto un collegamento al pannello delle funzionalità B2C.

1. Accedere al portale di Azure come amministratore globale del tenant B2C. Se si è già connessi a un tenant diverso, passare al tenant desiderato (nell'angolo superiore destro).
2. Fare clic su **Sfoglia** nel riquadro di spostamento sinistro.
3. Fare clic su **Azure AD B2C** per accedere al pannello delle funzionalità B2C.
   
    ![Screenshot del pannello delle funzionalità B2C](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Passaggi successivi
Per informazioni su come registrare un'applicazione in Azure AD B2C e creare un'applicazione di avvio rapido, vedere [Azure Active Directory B2C: registrare l'applicazione](active-directory-b2c-app-registration.md).




<!--HONumber=Jan17_HO4-->


