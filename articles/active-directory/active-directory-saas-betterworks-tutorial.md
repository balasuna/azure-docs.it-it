---
title: 'Esercitazione: Integrazione di Azure Active Directory con BetterWorks | Documentazione Microsoft'
description: Informazioni su come configurare l&quot;accesso Single Sign-On tra Azure Active Directory e BetterWorks.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5c66450fd735b3d9e4d0f7835c6d607368007388


---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a>Esercitazione: Integrazione di Azure Active Directory con BetterWorks
L'obiettivo di questa esercitazione è descrivere l'integrazione di BetterWorks con Azure Active Directory (Azure AD).

L'integrazione di BetterWorks con Azure AD offre i vantaggi seguenti:

* È possibile controllare in Azure AD chi può accedere a BetterWorks
* È possibile abilitare gli utenti per l'accesso automatico a BetterWorks (Single Sign-On) con i propri account Azure AD
* È possibile gestire gli account da una posizione centrale: il portale di Azure classico

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Prerequisiti
Per configurare l'integrazione di Azure AD con BetterWorks, sono necessari gli elementi seguenti:

* Sottoscrizione di Azure AD.
* Sottoscrizione di BetterWorks abilitata per l'accesso Single Sign-On

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

1. Aggiunta di BetterWorks dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-betterworks-from-the-gallery"></a>Aggiunta di BetterWorks dalla raccolta
Per configurare l'integrazione di BetterWorks in Azure AD, è necessario aggiungere BetterWorks dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere BetterWorks dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro. 
   
    ![Active Directory][1]
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
    ![Applications][2]
4. Fare clic su **Add** nella parte inferiore della pagina.
   
    ![Applicazioni][3]
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
    ![Applicazioni][4]
6. Nella casella di ricerca digitare **BetterWorks**.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_01.png)
7. Nel riquadro dei risultati selezionare **BetterWorks** e quindi fare clic su **Completa** per aggiungere l'applicazione.
   
    ![Selezione dell'app nella raccolta](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_001.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come configurare e testare l'accesso Single Sign-On di Azure AD con BetterWorks con un utente di test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di BetterWorks che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in BetterWorks.

La relazione di collegamento viene stabilita assegnando al valore di **nome utente** in Azure AD lo stesso valore di **Username** (Nome utente) in BetterWorks.

Per configurare e testare l'accesso Single Sign-On di Azure AD con BetterWorks, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on)** : per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creazione di un utente di test per BetterWorks](#creating-a-betterworks-test-user)** : per avere una controparte di Britta Simon in BetterWorks collegata alla relativa rappresentazione in Azure AD.
4. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure classico e configurare l'accesso Single Sign-On nell'applicazione BetterWorks.

L'applicazione BetterWorks si aspetta che le asserzioni SAML abbiano un formato specifico. Configurare le attestazioni seguenti per questa applicazione. È possibile gestire i valori di questi attributi dalla scheda degli**attributi**dell'applicazione. La schermata seguente illustra un esempio relativo a questa operazione. 

![Configura accesso Single Sign-On](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_06.png)

**Per configurare l'accesso Single Sign-On di Azure AD con BetterWorks, seguire questa procedura:**

1. Fare clic su **Attributi** Nel menu visualizzato nella parte superiore della pagina di integrazione dell'applicazione **BetterWorks** del portale di Azure classico.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_07.png)
2. Nella finestra di dialogo **Attributi token SAML** seguire questa procedura per ciascuna riga della tabella:

    | Nome attributo | Valore attributo |
    | --- | --- |    
    | saml_token | bd189cf6-1701-11e6-8f90-d26992eca2a5 |

    a. Fare clic su **aggiungi attributo utente** per aprire la finestra di dialogo **Aggiungi attributo utente**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_12.png)

    b. Nella casella di testo **Nome attributo** , digitare il nome dell'attributo indicato per quella riga.

    c. Dall'elenco **Valore attributo** digitare l'ID del token SAML mostrato per quella riga.

    d. Fare clic su **Completa**

