---
title: 'Esercitazione: Integrazione di Azure Active Directory con QuickHelp | Microsoft Docs'
description: Informazioni su come configurare l&quot;accesso Single Sign-On tra Azure Active Directory e QuickHelp.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/17/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5e67654ceaa7420615c63fae288da4fa048110c7


---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Esercitazione: Integrazione di Azure Active Directory con QuickHelp
Questa esercitazione descrive l'integrazione di QuickHelp con Azure Active Directory (Azure AD).  
L'integrazione di QuickHelp con Azure AD offre i vantaggi seguenti: 

* È possibile controllare in Azure AD chi può accedere a QuickHelp. 
* È possibile abilitare gli utenti per l'accesso automatico a QuickHelp (Single Sign-On) con i propri account Azure AD.
* È possibile gestire gli account da una posizione centrale: il portale di Azure classico

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Prerequisiti
Per configurare l'integrazione di Azure AD con QuickHelp, sono necessari gli elementi seguenti:

* Sottoscrizione di Azure AD.
* Sottoscrizione di QuickHelp abilitata per l'accesso Single Sign-On

> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.
> 
> 

A questo scopo, è consigliabile seguire le indicazioni seguenti:

* Non usare l'ambiente di produzione, a meno che non sia necessario.
* Se non è disponibile un ambiente di prova di Azure AD, è possibile ottenere una versione di prova di un mese [qui](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Descrizione dello scenario
L'obiettivo di questa esercitazione è testare l'accesso Single Sign-On di Azure AD in un ambiente di test.  
Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di QuickHelp dalla raccolta 
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-quickhelp-from-the-gallery"></a>Aggiunta di QuickHelp dalla raccolta
Per configurare l'integrazione di QuickHelp in Azure AD, è necessario aggiungere QuickHelp dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere QuickHelp dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro. 
   
    ![Active Directory][1]
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
    ![Applications][2]
4. Fare clic su **Add** nella parte inferiore della pagina.
   
    ![Applicazioni][3]
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
    ![Applicazioni][4]
6. Nella casella di ricerca digitare **QuickHelp**.
   
    ![Applicazioni][5]
7. Nel riquadro dei risultati selezionare **QuickHelp** e quindi fare clic su **Completa** per aggiungere l'applicazione.
   
    ![Applicazioni][500]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come configurare e testare l'accesso Single Sign-On di Azure AD con QuickHelp in base a un utente test di nome "Britta Simon".

Per configurare e testare l'accesso Single Sign-On di Azure AD con QuickHelp, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on)** : per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creazione di un utente test di QuickHelp](#creating-a-quickhelp-test-user)** - per avere una controparte di Britta Simon in QuickHelp collegata alla relativa rappresentazione in Azure AD.
4. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure classico e configurare l'accesso Single Sign-On nell'applicazione QuickHelp.

**Per configurare Single Sign-On di Azure AD con QuickHelp, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **QuickHelp** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
    ![Configura accesso Single Sign-On][6] 
2. Nella pagina **Stabilire come si desidera che gli utenti accedano a QuickHelp** selezionare **Single Sign-On di Azure AD** e quindi fare clic su **Avanti**.
   
    ![Single Sign-On di Microsoft Azure AD][7] 
3. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura:
   
    ![Configurare le impostazioni dell'app][8] 
   
    a. Nella casella di testo **URL di accesso**, digitare l'URL usato dagli utenti per accedere al sito QuickHelp, ad esempio: * https://quickhelp.com/bsiazure/*).
   
    > [!NOTE]
    > Se non si conosce il valore dell'URL di accesso, contattare il team di supporto di QuickHelp.
    > 
    > 
   
    b. Fare clic su **Avanti**.
4. Nella pagina **Configura accesso Single Sign-On in QuickHelp** seguire questa procedura: fare clic su **Scarica metadati** e quindi salvare il file di metadati in locale nel computer.
   
    ![Cos'è Azure AD Connect][9] 
5. Accedere al sito aziendale di QuickHelp come amministratore.
6. Nel menu in alto fare clic su **Admin**.
   
    ![Configura accesso Single Sign-On][21]
7. Nel menu **QuickHelp Admin** fare clic su **Settings**.
   
    ![Configura accesso Single Sign-On][22]
8. Fare clic su **Authentication Settings**.
9. Nella pagina **Authentication Settings** seguire questa procedura:
   
    ![Configura accesso Single Sign-On][23]
   
    a. Come **tipo SSO**, selezionare **WSFederation**.
   
    b. Per caricare il file dei metadati di Azure scaricato, fare clic su **Sfoglia**, passare al file e quindi fare clic su **Carica metadati**.
   
    c. Nella casella di testo **Email** digitare **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
   
    d. Nella casella di testo **First Name** (Nome) digitare **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
   
    e. Nella casella di testo **Last Name** (Cognome) digitare **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
   
    f. Nella **barra delle azioni**, fare clic su **Salva**.
10. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**. 
    
     ![Cos'è Azure AD Connect][10]
11. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.  
    
     ![Cos'è Azure AD Connect][11]

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test chiamato Britta Simon nel portale di Azure classico.  
Nell'elenco di utenti selezionare **Britta Simon**.

![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per visualizzare l'elenco di utenti, fare clic su **Utenti**nel menu in alto.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 
4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** nella barra degli strumenti in basso. 
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 
5. Nella pagina **Informazioni sull'utente** seguire questa procedura: 
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_05.png) 
   
    a. In Tipo di utente selezionare Nuovo utente nell'organizzazione.
   
    b. Nella casella di testo **Nome utente** digitare **BrittaSimon**.
   
    c. Fare clic su **Avanti**.
6. Nella pagina **Profilo utente** seguire questa procedura: 
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_06.png) 
   
    a. Nella casella di testo **Nome** digitare **Britta**.  
   
    b. Nella casella di testo **Cognome** digitare **Simon**.
   
    c. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.
   
    d. Nell'elenco **Ruolo** selezionare **Utente**.
    e. Fare clic su **Avanti**.
7. Nella pagina **Ottieni password temporanea** fare clic su **crea**.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_07.png) 

1. Nella pagina **Ottieni password temporanea** seguire questa procedura:
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_08.png) 
   
    a. Prendere nota del valore visualizzato in **Nuova password**.
   
    b. Fare clic su **Complete**.   

### <a name="creating-a-quickhelp-test-user"></a>Creazione di un utente test di QuickHelp
Questa sezione descrive come creare un utente chiamato Britta Simon in QuickHelp.
Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di QuickHelp che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in QuickHelp.

QuickHelp supporta il provisioning just-in-time. Ciò significa che, se necessario, un account utente viene creato automaticamente in QuickHelp e che l'account è collegato all'account di Azure AD.

Non è necessario alcun intervento dell'utente in questa sezione.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD
Questa sezione descrive come abilitare Britta Simon per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a QuickHelp.

![Assegna utente][200] 

**Per assegnare Britta Simon a QuickHelp, seguire questa procedura:**

1. Per aprire la visualizzazione applicazioni nel portale di Azure classico, nella visualizzazione directory scegliere **Applicazioni** dal menu in alto.
   
    ![Assegna utente][201] 
2. Nell'elenco delle applicazioni selezionare **QuickHelp**.
   
    ![Assegna utente][202] 
3. Scegliere **Utenti**dal menu in alto.
   
    ![Assegna utente][203] 
4. Nell'elenco di utenti selezionare **Britta Simon**.
5. Fare clic su **Assegna**sulla barra degli strumenti in basso.
   
    ![Assegna utente][205]

### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On
Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.  
Quando si fa clic sul riquadro QuickHelp nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione QuickHelp.

## <a name="additional-resources"></a>Risorse aggiuntive
* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_01.png
[500]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_14.png


[6]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_02.png
[8]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_03.png
[9]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_04.png
[10]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
[24]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_08.png
[25]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_09.png
[26]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_10.png
[27]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_11.png
[28]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_12.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_13.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_400.png
[401]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_401.png
[402]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_402.png







<!--HONumber=Nov16_HO3-->


