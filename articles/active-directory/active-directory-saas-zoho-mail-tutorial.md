---
title: 'Esercitazione: Integrazione di Azure Active Directory con Zoho Mail | Microsoft Docs'
description: Informazioni su come usare Zoho Mail con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/09/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d6983b7efb414b42436b63e61b11a791cb0185ec


---
# <a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Esercitazione: Integrazione di Azure Active Directory con Zoho Mail
Questa esercitazione descrive l'integrazione di Azure e Zoho Mail.  
Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

* Sottoscrizione di Azure valida
* Tenant di Zoho Mail

Al termine dell'esercitazione, gli utenti di Azure AD assegnati a Zoho Mail potranno accedere all'applicazione tramite il sito aziendale di Zoho Mail (accesso avviato dal provider di servizi) o seguendo le istruzioni riportate in [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1. Abilitazione dell'integrazione dell'applicazione Zoho Mail
2. Configurazione dell'accesso Single Sign-On
3. Configurazione del provisioning utente
4. Assegnazione degli utenti

![Scenario](./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Scenario")

## <a name="enabling-the-application-integration-for-zoho-mail"></a>Abilitazione dell'integrazione dell'applicazione Zoho Mail
Questa sezione descrive come abilitare l'integrazione dell'applicazione per Zoho Mail.

### <a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Per abilitare l'integrazione dell'applicazione per Zoho Mail, seguire questa procedura:
1. Nel portale di Azure classico fare clic su **Active Directory**nel riquadro di spostamento sinistro.
   
   ![Active Directory](./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
   ![Applications](./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Applications")
4. Fare clic su **Add** nella parte inferiore della pagina.
   
   ![Aggiungi applicazione](./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Add application")
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
   ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Add an application from gallerry")
6. Nella **casella di ricerca** digitare **Zoho Mail**.
   
   ![Raccolta di applicazioni](./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Application Gallery")
7. Nel riquadro dei risultati selezionare **Zoho Mail** e quindi fare clic su **Completa** per aggiungere l'applicazione.
   
   ![Zoho Mail](./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho Mail")

## <a name="configuring-single-sign-on"></a>Configurazione dell'accesso Single Sign-On
Questa sezione descrive come consentire agli utenti di eseguire l'autenticazione a Zoho Mail tramite il proprio account in Azure AD usando la federazione basata sul protocollo SAML.  
Come parte di questa procedura, verrà richiesto di creare un file di certificato con codifica Base 64.  
Se non si ha familiarità con questa procedura, vedere il video che illustra [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o).

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Per configurare l'accesso Single Sign-On, seguire questa procedura:
1. Nella pagina di integrazione dell'applicazione **Zoho Mail** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Configure Single Sign-On")
2. Nella pagina **Stabilire come si desidera che gli utenti accedano a Zoho Mail** selezionare **Single Sign-On di Microsoft Azure AD** e quindi fare clic su **Avanti**.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Configure Single Sign-On")
3. Nella pagina **Configura URL app** seguire questa procedura:
   
   ![Configura URL app](./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Configure App URL")
   
   a. Nella casella di testo **URL di accesso Zoho** digitare l'URL usando il modello seguente: `http://<company name>.ZohoMail.com`
   
   b. Fare clic su **Avanti**.
4. Nella pagina **Configura accesso Single Sign-On in Zoho Mail** fare clic su **Scarica certificato** e quindi salvare il file di certificato nel computer.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Configure Single Sign-On")
5. In un'altra finestra del Web browser accedere al sito aziendale di Zoho Mail come amministratore.
6. Passare a **Control panel**.
   
   ![Control panel](./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Control Panel")
7. Fare clic sulla scheda **SAML Authentication** .
   
   ![SAML Authentication](./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML Authentication")
8. Nella sezione **SAML Authentication Details** seguire questa procedura:
   
   ![SAML Authentication Details](./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "SAML Authentication Details")
   
   1. Nella finestra di dialogo **Configura accesso Single Sign-On in Zoho Mail** del portale di Azure classico copiare il valore di **URL accesso remoto** e quindi incollarlo nella casella di testo **URL di accesso**.
   2. Nella finestra di dialogo **Configura accesso Single Sign-On in Zoho Mail** del portale di Azure classico copiare il valore di **URL disconnessione remota** e quindi incollarlo nella casella di testo **URL di disconnessione**.
   3. Nella finestra di dialogo **Configura accesso Single Sign-On in Zoho Mail** del portale di Azure classico copiare il valore di **Modifica URL password** e quindi incollarlo nella casella di testo **Modifica URL password**.
   4. Creare un file **con codifica Base 64** dal certificato scaricato.  
      
      > [!TIP]
      > Per informazioni dettagliate, vedere [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   5. Aprire il certificato con codifica Base 64 nel Blocco note, copiarne il contenuto negli Appunti e quindi incollarlo nella casella di testo **PublicKey**
   6. In **Algorithm** (Algoritmo) selezionare **RSA**.
   7. Fare clic su **OK**.
9. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Complete** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.
   
   ![Configura accesso Single Sign-On](./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Configure Single Sign-On")

## <a name="configuring-user-provisioning"></a>Configurazione del provisioning utente
Per consentire agli utenti di Azure AD di accedere a Zoho Mail, è necessario eseguirne il provisioning in Zoho Mail.  
Nel caso di Zoho Mail, il provisioning è un'attività manuale.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Per eseguire il provisioning di un account utente, eseguire la procedura seguente:
1. Accedere al sito aziendale di **Zoho Mail** come amministratore.
2. Passare a **Control Panel \> Mail & Docs**.
3. Passare a **User Details \> Add User** (Dettagli utente > Aggiungi utente).
   
   ![Aggiunta di un utente](./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Add User")
4. Nella finestra di dialogo **Add users** seguire questa procedura:
   
   ![Aggiunta di un utente](./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Add User")
   
   1. Nelle apposite caselle di testo immettere i valori di **First Name**, **Last Name**, **Email ID** e **Password** di un account Azure Active Directory valido di cui si vuole eseguire il provisioning.
   2. Fare clic su **OK**.  
      
      > [!NOTE]
      > Il titolare dell'account Azure Active Directory riceve un messaggio di posta elettronica con un collegamento da selezionare per confermare l'account e attivarlo.
      > 
      > 

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Zoho Mail per eseguire il provisioning degli account utente Azure AD.
> 
> 

## <a name="assigning-users"></a>Assegnazione degli utenti
Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

### <a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Per assegnare gli utenti a Zoho Mail, seguire questa procedura:
1. Nel portale di Azure classico creare un account di test.
2. Nella pagina di integrazione dell'applicazione **Zoho Mail** fare clic su **Assegna utenti**.
   
   ![Assegna utenti](./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Assign Users")
3. Selezionare l'utente di test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.
   
   ![Sì](./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Yes")

Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


