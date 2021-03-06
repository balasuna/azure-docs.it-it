---
title: Aggiungere l&quot;azione HTTP nelle app per la logica | Documentazione Microsoft
description: "Panoramica dell&quot;azione HTTP con proprietà"
services: 
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
tags: connectors
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 9c74b25a2ac5e2088a841d97920035376b7f3f11
ms.openlocfilehash: d3514dad84bea024ad6215711877a9784a8d8ffd


---
# <a name="get-started-with-the-http-action"></a>Introduzione all'azione HTTP
Con l'azione HTTP è possibile estendere i flussi di lavoro per l'organizzazione e comunicare con qualsiasi endpoint su HTTP.

È possibile:

* Creare flussi di lavoro con app per la logica che si attivano (trigger) quando un sito Web gestito diventa inattivo.
* Comunicare con qualsiasi endpoint su HTTP per estendere i flussi di lavoro ad altri servizi.

Per informazioni su come iniziare a usare un'azione HTTP in un'app per la logica, vedere [Creare un'app per la logica](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>Usare un trigger HTTP
Un trigger è un evento che può essere usato per avviare il flusso di lavoro definito in un'app per la logica. [Altre informazioni sui trigger](connectors-overview.md).

Ecco una sequenza di esempio di come configurare un trigger HTTP nella finestra di progettazione dell'app per la logica.

1. Aggiungere il trigger HTTP all'app per la logica.
2. Specificare i parametri per l'endpoint HTTP di cui si vuole eseguire il polling.
3. Modificare l'intervallo di ricorrenza che stabilisce la frequenza del polling.
4. L'app per la logica ora si attiva con il contenuto restituito durante ogni controllo.

![Trigger HTTP](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>Come funziona il trigger HTTP
Il trigger HTTP esegue una chiamata a un endpoint HTTP a intervalli ricorrenti. Per impostazione predefinita, il codice di risposta HTTP minore di 300 genera un'esecuzione dell'app per la logica. È possibile aggiungere una condizione nella visualizzazione Codice che verrà valutata dopo la chiamata HTTP per determinare se l'app per la logica deve essere eseguita. Di seguito è riportato un esempio di trigger HTTP che viene attivato ogni volta che il codice di stato restituito è maggiore di o uguale a `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Tutti i dettagli sui parametri dei trigger HTTP sono disponibili in [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-the-http-action"></a>Usare l'azione HTTP
Un'azione è un'operazione eseguita dal flusso di lavoro e definita in un'app per la logica. [Altre informazioni sulle azioni](connectors-overview.md).

1. Fare clic sul pulsante **Nuovo passaggio** .
2. Scegliere **Aggiungi un'azione**.
3. Nella casella di ricerca dell'azione digitare **http** per elencare l'azione HTTP.
   
    ![Selezionare l'azione HTTP](./media/connectors-native-http/using-action-1.png)
4. Aggiungere i parametri richiesti per la chiamata HTTP.
   
    ![Completare l'azione HTTP](./media/connectors-native-http/using-action-2.png)
5. Fare clic nell'angolo in alto a sinistra della barra degli strumenti per salvare. L'app per la logica verrà salvata e pubblicata (attivata).

## <a name="http-trigger"></a>Trigger HTTP
Ecco i dettagli per il trigger supportato da questo connettore. Il connettore HTTP supporta un solo trigger.

| Trigger | Descrizione |
| --- | --- |
| http |Esegue una chiamata HTTP e restituisce il contenuto della risposta. |

## <a name="http-action"></a>Azione HTTP
Ecco i dettagli per l'azione supportata da questo connettore. Il connettore HTTP supporta una sola azione possibile.

| Azione | Descrizione |
| --- | --- |
| http |Esegue una chiamata HTTP e restituisce il contenuto della risposta. |

## <a name="http-details"></a>Dettagli di HTTP
La tabella seguente descrive i campi di input obbligatori e facoltativi per l'azione e i dettagli di output corrispondenti associati all'uso dell'azione.

#### <a name="http-request"></a>Richiesta HTTP
Di seguito sono riportati campi di input per l'azione, che esegue una richiesta HTTP in uscita.
Un asterisco (*) indica che è un campo obbligatorio.

| Nome visualizzato | Nome proprietà | Descrizione |
| --- | --- | --- |
| Metodo* |statico |Verbo HTTP da usare |
| URI* |Uri |URI per la richiesta HTTP |
| Headers |Headers |Un oggetto JSON delle intestazioni HTTP da includere |
| Corpo |Corpo |Il corpo della richiesta HTTP |
| Autenticazione |Autenticazione |Dettagli nella sezione [Autenticazione](#authentication) . |

<br>

#### <a name="output-details"></a>Dettagli dell'output
Di seguito sono riportati i dettagli di output per la risposta HTTP.

| Nome proprietà | Tipo di dati | Descrizione |
| --- | --- | --- |
| headers |object |Intestazioni della risposta |
| Corpo |object |Oggetto della risposta |
| Codice di stato |int |Stato codice HTTP |

## <a name="authentication"></a>Autenticazione
La funzionalità App per la logica permette di usare diversi tipi di autenticazione per gli endpoint HTTP. Questo tipo di autenticazione può essere usato con i connettori **HTTP**, **[HTTP e Swagger](connectors-native-http-swagger.md)** e **[Webhook HTTP](connectors-native-webhook.md)**. È possibile configurare i seguenti tipi di autenticazione:

* [Autenticazione di base](#basic-authentication)
* [Autenticazione con certificato client](#client-certificate-authentication)
* [Autenticazione OAuth di Azure Active Directory (Azure AD)](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Autenticazione di base
Per l'autenticazione di base è necessario l'oggetto di autenticazione seguente.
Un asterisco (*) indica che è un campo obbligatorio.

| Nome proprietà | Tipo di dati | Descrizione |
| --- | --- | --- |
| Type* |type |Tipo di autenticazione (deve essere `Basic` per l'autenticazione di base) |
| Username* |username |Nome utente da autenticare |
| Password* |password |Password da autenticare |

> [!TIP]
> Se si vuole usare una password che non può essere recuperata dalla definizione, usare un parametro `securestring` e la [funzione di definizione del flusso di lavoro](http://aka.ms/logicappdocs) `@parameters()`.
> 
> 

Pertanto è possibile creare un oggetto simile al seguente nel campo autenticazione:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Autenticazione con certificato client
Per l'autenticazione con certificato client è necessario l'oggetto di autenticazione seguente. Un asterisco (*) indica che è un campo obbligatorio.

| Nome proprietà | Tipo di dati | Descrizione |
| --- | --- | --- |
| Type* |type |Tipo di autenticazione (deve essere `ClientCertificate` per i certificati client SSL) |
| PFX* |pfx |Contenuto con codifica Base&64; del file di scambio di informazioni personali (PFX, Personal Information Exchange) |
| Password* |password |La password per accedere al file PFX |

> [!TIP]
> È possibile usare un parametro `securestring` e la [funzione di definizione del flusso di lavoro](http://aka.ms/logicappdocs) `@parameters()` per usare un parametro che non sarà leggibile nella definizione dopo il salvataggio dell'app per la logica.
> 
> 

Ad esempio:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Autenticazione OAuth di Azure AD
Per l'autenticazione OAuth di Azure Ad è necessario l'oggetto di autenticazione seguente. Un asterisco (*) indica che è un campo obbligatorio.

| Nome proprietà | Tipo di dati | Descrizione |
| --- | --- | --- |
| Type* |type |Tipo di autenticazione (deve essere `ActiveDirectoryOAuth` per OAuth di Azure AD) |
| Tenant* |tenant |L'identificatore del tenant di Azure AD |
| Pubblico* |audience |Impostare su `https://management.core.windows.net/` |
| ID cliente* |clientId |Identificatore client per l'applicazione Azure AD |
| Segreto* |secret |Segreto del client che richiede il token |

> [!TIP]
> È possibile usare un parametro `securestring` e la [funzione di definizione del flusso di lavoro](http://aka.ms/logicappdocs) `@parameters()` per usare un parametro che non sarà leggibile nella definizione dopo il salvataggio.
> 
> 

Ad esempio:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Passaggi successivi
Provare ora a usare la piattaforma e [creare un'app per la logica](../logic-apps/logic-apps-create-a-logic-app.md). È possibile esplorare gli altri connettori disponibili nelle app per la logica esaminando l' [elenco di API](apis-list.md).




<!--HONumber=Jan17_HO3-->


