---
title: Introduzione a iOS per Azure AD | Microsoft Docs
description: Come compilare un&quot;applicazione iOS che si integra con Azure AD per l&quot;accesso e chiama le API protette di Azure AD usando OAuth.
services: active-directory
documentationcenter: ios
author: xerners
manager: mbaldwin
editor: 
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
translationtype: Human Translation
ms.sourcegitcommit: c579135f798ea0c2a5461fdd7c88244d2d6d78c6
ms.openlocfilehash: c74e63621d422f8fa13bc1dd2730ec2c3325a46a


---
# <a name="integrate-azure-ad-into-an-ios-app"></a>Integrare Azure AD in un'app iOS
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure AD fornisce Active Directory Authentication Library, o ADAL, per i client iOS che devono accedere a risorse protette.  La funzione di ADAL è di permettere all'app di ottenere facilmente i token di accesso.  Per far capire quanto è semplice, verrà compilata un'applicazione Objective C To-Do List che:

* Ottiene i token di accesso per la chiamata all'API Graph di Azure AD con il [protocollo di autenticazione OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Cerca in una directory gli utenti con un determinato alias.

Per compilare l'applicazione funzionante completa, sarà necessario:

1. Registrare l'applicazione con Azure AD.
2. Installare e configurare ADAL.
3. Usare ADAL per ottenere i token da Azure AD.

Per iniziare, [scaricare la struttura dell'app](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) o [scaricare l'esempio completato](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  Sarà necessario anche un tenant di Azure AD in cui poter creare gli utenti e registrare un'applicazione.  Se non si ha già un tenant, vedere le [informazioni su come ottenerne uno](active-directory-howto-tenant.md).

> [!TIP]
> Si consiglia di provare l'anteprima del nuovo [portale per sviluppatori](https://identity.microsoft.com/Docs/iOS) che consentirà di imparare a usare Azure Active Directory in pochi minuti.  Il portale per sviluppatori guida l'utente nel processo di registrazione di un'app e di integrazione di Azure AD nel codice.  Al termine si otterrà una semplice applicazione in grado di autenticare gli utenti nel tenant e un back-end che può accettare i token ed eseguire la convalida. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>1. Determinare quale sarà l'URI di reindirizzamento per iOS
Per avviare in modo sicuro le applicazioni in determinati scenari SSO è necessario creare un **URI di reindirizzamento** in un formato particolare. Un URI di reindirizzamento viene usato per assicurarsi che i token vengano restituiti all'applicazione corretta che li aveva richiesti.

Il formato iOS per un URI di reindirizzamento è:

```
<app-scheme>://<bundle-id>
```

* **aap-scheme**: è registrato nel progetto XCode. ed è il modo in cui altre applicazioni possono eseguire la chiamata. È possibile trovarlo in Info.plist -> URL types -> URL Identifier. È consigliabile crearne uno se non sono stati configurati uno o più URI.
* **bundle-id** : è il Bundle Identifier presente in "identity" nelle impostazioni del progetto in XCode.

Un esempio di questo codice QuickStart sarebbe: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>2. Registrare l'applicazione DirectorySearcher
Per consentire all'applicazione di ottenere i token, sarà innanzitutto necessario registrarla nel tenant di Azure AD e concederle l'autorizzazione per accedere all'API Graph di Azure AD:

1. Accedere al [portale di Azure](https://portal.azure.com).
2. Nella barra in alto fare clic sull'account e nell'elenco **Directory** scegliere il tenant di Active Directory in cui si vuole registrare l'applicazione.
3. Fare clic su **Altri servizi** nella barra di spostamento a sinistra e scegliere **Azure Active Directory**.
4. Fare clic su **App registrations (Registrazioni app)** e scegliere **Aggiungi**.
5. Seguire le istruzioni e creare una nuova **Applicazione client nativa**.
  * Il **Nome** dell'applicazione deve essere una descrizione per gli utenti finali.
  * L' **URI di reindirizzamento** è una combinazione dello schema e della stringa che Azure AD userà per restituire le risposte dei token.  Immettere un valore specifico per l'applicazione in base alle informazioni sopra riportate.
6. Dopo avere completato la registrazione, AAD assegnerà all'app un ID app univoco.  Poiché questo valore sarà necessario nelle sezioni successive, copiarlo dalla scheda dell'applicazione.
7. Nella pagina **Impostazioni** scegliere **Autorizzazioni necessarie** e quindi scegliere **Aggiungi**. Selezionare **Microsoft Graph** come API e aggiungere l'autorizzazione **Lettura dati directory** in **Autorizzazioni delegate**.  In questo modo l'applicazione potrà cercare gli utenti nell'API Graph.

## <a name="3-install--configure-adal"></a>3. Installare e configurare ADAL
Ora che si dispone di un'applicazione in Azure AD, è possibile installare ADAL e scrivere il codice relativo all'identità.  Affinché la libreria ADAL possa comunicare con Azure AD, è necessario fornire alcune informazioni relative alla registrazione dell'app.

* A questo scopo, aggiungere ADAL al progetto DirectorySearcher usando Cocapods.

```
$ vi Podfile
```
Aggiungere il codice seguente al podfile:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Caricare ora il podfile usando cocoapods. Verrà creata una nuova area di lavoro XCode che verrà caricata.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

* Nel progetto QuickStart, aprire il file plist `settings.plist`.  Sostituire i valori degli elementi nella sezione in modo che corrispondano ai valori inseriti nel portale di Azure.  Il codice farà riferimento a questi valori ogni volta che userà ADAL.
  * `tenant` è il dominio del tenant di Azure AD, ad esempio, contoso.onmicrosoft.com.
  * `clientId` è l'ID client dell'applicazione copiato dal portale.
  * `redirectUri` è l'URL di reindirizzamento registrato nel portale.

## <a name="4----use-adal-to-get-tokens-from-aad"></a>4.    Usare ADAL per ottenere i token da AAD
Il principio alla base di ADAL è che l'app, ogni volta che ha bisogno di un token di accesso, deve solo chiamare un CompletionBlock `+(void) getToken : ` e ADAL fa il resto.  

* Nel progetto `QuickStart` aprire `GraphAPICaller.m` e trovare il commento `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` nella parte superiore,  dove si passano ad ADAL le coordinate attraverso un CompletionBlock per comunicare con Azure AD e gli si indica come memorizzare i token nella cache.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

* A questo punto è necessario usare questo token per cercare gli utenti nel grafico. Trovare il commento `// TODO: implement SearchUsersList` Questo metodo invia una richiesta GET all'API Graph di Azure AD per eseguire una query sugli utenti il cui UPN inizia con il termine di ricerca specificato.  Per eseguire una query nell'API Graph, è però necessario includere un oggetto access_token nell'intestazione `Authorization` della richiesta, dove entra in gioco ADAL.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                     // We can grab the top most JSON node to get our graph data.
                     NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                     // Don't be thrown off by the key name being "value". It really is the name of the
                     // first node. :-)

                     //each object is a key value pair
                     NSDictionary *keyValuePairs;
                     NSMutableArray* Users = [[NSMutableArray alloc]init];

                     for(int i =0; i < graphDataArray.count; i++)
                     {
                         keyValuePairs = [graphDataArray objectAtIndex:i];

                         User *s = [[User alloc]init];
                         s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                         s.name =[keyValuePairs valueForKey:@"givenName"];

                         [Users addObject:s];
                     }

                     completionBlock(Users, nil);
                 }
                 else
                 {
                     completionBlock(nil, error);
                 }

             }];
         }
     }];

}

