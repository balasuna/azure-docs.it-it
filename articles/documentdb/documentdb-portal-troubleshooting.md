---
title: Risoluzione dei problemi relativi al portale di DocumentDB | Documentazione Microsoft
description: Scopri come risolvere i problemi nel portale di Azure DocumentDB.
services: documentdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: monicar
ms.assetid: 36ad9bf4-2617-4347-ba29-edebf62fc3ec
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2016
ms.author: mimig
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d41cbded31d64525a3e04ef742091c8d87760416


---
# <a name="azure-documentdb-portal-troubleshooting-tips"></a>Suggerimenti per la risoluzione dei problemi relativi al portale di Azure DocumentDB
Questo articolo descrive come risolvere i problemi relativi a DocumentDB nel portale di Azure. 

## <a name="resources-are-missing"></a>Le risorse risultano mancanti
**Sintomo**: i pannelli del portale non includono i database o le raccolte.

**Soluzione**: ridurre l'uso delle applicazioni a funzionare con la quota di velocità effettiva massima per la raccolta. 

**Spiegazione**: il portale è un'applicazione come qualsiasi altra, che esegue chiamate alla raccolta e al database di DocumentDB. Se le richieste sono attualmente limitate a causa di chiamate effettuate da un'applicazione separata, anche il portale può essere limitato, causando la mancata visualizzazione delle risorse nel portale. Per risolvere il problema, correggere la causa dell'uso di velocità effettiva elevata e quindi aggiornare il pannello del portale. Informazioni su come misurare e ridurre l'uso della velocità effettiva sono reperibili nella sezione [Velocità effettiva](documentdb-performance-tips.md#throughput) dell'articolo [Suggerimenti per incrementare le prestazioni](documentdb-performance-tips.md).

## <a name="pages-or-blades-wont-load"></a>Le pagine o i pannelli non vengono caricati
**Sintomo**: le pagine e i pannelli del portale non vengono visualizzati.

**Soluzione**: ridurre l'uso delle applicazioni a funzionare con la quota di velocità effettiva massima per la raccolta. 

**Spiegazione**: il portale è un'applicazione come qualsiasi altra, che esegue chiamate alla raccolta e al database di DocumentDB. Se le richieste sono attualmente limitate a causa di chiamate effettuate da un'applicazione separata, anche il portale può essere limitato, causando la mancata visualizzazione delle risorse nel portale. Per risolvere il problema, correggere la causa dell'uso di velocità effettiva elevata e quindi aggiornare il pannello del portale. Informazioni su come misurare e ridurre l'uso della velocità effettiva sono reperibili nella sezione [Velocità effettiva](documentdb-performance-tips.md#throughput) dell'articolo [Suggerimenti per incrementare le prestazioni](documentdb-performance-tips.md).

## <a name="add-collection-button-is-disabled"></a>Il pulsante Aggiungi raccolta è disabilitato
**Sintomo**: il pulsante **Aggiungi raccolta** nel pannello Database è disabilitato.

**Spiegazione**: se la sottoscrizione di Azure è associata a crediti vantaggio, ad esempio crediti offerti da una sottoscrizione MSDN, e sono stati usati tutti i crediti per il mese,non è possibile creare le raccolte in DocumentDB.

**Soluzione**: rimuovere il limite di spesa dall'account.

1. Nell'indice del portale di Azure fare clic su **Sottoscrizioni**, scegliere la sottoscrizione associata al database DocumentDB e quindi nel pannello **Sottoscrizione** fare clic su **Gestisci**. 
    ![DocumentDB offre numerosi modelli di coerenza, ampi e ben definiti, tra cui scegliere](./media/documentdb-portal-troubleshooting/documentdb-change-billing.png)
2. Nella nuova finestra del browser si noterà che non esistono crediti residui. Fare clic sul pulsante **Rimuovere il limite di spesa** per rimuovere la spesa solo per il periodo di fatturazione corrente o per un tempo indefinito. Completare quindi la procedura guidata per aggiungere o confermare le informazioni sulla carta di credito. 
    ![DocumentDB offre numerosi modelli di coerenza, ampi e ben definiti, tra cui scegliere](./media/documentdb-portal-troubleshooting/documentdb-remove-spending-limit.png)

## <a name="query-explorer-completes-with-errors"></a>L'operazione di Esplora query è stata completata con errori
Vedere la sezione [Risoluzione dei problemi](documentdb-query-collections-query-explorer.md#troubleshoot)relativi ad Esplora query.

## <a name="no-data-available-in-monitoring-tiles"></a>Nessun dato disponibile nelle sezioni di monitoraggio
Vedere la sezione [Risoluzione dei problemi](documentdb-monitor-accounts.md#troubleshooting)relativi ai riquadri di monitoraggio.

## <a name="no-documents-returned-in-document-explorer"></a>Nessun documento restituito in Esplora documenti
Vedere la sezione [Risoluzione dei problemi](documentdb-view-json-document-explorer.md#troubleshoot)relativi a Esplora documenti.

## <a name="next-steps"></a>Passaggi successivi
Se si verificano ancora problemi nel portale, inviare un messaggio e-mail all'indirizzo [askdocdb@microsoft.com](mailto:askdocdb@microsoft.com) per ottenere assistenza o creare una richiesta di supporto nel portale facendo clic su **Sfoglia**, **Guida e supporto** e quindi su **Crea richiesta di supporto**.




<!--HONumber=Nov16_HO3-->


