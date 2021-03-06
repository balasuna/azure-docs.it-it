---
title: Soluzione predittiva per il rischio di credito con Machine Learning | Documentazione Microsoft
description: Una procedura dettagliata che mostra come creare una soluzione di analisi predittiva per la valutazione del rischio di credito in Azure Machine Learning Studio.
keywords: rischio di credito, soluzione di analisi predittiva, valutazione del rischio
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 07cb3fe0d5688d5b63fe3312cad14c2274a58a09
ms.openlocfilehash: e98a64910f28da0a8a9b4a58c717c40d791ccf00


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Procedura dettagliata: Sviluppare una soluzione di analisi predittiva per la valutazione del rischio di credito in Azure Machine Learning

In questa procedura dettagliata verrà esaminato il processo di sviluppo di una soluzione in Machine Learning Studio. Verrà sviluppato un modello di analisi predittiva in Machine Learning Studio, che verrà quindi distribuito come servizio Web di Azure Machine Learning, in cui il modello può creare previsioni usando nuovi dati. 

> [!TIP]
> In questa procedura dettagliata si presuppone che Machine Learning Studio sia già stato usato almeno una volta e che i concetti di Machine Learning siano noti, anche se non è necessario essere esperti in questo campo.
> 
>Se non si ha alcuna esperienza di **Azure Machine Learning Studio**, è consigliabile iniziare con l'esercitazione [Creare il primo esperimento di analisi scientifica dei dati in Azure Machine Learning Studio](machine-learning-create-experiment.md), che fornisce informazioni preliminari su Machine Learning Studio, illustrando i concetti di base quali il trascinamento di moduli nell'esperimento, la connessione dei moduli, l'esecuzione dell'esperimento e l'analisi dei risultati.
>
>Se non si ha familiarità con Machine Learning, è consigliabile vedere la serie di video [Analisi scientifica dei dati per principianti](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Questa serie di video rappresenta un'introduzione completa a Machine Learning e usa linguaggio e concetti semplici.
> 

## <a name="the-problem"></a>Problema

Si supponga di dover prevedere il rischio di credito di un soggetto in base alle informazioni fornite in una richiesta di credito.  

La valutazione del rischio di credito è naturalmente un problema complesso, ma semplificando i parametri della questione è possibile usarlo come esempio per illustrare come si può usare Microsoft Azure Machine Learning con Machine Learning Studio e il servizio Web di Machine Learning per creare questo tipo di soluzione di analisi predittiva.  

## <a name="the-solution"></a>Soluzione

In questa procedura dettagliata verranno usati dati sul rischio di credito disponibili pubblicamente, quindi si passerà allo sviluppo e all'esecuzione del training di un modello predittivo in base a tali dati. Infine si distribuirà il modello come un servizio Web che può essere usato da altri utenti per la valutazione del rischio di credito.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Per creare una soluzione di valutazione del rischio di credito, seguire la procedura seguente:  

1. [Creare un'area di lavoro di Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Caricare i dati esistenti](machine-learning-walkthrough-2-upload-data.md)
3. [Creare un nuovo esperimento](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Eseguire il training e valutare i modelli](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Distribuire il servizio web](machine-learning-walkthrough-5-publish-web-service.md)
6. [Accedere al servizio Web](machine-learning-walkthrough-6-access-web-service.md)

Questa procedura dettagliata è basata su una versione semplificata dell'esperimento di esempio [Binary Classfication: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270) (Classificazione binaria: previsione del rischio di credito) in [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).


> [!TIP]
> Per scaricare e stampate un diagramma che offra una panoramica delle funzionalità di Machine Learning Studio, vedere [Diagramma della panoramica delle funzionalità di Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).
> 
> 



<!--HONumber=Dec16_HO3-->


