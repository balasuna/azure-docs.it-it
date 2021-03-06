---
title: Compilare e distribuire un&quot;app per le API Java nel servizio app di Azure
description: Informazioni su come creare un pacchetto dell&quot;app per le API Java e distribuirlo nel servizio app di Azure.
services: app-service\api
documentationcenter: java
author: rmcmurray
manager: erikre
editor: tdykstra
ms.assetid: 8d21ba5f-fc57-4269-bc8f-2fcab936ec22
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: get-started-article
ms.date: 12/22/2016
ms.author: rachelap;robmcm
translationtype: Human Translation
ms.sourcegitcommit: ff60ebaddd3a7888cee612f387bd0c50799496ac
ms.openlocfilehash: 26f2fba953870564292ef3e986a3151373f6babf


---
# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Compilare e distribuire un'app per le API Java nel servizio app di Azure
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Questa esercitazione illustra come creare un'applicazione Java e come distribuirla nelle app per le API del servizio app di Azure tramite [Git]. Le istruzioni di questa esercitazione possono essere eseguite in qualsiasi sistema operativo in grado di eseguire Java. Il codice in questa esercitazione viene compilato con [Maven]. [Jax-RS] consente invece di creare il servizio RESTful e viene generato in base alle specifiche dei metadati [Swagger] usando l'[editor Swagger].

## <a name="prerequisites"></a>Prerequisiti
1. [Java Development Kit 8] \((o versione successiva)
2. [Maven] installato nel computer di sviluppo
3. [Git] installato nel computer di sviluppo
4. Una sottoscrizione di [valutazione gratuita] o a pagamento a [Microsoft Azure]
5. Un'applicazione di test HTTP come [Postman]

## <a name="scaffold-the-api-using-swaggerio"></a>Eseguire lo scaffolding dell'API con Swagger.IO
Nell'editor online Swagger.io è possibile immettere il codice YAML o JSON Swagger che rappresenta la struttura dell'API. Dopo aver progettato la superficie di attacco dell'API, è possibile esportare il codice per vari tipi di piattaforme e framework. Nella sezione successiva, il codice sottoposto a scaffolding verrà modificato in modo da includere la funzionalità di implementazione fittizia. 

In questa dimostrazione, un corpo JSON Swagger iniziale verrà incollato nell'editor Swagger.io, che verrà quindi usato per generare codice che sfrutta JAX-RS per accedere a un endpoint API REST. Si modificherà quindi il codice con scaffolding in modo che restituisca dati fittizi, simulando un'API REST creata in base a un meccanismo di persistenza dei dati.  

1. Copiare negli appunti il codice JSON Swagger seguente:
   
        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }
2. Passare all' [editor Swagger online]. Fare clic sulla voce di menu **File -> Paste JSON** (Incolla JSON).
   
    ![Voce di menu Paste JSON (Incolla JSON)][paste-json]
3. Incollare il codice JSON Swagger relativo all'API dell'elenco dei contatti copiato in precedenza. 
   
    ![Incollare codice JSON in Swagger][pasted-swagger]
4. Visualizzare le pagine di documentazione e il riepilogo dell'API restituito nell'editor. 
   
    ![Visualizzare documenti generati da Swagger][view-swagger-generated-docs]
5. Selezionare l'opzione di menu **Generate Server -> JAX-RS** (Genera server -> JAX-RS) per eseguire lo scaffolding del codice lato server che dovrà essere modificato per aggiungere l'implementazione fittizia. 
   
    ![Generare una voce di menu del codice][generate-code-menu-item]
   
    Al termine della generazione del codice, verrà visualizzato un file ZIP per il download. Il file contiene il codice sottoposto a scaffolding dal generatore di codice Swagger e tutti gli script di compilazione associati. Decomprimere l'intera libreria in una directory della workstation di sviluppo. 

## <a name="edit-the-code-to-add-api-implementation"></a>Modificare il codice per aggiungere l'implementazione dell'API
In questa sezione si sostituirà l'implementazione sul lato server del codice generato da Swagger con il codice personalizzato. Il nuovo codice restituirà al client chiamante un'entità ArrayList of Contact. 

1. Aprire il file di modello *Contact.java* incluso nella cartella *src/gen/java/io/swagger/model* usando [Visual Studio Code] o l'editor di testo preferito. 
   
    ![Aprire un file di modello dei contatti][open-contact-model-file]
2. Aggiungere il costruttore seguente alla classe **Contact** . 
   
        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }
3. Aprire il file di implementazione del servizio *ContactsApiServiceImpl.java* incluso nella cartella *src/main/java/io/swagger/api/impl* usando [Visual Studio Code] o l'editor di testo preferito.
   
    ![Aprire il file del codice di servizio dei contatti][open-contact-service-code-file]
4. Sovrascrivere il codice del file con questo nuovo codice per aggiungere un'implementazione fittizia al codice del servizio. 
   
        package io.swagger.api.impl;
   
        import io.swagger.api.*;
        
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
               
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;
   
        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
   
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
   
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
   
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
   
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }
5. Aprire il prompt dei comandi e passare alla cartella radice dell'applicazione.
6. Eseguire il comando Maven seguente per compilare il codice ed eseguirlo in locale tramite il server di app Jetty. 
   
        mvn package jetty:run
7. Dovrebbe essere visualizzata la finestra di comando in cui si specifica che Jetty ha avviato il codice sulla porta 8080. 
   
    ![Aprire il file del codice di servizio dei contatti][run-jetty-war]
