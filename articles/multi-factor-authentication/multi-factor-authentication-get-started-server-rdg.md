---
title: Gateway Desktop remoto e server Azure MFA con RADIUS | Documentazione Microsoft
description: "Questa è la pagina di Azure Multi-factor authentication che sarà utile per la distribuzione di Gateway Desktop remoto (RD) e Server Azure Multi-Factor Authentication tramite RADIUS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: f2354ac4-a3a7-48e5-a86d-84a9e5682b42
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 4547a805c1827a703bf0ef118387882e45c3f241
ms.openlocfilehash: 4b117f03a8f769cbd2ecf1fca2653e8c343b6aa0


---
# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Gateway Desktop remoto e server Azure Multi-Factor Authentication utilizzando RADIUS
In molti casi, il Gateway Desktop remoto utilizza i criteri di rete locale per autenticare gli utenti. In questo documento viene descritto come inoltrare richieste RADIUS dal Gateway Desktop remoto (tramite criteri di rete locale) al Server Multi-Factor Authentication.

Il Server Multi-Factor Authentication deve essere installato su un server separato, che inoltrerà poi la richiesta RADIUS ai criteri di rete nel Server Gateway Desktop remoto. Dopo che i criteri di rete convalidano il nome utente e password, verrà restituita una risposta al Server Multi-Factor Authentication che eseguirà il secondo fattore di autenticazione prima di restituire un risultato al gateway.

## <a name="configure-the-rd-gateway"></a>Configurare il Gateway Desktop remoto
Il gateway Desktop remoto deve essere configurato per inviare l'autenticazione RADIUS a un Server Azure Multi-Factor Authentication. Una volta che il Gateway Desktop remoto è stato installato, configurato ed è funzionante, passare alle proprietà Gateway Desktop remoto. Passare alla scheda archivio connessioni desktop remoto e modificarla in modo da utilizzare un server centrale che esegue i criteri di rete, anziché il server locale che esegue i criteri di rete. Aggiungere uno o più server Azure Multi-Factor Authentication come server RADIUS e specificare un segreto condiviso per ogni server.

## <a name="configure-nps"></a>Configurazione dei criteri di rete
Il Gateway Desktop remoto utilizza criteri di rete per inviare richieste RADIUS a Azure Multi-Factor Authentication. Per impedire che il Gateway Desktop remoto scada prima che l’autenticazione a più fattori sia completata, è necessario modificare un timeout. Utilizzare la procedura seguente per configurare criteri di rete.

1. In Criteri di rete, espandere il menu client e Server RADIUS nella colonna a sinistra e fare clic su gruppi di Server RADIUS remoti. Accedere alle proprietà del gruppo di TS GATEWAY SERVER. Modificare i server RADIUS visualizzati e passare alla scheda bilanciamento del carico. Modificare il "Numero di secondi senza risposta prima che la richiesta venga considerata come eliminata" e "Numero di secondi tra le richieste quando il server viene identificato come non disponibile" a 30-60 secondi. Fare clic sulla scheda Autenticazione/Accounting e assicurarsi che le porte RADIUS specificate corrispondano alle porte di attesa del Server Multi-Factor Authentication.
2. I criteri di rete devono inoltre essere configurati per ricevere le autenticazioni RADIUS dal Server Azure Multi-Factor Authentication. Fare clic su client RADIUS nel menu a sinistra. Aggiungere il Server Azure Multi-Factor Authentication come client RADIUS. Scegliere un nome descrittivo e specificare un segreto condiviso.
3. Espandere la sezione criteri nel riquadro di navigazione sinistro e fare clic su criteri richiesta di connessione. Deve contenere un criterio di richiesta di connessione denominata TS GATEWAY AUTHORIZATION POLICY, che è stata creata durante la configurazione di Gateway Desktop remoto. Questo criterio inoltra le richieste RADIUS al Server Multi-Factor Authentication.
4. Copiare questo criterio per crearne uno nuovo. Nel nuovo criterio, aggiungere una condizione che abbini il nome descrittivo del Client con il nome descrittivo impostato nel precedente passaggio 2 per il client RADIUS del Server Azure Multi-Factor Authentication. Modificare il Provider di autenticazione nel computer locale. In questo modo quando viene ricevuta una richiesta RADIUS dal Server Azure Multi-Factor Authentication, l'autenticazione viene eseguita localmente anziché inviare una richiesta RADIUS al Server Azure Multi-Factor Authentication con la conseguenza di una condizione di ciclo. Per evitare la condizione di ciclo, il nuovo criterio deve essere inserito SOPRA il criterio originale che inoltra al Server Multi-Factor Authentication.

## <a name="configure-azure-multi-factor-authentication"></a>Configurazione di Azure Multi-Factor Authentication

Il Server Azure Multi-Factor Authentication è configurato come proxy RADIUS tra Gateway Desktop remoto e criteri di rete.  È necessario installarlo in un server di dominio che è separato dal server Gateway Desktop remoto. Utilizzare la procedura seguente per configurare il Server Azure Multi-Factor Authentication.

1. Aprire il Server Azure Multi-Factor Authentication e fare clic sull'icona autenticazione RADIUS. Selezionare la casella di controllo di autenticazione Abilitare RADIUS.
2. Nella scheda client assicurarsi che le porte corrispondano a quelle configurate in Criteri di rete e fare clic sul pulsante Aggiungi... . Aggiungere l'indirizzo IP del server Gateway Desktop remoto, il nome dell'applicazione (facoltativo) e un segreto condiviso. Il segreto condiviso dovrà essere lo stesso sul Server Azure Multi-Factor Authentication e sul Gateway Desktop remoto.
3. Fare clic sulla scheda destinazione e scegliere il pulsante di opzione server RADIUS.
4. Fare clic sul pulsante Aggiungi... . Immettere l'indirizzo IP, il segreto condiviso e le porte del server dei criteri di rete. Se non si utilizzano dei criteri di rete centrali, il client RADIUS e la destinazione RADIUS saranno uguali. Il segreto condiviso deve corrispondere a quello impostato nella sezione client RADIUS del server dei criteri di rete.

![Autenticazione RADIUS](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="next-steps"></a>Passaggi successivi

- Integrare Azure MFA e [app Web IIS](multi-factor-authentication-get-started-server-iis.md)

- Vedere le [Domande frequenti su Azure Multi-Factor Authentication](multi-factor-authentication-faq.md)



<!--HONumber=Feb17_HO1-->


