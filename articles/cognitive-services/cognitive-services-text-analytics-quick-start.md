---
title: 'Guida introduttiva: API di analisi del testo di Machine Learning | Microsoft Docs'
description: Text Analytics di Azure Machine Learning - Guida introduttiva
services: cognitive-services
documentationcenter: 
author: onewth
manager: jhubbard
editor: cgronlun
ms.assetid: e8b9e98c-40e7-4425-ae16-d1eaa7d2f837
ms.service: cognitive-services
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: onewth
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 663b99c2491bebf49f950605152604b040ade070


---
# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>Introduzione alle API Text Analytics per rilevare sentimenti, frasi chiave, argomenti e lingua
<a name="HOLTop"></a>

Questo documento illustra come configurare il servizio o l'applicazione per l'uso di [API Text Analytics](//go.microsoft.com/fwlink/?LinkID=759711).
È possibile usare queste API per rilevare sentimenti, frasi chiave, argomenti e lingua dal testo. [Fare clic qui per visualizzare una demo interattiva dell'esperienza.](//go.microsoft.com/fwlink/?LinkID=759712)

Per la documentazione tecnica delle API, vedere le [definizioni delle API](//go.microsoft.com/fwlink/?LinkID=759346) .

Questa guida è destinata alla versione 2 delle API. Per altre informazioni sulla versione 1 delle API, [fare riferimento a questo documento](../machine-learning/machine-learning-apps-text-analytics.md).

Al termine di questa esercitazione, si sarà in grado di rilevare a livello di codice:

* **Sentimento** : il testo positivo o negativo?
* **Frasi chiave** : di cosa stanno discutendo le persone in singolo articolo?
* **Argomenti** : di cosa stanno discutendo le persone in molti articoli?
* **Lingue** : in quale lingua è scritto il testo?

Tenere presente che con questa API viene addebitata 1 transazione per ciascun documento inviato. Quindi, ad esempio, se si richiede una valutazione per 1.000 documenti in una singola chiamata, verranno dedotte 1.000 transazioni.

<a name="Overview"></a>

## <a name="general-overview"></a>Panoramica generale
Questo argomento costituisce una guida dettagliata. L'obiettivo è illustrare in dettaglio i passaggi necessari per eseguire il training di un modello e indicare le risorse che consentiranno di inserirlo nell'ambiente di produzione. Questo esercizio richiederà circa 30 minuti.

Per eseguire queste attività, sarà necessario un editor e chiamare gli endpoint RESTful nel linguaggio preferito.

Ecco come procedere.

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>Attività 1: iscrizione alle API Text Analytics
In questa attività si effettuerà l'iscrizione al servizio di analisi di testo.

1. Passare a** Servizi cognitivi** nel [portale di Azure](//go.microsoft.com/fwlink/?LinkId=761108) e assicurarsi che come "tipo di API" sia selezionato **analisi del testo**.
2. Selezionare un piano. È possibile selezionare il **livello gratuito per 5.000 transazioni al mese**. Essendo un piano gratuito, non verranno addebitati costi per l'uso del servizio. Sarà necessario accedere alla sottoscrizione di Azure. 
3. Completare gli altri campi e creare l'account.
4. Dopo l'iscrizione per Text Analytics, trovare la **Chiave API**. Copiare la chiave primaria, perché sarà necessaria quando si usano i servizi API.

## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Attività 2: rilevare sentimenti, frasi chiave e lingue
È facile rilevare sentimenti, frasi chiave e lingue presenti nel testo. A livello di codice si otterranno gli stessi risultati restituiti dall' [esperienza della demo](//go.microsoft.com/fwlink/?LinkID=759712) .

> [!TIP]
> Per l'analisi dei sentimenti, è consigliabile dividere il testo in frasi. In questo modo è possibile ottenere in genere una precisione superiore nelle stime dei sentimenti.
> 
> 

Si noti che i linguaggi supportati sono i seguenti:

| Funzionalità | Codici lingua supportati |
|:--- |:--- |
| Valutazione |`en` (inglese), `es` (spagnolo) `fr` (francese), `pt` (portoghese) |
| Frasi chiave |`en` (inglese), `es` (spagnolo) `de` (tedesco), `ja` (giapponese) |

1. È necessario impostare le intestazioni come indicato di seguito. Si noti che JSON è attualmente l'unico formato di input accettato per le API. XML non è supportato.
   
        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json
2. Formattare quindi le righe di input in JSON. Il formato è lo stesso per sentimenti, frasi chiave e lingua. Si noti che ogni ID deve essere univoco e sarà l'ID restituito dal sistema. Le dimensioni massime di un singolo documento che è possibile inviare sono pari a 10 KB, mentre le dimensioni massime complessive dell'input inviato sono pari a 1 MB. In una sola chiamata non è possibile inviare più di 1.000 documenti. Esistono limitazioni di frequenza che non consentono più di 100 chiamate al minuto: è quindi consigliabile inviare grandi quantità di documenti in un'unica chiamata. Language è un parametro facoltativo che deve essere specificato se si analizzano testi in una lingua diversa dall'inglese. Di seguito è riportato un esempio di input in cui è incluso il parametro facoltativo `language` per l'analisi di sentimenti o l'estrazione di frasi chiave:
   
        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }
3. Eseguire una chiamata **POST** al sistema con l'input per sentimenti, frasi chiave e lingua. Gli URL saranno simili ai seguenti:
   
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages
4. Questa chiamata restituirà una risposta formattata in JSON con gli ID e le proprietà rilevate. Di seguito è riportato un esempio dell'output per i sentimenti, esclusi i dettagli dell'errore. Nel caso dei sentimenti verrà restituito un punteggio compreso tra 0 e 1 per ogni documento:
   
        // Sentiment response
        {
              "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }
   
        // Key phrases response
        {
              "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }
   
        // Languages response
        {
              "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }

## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Attività 3: rilevare gli argomenti in un corpo di testo
Si tratta di un'API rilasciata di recente che restituisce i primi argomenti rilevati a fronte di un elenco di record di testo inviati. Un argomento viene identificato da una frase chiave, che può essere costituita da una o più parole correlate. L'API è progettata per funzionare al meglio con testi brevi in linguaggio naturale, ad esempio recensioni e commenti degli utenti.

Questa API richiede un **minimo di 100 record di testo** da inviare, ma è progettata per rilevare gli argomenti in centinaia o addirittura migliaia di record. Qualsiasi record non in lingua inglese o con meno di 3 parole sarà rimosso e quindi non verranno assegnati agli argomenti. Per il rilevamento di argomenti, le dimensioni massime di un singolo documento che è possibile inviare sono pari a 30 KB, mentre le dimensioni massime complessive dell'input inviato sono pari a 30 MB. Il limite di frequenza per il rilevamento degli argomenti è pari a 5 invii ogni 5 minuti.

Sono disponibili altri due parametri di input **facoltativi** che possono contribuire a migliorare la qualità dei risultati:

* **Parole non significative.**  Queste parole e le relative forme chiuse, ad esempio i plurali, verranno escluse dall'intera pipeline di rilevamento degli argomenti. Usare questa opzione per le parole comuni. Ad esempio, "issue", "error" e "user" possono essere scelte appropriate per reclami relativi al software da parte dei clienti. Ogni stringa deve contenere una singola parola.
* **Frasi non significative** : le frasi verranno escluse dall'elenco di argomenti restituiti. Usare questa opzione per escludere gli argomenti generici da non visualizzare nei risultati. Ad esempio, "Microsoft" e "Azure" sono scelte appropriate per gli argomenti da escludere. Le stringhe possono contenere più parole.

Seguire questi passaggi per rilevare gli argomenti nel testo.

1. Formattare l'input in JSON. Questa volta, è possibile definire le parole e le frasi non significative.
   
        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }
2. Usando le stesse intestazioni definite nel passaggio 2, eseguire una chiamata **POST** all'endpoint di argomenti:
   
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics
3. Verrà restituita `operation-location` come intestazione nella risposta, dove il valore è l'URL per effettuare una query degli argomenti risultanti:
   
        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'
4. Eseguire periodicamente una query sull'intestazione `operation-location` restituita con una richiesta **GET** . È consigliabile eseguire questa operazione una volta al minuto.
   
        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>
5. L'endpoint restituirà una risposta che include `{"status": "notstarted"}` prima dell'elaborazione, `{"status": "running"}` durante l'elaborazione e `{"status": "succeeded"}` con l'output al termine. È quindi possibile utilizzare l'output che sarà nel formato seguente. Notare che i dettagli, come il formato dell'errore e le date, sono stati esclusi da questo esempio:
   
        {
            "status": "succeeded",
            "operationProcessingResult": {
                  "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                  "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Si noti che la risposta con esito positivo per gli argomenti dall'endpoint `operations` avrà il seguente schema:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

Di seguito viene spiegata ogni parte della risposta:

**topics**

| Chiave | Description |
|:--- |:--- |
| id |Identificatore univoco di ogni argomento. |
| score |Numero di documenti assegnati all'argomento. |
| keyPhrase |Parola o frase di riepilogo dell'argomento. |

**topicAssignments**

| Chiave | Description |
|:--- |:--- |
| documentId |Identificatore del documento. Equivale all'ID incluso nell'input. |
| topicId |ID argomento a cui è stato assegnato il documento. |
| distance |Punteggio di un'associazione documento-argomento compreso tra 0 e 1. Minore è il valore della distanza, più forte è l'associazione all'argomento. |

**errors**

| Chiave | Description |
|:--- |:--- |
| id |Identificatore univoco del documento di input cui fa riferimento l'errore. |
| message |Messaggio di errore. |

## <a name="next-steps"></a>Passaggi successivi
Congratulazioni. L'analisi di testo sui dati è stata completata. Ora è possibile imparare a usare uno strumento come [Power BI](//powerbi.microsoft.com) per visualizzare i dati e per automatizzare l'analisi al fine di ottenere una visualizzazione in tempo reale dei dati di testo.

Per scoprire come le funzionalità di analisi del testo, ad esempio una valutazione, possano essere usate come parte di un bot, vedere l'esempio [Emotional Bot](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) (Bot emotivo) sul sito Bot Framework.




<!--HONumber=Nov16_HO3-->