8. Usare [Postman] per effettuare una richiesta al metodo API "ottieni tutti i contatti" in http://localhost:8080/api/contacts.
   
    ![Chiamare l'API dei contatti][calling-contacts-api]
9. Usare [Postman] per effettuare una richiesta al metodo API "ottieni tutti i contatti" in http://localhost:8080/api/contacts/2.
   
    ![Chiamare l'API dei contatti][calling-specific-contact-api]
10. Compilare infine il file Java WAR (Web ARchive) eseguendo il seguente comando Maven nella console. 
    
         mvn package war:war
11. Una volta creato, il file WAR verrà inserito nella cartella di **destinazione**. Passare alla cartella di **destinazione** e rinominare il file WAR **ROOT.war**, accertandosi di rispettare le lettere maiuscole.
    
          rename swagger-jaxrs-server-1.0.0.war ROOT.war
12. Eseguire infine i comandi seguenti dalla cartella radice dell'applicazione per creare una cartella di **distribuzione** da usare per distribuire il file WAR in Azure. 
    
          mkdir deploy
          mkdir deploy\webapps
          copy target\ROOT.war deploy\webapps
          cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>Pubblicare l'output nel servizio app di Azure
Questa sezione descrive come creare una nuova app per le API tramite il portale di Azure, preparare l'app per le API per l'hosting di applicazioni Java e distribuire il file WAR appena creato nel servizio app di Azure per eseguire la nuova app per le API. 

1. Creare una nuova app per le API nel [Portale di Azure]. A tale scopo, fare clic sulla voce di menu **Nuovo -> Web e dispositivi mobili -> API App**, immettere i dettagli dell'app e quindi fare clic su **Crea**.
   
    ![Creare una nuova app per le API][create-api-app]
2. Dopo aver creato l'app per le API, aprire il pannello **Impostazioni** dell'app e quindi fare clic sulla voce di menu **Impostazioni dell'applicazione**. Selezionare le versioni più recenti di Java nelle opzioni disponibili e quindi la versione più recente di Tomcat nel menu **Contenitore Web** e infine fare clic su **Salva**.
   
    ![Configurare Java nel pannello dell'app per le API][set-up-java]
3. Fare clic sulla voce di menu delle impostazioni **Credenziali per la distribuzione** e specificare il nome utente e la password da usare per la pubblicazione dei file nell'app per le API. 
   
    ![Reimpostare le credenziali di distribuzione][deployment-credentials]
4. Fare clic sulla voce di menu delle impostazioni **Origine distribuzione** . Fare quindi clic sul pulsante **Scegliere l'origine**, selezionare l'opzione **Local Git Repository** e infine fare clic su **OK**. Verrà creato un repository Git in esecuzione in Azure, contenente un'associazione con l'app per le API. Ogni volta che si esegue il commit di un codice nel ramo *master* del repository Git, il codice verrà pubblicato nell'istanza dell'app per le API in esecuzione. 
   
    ![Configurare un nuovo repository Git locale][select-git-repo]
5. Copiare negli appunti l'URL del nuovo repository Git. Salvare il file, poiché risulterà di grande importanza più avanti. 
   
    ![Configurare un nuovo repository Git per l'app][copy-git-repo-url]
6. Eseguire il push GIT del file WAR nel repository online. A tale scopo, passare alla cartella di **distribuzione** creata in precedenza, in modo da poter facilmente eseguire il commit del codice nel repository in cui è in esecuzione il servizio app. Dopo aver attivato la finestra della console ed essersi spostati nella directory in cui si trova la cartella webapps, inviare i comandi Git seguenti per avviare il processo e attivare una distribuzione. 
   
         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master
   
    Dopo aver emesso la richiesta di **push** , viene richiesta la password creata in precedenza per le credenziali di distribuzione. Dopo l'immissione delle credenziali, nel portale dovrebbe essere visualizzata la distribuzione dell'aggiornamento.
7. Se si accede alla nuova app per le API in esecuzione nel servizio app di Azure con Postman, si noterà che il comportamento è coerente, che ora restituisce i dati dei contatti previsti e, tramite un semplice codice, si trasforma in codice Java sottoposto a scaffolding da Swagger.io. 
   
    ![Usare l'API REST dei contatti Java in Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a>Passaggi successivi
Questo articolo è iniziato con un file JSON Swagger e il codice Java sottoposto a scaffolding ottenuto dall'editor Swagger.io. Con alcune modifiche e un processo di distribuzione di un repository Git è stata creata un'app per le API funzionale, scritta in Java. L'esercitazione successiva mostra come [utilizzare app per le API da client JavaScript tramite CORS][App Service API CORS]. Le esercitazioni successive della serie illustrano come implementare l'autenticazione e l'autorizzazione.

Per approfondire questo esempio, è possibile acquisire informazioni su [Storage SDK per Java] per rendere permanenti i BLOB JSON. In alternativa, è possibile usare [DocumentDB Java SDK] per salvare i dati dei contatti in Azure DocumentDB. 

<a name="see-also"></a>

## <a name="see-also"></a>Vedere anche
Per altre informazioni su come usare Azure con Java, vedere il [Centro per sviluppatori Java di Azure].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Portale di Azure]: https://portal.azure.com/
[DocumentDB Java SDK]: ../documentdb/documentdb-java-application.md
[valutazione gratuita]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Centro per sviluppatori Java di Azure]: /develop/java/
[Java Development Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[editor Swagger online]: http://editor.swagger.io/
[Postman]: https://www.getpostman.com/
[Storage SDK per Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[editor Swagger]: http://editor.swagger.io/
[Visual Studio Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png



<!--HONumber=Jan17_HO1-->


