---
title: Distribuire la prima app Web HTML in Azure in cinque minuti (anteprima dell&quot;interfaccia della riga di comando 2.0) | Documentazione Microsoft
description: "Informazioni su come eseguire facilmente app Web nel servizio app mediante la distribuzione di un&quot;app di esempio. È possibile eseguire rapidamente vere e proprie attività di sviluppo con risultati immediati."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: fccbab2baafed3b0347f2c35b7926200ec69a450
ms.openlocfilehash: 640f04ca9a8351543d44899946464ed7cd4db437


---
# <a name="deploy-your-first-html-web-app-to-azure-in-five-minutes-cli-20-preview"></a>Distribuire la prima app Web HTML in Azure in cinque minuti (anteprima dell'interfaccia della riga di comando 2.0)

> [!div class="op_single_selector"]
> * [Primo sito HTML](app-service-web-get-started-html.md)
> * [Prima app .NET](app-service-web-get-started-dotnet.md)
> * [Prima app PHP](app-service-web-get-started-php.md)
> * [Prima app Node.js](app-service-web-get-started-nodejs.md)
> * [Prima app Python](app-service-web-get-started-python.md)
> * [Prima app Java](app-service-web-get-started-java.md)
> 
> 

Questa esercitazione illustra come distribuire una semplice app Web HTML e CSS nel [servizio app di Azure](../app-service/app-service-value-prop-what-is.md).
Il servizio app consente di creare app Web, [back-end di app per dispositivi mobili](/documentation/learning-paths/appservice-mobileapps/) e [app per le API](../app-service-api/app-service-api-apps-why-best-platform.md).

Si apprenderà come: 

