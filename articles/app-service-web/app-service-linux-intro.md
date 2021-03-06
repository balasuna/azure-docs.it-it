---
title: Introduzione al servizio app in Linux | Documentazione Microsoft
description: Informazioni sul servizio app in Linux.
keywords: Servizio app di Azure, Linux, OSS
services: app-service
documentationcenter: 
author: naziml
manager: wpickett
editor: 
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: naziml
translationtype: Human Translation
ms.sourcegitcommit: a3df293a056f4b06281bf8b80717529286d528b8
ms.openlocfilehash: 48f26113d129ebfa613f1b8f2a372d0e7286aea1


---
# <a name="introduction-to-app-service-on-linux"></a>Introduzione al servizio app in Linux
Il servizio app di Azure in Linux è attualmente in anteprima pubblica e supporta l'esecuzione di app Web in modo nativo in Linux.

## <a name="overview"></a>Panoramica
I clienti possono usare il servizio app in Linux per ospitare app Web in modo nativo in Linux per stack di applicazioni supportate. La sezione seguente elenca gli stack di applicazioni attualmente supportati. 

## <a name="features"></a>Funzionalità
Il servizio app in Linux supporta attualmente gli stack di applicazioni seguenti:

* Node.js
* PHP
* .Net Core

I clienti possono distribuire le applicazioni tramite:

* FTP
* Repository Git locale
* GitHub o Bitbucket

Per il ridimensionamento delle applicazioni:

* I clienti possono aumentare e ridurre le prestazioni delle app Web modificando il livello nel piano di servizio app.
* I clienti possono aumentare il numero di istanze delle applicazioni ed eseguire l'app in più istanze entro i confini dello SKU.

Per Kudu, alcune delle funzionalità di base vengono usate con gli elementi seguenti:

* Ambienti
* Distribuzioni
* Console di base

## <a name="limitations"></a>Limitazioni
Il portale di Azure visualizza solo le funzionalità che possono essere usate attualmente per il servizio app in Linux e nasconde le altre. Altre funzionalità saranno visibili nel portale man mano che verranno abilitate.

Alcune funzionalità, ad esempio l'integrazione delle reti virtuali, l'autenticazione di Azure Active Directory o di terze parti o le estensioni del sito Kudu, attualmente non funzionano. Man mano che queste funzionalità verranno abilitate, la documentazione verrà aggiornata e le modifiche pubblicate nei blog.

Questa anteprima pubblica è attualmente disponibile solo nelle aree seguenti:

* Stati Uniti occidentali
* Europa occidentale 
* Asia sudorientale

App Web in Linux è supportato solo nei piani di servizio app dedicati e non ha un livello Gratuito o Condiviso. I piani di servizio app per le app Web normali e di Linux si escludono a vicenda, quindi non è possibile creare un'app Web di Linux in un piano di servizio app non Linux.

Le app Web in Linux devono essere create in un gruppo di risorse che non contiene app Web non Linux nella stessa area.

## <a name="next-steps"></a>Passaggi successivi
Vedere i collegamenti seguenti per iniziare a usare il servizio app in Linux. È possibile pubblicare domande e dubbi nel [forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Creating Apps in an App Service on Linux](app-service-linux-how-to-create-a-web-app.md) (Creazione di app Web nel servizio app in Linux)
* [Come usare un'immagine Docker personalizzata per il servizio app in Linux](app-service-linux-using-custom-docker-image.md)
* [Uso della configurazione PM2 per Node.js nelle app Web in Linux](app-service-linux-using-nodejs-pm2.md)
* [Uso di .NET Core nelle app Web del servizio App di Azure in Linux](app-service-linux-using-dotnetcore.md)



<!--HONumber=Jan17_HO1-->