1. Nel menu in alto fare clic su **Avvio rapido**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_11.png) 
2. Nella pagina **Stabilire come si desidera che gli utenti accedano a BetterWorks** selezionare **Single Sign-On di Azure AD** e fare clic su **Avanti**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_03.png)
3. Nella pagina della finestra di dialogo **Configurare le impostazioni dell'app**, per configurare l'applicazione in **modalità avviata da IDP** seguire questa procedura:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_04.png)

    a. Nella casella di testo **Identificatore** digitare l'URL nel formato seguente: `https://app.betterworks.com/saml2/metadata/`


    b. Nella casella di testo **URL di risposta** digitare l'URL nel formato seguente: `https://app.betterworks.com/saml2/acs/`


    c. Fare clic su **Avanti**

1. Nella pagina della finestra di dialogo **Configurare le impostazioni dell'app**, per configurare l'applicazione in **modalità avviata da SP** seguire questa procedura:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_10.png)
   
    a.    Selezionare **Mostra impostazioni avanzate (facoltativo)**.

    b. Nella casella di testo **URL di accesso** digitare l'URL usato dagli utenti per accedere all'applicazione BetterWorks nel formato seguente: `https://app.betterworks.com`

    b. Fare clic su **Avanti**.

1. Nella pagina **Configura accesso Single Sign-On in BetterWorks** seguire questa procedura e fare clic su **Avanti**:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_05.png)
   
    a. Fare clic su **Scarica metadati**e quindi salvare il file nel computer.
   
    b. Fare clic su **Next**.
2. Per ottenere la configurazione dell'accesso SSO per l'applicazione, contattare il team di supporto di BetterWorks all'indirizzo <mailto:support@betterworks.com>. Allegare il file dei metadati scaricato e condividerlo con il team BetterWorks per la configurazione di SSO sul relativo lato.
3. Nel portale di Azure classico selezionare la conferma della configurazione e fare clic su **Avanti**.
   
    ![Single Sign-On di Microsoft Azure AD][10]
4. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.  
   
   ![Single Sign-On di Microsoft Azure AD][11]

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente di test chiamato Britta Simon nel portale classico.

![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_09.png)
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per visualizzare l'elenco di utenti, fare clic su **Utenti**nel menu in alto.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_03.png)
4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** nella barra degli strumenti in basso.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_04.png)
5. Nella pagina **Informazioni sull'utente** seguire questa procedura:
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_05.png)
   
    a. In Tipo di utente selezionare Nuovo utente nell'organizzazione.
   
    b. Nella casella di testo **Nome utente** digitare **BrittaSimon**.
   
    c. Fare clic su **Avanti**.
6. Nella pagina **Profilo utente** seguire questa procedura:
   
   ![Creazione di un utente test di Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_06.png)
   
   a. Nella casella di testo **Nome** digitare **Britta**.  
   
   b. Nella casella di testo **Cognome** digitare **Simon**.
   
   c. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.
   
   d. Nell'elenco **Ruolo** selezionare **Utente**.
   
   e. Fare clic su **Avanti**.
7. Nella pagina **Ottieni password temporanea** fare clic su **crea**.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_07.png)
8. Nella pagina **Ottieni password temporanea** seguire questa procedura:
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_08.png)
   
    a. Prendere nota del valore visualizzato in **Nuova password**.
   
    b. Fare clic su **Complete**.   

### <a name="creating-a-betterworks-test-user"></a>Creazione di un utente di test per BetterWorks
In questa sezione viene creato un utente di nome Britta Simon in BetterWorks. 

Per l'aggiunta di utenti alla piattaforma BetterWorks, rivolgersi al team di supporto di BetterWorks all'indirizzo <mailto:support@betterworks.com>.

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD
Questa sezione descrive come abilitare Britta Simon per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a BetterWorks.

   ![Assegna utente][200]

**Per assegnare Britta Simon a BetterWorks, seguire questa procedura:**

1. Per aprire la visualizzazione applicazioni nel portale classico, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.
   
    ![Assegna utente][201]
2. Nell'elenco delle applicazioni selezionare **BetterWorks**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_50.png)
3. Scegliere **Utenti**dal menu in alto.
   
    ![Assegna utente][203]
4. Nell'elenco di utenti selezionare **Britta Simon**.
5. Fare clic su **Assegna**sulla barra degli strumenti in basso.
   
    ![Assegna utente][205]

### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On
Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro BetterWorks nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione BetterWorks.

## <a name="additional-resources"></a>Risorse aggiuntive
* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


