---
title: Creare un&quot;app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio app di Azure
description: Informazioni su come sviluppare un&quot;app ASP.NET MVC 5 con il back-end del database SQL, aggiungere autenticazione e autorizzazione e distribuirla in Azure.
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
writer: Rick-Anderson
manager: wpickett
editor: 
ms.assetid: c0de419d-db6f-4157-94ca-f75d0ba6c0e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/21/2016
ms.author: riande
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 11d9869e656014fe7106f9c66975792c5faa357d


---
# <a name="create-an-aspnet-mvc-app-with-auth-and-sql-db-and-deploy-to-azure-app-service"></a>Creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio app di Azure
Questa esercitazione illustra come creare un'app Web ASP.NET MVC 5 sicura che consente agli utenti di effettuare l'accesso con le credenziali di Facebook o Google. L'applicazione è un semplice elenco di contatti che utilizza ADO.NET Entity Framework per l'accesso al database. Verrà distribuita l'applicazione per [Servizio App Azure](http://go.microsoft.com/fwlink/?LinkId=529714). 

Al termine dell'esercitazione, si disporrà di un'applicazione Web sicura, basata sui dati, in esecuzione nel cloud e in grado di utilizzare un database cloud. La figura seguente mostra la pagina di accesso dell'applicazione completata:

![pagina di accesso][rxb]

Contenuto dell'esercitazione:

