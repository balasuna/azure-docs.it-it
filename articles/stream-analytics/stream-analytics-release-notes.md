---
title: Note sulla versione di analisi di flusso | Microsoft Docs
description: Note sulla versione di analisi di flusso
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5e59f893-cd2c-43fb-9eca-c146ce637203
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/01/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 2fe28e7b52af579038ec145c028a6d2796d6cc6e
ms.openlocfilehash: f47b719f7b1a78264e054ee15edacc32ca35aaf7


---
# <a name="stream-analytics-release-notes"></a>Note sulla versione di analisi di flusso
## <a name="notes-for-02012017-release-of-stream-analytics"></a>Note per la versione 01/02/2017 di Analisi di flusso
Questa versione contiene l'aggiornamento seguente.

| Titolo | Descrizione |
| --- | --- |
| Introduzione alle funzioni JavaScript definite dall'utente (UDF) |Le [funzioni Java definite dall'utente](stream-analytics-javascript-user-defined-functions.md) sono ora disponibili per una maggiore flessibilità nella creazione di query. |
| Introduzione agli strumenti per Visual Studio e analisi di flusso |Gli [strumenti per Visual Studio](stream-analytics-tools-for-visual-studio.md) sono ora disponibili per il debug e per le utility. |
| Abilitare la registrazione diagnostica |La [registrazione diagnostica](stream-analytics-job-diagnostic-logs.md) è ora disponibile per altre opzioni di risoluzione dei problemi. |
| Introduzione alle funzioni geospaziali |Le [funzioni geospaziali](http://msdn.microsoft.com/library/mt778980(Azure.100).aspx) sono ora disponibili a livello generale. |

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Note per la versione 15/04/2016 di Analisi di flusso
Questa versione contiene l'aggiornamento seguente.

| Titolo | Descrizione |
| --- | --- |
| Disponibilità generale per gli output di Power BI |[Gli output di Power BI](stream-analytics-power-bi-dashboard.md) sono ora disponibili a livello generale. La scadenza dell'autorizzazione per Power BI dopo 90 giorni è stata rimossa. Per altre informazioni sugli scenari in cui è necessario rinnovare l'autorizzazione, vedere la sezione relativa al [rinnovo dell'autorizzazione](stream-analytics-power-bi-dashboard.md#renew-authorization) di Creare un dashboard di Power BI. |

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Note per la versione 03/03/2016 di Analisi di flusso
Questa versione contiene l'aggiornamento seguente.

| Titolo | Descrizione |
| --- | --- |
| Nuovi elementi del linguaggio di query di Analisi di flusso |Ora SAQL ha [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "Pagina MSDN su GetType"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "Pagina MSDN su TRY_CAST") e [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "Pagina MSDN su REGEXMATCH"). |

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Note per la versione 10/12/2015 di Analisi di flusso
Questa versione contiene l'aggiornamento seguente.

| Titolo | Descrizione |
| --- | --- |
| Aggiornamento della versione dell'API REST |La versione dell'API REST è stata aggiornata a 2015-10-01. I dettagli sono disponibili su MSDN nella sezione di [riferimento dell'API REST di gestione di Analisi di flusso](https://msdn.microsoft.com/library/azure/dn835031.aspx) e [Integrazione di Machine Learning in Analisi di flusso](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md). |
| Integrazione con Azure Machine Learning |Questa versione offre il supporto per le funzioni definite dall'utente di Azure Machine Learning. Per altre informazioni, vedere l'[esercitazione](stream-analytics-machine-learning-integration-tutorial.md) e l'[annuncio generale sul blog](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx). |

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Note per la versione 12/11/2015 di Analisi di flusso
Questa versione contiene l'aggiornamento seguente.

| Titolo | Descrizione |
| --- | --- |
| Nuovo comportamento di SELECT |SELECT in Analisi di flusso è stato esteso per consentire * come funzione di accesso alla proprietà di un record annidato. Per altre informazioni, vedere [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Tipi di dati complessi"). |

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Note per la versione 22/10/2015 di Analisi di flusso
Questa versione contiene gli aggiornamenti seguenti.

| Titolo | Descrizione |
| --- | --- |
| Funzionalità del linguaggio di query aggiuntive |Analisi di flusso ha espanso il linguaggio delle query includendo le funzionalità seguenti: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [CEILING](https://msdn.microsoft.com/library/azure/mt605286.aspx), [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [FLOOR](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [SIGN](https://msdn.microsoft.com/library/azure/mt605290.aspx), [SQUARE](https://msdn.microsoft.com/library/azure/mt605288.aspx)e [SQRT](https://msdn.microsoft.com/library/azure/mt605238.aspx). |
| Rimuovere le limitazioni di aggregazione |Questa versione consente di rimuovere il limite di 15 aggregazioni in una query. A questo punto non vi sono limiti al numero di aggregazioni per ogni query. |
| Nuova funzionalità GROUP BY di System.Timestamp |La funzione [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) consente ora sia window_type sia [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx). |
| OFFSET aggiunto per le finestre a cascata e di salto. |Per impostazione predefinita, le finestre [a cascata](https://msdn.microsoft.com/library/azure/dn835055.aspx) e [di salto](https://msdn.microsoft.com/library/azure/dn835041.aspx) sono allineate rispetto al tempo zero (1/1/0001 12:00:00 AM UTC). Il nuovo parametro (facoltativo) 'offsetsize' consente di specificare un offset personalizzato (o allineamento). |

## <a name="notes-for-09292015-release-of-stream-analytics"></a>Note per la versione 29/09/2015 di Analisi di flusso
Questa versione contiene gli aggiornamenti seguenti.

| Titolo | Descrizione |
| --- | --- |
| Anteprima pubblica di Azure IoT Suite |L’analisi di flusso è inclusa nell’anteprima pubblica di Azure IoT Suite. |
| Integrazione del portale di Azure |Oltre a essere sempre presente nel portale di gestione di Azure, Analisi di flusso è ora integrata nel [portale di Azure](https://azure.microsoft.com/overview/preview-portal/). Si noti che le funzionalità di analisi di flusso nel portale di anteprima sono attualmente un sottoinsieme delle funzionalità offerte nel portale di gestione di Azure, senza il supporto per il test di query nel browser, la configurazione di output Power BI e l'esplorazione o la creazione di nuove risorse di input e output nelle sottoscrizioni a cui si ha accesso. |
| Supporto per l'output di DocumentDB |I processi di Analisi di flusso ora possono inviare output a [DocumentDB](https://azure.microsoft.com/services/documentdb/). |
| Supporto per l'input dell’hub IoT |I processi di analisi di flusso sono ora in grado di acquisire dati dagli hub IoT. |
| TIMESTAMP BY per eventi eterogenei |Quando un unico flusso di dati contiene più tipi di evento con timestamp in campi diversi, è ora possibile usare [TIMESTAMP BY](http://msdn.microsoft.com/library/mt573293.aspx) con espressioni per specificare i campi di timestamp diversi per ogni caso. |

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Note per la versione 10/09/2015 di Analisi di flusso
Questa versione contiene gli aggiornamenti seguenti.

| Titolo | Descrizione |
| --- | --- |
| Supporto per i gruppi PowerBI |Per abilitare la condivisione dei dati con altri utenti di Power BI, i processi di Analisi di flusso ora possono eseguire la scrittura in [gruppi PowerBI](stream-analytics-define-outputs.md#power-bi) all'interno dell'account Power BI. |

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Note per la versione 20/08/2015 di Analisi di flusso
Questa versione contiene gli aggiornamenti seguenti.

| Titolo | Descrizione |
| --- | --- |
| Aggiunta funzione LAST |La funzione [LAST](http://msdn.microsoft.com/library/mt421186.aspx) è ora disponibile nei processi di Analisi di flusso. Consente di recuperare l'evento più recente in un flusso di eventi all'interno di un intervallo di tempo specificato. |
| Nuove funzioni di matrice |Ora sono disponibili le funzioni di matrice [GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) e [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx). |
| Nuove funzioni di record |Le funzioni di record [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) e [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) sono ora disponibili. |

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Note per la versione del 30/07/2015 di Analisi di flusso
Questa versione contiene gli aggiornamenti seguenti.

| Titolo | Descrizione |
| --- | --- |
| ID organizzazione di Power BI separato dall'ID di Azure |Questa funzionalità abilita l' [output di Power BI](stream-analytics-power-bi-dashboard.md) per i processi ASA in qualsiasi tipo di account di Azure (Live ID o ID organizzazione). Inoltre, è possibile disporre di un ID organizzazione per l'account Azure e utilizzarne uno diverso per l'autorizzazione dell'output di Power BI. |
| Supporto per l'output di code del bus di servizio |Gli output di [code del bus di servizio](stream-analytics-define-outputs.md#service-bus-queues) sono ora disponibili nei processi di Analisi di flusso. |
| Supporto per l'output di argomenti del bus di servizio |Gli output di [argomenti del bus di servizio](stream-analytics-define-outputs.md#service-bus-topics) sono ora disponibili nei processi di Analisi di flusso. |

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Note per la versione 09/07/2015 di Analisi di flusso
Questa versione contiene gli aggiornamenti seguenti.

| Titolo | Descrizione |
| --- | --- |
| Partizionamento output BLOB personalizzato |Gli output di archiviazione BLOB includono ora un'opzione per specificare la frequenza con cui i BLOB di output vengono scritti e la struttura e il formato della struttura di cartelle del percorso dati di output. |

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Note per la versione 03/05/2015 di Analisi di flusso
Questa versione contiene gli aggiornamenti seguenti.

| Titolo | Descrizione |
| --- | --- |
| Aumento del valore massimo per Finestra di tolleranza elementi non in ordine |La dimensione massima per Finestra di tolleranza elementi non in ordine è ora 59:59 (mm:ss) |
| Formato di output JSON: Separato da righe o matrice |Ora è disponibile un'opzione per l'output in Archiviazione Blob o in Hub di eventi come matrice di oggetti JSON o con gli oggetti JSON separati da una nuova riga. |

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Note per la versione 16/04/2015 di Analisi di flusso
| Titolo | Descrizione |
| --- | --- |
| Ritardo nella configurazione dell'account di archiviazione di Azure |Quando si crea per la prima volta un processo di Analisi dei flussi in un’area, verrà richiesto di creare un nuovo account di archiviazione, o di specificarne uno esistente, per il monitoraggio dei processi di Analisi dei flussi in tale area. A causa della latenza nella configurazione del monitoraggio, la creazione di un altro processo di Analisi dei flussi nella stessa area entro 30 minuti determinerà la visualizzazione della richiesta di specificare un secondo account di archiviazione anziché la visualizzazione di quello appena configurato nel menu a discesa relativo all'account di archiviazione per il monitoraggio. Per evitare la creazione di account di archiviazione non necessari, dopo aver creato per la prima volta un processo in una determinata area, attendere 30 minuti prima di eseguire il provisioning di altri processi nella stessa area. |
| Aggiornamento dei processi |Analisi dei flussi non supporta al momento modifiche di definizione o configurazione per processi in corso di esecuzione. Per modificare input, output, query, dimensioni o configurazione di un processo in esecuzione, è necessario innanzitutto arrestare il processo stesso. |
| Tipi di dati dedotti da origine di input |Se non viene usata un'istruzione CREATE TABLE, il tipo di input è derivato dal formato di input, ad esempio tutti i campi da CSV sono stringhe. I campi devono essere convertiti esplicitamente nel tipo corretto tramite la funzione CAST per evitare errori di tipo non corrispondente. |
| I campi mancanti sono generati come valori null |Il riferimento a un campo non presente nell’origine di input causerà valori null nell’evento di output. |
| Le istruzioni WITH devono precedere le istruzioni SELECT |Nella query, le istruzioni SELECT devono seguire le sottoquery definite nelle istruzioni WITH. |
| Problema relativo a memoria insufficiente |I processi di Analisi dei flussi con un'elevata tolleranza per eventi non ordinati e/o query complesse, con relativa conservazione di grandi quantità di stati, possono causare l'esaurimento della memoria, il che a sua volta causerebbe il riavvio del processo. Le operazioni di avvio e di arresto saranno visibili nei log delle operazioni del processo. Per evitare questo comportamento, ridimensionare la query orizzontalmente occupando più partizioni. In una versione futura questa limitazione verrà risolta riducendo le prestazioni dei processi interessati anziché causandone il riavvio. |
| Input blob di grandi dimensioni senza timestamp payload possono causare problemi relativi a memoria insufficiente |L’utilizzo di file di grandi dimensioni dall’Archiviazione BLOB può causare l’arresto anomalo dei processi di Analisi dei flussi se un campo timestamp non è specificato con TIMESTAMP BY. Per evitare questo problema, mantenere ogni BLOB entro i 10 MB di dimensione. |
| Limitazione del volume di eventi del database SQL |Quando si usa un database SQL come destinazione di output, un volume troppo elevato di dati di output può causare il timeout del processo di Analisi dei flussi. Per risolvere il problema, ridurre il volume di output tramite operatori di filtro o aggregati oppure scegliere Archiviazione BLOB di Azure o Hub eventi come destinazione di output. |
| I dataset PowerBI possono contenere solo una tabella |PowerBI non supporta più di una tabella in un dato dataset. |

## <a name="get-help"></a>Ottenere aiuto
Per assistenza, provare il [Forum di Analisi di flusso di Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Passaggi successivi
* [Introduzione ad Analisi dei flussi di Azure](stream-analytics-introduction.md)
* [Introduzione all'uso di Analisi dei flussi di Azure](stream-analytics-get-started.md)
* [Ridimensionare i processi di Analisi dei flussi di Azure](stream-analytics-scale-jobs.md)
* [Informazioni di riferimento sul linguaggio di query di Analisi dei flussi di Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Informazioni di riferimento sulle API REST di gestione di Analisi di flusso di Azure](https://msdn.microsoft.com/library/azure/dn835031.aspx)



<!--HONumber=Feb17_HO1-->


