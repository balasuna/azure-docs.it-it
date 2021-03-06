---
title: Panoramica del servizio DNS di Azure | Documenti Microsoft
description: Panoramica del servizio di hosting DNS in Microsoft Azure. Ospitare il dominio in Microsoft Azure.
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 87122cb432f90573fdb0a35943ce14c8d62e84a5
ms.openlocfilehash: f8ccf5c0fab1e4aca85b22b99a1a5b48f35dfcbc

---

# <a name="azure-dns-overview"></a>Panoramica di DNS di Azure

Il nome DNS (Domain Name System) è responsabile della conversione (o risoluzione) del nome di un sito Web o del servizio nel relativo indirizzo IP. DNS di Azure è un servizio di hosting per i domini DNS, che fornisce la risoluzione dei nomi usando l'infrastruttura di Microsoft Azure. Ospitando i domini in Azure, è possibile gestire i record DNS usando le stesse credenziali, API, strumenti e fatturazione come per gli altri servizi Azure.

I domini DNS nel servizio DNS di Azure sono ospitati nella rete globale di Azure dei server dei nomi DNS. Usiamo le reti Anycast, in modo che ogni query DNS riceva una risposta dal server DNS disponibile più vicino. Ciò consente di accelerare le prestazioni e ottenere la disponibilità elevata per il dominio.

Il servizio DNS di Azure è basato su Azure Resource Manager. Usufruisce quindi delle funzionalità di Resource Manager, come il controllo degli accessi in base al ruolo, i log di controllo e il blocco delle risorse. I domini e i record possono essere gestiti tramite il portale di Azure, i cmdlet di Azure PowerShell e l'interfaccia della riga di comando di Azure. Le applicazioni che richiedono la gestione automatica di DNS possono essere integrate con il servizio tramite l'API REST e gli SDK.

DNS di Azure attualmente non supporta l'acquisto dei nomi di dominio. Se si vogliono acquistare domini, è necessario ricorrere a un registrar di terze parti, che addebita in genere una tariffa annuale contenuta. I domini possono quindi essere ospitati in DNS di Azure per la gestione dei record DNS. Per informazioni dettagliate, vedere [Delegare un dominio a DNS di Azure](dns-domain-delegation.md) .

### <a name="next-steps"></a>Passaggi successivi

[Creare una zona DNS](./dns-getstarted-create-dnszone-portal.md)




<!--HONumber=Dec16_HO3-->