* Come creare un progetto web ASP.NET MVC 5 sicuro in Visual Studio.
* Come autenticare e autorizzare gli utenti che accedono con le credenziali di account Google o Facebook (autenticazione con il social provider tramite [OAuth 2.0](http://oauth.net/2 "http://oauth.net/2")).
* Come autenticare e autorizzare gli utenti registrati in un database gestito dall'applicazione (utilizzando l'autenticazione locale [identità ASP.NET](http://asp.net/identity/)).
* Come usare ADO.NET Entity Framework 6 Code First per la lettura e la scrittura di dati in un database SQL.
* Come usare le Migrazioni Code First di Entity Framework per distribuire un database.
* Come archiviare dati relazionali nel cloud usando il Database SQL Azure.
* Come distribuire un progetto Web che usa un database in un' [app Web](http://go.microsoft.com/fwlink/?LinkId=529714) nel servizio app di Azure.

> [!NOTE]
> Si tratta di un'esercitazione lunga. Per una rapida introduzione ai progetti Web del Servizio app di Azure e di Visual Studio, vedere [Creare un'app Web ASP.NET nel Servizio app di Azure](web-sites-dotnet-get-started.md). Per informazioni su come risolvere i problemi, vedere la sezione [Risoluzione dei problemi](#troubleshooting) .
> 
> Oppure, per iniziare a usare il servizio app di Azure prima di iscriversi per ottenere un account Azure, passare al sito [Prova il servizio app](https://azure.microsoft.com/try/app-service/)in cui è possibile creare immediatamente un'app Web temporanea nel servizio app. Non è necessario fornire una carta di credito né impegnarsi in alcun modo.
> 
> 

## <a name="prerequisites"></a>Prerequisiti
Per completare l'esercitazione, è necessario un account Microsoft Azure. Se non si dispone di un account, è possibile [attivare i benefici della sottoscrizione Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) oppure [iscriversi per ottenere una versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

Per configurare l'ambiente di sviluppo, è necessario installare [Visual Studio 2013 Update 5](http://go.microsoft.com/fwlink/?LinkId=390521) o versione superiore e la versione più recente di [Azure SDK per .NET](http://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409). Il presente articolo è stato scritto per Visual Studio Update 4 e SDK 2.8.1. Le stesse istruzioni si applicano a Visual Studio 2015 con la versione più recente di [Azure SDK per .NET](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409) installata. Tenere presente che alcune schermate potrebbero essere diverse da quelle illustrate.

## <a name="create-an-aspnet-mvc-5-application"></a>Creazione di un'applicazione ASP.NET MVC 5
### <a name="create-the-project"></a>Creare il progetto
1. Scegliere **Nuovo progetto** dal menu **File**.
   
    ![Nuovo progetto nel menu File](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/gs13newproj.png)
2. Nella finestra di dialogo **Nuovo progetto** espandere **C#** e selezionare **Web** in **Modelli installati**, quindi selezionare **Applicazione Web ASP.NET**. Assegnare all'applicazione il nome **ContactManager** e quindi fare clic su **OK**.
   
    ![Finestra di dialogo Nuovo progetto](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13newprojdb.png)
   
    **Nota:** assicurarsi di immettere "ContactManager". Nei blocchi di codice che verranno copiati in seguito si presuppone che il nome del progetto sia ContactManager. 
3. Nella finestra di dialogo **Nuovo progetto ASP.NET** selezionare il modello **MVC**. Verificare che **Autenticazione** sia impostato su **Account utente individuali** e che le opzioni **Ospita nel cloud** e **Servizio app** siano selezionate.
   
    ![Finestra di dialogo Nuovo progetto ASP.NET](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/newproject.png)
4. Fare clic su **OK**.
5. Viene visualizzata la finestra di dialogo **Configura impostazioni app Web di Microsoft Azure** . Potrebbe essere necessario eseguire l’accesso se questa operazione non è già stata eseguita o immettere nuovamente le credenziali se la sessione è scaduta.
6. Facoltativo: modificare il valore nella casella **Nome app Web** (vedere la figura seguente).
   
    L'URL dell'app Web sarà {nome}.azurewebsites.net, di conseguenza questo nome deve essere univoco nel dominio azurewebsites.net. La configurazione guidata suggerisce un nome univoco tramite l'aggiunta di un numero al nome del progetto "ContactManager" e per questa esercitazione verrà mantenuto quello.
7. Nell'elenco a discesa **Gruppo di risorse**, selezionare un gruppo esistente o **Crea nuovo gruppo di risorse** (vedere l'immagine seguente). 
   
    Se si preferisce, è possibile selezionare un gruppo di risorse già esistente. Tuttavia, se si crea un nuovo gruppo di risorse e lo si usa solo per questa esercitazione, sarà più facile eliminare tutte le risorse di Azure create nel corso delle procedure al termine dell'esercitazione. Per informazioni sui gruppi di risorse, vedere [Panoramica di Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). 
8. Nell'elenco a discesa **Piano di servizio app** selezionare un piano esistente o **Crea nuovo piano di servizio app** (vedere l'immagine seguente).
   
    Se si preferisce, è possibile selezionare un piano di servizio app già esistente. Per informazioni sui piani di servizio app, vedere [Panoramica approfondita dei piani del servizio app di Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 
9. Toccare **Esplorare servizi di Azure aggiuntivi** per aggiungere un database SQL.
   
    ![Aggiungere nuovi servizi](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/n2.png)
10. Toccare l’icona **+** per aggiungere un database SQL.
    
     ![Nuovo database SQL](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/nsql.png)
11. Toccare **Nuovo** nella finestra di dialogo **Configura database SQL**:
    
     ![Password e nome amministratore SQL](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/nc.png)
12. Immettere un nome per l'amministratore e una password complessa.
    
     ![Nuovo database SQL](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/np.png)
    
     Il nome del server deve essere univoco. Può contenere lettere minuscole, numeri e trattini, ma non può terminare con un trattino. Il nome utente e la password sono le nuove credenziali create per il nuovo server. 
    
     Se si dispone già di un server di database, è possibile selezionare quello esistente anziché crearne uno nuovo. I server di database sono una risorsa preziosa e in genere è utile creare più database nello stesso server per attività di test e sviluppo anziché creare un server di database per ogni database. Per questa esercitazione, tuttavia, il server verrà usato solo temporaneamente, quindi creandolo nello stesso gruppo di risorse del sito Web sarà più facile eliminare sia l'app Web che il database eliminando l'intero gruppo di risorse al termine dell'esercitazione. 
    
     Se si seleziona un server di database esistente, assicurarsi che l'app Web e il database si trovino nella stessa area.
    
     ![Utilizzare il nuovo database](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/newdb.png)
13. Fare clic su **Crea**.
    
     Visual Studio crea il progetto Web ContactManager, crea il gruppo di risorse e il piano di servizio app specificati e crea un'app Web con il nome specificato nel servizio app di Azure.

### <a name="set-the-page-header-and-footer"></a>Impostare intestazione e piè di pagina
1. In **Esplora soluzioni** aprire il file *Layout.cshtml* nella cartella *Views\Shared*.
   
    ![File _Layout.cshtml in Esplora soluzioni][newapp004]
2. Sostituire ActionLink nel file *Layout.cshtml* con il codice seguente.

```
   @Html.ActionLink("CM Demo", "Index", "Contacts", new { area = "" }, new { @class = "navbar-brand" })
```
   Assicurarsi di modificare il terzo parametro da "Home" in "Contatti". Il markup precedente creerà un collegamento "Contatti" in ogni pagina al metodo Index del controller Contacts. Modificare il nome dell'applicazione nell'intestazione e nel piè di pagina da "My ASP.NET Application" e "Application name" a "Contact Manager" e "CM Demo". 

### <a name="run-the-application-locally"></a>Eseguire l'applicazione in locale
1. Premere CTRL+F5 per eseguire l'app.
   
    La home page dell'applicazione verrà visualizzata nel browser predefinito.
   
    ![App Web in esecuzione in locale](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr2.png)

Non è necessario eseguire per il momento altre operazioni per creare l'applicazione che verrà distribuita in Azure. 

## <a name="deploy-the-application-to-azure"></a>Distribuire l'applicazione in Azure
1. In Visual Studio fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica** dal menu di scelta rapida.
   
    ![Opzione Pubblica nel menu di scelta rapida del progetto](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13publish.png)
   
    Viene visualizzata la procedura guidata **Pubblica sito Web** .
2. Nella finestra di dialogo **Pubblica sito Web** fare clic su **Pubblica**.
   
    ![Pubblica](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr3.png)
   
    L'applicazione creata verrà ora eseguita nel cloud. Alla successiva distribuzione dell'applicazione, verranno distribuiti solo i file modificati o nuovi.
   
    ![Esecuzione nel cloud](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss4.PNG)

## <a name="enable-ssl-for-the-project"></a>Abilitare SSL per il progetto
1. In **Esplora soluzioni** fare clic sul progetto **ContactManager**, quindi premere F4 per aprire la finestra di dialogo **Proprietà**.
2. Impostare il valore di **SSL abilitato** su **True**. 
3. Copiare **SSL URL**.
   
    L'URL SSL sarà https://localhost:44300/ a meno che non siano state create app Web SSL in precedenza.
   
    ![Abilitazione di SSL][rxSSL]
4. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto **Contact Manager**, quindi scegliere **Proprietà**.
5. Fare clic sulla scheda **Web** .
6. Modificare **URL progetto** in modo da usare **URL SSL**, quindi salvare la pagina (CTRL+S).
   
    ![Abilitazione di SSL](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr1.png)
7. Verificare che Internet Explorer sia il browser avviato da Visual Studio, come illustrato nell'immagine seguente:
   
    ![Browser predefinito](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss12.PNG)
   
    Lo strumento di selezione del browser consente di specificare il browser predefinito avviato da Visual Studio. È possibile selezionare più browser in modo che Visual Studio li aggiorni automaticamente ogni volta che vengono apportate modifiche. Per altre informazioni, vedere l'articolo relativo all' [uso di Browser Link in Visual Studio 2013](http://www.asp.net/visual-studio/overview/2013/using-browser-link).
   
     ![Selezione browser](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss13.png)
8. Premere CTRL+F5 per eseguire l'applicazione. Fare clic su **Sì** per considerare attendibile il certificato autofirmato generato da IIS Express.
   
     ![Istruzioni per considerare attendibile il certificato autofirmato generato da IIS Express](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss26.PNG)
9. Leggere quanto riportato nella finestra di dialogo **Avviso di protezione** e quindi fare clic su **Sì** se si desidera installare il certificato che rappresenta **localhost**.
   
     ![Avviso relativo al certificato di IIS Express per localhost ](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss27.PNG)
10. In Internet Explorer viene visualizzata la pagina *Home* e non viene visualizzato alcun avviso SSL.
    
      ![IE SSL localhost e nessun avviso](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss28.PNG)
    
      Quando si usa SSL, Internet Explorer è un'ottima scelta perché accetta il certificato e mostra il contenuto HTTPS senza alcun avviso. Anche Microsoft Edge e Google Chrome accettano il certificato. Firefox usa un archivio certificati personalizzato, quindi verrà visualizzato un avviso.
    
      ![Avviso di Firefox relativo al certificato](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss30.PNG)

## <a name="add-a-database-to-the-application"></a>Aggiunta di un database all'applicazione
Si passerà quindi all'aggiornamento dell'app, in modo da aggiungere la possibilità di visualizzare e aggiornare i contatti e di archiviare i dati in un database. L'app userà Entity Framework per creare il database e per leggere e aggiornare i dati nel database.

### <a name="add-data-model-classes-for-the-contacts"></a>Aggiungere classi del modello di dati per i contatti
Creare innanzitutto un semplice modello di dati nel codice.

1. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella Modelli, quindi scegliere **Aggiungi** e infine **Classe**.
   
    ![Aggiungi classe nel menu di scelta rapida della cartella Modelli](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr5.png)
2. Nella finestra di dialogo **Aggiungi nuovo elemento** assegnare al nuovo file di classe il nome *Contact.cs*, quindi fare clic su **Aggiungi**.
   
    ![Finestra di dialogo Aggiungi nuovo elemento][adddb002]
3. Sostituire il contenuto del file Contact.cs con il codice seguente.
   
        using System.ComponentModel.DataAnnotations;
        using System.Globalization;
        namespace ContactManager.Models
        {
            public class Contact
            {
                public int ContactId { get; set; }
                public string Name { get; set; }
                public string Address { get; set; }
                public string City { get; set; }
                public string State { get; set; }
                public string Zip { get; set; }
                [DataType(DataType.EmailAddress)]
                public string Email { get; set; }
            }
        }
   La classe **Contact** definisce i dati che verranno archiviati per ogni contatto, oltre a una chiave primaria, *ContactID*, necessaria per il database.

### <a name="create-web-pages-that-enable-app-users-to-work-with-the-contacts"></a>Creare pagine Web che consentono agli utenti dell'app di utilizzare i contatti
La funzionalità di scaffolding di ASP.NET MVC consente di generare automaticamente codice per l'esecuzione di azioni di creazione, lettura, aggiornamento ed eliminazione (CRUD, Create, Read, Update, Delete). 

1. Creare il progetto **(CTRL+MAIUSC+B)**. Per usare il meccanismo di scaffolding, è innanzitutto necessario creare il progetto.
2. In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella Controllers, quindi scegliere **Aggiungi** e infine **Controller**.
   
    ![Opzione Aggiungi controller nel menu di scelta rapida della cartella Controllers][addcode001]
3. Nella finestra di dialogo **Aggiungi scaffold** selezionare **Controller MVC 5 con visualizzazioni, che usano Entity Framework**, quindi fare clic su **Aggiungi**.
   
    ![Finestra di dialogo Add Scaffold](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr6.png)
4. Nell'elenco a discesa **Classe modello** selezionare **Contact (ContactManager.Models)**. Vedere l'immagine di seguito.
5. In **Classe contesto di dati** selezionare **ApplicationDbContext (ContactManager.Models)**. La classe **ApplicationDbContext** verrà utilizzata per il database di appartenenza e per i dati relativi ai contatti.

    ![Finestra di dialogo relativa ai nuovi dati contesto](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss5.PNG)

1. Fare clic su **Aggiungi**.
   
   Visual Studio crea un controller con metodi e visualizzazioni di un controller per operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD) su database per oggetti **Contact** .

## <a name="enable-migrations-create-the-database-add-sample-data-and-a-data-initializer"></a>Abilitazione delle migrazioni, creazione del database, aggiunta di dati di esempio e di un inizializzatore di dati
L'attività successiva consiste nell'abilitare la funzionalità [Migrazioni Code First](http://msdn.microsoft.com/library/hh770484.aspx) , in modo da creare le tabelle di database in base al modello di dati creato.

1. Scegliere **Gestione pacchetti NuGet** dal menu **Strumenti**, quindi fare clic su **Console di Gestione pacchetti**.
   
    ![Console di Gestione pacchetti nel menu Strumenti](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/SS6.png)
2. Nella finestra **Console di Gestione pacchetti** immettere il comando seguente:
   
        enable-migrations
   
    Il comando **enable-migrations** consente di creare una cartella *Migrations* e di inserire in tale tabella il file *Configuration.cs*, che può essere modificato per effettuare il seeding del database e configurare le migrazioni. 
3. Nella finestra **Console di Gestione pacchetti** immettere il comando seguente:
   
        add-migration Initial

    Il comando **add-migration Initial** consente di generare un file con nome **&lt;date_stamp&gt;Initial** nella cartella *Migrations*. Il codice in questo file crea le tabelle di database. Il primo parametro **(Initial)** viene usato per creare il nome del file. È possibile visualizzare i nuovi file di classe in **Esplora soluzioni**.

    Nella classe **Initial** il metodo **Up** consente di creare la cartella Contacts e il metodo **Down**, usato quando si vuole tornare allo stato precedente, consente di rimuoverla.

1. Aprire il file *Migrations\Configuration.cs*. 
2. Aggiungere l'istruzione `using` seguente. 
   
         using ContactManager.Models;
3. Sostituire il metodo *Seed* con il codice seguente:
   
        protected override void Seed(ContactManager.Models.ApplicationDbContext context)
        {
            context.Contacts.AddOrUpdate(p => p.Name,
               new Contact
               {
                   Name = "Debra Garcia",
                   Address = "1234 Main St",
                   City = "Redmond",
                   State = "WA",
                   Zip = "10999",
                   Email = "debra@example.com",
               },
                new Contact
                {
                    Name = "Thorsten Weinrich",
                    Address = "5678 1st Ave W",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "thorsten@example.com",
                },
                new Contact
                {
                    Name = "Yuhong Li",
                    Address = "9012 State st",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "yuhong@example.com",
                },
                new Contact
                {
                    Name = "Jon Orton",
                    Address = "3456 Maple St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "jon@example.com",
                },
                new Contact
                {
                    Name = "Diliana Alexieva-Bosseva",
                    Address = "7890 2nd Ave E",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "diliana@example.com",
                }
                );
        }
   
    Questo codice consente di inizializzare (effettuare il seeding) il database con le informazioni sui contatti. Per ulteriori informazioni sul seeding del database, vedere la pagina relativa al [seeding e al debug di database di Entity Framework (EF)](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx). Creare il progetto per verificare che non siano presenti errori di compilazione.
4. In **Console di Gestione pacchetti** immettere il comando seguente:
   
        update-database
   
    ![Comandi di Console di Gestione pacchetti][addcode009]
   
    Il comando **update-database** consente di eseguire la prima migrazione al fine di creare il database. Per impostazione predefinita, il database viene creato come database LocalDB SQL Server Express. 
5. Premere CTRL+F5 per eseguire l'applicazione, quindi fare clic sul link **CM Demo** oppure passare a https://localhost:(port#)/Cm. 
   
    Nell'applicazione vengono mostrati i dati di seeding e sono disponibili collegamenti per la modifica, i dettagli e l'eliminazione. È possibile creare, modificare, eliminare e visualizzare i dati.
   
    ![Visualizzazione MVC dei dati][rx2]

## <a name="add-an-oauth2-provider"></a>Aggiungere un provider OAuth2
> [!NOTE]
> Per istruzioni dettagliate su come usare i siti portale per sviluppatori di Google e Facebook, in questa esercitazione sono disponibili collegamenti alle esercitazioni del sito ASP.NET. Google e Facebook, tuttavia, modificano i siti più spesso di quanto vengano aggiornate le esercitazioni, che risultano attualmente obsolete. In caso di problemi a seguire le indicazioni, vedere il commento Disqus alla fine di questa esercitazione per un elenco delle modifiche. 
> 
> 

[OAuth](http://oauth.net/ "http://oauth.net/") è un protocollo aperto che consente di eseguire l'autenticazione sicura in un metodo semplice e standard da applicazioni Web, per dispositivi mobili e desktop. Il modello Internet ASP.NET MVC usa OAuth per esporre Facebook, Twitter, Google e Microsoft come provider di autenticazione. Benché in questa esercitazione venga usato solo Google come provider di autenticazione, è possibile modificare con facilità il codice per usare un altro provider. I passaggi per l'implementazione di altri provider sono molto simili ai passaggi illustrati in questa esercitazione. Per usare Facebook come provider di autenticazione, vedere l'esercitazione relativa alla [creazione di un'app MVC 5 con autenticazione OAuth2 mediante Facebook, Twitter, LinkedIn e Google ](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on).

Oltre all'autenticazione, questa esercitazione usa anche i ruoli per implementare l'autorizzazione. Solo gli utenti aggiunti al ruolo *canEdit* potranno modificare i dati, ovvero creare, modificare o eliminare contatti.

1. Seguire le istruzioni in [MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on#goog) (App MVC 5 con autenticazione OAuth2 mediante Facebook, Twitter, LinkedIn e Google) nella sezione **Creating a Google app for OAuth 2 to set up a Google app for OAuth2** (Creazione di un'app Google per OAuth 2 per configurare un'app Google per OAuth2).
2. Eseguire e testare l'applicazione per verificare che sia possibile accedere usando l'autenticazione di Google.
3. Per creare pulsanti per l'accesso tramite social network con icone specifiche del provider, vedere l'articolo relativo alla [creazione di pulsanti di accesso dall'aspetto gradevole per ASP.NET MVC 5](http://www.jerriepelser.com/blog/pretty-social-login-buttons-for-asp-net-mvc-5)

## <a name="using-the-membership-api"></a>Utilizzo dell'API di appartenenza
In questa sezione verranno aggiunti al database di appartenenza un utente locale e il ruolo *canEdit* . Solo gli utenti con ruolo *canEdit* saranno in grado di modificare i dati. È consigliabile assegnare ai ruoli un nome corrispondente alle azioni che consentono di eseguire, quindi il nome *canEdit* è preferibile rispetto ad *admin*. Nel corso dell'evolversi dell'applicazione, sarà possibile aggiungere nuovi ruoli quali *canDeleteMembers*, invece di ruoli con nomi meno descrittivi quali *superAdmin*.

1. Aprire il file *migrations\configuration.cs* e aggiungere le istruzioni `using` seguenti:
   
        using Microsoft.AspNet.Identity;
        using Microsoft.AspNet.Identity.EntityFramework;
2. Aggiungere il metodo **AddUserAndRole** seguente alla classe:
   
        bool AddUserAndRole(ContactManager.Models.ApplicationDbContext context)
        {
            IdentityResult ir;
            var rm = new RoleManager<IdentityRole>
                (new RoleStore<IdentityRole>(context));
            ir = rm.Create(new IdentityRole("canEdit"));
            var um = new UserManager<ApplicationUser>(
                new UserStore<ApplicationUser>(context));
            var user = new ApplicationUser()
            {
                UserName = "user1@contoso.com",
            };
            ir = um.Create(user, "P_assw0rd1");
            if (ir.Succeeded == false)
                return ir.Succeeded;
            ir = um.AddToRole(user.Id, "canEdit");
            return ir.Succeeded;
        }
3. Chiamare il nuovo metodo dal metodo **Seed** :
   
        protected override void Seed(ContactManager.Models.ApplicationDbContext context)
        {
            AddUserAndRole(context);
            context.Contacts.AddOrUpdate(p => p.Name,
                // Code removed for brevity
        }
   
    Nelle immagini seguenti sono illustrate le modifiche apportate al metodo *Seed* :
   
    ![Immagine del codice](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss24.PNG)
   
    Questo codice consente di creare un nuovo codice chiamato *canEdit*, di creare un nuovo utente locale *user1@contoso.com*, quindi di aggiungere *user1@contoso.com* al ruolo *canEdit*. Per altre informazioni, vedere la pagina relativa alle [esercitazioni per ASP.NET Identity](http://www.asp.net/identity/overview/features-api) nel sito ASP.NET.

## <a name="use-temporary-code-to-add-new-social-login-users-to-the-canedit-role"></a>Utilizzo di codice temporaneo per aggiungere al ruolo canEdit nuovi utenti di accesso a social networking
In questa sezione sarà possibile modificare temporaneamente il metodo **ExternalLoginConfirmation** nel controller Account per aggiungere nuovi utenti che eseguono l'autenticazione con un provider OAuth oppure OpenID al ruolo *canEdit* . Ci auguriamo di riuscire a rendere disponibile in futuro uno strumento simile a [WSAT](http://msdn.microsoft.com/library/ms228053.aspx) che consentirà di creare e modificare account utente e ruoli. Fino a quel momento, sarà possibile ottenere lo stesso risultato usando il codice temporaneo.

1. Aprire il file **Controllers\AccountController.cs** e passare al metodo **ExternalLoginConfirmation**.
2. Aggiungere la chiamata seguente a **AddToRoleAsync** immediatamente prima della chiamata **SignInAsync**.
   
        await UserManager.AddToRoleAsync(user.Id, "canEdit");
   
   Il codice riportato sopra consente di aggiungere l'utente appena registrato al ruolo "canEdit", in modo da concedere all'utente l'accesso ai metodi di azione per la modifica dei dati. Il frammento di codice seguente mostra la nuova riga di codice nel contesto.
   
          // POST: /Account/ExternalLoginConfirmation
          [HttpPost]
          [AllowAnonymous]
          [ValidateAntiForgeryToken]
          public async Task ExternalLoginConfirmation(ExternalLoginConfirmationViewModel model, string returnUrl)
          {
             if (User.Identity.IsAuthenticated)
             {
                return RedirectToAction("Index", "Manage");
             }
             if (ModelState.IsValid)
             {
                // Get the information about the user from the external login provider
                var info = await AuthenticationManager.GetExternalLoginInfoAsync();
                if (info == null)
                {
                   return View("ExternalLoginFailure");
                }
                var user = new ApplicationUser { UserName = model.Email, Email = model.Email };
                var result = await UserManager.CreateAsync(user);
                if (result.Succeeded)
                {
                   result = await UserManager.AddLoginAsync(user.Id, info.Login);
                   if (result.Succeeded)
                   {
                      await UserManager.AddToRoleAsync(user.Id, "canEdit");
                      await SignInManager.SignInAsync(user, isPersistent: false, rememberBrowser: false);
                      return RedirectToLocal(returnUrl);
                   }
                }
                AddErrors(result);
             }
             ViewBag.ReturnUrl = returnUrl;
             return View(model);
          }

Più avanti nell'esercitazione l'applicazione verrà distribuita in Azure, a cui si effettuerà l'accesso tramite Google o un altro provider di autenticazione di terze parti. L'account appena registrato verrà aggiunto al ruolo *canEdit* . Chiunque abbia a disposizione l'URL dell'app Web e un ID di Google potrà effettuare la registrazione e aggiornare il database. Per impedire ad altri utenti di eseguire tali operazioni, è possibile arrestare il sito. Per verificare gli utenti appartenenti al ruolo *canEdit* , è possibile esaminare il database.

In **Console di Gestione pacchetti** fare clic sul tasto di direzione per visualizzare il comando seguente:

        Update-Database

Il comando **Update-Database** esegue il metodo **Seed**, che a sua volta esegue il metodo **AddUserAndRole** aggiunto in precedenza. Il metodo **AddUserAndRole** crea l'utente *user1@contoso.com* e lo aggiunge al ruolo *canEdit*.

## <a name="protect-the-application-with-ssl-and-the-authorize-attribute"></a>Protezione dell'applicazione con SSL e l'attributo Authorize
In questa sezione verrà applicato l'attributo [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) per limitare l'accesso ai metodi di azione. Gli utenti anonimi potranno visualizzare solo il metodo di azione **Index** del controller Home. Gli utenti registrati potranno visualizzare i dati sui contatti (pagine **Index** e **Details** del controller Cm) e le pagine About e Contact. Solo gli utenti appartenenti al ruolo *canEdit* saranno in grado di accedere ai metodi di azione e di modificare i dati.

1. Aprire il file *App_Start\FilterConfig.cs* e sostituire il metodo *RegisterGlobalFilters* con il codice seguente, che consente di aggiungere due filtri:
   
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new HandleErrorAttribute());
            filters.Add(new System.Web.Mvc.AuthorizeAttribute());
            filters.Add(new RequireHttpsAttribute());
        }
   
    Questo codice aggiunge il filtro [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) e il filtro [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) all'applicazione. Il filtro [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) consente di impedire agli utenti anonimi di accedere ai metodi dell'applicazione. È possibile utilizzare l'attributo [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) per rifiutare esplicitamente il requisito di autorizzazione in un paio di metodi, per consentire agli utenti anonimi di effettuare l'accesso e visualizzare la home page. Con [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) si rende obbligatorio l'accesso all'App Web tramite HTTPS.
   
    Un approccio alternativo consiste nell'aggiungere l'attributo [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) e l'attributo [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) a ogni controller, tuttavia è consigliabile applicarli all'intera applicazione per assicurare una protezione migliore. Grazie all'aggiunta a livello globale, ogni nuovo controller e ogni metodo di azione aggiunti saranno protetti automaticamente e non sarà necessario ricordarsi di applicare gli attributi. Per altre informazioni, vedere [Securing your ASP.NET MVC  App and the new AllowAnonymous Attribute](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) (Protezione dell'app ASP.NET MVC e del nuovo attributo AllowAnonymous). 
2. Aggiungere l'attributo [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) al metodo **Index** del controller Home. L'attributo [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) consente di aggiungere all'elenco di metodi consentiti i metodi che si desidera rifiutino in modo esplicito l'autorizzazione. 
   
        public class HomeController : Controller
        {
          [AllowAnonymous]
          public ActionResult Index()
          {
             return View();
          }
   
    Se si esegue una ricerca globale per *AllowAnonymous*, si noterà che viene usato nei metodi di accesso e registrazione del controller Account.
3. In *ContactsController.cs* aggiungere `[Authorize(Roles = "canEdit")]` ai metodi HttpGet e HttpPost che consentono di modificare i dati (Create, Edit, Delete, qualsiasi metodo di azione ad eccezione di Index e Details) nel controller *Cm*. Di seguito è riportata una parte del codice completato: 
   
        // GET: Cm/Create
        [Authorize(Roles = "canEdit")]
        public ActionResult Create()
        {
           return View(new Contact { Address = "123 N 456 W",
            City="Great Falls", Email = "ab@cd.com", Name="Joe Smith", State="MT",
           Zip = "59405"});
        }
        // POST: Cm/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
         [Authorize(Roles = "canEdit")]
        public ActionResult Create([Bind(Include = "ContactId,Name,Address,City,State,Zip,Email")] Contact contact)
        {
            if (ModelState.IsValid)
            {
                db.Contacts.Add(contact);
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(contact);
        }
        // GET: Cm/Edit/5
        [Authorize(Roles = "canEdit")]
        public ActionResult Edit(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Contact contact = db.Contacts.Find(id);
            if (contact == null)
            {
                return HttpNotFound();
            }
            return View(contact);
        }
4. Premere CTRL+F5 per eseguire l'applicazione.
5. Se si è ancora connessi da una sessione precedente, fare clic sul collegamento **Disconnetti** .
6. Fare clic sui collegamenti **About** o **Contact**. Si verrà reindirizzati alla pagina di accesso perché gli utenti anonimi non sono autorizzati a visualizzare tali pagine.
7. Fare clic sul collegamento **Esegui registrazione come nuovo utente** e aggiungere un utente locale con l'indirizzo e-mail *joe@contoso.com*. Assicurarsi che *Joe* sia in grado di visualizzare le pagine Home, About e Contact. 
   
    ![Accesso](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss14.PNG)
8. Fare clic sul collegamento *CM Demo* e verificare che i dati siano visibili.
9. Fare clic su un collegamento di modifica nella pagina. Si verrà reindirizzati alla pagina di accesso, perché il nuovo utente locale non è stato aggiunto al ruolo *canEdit*.
10. Accedere usando il nome utente *user1@contoso.com* e la password "P_assw0rd1" (il carattere "0" in "word" è uno zero). Si verrà reindirizzati alla pagina di modifica selezionata in precedenza. 
11.  Se non è possibile effettuare l'accesso con tale account e password, provare a copiare la password dal codice sorgente e incollarla. Se ancora non è possibile accedere, verificare la colonna **UserName** della tabella **AspNetUsers** per accertarsi che *user1@contoso.com* sia stato aggiunto. 
12. Verificare che sia possibile apportare modifiche ai dati.

## <a name="deploy-the-app-to-azure"></a>Distribuire l'app in Azure
1. In Visual Studio fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica** dal menu di scelta rapida.
   
    ![Opzione Pubblica nel menu di scelta rapida del progetto][firsdeploy003]
   
    Viene visualizzata la procedura guidata **Pubblica sito Web** .
2. Fare clic sulla scheda **Impostazioni** nella parte sinistra della finestra di dialogo **Pubblica sito Web**. 
3. In **ApplicationDbContext** selezionare il database creato al momento della creazione del progetto.
4. In **ContactManagerContext** selezionare **Esegui migrazioni Code First**.
   
    ![Scheda Impostazioni](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrc2.png)
5. Fare clic su **Pubblica**.
6. Effettuare l'accesso come *user1@contoso.com* (usando la password "P_assw0rd1") e verificare che sia possibile modificare i dati.
7. Effettuare la disconnessione.
8. Passare alla [Google Developers Console](https://console.developers.google.com/) e nella scheda **Credentials** aggiornare gli URI di reindirizzamento e le origini JavaScript in modo che usino l'URL di Azure.
9. Effettuare l'accesso utilizzando Google o Facebook. L'account Google o Facebook verrà aggiunto al ruolo **canEdit** . Se viene visualizzato un errore HTTP 400 e il messaggio *The redirect URI in the request: https://contactmanager{my version}.azurewebsites.net/signin-google did not match a registered redirect URI.* (L'URI di reindirizzamento nella richiesta https://contactmanager{my version}.azurewebsites.net/signin-google non corrisponde a un URI di reindirizzamento registrato), sarà necessario attendere che le modifiche apportate vengano propagate. Se questo errore viene visualizzato dopo qualche minuto, verificare che gli URI siano corretti.

### <a name="stop-the-web-app-to-prevent-other-people-from-registering"></a>Arrestare l'app Web per impedire la registrazione da parte di altri utenti
1. In **Esplora server** passare a **Azure > Servizio app > {gruppo di risorse} > {App Web}**.
2. Fare clic con il pulsante destro del mouse sull'app Web e scegliere **Arresta**. 
   
    In alternativa, dal [portale di Azure](https://portal.azure.com/)è possibile passare al pannello dell'app Web, quindi fare clic sull'icona **Arresta** nella parte superiore del pannello.
   
    ![Opzione Arresta il portale app Web](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/stopweb.png)

### <a name="remove-addtoroleasync-publish-and-test"></a>Rimuovere AddToRoleAsync, Publish e Test
1. Impostare come commento o rimuovere il codice seguente dal metodo **ExternalLoginConfirmation** nel controller Account:
   
        await UserManager.AddToRoleAsync(user.Id, "canEdit");
2. Creare il progetto. Le modifiche apportate al file verranno salvate. Controllare che non si siano verificati errori di compilazione.
3. Fare clic con il pulsante destro del mouse in **Esplora soluzioni**, quindi scegliere **Pubblica**.
   
       ![Publish in project context menu](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13publish.png)
4. Fare clic sul pulsante **Start Preview** . Vengono distribuiti solo i file che necessitano di aggiornamento.
5. Avviare l'app Web da Visual Studio o dal portale. **Non sarà possibile eseguire la pubblicazione durante l'interruzione dell'app Web**.
   
    ![Opzione Avvia app Web](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss15.png)
6. Tornare in Visual Studio e fare clic su **Pubblica**.
7. L'app Azure verrà aperta nel browser predefinito. Se si è effettuato l'accesso, disconnettersi in modo da visualizzare la home page come un utente anonimo.  
8. Fare clic sul collegamento **About** . Si verrà reindirizzati alla pagina di accesso.
9. Fare clic sul collegamento **Register** nella pagina di accesso, quindi creare un account locale. L'account locale verrà utilizzato per verificare se si è in grado di accedere alle pagine di sola lettura ma non alle pagine che consentono la modifica dei dati, protette dal ruolo *canEdit* . L'accesso dell'account locale verrà rimosso più avanti nell'esercitazione. 
   
    ![Register](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss16.PNG)
10. Verificare che sia possibile passare alle pagine *About* e *Contact*.
    
     ![Disconnetti](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss17.PNG)
11. Fare clic sul collegamento **CM Demo** per passare al controller **Cm**. In alternativa è possibile aggiungere *Cm* alla fine dell'URL. 
    
     ![Pagina CM](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr4.png)
12. Fare clic su un collegamento per la modifica. 
    
     Si verrà reindirizzati alla pagina di accesso. 
13. In **Use another service to log in**fare clic su Google o Facebook ed effettuare l'accesso con l'account utilizzato in precedenza per la registrazione. (Se si lavora velocemente e il cookie della sessione non è scaduto, l'accesso verrà eseguito automaticamente con l'account di Google o Facebook utilizzato in precedenza).
14. Verificare che sia possibile modificare i dati quando si effettua l'accesso con tale account.
    
     **Nota:** non è possibile disconnettersi da Google da questa app e quindi effettuare l'accesso a un account Google diverso con lo stesso browser. Se si utilizza un browser, sarà necessario passare a Google e disconnettersi. È possibile effettuare l'accesso con un altro account dallo stesso autenticatore di terze parti, ad esempio Google, utilizzando un browser diverso.
    
     Se nelle informazioni sull'account Google non sono stati compilati i campi relativi a nome e cognome, verrà generata un'eccezione di tipo NullReferenceException.

## <a name="examine-the-sql-azure-db"></a>Esaminare il database SQL di Azure
1. In **Esplora server** passare a **Azure > Database SQL > {database}**
2. Fare clic con il pulsante destro del mouse sul database e scegliere **Apri in Esplora oggetti di SQL Server**.
   
    ![Apertura in SSOX](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr12.png)
3. Se non è stata effettuata la connessione a questo database in precedenza, potrebbe essere richiesto di aggiungere una regola del firewall per consentire l'accesso all'indirizzo IP corrente. L'indirizzo IP sarà precompilato. Fare clic su **Aggiungi regola firewall** per abilitare l'accesso.
   
    ![Aggiungi regola firewall](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/addfirewallrule.png)
4. Accedere al database con il nome utente e la password specificati durante la creazione del server di database. 
5. Fare clic con il pulsante destro del mouse sulla tabella **AspNetUsers**, quindi scegliere **Visualizza dati**.
   
    ![Pagina CM](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr8.png)
6. Prendere nota dell'ID dell'account Google usato per la registrazione nel ruolo **canEdit** e dell'ID di *user1@contoso.com*. Questi devono essere gli unici utenti nel ruolo **canEdit**. Questa verifica verrà eseguita nel passaggio successivo.
   
    ![Pagina CM](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/s2.png)
7. In **Esplora oggetti di SQL Server** fare clic con il pulsante destro del mouse su **AspNetUserRoles** e scegliere **Visualizza dati**.
   
    ![Pagina CM](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rs1.png)
8. Verificare che i valori di **UserId** provengano da *user1@contoso.com* e dall'account Google registrato. 

## <a name="troubleshooting"></a>Risoluzione dei problemi
In caso di problemi, ecco alcuni suggerimenti su come procedere.

* Errori durante il provisioning del database SQL: verificare che sia installato l'SDK corrente. Le versioni precedenti alla 2.8.1 contengono un bug che in alcuni scenari causa errori quando VS cerca di creare il server di database o il database.
* Messaggio di errore simile a "Operazione non supportata per il tipo di offerta di sottoscrizione" durante la creazione di risorse di Azure: come sopra.
* Errori durante la distribuzione: vedere l'articolo sulla [distribuzione ASP.NET di base](web-sites-dotnet-get-started.md) . Tale scenario di distribuzione è più semplice e, se si verifica lo stesso problema, potrebbe essere più facile isolarlo. In alcuni ambienti aziendali, ad esempio, un firewall aziendale potrebbe impedire a Distribuzione Web di stabilire i tipi di connessione ad Azure necessari.
* Nessuna opzione per selezionare la stringa di connessione nella procedura guidata Pubblica sito Web durante la distribuzione: se è stato usato un metodo diverso per creare le risorse di Azure (ad esempio, si sta cercando di eseguire la distribuzione in un'App Web e in un database SQL creati nel portale), il database SQL potrebbe non essere associato all'App Web. La soluzione più semplice consiste nel creare una nuova app Web e un nuovo database usando VS, come mostrato nell'esercitazione. Non è necessario ricominciare l'esercitazione da capo. Nella procedura guidata Pubblica sito Web è infatti possibile scegliere di creare una nuova app Web. In questo modo verrà aperta la stessa finestra di dialogo di creazione delle risorse di Azure che si apre quando si crea il progetto.
* Le indicazioni relative al portale per sviluppatori di Google o Facebook sono obsolete: vedere il commento Disqus alla fine di questa esercitazione.

## <a name="next-steps"></a>Passaggi successivi
È stata creata un'applicazione ASP.NET MVC di base che autentica gli utenti. Per altre informazioni sulle attività di autenticazione comuni e su come proteggere i dati sensibili, vedere le esercitazioni seguenti.

* [Creare un'app Web ASP.NET MVC 5 sicura con accesso, conferma tramite posta elettronica e reimpostazione della password](http://www.asp.net/mvc/overview/getting-started/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset)
* [App ASP.NET MVC 5 con SMS e messaggio di posta elettronica per autenticazione a due fattori](http://www.asp.net/mvc/overview/getting-started/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication)
* [Procedure consigliate per la distribuzione di password e altri dati sensibili in ASP.NET e Azure](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure) 
* [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on) (Creare un'app ASP.NET MVC 5 con autenticazione OAuth2 mediante Facebook e Google) - Sono fornite istruzioni su come aggiungere dati del profilo al database di registrazione utenti e sull'uso di Facebook come provider di autenticazione.
* [Introduzione ad ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)

Per un'esercitazione più avanzata che illustra come usare Entity Framework, vedere [Getting Started with EF and MVC](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application) (Introduzione a Entity Framework e MVC).

Questa esercitazione è stata scritta da [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) con il supporto di Tom Dykstra e Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart)). 

Se lo si desidera, ***inviare commenti e suggerimenti*** sugli aspetti ritenuti utili e su eventuali miglioramenti da apportare, non solo in merito all'esercitazione ma anche ai prodotti illustrati. I commenti e suggerimenti saranno utili per definire la priorità dei miglioramenti da apportare. È inoltre possibile richiedere nuovi argomenti e votare gli argomenti esistenti alla pagina [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

## <a name="whats-changed"></a>Modifiche apportate
* Per una guida relativa al passaggio da Siti Web al servizio app, vedere [Servizio app di Azure e impatto sui servizi di Azure esistenti](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Using the Membership API]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update the Membership Database]:#ppd2

[setupwindowsazureenv]: #bkmk_setupwindowsazure
[createapplication]: #bkmk_createmvc4app
[deployapp1]: #bkmk_deploytowindowsazure1
[deployapp11]: #bkmk_deploytowindowsazure11
[adddb]: #bkmk_addadatabase


<!-- images-->
[rx2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rx2.png

[rx5]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx5.png
[rx6]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx6.png
[rx7]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx7.png
[rx8]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx8.png
[rx9]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx9.png

[rxb]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rxb.png


[rxSSL]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rxSSL.png

[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rxNOT2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT2.png

[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rr1]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rr1.png

[rxPrevDB]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxPrevDB.png

[rxWSnew]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxWSnew2.png
[rxCreateWSwithDB]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxCreateWSwithDB.png

[setup007]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/dntutmobile-setup-azure-site-004.png

[newapp004]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-createapp-004.png

[firsdeploy003]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-deploy1-publish-001.png

[adddb002]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-adddatabase-002.png
[addcode001]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-controller-add-context-menu.png

[addcode008]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/dntutmobile-migrations-package-manager-menu.png
[addcode009]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-migrations-package-manager-console.png


[Important information about ASP.NET in Azure web apps]: #aspnetwindowsazureinfo
[Next steps]: #nextsteps

[ImportPublishSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/ImportPublishSettings.png




<!--HONumber=Jan17_HO3-->


