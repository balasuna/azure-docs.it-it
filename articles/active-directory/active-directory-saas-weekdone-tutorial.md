---
title: 'Esercitazione: Integrazione di Azure Active Directory con Weekdone | Microsoft Docs'
description: Informazioni su come configurare l&quot;accesso Single Sign-On tra Azure Active Directory e Weekdone.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 34921f9a-5637-4420-ab4c-9beb34421909
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: e1c9957f64e9a0a17823a881b060543a9346f457
ms.openlocfilehash: 8e69d4efe14e29d9f009a4b65416207bb39faa8b


---
# <a name="tutorial-azure-active-directory-integration-with-weekdone"></a>Esercitazione: Integrazione di Azure Active Directory con Weekdone
Questa esercitazione descrive l'integrazione di Weekdone con Azure Active Directory (Azure AD).

L'integrazione di Weekdone con Azure AD offre i vantaggi seguenti:

* È possibile controllare in Azure AD chi può accedere a Weekdone.
* È possibile abilitare gli utenti perché possano accedere automaticamente (accesso Single Sign-On) a Weekdone con i propri account Azure AD
* È possibile gestire gli account da una posizione centrale: il portale di Azure classico

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Prerequisiti
Per configurare l'integrazione di Azure AD con Weekdone, sono necessari gli elementi seguenti:

* Sottoscrizione di Azure AD.
* Sottoscrizione di Weekdone abilitata per l'accesso Single Sign-On (SSO)

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

1. Aggiunta di Weekdone dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="adding-weekdone-from-the-gallery"></a>Aggiunta di Weekdone dalla raccolta
Per configurare l'integrazione di Weekdone in Azure AD, è necessario aggiungere Weekdone dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Weekdone dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico**fare clic su **Active Directory**nel riquadro di spostamento sinistro. 
   
    ![Active Directory][1]
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
    ![Applications][2]
4. Fare clic su **Add** nella parte inferiore della pagina.
   
    ![Applicazioni][3]
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
    ![Applicazioni][4]
6. Nella casella di ricerca digitare **Weekdone**.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_01.png)
7. Nel riquadro dei risultati selezionare **Weekdone** e quindi fare clic su **Completa** per aggiungere l'applicazione.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come configurare e testare l'accesso Single Sign-On di Azure AD con Weekdone usando un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Weekdone che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Weekdone.

La relazione di collegamento viene stabilita assegnando il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente) in Weekdone.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Weekdone, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on)** : per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creazione di un utente test di Weekdone](#creating-a-weekdone-test-user)** : per avere una controparte di Britta Simon in Weekdone che sia collegata alla relativa rappresentazione in Azure AD.
4. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD
Questa sezione descrive come abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure classico e configurare l'accesso Single Sign-On nell'applicazione Weekdone.

**Per configurare Single Sign-On di Azure AD con Weekdone, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Weekdone** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
    ![Configura accesso Single Sign-On][6] 
2. Nella pagina **Stabilire come si desidera che gli utenti accedano a Weekdone** selezionare **Single Sign-On di Azure AD** e quindi fare clic su **Avanti**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_03.png) 
3. Nella pagina della finestra di dialogo **Configurare le impostazioni dell'app**, se si desidera configurare l'applicazione in **modalità iniziata da IDP**,seguire la procedura seguente e fare clic su **Avanti**:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_04.png) 

   1. Nella casella di testo **URL di risposta** digitare l'URL usando il modello seguente: **"https://weekdone.com/a/azure"**.

   2. Nella casella di testo **Identificatore** digitare l'URL usando il modello seguente: **"https://weekdone.com/a/azure/metadata"**.

   3. Fare clic su **Avanti**.

1. Se si vuole configurare l'applicazione in **modalità iniziata da SP**, nella finestra di dialogo **Configurare le impostazioni dell'app** selezionare **Mostra opzioni avanzate (facoltativo)**, immettere l'**URL di accesso** e l'**identificatore** e quindi fare clic su **Avanti**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_06.png) 
   
   1. Nella casella di testo **URL di accesso** digitare l'URL usato dagli utenti per accedere all'applicazione Weekdone usando il modello seguente: **"https://weekdone.com/a/azure"**.
   
   2. Nella casella di testo **Identificatore** digitare l'URL usando il modello seguente: **"https://weekdone.com/a/azure/metadata"**.
   
   3. Fare clic su **Avanti**.
