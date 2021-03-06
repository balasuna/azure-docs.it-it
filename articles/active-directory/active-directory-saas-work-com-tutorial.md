---
title: 'Esercitazione: Integrazione di Azure Active Directory con Work.com | Documentazione Microsoft'
description: Informazioni su come usare Work.com con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: f454e7e218764e00cc19ca67b0edade213834b75
ms.openlocfilehash: 69d94659f4eff72e1c449fd915616d81fd4712de


---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Esercitazione: Integrazione di Azure Active Directory con Work.com
Questa esercitazione descrive l'integrazione di Azure e Work.com.  
Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

* Sottoscrizione di Azure valida
* Sottoscrizione di Work.com abilitata per l'accesso Single Sign-On

Al termine dell'esercitazione, gli utenti di ADD assegnati a Work.com potranno accedere all'applicazione tramite il sito aziendale di Work.com (accesso avviato dal provider di servizi) o seguendo le istruzioni riportate in [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1. Abilitazione dell'integrazione dell'applicazione Work.com
2. Configurazione dell'accesso Single Sign-On
3. Configurazione del provisioning utente
4. Assegnazione degli utenti

![Scenario](./media/active-directory-saas-work-com-tutorial/IC794105.png "Scenario")

## <a name="enabling-the-application-integration-for-workcom"></a>Abilitazione dell'integrazione dell'applicazione Work.com
Questa sezione descrive come abilitare l'integrazione dell'applicazione per Work.com.

### <a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Per abilitare l'integrazione dell'applicazione per Work.com, seguire questa procedura:
1. Nel portale di Azure classico fare clic su **Active Directory**nel riquadro di spostamento sinistro.
   
    ![Active Directory](./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
    ![Applications](./media/active-directory-saas-work-com-tutorial/IC700994.png "Applications")

4. Fare clic su **Add** nella parte inferiore della pagina.
   
    ![Aggiungi applicazione](./media/active-directory-saas-work-com-tutorial/IC749321.png "Add application")

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
    ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-work-com-tutorial/IC749322.png "Add an application from gallerry")

6. Nella **casella di ricerca** digitare **Work.com**.
   
    ![Raccolta di applicazioni](./media/active-directory-saas-work-com-tutorial/IC794106.png "Application Gallery")

7. Nel riquadro dei risultati selezionare **Work.com** e quindi fare clic su **Complete** (Completa) per aggiungere l'applicazione.
   
    ![Work.com](./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

## <a name="configuring-single-sign-on"></a>Configurazione dell'accesso Single Sign-On
Questa sezione descrive come consentire agli utenti di eseguire l'autenticazione a Work.com tramite il proprio account in Azure AD usando la federazione basata sul protocollo SAML.  
Come parte di questa procedura, verrà richiesto di caricare un certificato in Work.com.com.

> [!NOTE]
> Per configurare l'accesso Single Sign-On, è necessario configurare un nome di dominio personalizzato Work.com. È necessario definire almeno un nome di dominio, testare il nome di dominio e distribuirlo in tutta l'organizzazione.
> 
> 

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Per configurare l'accesso Single Sign-On, seguire questa procedura:
1. Accedere al tenant di Work.com come amministratore.
2. Passare a **Setup**.
   
    ![Configurazione](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3. Nella sezione **Administer** (Amministra) del riquadro di spostamento sinistro fare clic su **Domain Management** (Gestione dominio) per espandere la sezione correlata e quindi fare clic su **My Domain** (Dominio personale) per aprire la pagina **My Domain** (Dominio personale). 
   
    ![My Domain](./media/active-directory-saas-work-com-tutorial/IC767825.png "My Domain")

4. Per verificare che il dominio sia stato configurato correttamente, verificare che sia presente in "**Step 4 Deployed to Users**" (Passaggio 4 Distribuzione a utenti) e quindi verificare le selezioni in "**My Domain Settings**" (Impostazioni dominio personale).
   
    ![Domain Deployed to User](./media/active-directory-saas-work-com-tutorial/IC784377.png "Doman Deployed to User")

5. In una finestra diversa del Web browser accedere al portale di Azure classico.

6. Nella pagina di integrazione dell'applicazione **Work.com **fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-work-com-tutorial/IC794109.png "Configure Single Sign-On")

7. Nella pagina **Stabilire come si desidera che gli utenti accedano a Work.com** selezionare **Single Sign-On di Microsoft Azure AD** e quindi fare clic su **Avanti**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-work-com-tutorial/IC794110.png "Configure Single Sign-On")

8. Nella casella di testo **URL di accesso Work.com** della pagina **Configura URL app** digitare l'URL usato dagli utenti per accedere all'applicazione Work.com, ad esempio "*http://company.my.salesforce.com*" e quindi fare clic su **Avanti**: 
   
    ![Configura URL app](./media/active-directory-saas-work-com-tutorial/IC794111.png "Configure App URL")

9. Nella pagina **Configura accesso Single Sign-On in Work.com** per scaricare il file del certificato, fare clic su **Download certificato** e quindi salvare il certificato localmente nel computer.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-work-com-tutorial/IC794112.png "Configure Single Sign-On")

10. Accedere al tenant di Work.com.

11. Passare a **Setup**.
    
    ![Configurazione](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

12. Espandere il menu **Security Controls** (Controlli di sicurezza) e quindi fare clic su **Single Sign-On Settings** (Impostazioni Single Sign-On).
    
    ![Single Sign-On Settings](./media/active-directory-saas-work-com-tutorial/IC794113.png "Single Sign-On Settings")

13. Nella finestra di dialogo **Single Sign-On Settings** seguire questa procedura:
    
    ![Abilitato SAML](./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML Enabled")
    
    a. Selezionare **Abilitato SAML**.
    
    b. Fare clic su **New**.

14. Nella sezione **SAML Single Sign-On Setting** seguire questa procedura:
    
    ![SAML Single Sign-On Setting](./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML Single Sign-On Setting")
    
    a. Nella casella di testo **Name** digitare un nome per la configurazione.  
       
    > [!NOTE]
    > Se si specifica un valore per **Name** (Nome) verrà popolata automaticamente la casella di testo **API Name** (Nome API).
    > 
    > 
    
    b. Nella finestra di dialogo **Configura accesso Single Sign-On in Work.com** del portale di Azure classico copiare il valore **URL autorità di certificazione** e quindi incollarlo nella casella di testo **Autorità di certificazione**.
    
    c. Per caricare il certificato scaricato, fare clic su **Browse**.
    
    d. Nella casella di testo **ID entità** digitare **https://salesforce-work.com**.
    
    e. In **SAML Identity Type** (Tipo di identità SAML) selezionare **Assertion contains the Federation ID from the User object** (L'asserzione contiene l'ID federazione dell'oggetto utente).
    
    f. In **SAML Identity Location** (Percorso identità SAML) selezionare **Identity is in the NameIdentfier element of the Subject statement** (L'identità è nell'elemento NameIdentfier dell'istruzione Subject).
    
    g. Nella finestra di dialogo **Configura accesso Single Sign-On in Work.com** del portale di Azure classico copiare il valore di **URL accesso remoto** e quindi incollarlo nella casella di testo **URL di accesso provider di identità**.
    
    h. Nella finestra di dialogo **Configura accesso Single Sign-On in Work.com** del portale di Azure classico copiare il valore di **URL di disconnessione remota** e quindi incollarlo nella casella di testo **URL di disconnessione provider di identità**.
    
    i. In **Service Provider Initiated Request Binding** (Binding richiesta avviato dal provider di servizi) selezionare **HTTP Post**.
    
    j. Fare clic su **Save**.

15. Nel pannello di navigazione a sinistra del portale di Work.com classico fare clic su **Domain Management** (Gestione dominio) per espandere la sezione correlata e quindi fare clic su **My Domain** (Dominio personale) per aprire la pagina **My Domain** (Dominio personale). 
    
    ![My Domain](./media/active-directory-saas-work-com-tutorial/IC794115.png "My Domain")

16. Nella sezione **Login Page Branding** (Personalizzazione pagina di accesso) della pagina **My Domain** (Dominio personale) fare clic su **Edit** (Modifica).
    
    ![Login Page Branding](./media/active-directory-saas-work-com-tutorial/IC767826.png "Login Page Branding")

17. Nella sezione**Authentication Service** (Servizio autenticazione) della pagina **Login Page Branding** (Personalizzazione pagina di accesso) viene visualizzato il nome delle **impostazioni SAML SSO**. Selezionarlo e quindi fare clic su **Save**.
    
    ![Login Page Branding](./media/active-directory-saas-work-com-tutorial/IC784366.png "Login Page Branding")

18. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Complete** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.
    
    ![Configura accesso Single Sign-On](./media/active-directory-saas-work-com-tutorial/IC794116.png "Configure Single Sign-On")

## <a name="configuring-user-provisioning"></a>Configurazione del provisioning utente
Per consentire l'accesso agli utenti di Azure Active Directory, è necessario che eseguano il provisioning a Work.com.  
Nel caso di Work.com, il provisioning è un'attività manuale.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Per configurare il provisioning utente, seguire questa procedura:
1. Accedere al sito aziendale di Work.com come amministratore.

2. Passare a **Setup**.
   
    ![Configurazione](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")
3. Passare a **Manage Users (Gestisci utenti) \> Users (Utenti)**.
   
    ![Gestire gli utenti](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")

4. Fare clic su **Nuovo utente**.
   
    ![All Users](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")

5. Nella sezione User Edit seguire questa procedura:
   
    ![User Edit](./media/active-directory-saas-work-com-tutorial/IC794118.png "User Edit")
   
    a. Nelle caselle di testo appropriate immettere gli attributi **Last Name** (Cognome), **Alias**, **Email** (E-mail), **Username** (Nome utente) e **Nickname** (Nome alternativo) di un account Azure Active Directory valido di cui si desidera eseguire il provisioning.
   
    b. Selezionare **Role** (Ruolo), **User License**(Licenza utente) e **Profile** (Profilo).
   
    c. Fare clic su **Save**.  
      
    > [!NOTE]
    > Il titolare dell'account Azure Active Directory riceve un messaggio di posta elettronica con un collegamento da selezionare per confermare l'account e attivarlo.
    > 
    > 

## <a name="assigning-users"></a>Assegnazione degli utenti
Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

### <a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Per assegnare gli utenti a Work.com, seguire questa procedura:
1. Nel portale di Azure classico creare un account di test.

2. Nella pagina di integrazione dell'applicazione Work.com fare clic su **Assegna utenti**.
   
    ![Assegna utenti](./media/active-directory-saas-work-com-tutorial/IC794119.png "Assign Users")

3. Selezionare l'utente di test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.
   
    ![Sì](./media/active-directory-saas-work-com-tutorial/IC767830.png "Yes")

È ora necessario attendere 10 minuti e verificare che l'account sia stato sincronizzato con Work.com.

Se si desidera testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO1-->


