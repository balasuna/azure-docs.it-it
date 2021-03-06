---
title: 'Esercitazione: Integrazione di Azure Active Directory con ClickTime | Documentazione Microsoft'
description: Informazioni su come usare ClickTime con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 786ffe97c794aa44dc5d2b8e1a9597387752409c


---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Esercitazione: Integrazione di Azure Active Directory con ClickTime
Questa esercitazione descrive come integrare ClickTime con Azure Active Directory (Azure AD).

L'integrazione di ClickTime con Azure AD offre i vantaggi seguenti:

* È possibile controllare in Azure AD chi può accedere a ClickTime
* È possibile abilitare gli utenti per l'accesso automatico a ClickTime (Single Sign-On) con i propri account Azure AD
* È possibile gestire gli account da una posizione centrale: il portale di Azure classico

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Prerequisiti
Per configurare l'integrazione di Azure AD con ClickTime, sono necessari gli elementi seguenti:

* Sottoscrizione di Azure AD.
* Sottoscrizione di ClickTime abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.
> 
> 

A questo scopo, è consigliabile seguire le indicazioni seguenti:

* Non usare l'ambiente di produzione, a meno che non sia necessario.
* Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test.

Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di ClickTime dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-clicktime-from-the-gallery"></a>Aggiunta di ClickTime dalla raccolta
Questa sezione descrive come abilitare l'integrazione dell'applicazione per ClickTime.

### <a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>Per abilitare l'integrazione dell'applicazione per ClickTime, seguire questa procedura:
1. Nel portale di Azure classico fare clic su **Active Directory**nel riquadro di spostamento sinistro.
   
   ![Active Directory](./media/active-directory-saas-clicktime-tutorial/tic700993.png "Active Directory")
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
   ![Applications](./media/active-directory-saas-clicktime-tutorial/tic700994.png "Applications")
4. Fare clic su **Add** nella parte inferiore della pagina.
   
   ![Aggiungi applicazione](./media/active-directory-saas-clicktime-tutorial/tic749321.png "Add application")
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
   ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-clicktime-tutorial/tic749322.png "Add an application from gallerry")
6. Nella **casella di ricerca** digitare **ClickTime**.
   
   ![Raccolta di applicazioni](./media/active-directory-saas-clicktime-tutorial/tic777275.png "Application gallery")
7. Nel riquadro dei risultati selezionare **ClickTime** e quindi fare clic su **Completa** per aggiungere l'applicazione.
   
   ![ClickTime](./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con ClickTime in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di ClickTime che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in ClickTime.

La relazione di collegamento viene stabilita assegnando il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente) in ClickTime.

Per configurare e testare l'accesso Single Sign-On di Azure AD con ClickTime, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creazione di un utente test di ClickTime](#creating-a-clicktime-test-user)**: per avere una controparte di Britta Simon in ClickTime collegata alla relativa rappresentazione in Azure AD.
4. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come consentire agli utenti di eseguire l'autenticazione a ClickTime tramite il proprio account in Azure AD usando la federazione basata sul protocollo SAML.  

> [!IMPORTANT]
> Per poter configurare l'accesso Single Sign-On nel tenant di ClickTime, è necessario contattare prima di tutto il supporto tecnico ClickTime per abilitare questa funzionalità.
> 
> 

**Per configurare Single Sign-On di Azure AD con ClickTime, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **ClickTime** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
   ![Abilita Single Sign-On](./media/active-directory-saas-clicktime-tutorial/tic777277.png "Enable single sign-on")
2. Nella pagina **Stabilire come si desidera che gli utenti accedano a ClickTime** selezionare **Single Sign-On di Microsoft Azure AD** e quindi fare clic su **Avanti**.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-clicktime-tutorial/tic777278.png "Configure single sign-on")
3. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 
   
    a. Nella casella di testo **Identificatore** digitare l'URL adottando il modello seguente: **https://app.clicktime.com/sp/**
   
    b. Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente: **https://app.clicktime.com/Login/**
   
    c. click **Avanti**
4. Nella pagina **Configura accesso Single Sign-On in ClickTime** fare clic su **Scarica certificato** e quindi salvare il file del certificato nel computer.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-clicktime-tutorial/tic777279.png "Configure single sign-on")
5. In un'altra finestra del Web browser accedere al sito aziendale di ClickTime come amministratore.
6. Nella barra degli strumenti in alto fare clic su **Preferences** (Preferenze) e quindi su **Security Settings** (Impostazioni di sicurezza).
7. Nella sezione di configurazione **Preferenza Single Sign-On** seguire questa procedura:
   
   ![Impostazioni di sicurezza](./media/active-directory-saas-clicktime-tutorial/tic777280.png "Security Settings")
   
   a.  Selezionare **Allow** sign-in using Single Sign-On (SSO) **Azure AD** (Consenti l'accesso Single Sign-On (SSO) con Azure AD).
   
   b.  Nella finestra di dialogo **Configura accesso Single Sign-On in ClickTime** del portale di Azure classico copiare il valore di **URL servizio Single Sign-On** e incollarlo nella casella di testo **Identity Provider Endpoint** (Endpoint del provider di identità).
   
   c.  Aprire il certificato con codifica Base64 nel **Blocco note**, copiarne il contenuto e quindi incollarlo nella casella di testo **X.509 Certificate** (Certificato X.509).
   
   d.  Fare clic su **Salva**.
8. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Complete** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-clicktime-tutorial/tic777281.png "Configure single sign-on")

## <a name="configuring-user-provisioning"></a>Configurazione del provisioning utente
Per consentire agli utenti di Azure AD di accedere a ClickTime, è necessario eseguirne il provisioning in ClickTime.  
Nel caso di ClickTime, il provisioning è un'attività manuale.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Per eseguire il provisioning di un account utente, eseguire la procedura seguente:
1. Accedere al tenant **ClickTime** .
2. Nel barra degli strumenti in alto fare clic su **Company** (Azienda) e quindi su **People** (Persone).
   
   ![Persone](./media/active-directory-saas-clicktime-tutorial/tic777282.png "People")
3. Fare clic su **Aggiungi persona**.
   
   ![Aggiungi persona](./media/active-directory-saas-clicktime-tutorial/tic777283.png "Add Person")
4. Nella sezione New Person seguire questa procedura:
   
   ![Persone](./media/active-directory-saas-clicktime-tutorial/tic777284.png "People")
   
   a.  Nella casella di testo **indirizzo email** digitare l'indirizzo di posta elettronica dell'account Azure AD.
   
   b.  Nella casella di testo **full name** digitare il nome dell'account Azure AD.  
   
   > [!NOTE]
   > È anche possibile impostare altre proprietà dell'oggetto new person.
   > 
   > 
   
   c.  Fare clic su **Save**.

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da ClickTime per effettuare il provisioning degli account utente di Azure AD.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD
In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure, concedendole così accesso a ClickTime.

![Assegna utente][200]

Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

**Per assegnare Britta Simon a ClickTime, seguire questa procedura:**

1. Per aprire la visualizzazione delle applicazioni nel portale classico, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.
   
    ![Assegna utente][201] 
2. Nell'elenco delle applicazioni, selezionare **ClickTime**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 
3. Scegliere **Utenti**dal menu in alto.
   
    ![Assegna utente][203]
4. Nell'elenco di utenti selezionare **Britta Simon**.
5. Fare clic su **Assegna**sulla barra degli strumenti in basso.
   
    ![Assegna utente][205]

## <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On
In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro ClickTime nel pannello di accesso, si accederà automaticamente all'applicazione ClickTime.

## <a name="additional-resources"></a>Risorse aggiuntive
* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


