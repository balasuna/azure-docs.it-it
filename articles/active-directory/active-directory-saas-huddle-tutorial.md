---
title: 'Esercitazione: Integrazione di Azure Active Directory con Huddle | Microsoft Docs'
description: Informazioni su come usare Huddle con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 85d7789599cfb39ce0de5d52e0fe057482a49039


---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Esercitazione: Integrazione di Azure Active Directory con Huddle
Questa esercitazione descrive l'integrazione di Azure e Huddle.  
Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

* Sottoscrizione di Azure valida
* Sottoscrizione di Huddle abilitata per l'accesso Single Sign-On

Al termine dell'esercitazione, gli utenti di Azure AD assegnati a Huddle potranno accedere all'applicazione tramite il sito aziendale di Huddle (accesso avviato dal provider di servizi) o seguendo le istruzioni riportate in [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1. Abilitazione dell'integrazione dell'applicazione per Huddle
2. Configurazione dell'accesso Single Sign-On
3. Configurazione del provisioning utente
4. Assegnazione degli utenti

![Configura accesso Single Sign-On](./media/active-directory-saas-huddle-tutorial/IC787830.png "Configure Single Sign-On")

## <a name="enabling-the-application-integration-for-huddle"></a>Abilitazione dell'integrazione dell'applicazione per Huddle
Questa sezione descrive come abilitare l'integrazione dell'applicazione per Huddle.

### <a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Per abilitare l'integrazione dell'applicazione per Huddle, seguire questa procedura:
1. Nel portale di Azure classico fare clic su **Active Directory**nel riquadro di spostamento sinistro.
   
   ![Active Directory](./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
   ![Applications](./media/active-directory-saas-huddle-tutorial/IC700994.png "Applications")
4. Fare clic su **Add** nella parte inferiore della pagina.
   
   ![Aggiungi applicazione](./media/active-directory-saas-huddle-tutorial/IC749321.png "Add application")
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
   ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-huddle-tutorial/IC749322.png "Add an application from gallerry")
6. Nella **casella di ricerca** digitare **Huddle**.
   
   ![Raccolta di applicazioni](./media/active-directory-saas-huddle-tutorial/IC787831.png "Application Gallery")
7. Nel riquadro dei risultati selezionare **Huddle** e quindi fare clic su **Completa** per aggiungere l'applicazione.
   
   ![Huddle](./media/active-directory-saas-huddle-tutorial/IC787832.png "Huddle")
   
   ## <a name="configuring-single-sign-on"></a>Configurazione dell'accesso Single Sign-On

Questa sezione descrive come consentire agli utenti di eseguire l'autenticazione a Huddle tramite il proprio account in Azure AD usando la federazione basata sul protocollo SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Per configurare l'accesso Single Sign-On, seguire questa procedura:
1. Nella pagina di integrazione dell'applicazione **Huddle** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-huddle-tutorial/IC787833.png "Configure Single Sign-On")
2. Nella pagina **Stabilire come si desidera che gli utenti accedano a Huddle** selezionare **Single Sign-On di Microsoft Azure AD** e quindi fare clic su **Avanti**.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-huddle-tutorial/IC787834.png "Configure Single Sign-On")
3. Nella casella di testo **Huddle Sign On URL** (URL di accesso Huddle) della pagina **Configura URL app** digitare l'URL del tenant Huddle usando il modello "*http://company.huddle.com*", quindi fare clic su **Avanti**.
   
   ![Configura URL app](./media/active-directory-saas-huddle-tutorial/IC787835.png "Configure App URL")
4. Nella pagina **Configura accesso Single Sign-On in Huddle** seguire questa procedura.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-huddle-tutorial/IC787836.png "Configure Single Sign-On")
   
   1. Fare clic su **Download certificato**e quindi salvare il certificato nel computer.
   2. Copiare il valore **URL autorità di certificazione**, il valore **SAML SSO URL** e il certificato scaricato, quindi inviarli al team di supporto di Huddle.
   
   > [!NOTE]
   > L'accesso Single Sign-On deve essere abilitato dal team di supporto di Huddle.
   > Al termine della configurazione, verrà visualizzata una notifica.
   > 
   > 
5. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Complete** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-huddle-tutorial/IC787837.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Configurazione del provisioning utente

Per consentire agli utenti di Azure AD di accedere a Huddle, è necessario eseguirne il provisioning in Huddle.  
Nel caso di Huddle, il provisioning è un'attività manuale.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Per configurare il provisioning utente, eseguire la procedura seguente:
1. Accedere al sito aziendale di **Huddle** come amministratore.
2. Fare clic su **Area di lavoro**.
3. Fare clic su **Persone \> Invite People (Invita persone)**.
   
   ![Persone](./media/active-directory-saas-huddle-tutorial/IC787838.png "People")
4. Nella sezione **Create a new invitation** seguire questa procedura:
   
   ![New Invitation](./media/active-directory-saas-huddle-tutorial/IC787839.png "New Invitation")
   
   1. Nell'elenco **Scegli un team per invitare persone a partecipare** selezionare **team**.
   2. Digitare nella casella di testo corrispondente il valore **Email Address** di un account AAD valido di cui si vuole eseguire il provisioning.
   3. Fare clic su **Invita**.
   
   > [!NOTE]
   > Il titolare dell'account Azure AD riceverà un messaggio di posta elettronica con un collegamento da selezionare per confermare l'account e attivarlo.
   > 
   > 

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Huddle per eseguire il provisioning degli account utente di AAD.
> 
> 

## <a name="assigning-users"></a>Assegnazione degli utenti
Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

### <a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Per assegnare gli utenti a Huddle, seguire questa procedura:
1. Nel portale di Azure classico creare un account di test.
2. Nella pagina di integrazione dell'applicazione **Huddle** fare clic su **Assegna utenti**.
   
   ![Assegna utenti](./media/active-directory-saas-huddle-tutorial/IC787840.png "Assign Users")
3. Selezionare l'utente di test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.
   
   ![Sì](./media/active-directory-saas-huddle-tutorial/IC767830.png "Yes")

Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


