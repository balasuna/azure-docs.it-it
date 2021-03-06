---
title: 'Esercitazione: Integrazione di Azure Active Directory con Thoughtworks Mingle | Microsoft Docs'
description: Informazioni su come usare Thoughtworks Mingle con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/06/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 49352a5e8255468bbc54c02e0cd9242d49002dbd
ms.openlocfilehash: 6a9a1cf616531de1d0b8194bc0e6c186acc27008


---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Esercitazione: Integrazione di Azure Active Directory con Thoughtworks Mingle
In questa esercitazione verrà illustrata l'integrazione di Azure e Thoughtworks Mingle.  
Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

* Sottoscrizione di Azure valida
* Tenant Thoughtworks Mingle

Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1. Abilitazione dell'integrazione dell'applicazione per Thoughtworks Mingle
2. Configurazione dell'accesso Single Sign-On
3. Configurazione del provisioning utente
4. Assegnazione degli utenti

![Scenario](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Scenario")

## <a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>Abilitazione dell'integrazione dell'applicazione per Thoughtworks Mingle
In questa sezione viene descritto come abilitare l'integrazione dell'applicazione per Thoughtworks Mingle.

### <a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Per abilitare l'integrazione dell'applicazione per Thoughtworks Mingle, eseguire la procedura seguente:
1. Nel portale di Azure classico fare clic su **Active Directory**nel riquadro di spostamento sinistro.
   
    ![Active Directory](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Active Directory")

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
    ![Applicazioni](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Applicazioni")

4. Fare clic su **Add** nella parte inferiore della pagina.
   
    ![Aggiungere un'applicazione](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Aggiungere un'applicazione")

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
    ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Aggiungere un'applicazione dalla raccolta")

6. Nella **casella di ricerca** digitare **thoughtworks mingle**.
   
    ![Raccolta di applicazioni](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Raccolta di applicazioni")

7. Nel riquadro dei risultati selezionare **Thoughtworks Mingle**, quindi fare clic su **Completa** per aggiungere l'applicazione.
   
    ![Thoughtworks Mingle](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Mingle")

## <a name="configuring-single-sign-on"></a>Configurazione dell'accesso Single Sign-On
In questa sezione viene descritto come consentire agli utenti di eseguire l'autenticazione a Thoughtworks Mingle tramite il relativo account in Azure AD utilizzando la federazione basata sul protocollo SAML.  
Come parte di questa procedura, verrà richiesto di caricare un certificato in Thoughtworks Mingle.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Per configurare l'accesso Single Sign-On, seguire questa procedura:
1. Nella pagina di integrazione dell'applicazione **Thoughtworks Mingle** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura Single Sign On**.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Configurare l'accesso Single Sign-On")

2. Nella pagina **Stabilire come si desidera che gli utenti accedano alla pagina Thoughtworks Mingle** selezionare **Single Sign-On di Microsoft Azure AD**, quindi fare clic su **Avanti**.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Configurare l'accesso Single Sign-On")

3. Nella casella di testo **URL di accesso Thoughtworks Mingle** della pagina **Configura URL app** digitare l'URL usando il modello seguente "*http://company.mingle.thoughtworks.com*", quindi fare clic su **Avanti**.
   
    ![Configurare l'URL dell'app](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Configurare l'URL dell'app")

4. Nella pagina **Configura accesso Single Sign-On in Thoughtworks Mingle** fare clic su Scarica metadati, quindi salvare i metadati nel computer.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Configurare l'accesso Single Sign-On")

5. Accedere al sito aziendale di **Thoughtworks Mingle** come amministratore.

6. Fare clic sulla scheda **Admin**, quindi fare clic su **Config SSO**.
   
    ![Configurazione dell'accesso SSO](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "Configurazione dell'accesso SSO")

7. Nella sezione **Config SSO** , eseguire la procedura seguente:
   
    ![Configurazione dell'accesso SSO](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "Configurazione dell'accesso SSO")
   
    a. Per caricare il file di metadati, fare clic su **Scegli file**.
   
    b. Fare clic su **Salva modifiche**.

8. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Complete** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Configurare l'accesso Single Sign-On")

## <a name="configuring-user-provisioning"></a>Configurazione del provisioning utente
Per consentire agli utenti di AAD di accedere, è necessario eseguirne il privisioning nell’applicazione Thoughtworks Mingle utilizzando i relativi nomi utente di Azure Active Directory.  
Nel caso di Thoughtworks Mingle, il provisioning è un'attività manuale.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Per configurare il provisioning utente, seguire questa procedura:
1. Accedere al sito aziendale di Thoughtworks Mingle come amministratore.

2. Fare clic su **Profilo**.
   
    ![Primo progetto](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Primo progetto")

3. Fare clic sulla scheda **Admin**, quindi fare clic su **Utenti**.
   
    ![Utenti](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Utenti")

4. Fare clic su **Nuovo utente**.
   
    ![Nuovo utente](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Nuovo utente")

5. Nella pagina **Nuovo utente** eseguire la procedura seguente:
   
    ![Nuovo utente](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Nuovo utente")
   
    a. Digitare nelle caselle di testo correlate **Nome accesso**, **Nome visualizzato**, **Scegli password**, **Conferma password** le informazioni relative a un account AAD valido di cui si desidera eseguire il provisioning.
   
    b. In **Tipo di utente** selezionare **Base**.
   
    c. Fare clic su **Crea questo profilo**.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Thoughtworks Mingle per eseguire il provisioning degli account utente di AAD.
> 
> 

## <a name="assigning-users"></a>Assegnazione degli utenti
Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

### <a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Per assegnare gli utenti a Thoughtworks Mingle, eseguire la procedura seguente:
1. Nel portale di Azure classico creare un account di test.

2. Nella pagina di integrazione dell'applicazione **Thoughtworks Mingle** fare clic su **Assegna utenti**.
   
    ![Assegnare utenti](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Assegnare utenti")

3. Selezionare l'utente di test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.
   
    ![Sì](./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Sì")

Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO2-->