* Creare un'app Web nel servizio app di Azure.
* Distribuirvi HTML e CSS.
* Osservare le pagine in esecuzione nell'ambiente di produzione.
* Aggiornare il contenuto nello stesso modo in cui si effettua il [push dei commit Git](https://git-scm.com/docs/git-push).

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>Versioni dell'interfaccia della riga di comando per completare l'attività

È possibile completare l'attività usando una delle versioni seguenti dell'interfaccia della riga di comando:

- [Interfaccia della riga di comando di Azure 1.0](app-service-web-get-started-html-cli-nodejs.md): l'interfaccia della riga di comando per i modelli di distribuzione classici e di gestione delle risorse
- [Interfaccia della riga di comando di Azure 2.0 (anteprima)](app-service-web-get-started-html.md): l'interfaccia della riga di comando di nuova generazione per il modello di distribuzione di gestione delle risorse

## <a name="prerequisites"></a>Prerequisiti
* [Git](http://www.git-scm.com/downloads).
* [Interfaccia della riga di comando di Azure 2.0 (anteprima)](/cli/azure/install-az-cli2).
* Un account Microsoft Azure. Se non si ha un account, è possibile [iscriversi per ottenere una versione di valutazione gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oppure [attivare i vantaggi per i sottoscrittori di Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> È possibile [provare il servizio app](https://azure.microsoft.com/try/app-service/) senza avere un account Azure. Creare un'app iniziale e provarla per un'ora, senza impegno e senza dover usare la carta di credito.
> 
> 

## <a name="deploy-a-simple-html-site"></a>Distribuire un semplice sito HTML
1. Aprire un nuovo prompt dei comandi di Windows, una finestra di PowerShell, una shell di Linux o un terminale di OS X. Eseguire `git --version` e `azure --version` per verificare che Git e l'interfaccia della riga di comando di Azure siano installati nel computer.
   
    ![Installazione di prova degli strumenti dell'interfaccia della riga di comando per la prima app Web in Azure](./media/app-service-web-get-started-languages/1-test-tools-2.0.png)
   
    Se gli strumenti non sono stati installati, vedere i collegamenti di download nella sezione [Prerequisiti](#Prerequisites) .
2. Accedere ad Azure come segue:
   
        az login
   
    Seguire le istruzioni del messaggio della Guida per continuare il processo di accesso.
   
    ![Accedere ad Azure per creare la prima app Web](./media/app-service-web-get-started-languages/3-azure-login-2.0.png)

3. Impostare l'utente di distribuzione per il servizio app. Si distribuirà il codice usando queste credenziali in un secondo momento.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Creare un nuovo [gruppo di risorse](../azure-resource-manager/resource-group-overview.md). Per questa prima esercitazione di servizio app, non è strettamente necessario conoscerne tutte le caratteristiche e funzioni.

        az group create --location "<location>" --name my-first-app-group

    Per visualizzare i possibili valori utilizzabili per `<location>`, usare il comando `az appservice list-locations` nell'interfaccia della riga di comando.

3. Creare un nuovo [piano di servizio app](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) "GRATUITO". Per questa prima esercitazione di servizio app, è sufficiente sapere che non sono previsti costi per le app Web in questo piano.

        az appservice plan create --name my-free-appservice-plan --resource-group my-first-app-group --sku FREE

4. Creare una nuova app Web con un nome univoco in `<app_name>`.

        az appservice web create --name <app_name> --resource-group my-first-app-group --plan my-free-appservice-plan

4. Successivamente, verrà fornito il sito HTML di esempio da distribuire. Passare alla directory di lavoro `CD` e clonare l'app di esempio come segue:
   
        cd <working_directory>
        git clone https://github.com/Azure-Samples/app-service-web-html-get-started.git

5. Passare al repository dell'app di esempio. 
   
        cd app-service-web-html-get-started
5. Configurare la distribuzione Git locale pe l'app Web del servizio app con il comando seguente:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-first-app-group

    Si otterrà un output JSON come questo, a indicare la configurazione del repository Git remoto:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Aggiungere l'URL in JSON come Git remoto per il repository locale (chiamato `azure` per motivi di semplicità).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Distribuire il codice di esempio nell'app Azure nello stesso modo in cui si effettua il push di qualsiasi codice con Git. Quando richiesto, usare la password configurata prima.
   
        git push azure master
   
    ![Effettuare il push di codice nella prima app Web in Azure](./media/app-service-web-get-started/5-push-code.png)
   
    Se è stato usato uno dei framework di linguaggio, verrà visualizzato un output diverso. Ciò dipende dal fatto che `git push` non solo inserisce il codice in Azure, ma attiva anche le attività di distribuzione nel motore di distribuzione. Se nella radice del progetto (repository) sono presenti file package.json (Node.js) o requirements.txt (Python) o se nel progetto ASP.NET è presente un file packages.config, lo script di distribuzione ripristina automaticamente i pacchetti necessari. È anche possibile [abilitare l'estensione Composer](web-sites-php-mysql-deploy-use-git.md#composer) per elaborare automaticamente i file composer.json nell'app PHP.

La distribuzione dell'app in Servizio app di Azure è stata completata.

## <a name="see-your-app-running-live"></a>Visualizzare l'app eseguita dinamicamente
Per visualizzare l'app eseguita dinamicamente in Azure, eseguire questo comando da qualsiasi directory del repository:

    azure site browse

## <a name="make-updates-to-your-app"></a>Eseguire aggiornamenti dell'app
Ora è possibile usare Git per effettuare in qualsiasi momento il push dalla radice del progetto (repository) per eseguire un aggiornamento del sito live. La procedura è simile a quella usata per la prima distribuzione del codice. Quando si vuole effettuare il push di una nuova modifica testata in locale, ad esempio, è sufficiente eseguire i comandi seguenti dalla radice del progetto (repository):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Passaggi successivi
Trovare la procedura di sviluppo e distribuzione più adatta al framework del linguaggio:

* [.NET](web-sites-dotnet-get-started.md)
* [PHP](app-service-web-php-get-started.md)
* [Node.js](app-service-web-nodejs-get-started.md)
* [Python](web-sites-python-ptvs-django-mysql.md)
* [Java](web-sites-java-get-started.md)

In alternativa, è possibile fare altre prove con la prima app Web, ad esempio:

* Provare [altri modi per distribuire il codice in Azure](web-sites-deploy.md). Per eseguire la distribuzione da un repository GitHub, ad esempio, è sufficiente selezionare **GitHub** anziché **Repository Git locale** in **Opzioni di distribuzione**.
* Ottimizzare l'app Azure: autenticare gli utenti, ridimensionare l'app in base alla richiesta e configurare alcuni avvisi sulle prestazioni, tutto con pochi clic. Vedere [Aggiungere funzionalità alla prima app Web](app-service-web-get-started-2.md).




<!--HONumber=Feb17_HO3-->