2. Nella pagina **Configura accesso Single Sign-On in Weekdone** seguire la procedura seguente e fare clic su **Avanti**:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_05.png) 
   
   1. Fare clic su **Download certificato**e quindi salvare il certificato nel computer.
   2. Fare clic su **Avanti**.
    
3. Per la configurazione dell'accesso Single Sign-On per l'applicazione, contattare il team di supporto tecnico di Weekdone all'indirizzo hello@weekdone.com. Allegare il file del certificato scaricato al messaggio di posta elettronica e condividere gli URL dei metadati, ossia URL AUTORITÀ DI CERTIFICAZIONE, URL SSO SAML e URL SERVIZIO SINGLE SIGN-ON, con il team di Weekdone perché possa configurare l'accesso SSO.
4. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e fare clic su **Avanti**.
   
    ![Single Sign-On di Microsoft Azure AD][10]
5. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.  
   
    ![Single Sign-On di Microsoft Azure AD][11]

### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test chiamato Britta Simon nel portale di Azure classico.

![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.
   
   ![Creazione di un utente test di Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_09.png) 
    
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per visualizzare l'elenco di utenti, fare clic su **Utenti**nel menu in alto.
4.    
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_03.png) 
    
4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** nella barra degli strumenti in basso.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_04.png) 
    
5. Nella pagina **Informazioni sull'utente** seguire questa procedura:

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_05.png) 
   
    1. In Tipo di utente selezionare Nuovo utente nell'organizzazione.
    2. Nella casella di testo **Nome utente** digitare **BrittaSimon**.
    3. Fare clic su **Avanti**.
    
6. Nella pagina **Profilo utente** seguire questa procedura:

   ![Creazione di un utente test di Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_06.png) 
   
   1. Nella casella di testo **Nome** digitare **Britta**.  
   2. Nella casella di testo **Cognome** digitare **Simon**.
   3. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.
   4. Nell'elenco **Ruolo** selezionare **Utente**.
   5. Fare clic su **Avanti**.
  
7. Nella pagina **Ottieni password temporanea** fare clic su **crea**.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_07.png) 
    
8. Nella pagina **Ottieni password temporanea** seguire questa procedura:

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_08.png) 
   
   1. Prendere nota del valore visualizzato in **Nuova password**. 
   2. Fare clic su **Complete**.   

### <a name="creating-a-weekdone-test-user"></a>Creazione di un utente test di Weekdone
Questa sezione descrive come creare un utente di nome Britta Simon in Weekdone. Weekdone supporta il provisioning JIT, abilitato per impostazione predefinita.

Non è necessario alcun intervento dell'utente in questa sezione. Durante un tentativo di accesso a Weekdone verrà creato un nuovo utente se questo non è già esistente. [Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on).

> [!NOTE]
> Per creare un utente manualmente, è necessario contattare il team di supporto tecnico di Weekdone all'indirizzo hello@weekdone.com.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD
Questa sezione descrive come abilitare Britta Simon per l'uso dell'accesso Single Sign-On di Azure, concedendole così l'accesso a Weekdone.

![Assegna utente][200] 

**Per assegnare Britta Simon a Weekdone, seguire questa procedura:**

1. Per aprire la visualizzazione applicazioni nel portale di Azure classico, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.
   
    ![Assegna utente][201] 
2. Nell'elenco delle applicazioni selezionare **Weekdone**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_50.png) 
3. Scegliere **Utenti**dal menu in alto.
   
    ![Assegna utente][203] 
4. Nell'elenco di utenti selezionare **Britta Simon**.
5. Fare clic su **Assegna**sulla barra degli strumenti in basso.
   
    ![Assegna utente][205]

### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On
Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Weekdone nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Weekdone.

## <a name="additional-resources"></a>Risorse aggiuntive
* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_205.png



<!--HONumber=Feb17_HO1-->