```
* Quando l'app richiede un token chiamando `getToken(...)`, ADAL tenterà di restituire un token senza chiedere le credenziali all'utente.  Se ADAL determina che l'utente deve effettuare l'accesso per ottenere un token, visualizzerà una finestra di dialogo di accesso, raccoglierà le credenziali dell'utente e restituirà un token al termine dell'autenticazione.  Se ADAL non può restituire un token per qualsiasi motivo, genera una `AdalException`.
* Si noti che l'oggetto `AuthenticationResult` contiene un oggetto `tokenCacheStoreItem` che può essere usato per raccogliere informazioni che potrebbero essere richieste dall'app.  In QuickStart, `tokenCacheStoreItem` viene usato per determinare se l'autenticazione è già stata eseguita.

## <a name="5-build-and-run-the-application"></a>5: Compilare ed eseguire l'applicazione
Congratulazioni. È stata compilata un'applicazione iOS funzionante che può autenticare gli utenti, chiamare in modo sicuro le API Web usando OAuth 2.0 e ottenere informazioni di base sull'utente.  Se non si è ancora popolato il tenant con alcuni utenti, ora è possibile farlo.  Eseguire l'applicazione QuickStart e accedere come uno di questi utenti.  Cercare altri utenti in base al relativo UPN.  Chiudere l'app e rieseguirla.  Si noti che la sessione dell'utente non è stata modificata.

ADAL consente di incorporare facilmente nell'applicazione tutte queste funzionalità comuni relative alle identità.  Esegue automaticamente le attività più complesse: gestione della cache, supporto del protocollo OAuth, presentazione all'utente di un'interfaccia utente di accesso, aggiornamento dei token scaduti e altro.  Tutto ciò che occorre conoscere è una sola chiamata all'API, `getToken`.

Come riferimento, viene fornito l'esempio completato (senza i valori di configurazione) [qui](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Scenari aggiuntivi
Ora è possibile passare ad altri scenari.  È possibile consultare:

* [Proteggere un'API Web Node.js con Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
* Ottenere informazioni su [Come abilitare l'accesso Single Sign-On tra app in iOS usando ADAL](active-directory-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]




<!--HONumber=Jan17_HO3-->


