---
title: 'Servizi di dominio Azure A: Abilitare Servizi di dominio Azure AD | Microsoft Docs'
description: Introduzione a Servizi di dominio Azure Active Directory
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/19/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 64bb5f7e78a6e48faf3487da780597b25891a2fa


---
# <a name="enable-azure-ad-domain-services"></a>Abilitare Azure Active Directory Domain Services
## <a name="task-3-enable-azure-ad-domain-services"></a>Attività 3: Abilitare Servizi di dominio Azure AD
In questa attività si abilita Azure Active Directory Domain Services per la directory. Eseguire i passaggi di configurazione seguenti per abilitare Azure Active Directory Domain Services per la directory.

1. Passare al **portale di Azure classico** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. Selezionare il nodo **Active Directory** nel riquadro sinistro.
3. Selezionare il tenant di Azure AD (directory) per il quale si desidera abilitare Servizi di dominio Azure AD.
   
    ![Selezionare una directory di Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Fare clic sulla scheda **Configure** .
   
    ![Scheda Configura della directory](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Scorrere fino alla sezione **servizi di dominio**.
   
    ![Sezione per la configurazione di Servizi di dominio](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)
6. Impostare l'opzione **Abilita Servizi di dominio per la directory** su **Sì**. Nella pagina vengono visualizzate altre opzioni di configurazione per Azure Active Directory Domain Services.
   
    ![Abilita servizi di dominio](./media/active-directory-domain-services-getting-started/enable-domain-services.png)
   
   > [!NOTE]
   > Quando si abilita Azure Active Directory Domain Services per il tenant, Azure AD genera e archivia gli hash delle credenziali Kerberos e NTLM necessari per l'autenticazione degli utenti.
   > 
   > 
7. Specificare il **Nome di dominio DNS di Servizi di dominio**.
   
   * Il nome di dominio predefinito della directory (ad esempio, un nome che termina con il suffisso di dominio **.onmicrosoft.com** ) viene selezionato per impostazione predefinita.
   * Nell'elenco sono contenuti tutti i domini che sono stati configurati per la directory di Azure AD, inclusi i domini verificati e non verificati configurati nella scheda 'Domini'.
   * È anche possibile digitare un nome di dominio personalizzato. In questo esempio è stato digitato il nome di dominio personalizzato "contoso100.com".
     
     > [!WARNING]
     > Assicurarsi che il prefisso del dominio del nome di dominio specificato, ad esempio "contoso100" nel nome di dominio "contoso100.com", contenga meno di 15 caratteri. Non è possibile creare un dominio servizi di dominio di Azure AD con un prefisso di dominio con più di 15 caratteri.
     > 
     > 
8. Assicurarsi che il nome di dominio DNS scelto per il dominio gestito non esista già nella rete virtuale. In particolare, controllare se:
   
   * È già presente un dominio con lo stesso nome di dominio DNS nella rete virtuale.
   * La rete virtuale selezionata ha una connessione VPN alla rete locale, dove è presente un dominio con lo stesso nome di dominio DNS.
   * Esiste un servizio cloud con lo stesso nome della rete virtuale.
9. Il passaggio successivo consiste nel selezionare una rete virtuale in cui si desidera rendere disponibile Servizi di dominio Azure AD. Selezionare la rete virtuale e la subnet dedicata create nell'elenco a discesa **Connetti Servizi di dominio a questa rete virtuale**.
   
   * Assicurarsi che la rete virtuale specificata appartenga a un'area di Azure supportata da servizi di dominio di Azure AD. Per informazioni sulle aree di Azure in cui è disponibile Azure Active Directory Domain Services, vedere i [servizi di Azure per area](https://azure.microsoft.com/regions/#services/) .
   * Le reti virtuali appartenenti a un'area in cui Azure Active Directory Domain Services non è supportato non vengono visualizzate nell'elenco a discesa.
   * Usare una subnet dedicata nella rete virtuale per Azure AD Domain Services. Assicurarsi di non selezionare la subnet del gateway. Vedere le [considerazioni sulla rete](active-directory-ds-networking.md). 
   * Analogamente, le reti virtuali create con Azure Resource Manager non vengono visualizzate nell'elenco a discesa. Le reti virtuali basate su Resource Manager attualmente non sono supportate da Azure Active Directory Domain Services.
10. Per abilitare Azure Active Directory Domain Services, fare clic su **Salva** nel riquadro attività nella parte inferiore della pagina.
11. Nella pagina viene visualizzato "In sospeso..." durante l'abilitazione di Azure Active Directory Domain Services per la directory.
    
    ![Abilitare i servizi di dominio - stato in sospeso](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)
    
    > [!NOTE]
    > Servizi di dominio Azure AD garantisce un'elevata disponibilità per il dominio gestito. Dopo avere abilitato Azure Active Directory Domain Services, si noti che vengono visualizzati uno alla volta gli indirizzi IP per cui è disponibile Domain Services nella rete virtuale. Il secondo indirizzo IP viene visualizzato dopo breve tempo, non appena il servizio abilita la disponibilità elevata per il dominio. Al termine della configurazione e attivazione della disponibilità elevata per il dominio, nella sezione **Servizi di dominio** della scheda **Configura** dovrebbero comparire due indirizzi IP.
    > 
    > 
12. Dopo circa 20-30 minuti, il primo indirizzo IP in cui è disponibile Domain Services nella rete virtuale viene visualizzato nel campo **Indirizzo IP** nella pagina **Configura**.
    
    ![Servizi di dominio abilitato - provisioning del primo indirizzo IP completato](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
13. Quando la disponibilità elevata è operativa per il dominio, nella pagina sono visualizzati due indirizzi IP. Il dominio gestito è disponibile nella rete virtuale selezionata a questi due indirizzi IP. Prendere nota degli indirizzi IP per poter aggiornare le impostazioni DNS per la rete virtuale. Questo passaggio consente alle macchine virtuali nella rete virtuale di connettersi al dominio per operazioni quali l'aggiunta al dominio.
    
    ![Servizi di dominio abilitato - provisioning di entrambi gli indirizzi IP completato](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> A seconda delle dimensioni del tenant di Azure AD (numero di utenti, gruppi e così via), la sincronizzazione con il dominio gestito richiede tempo. Questo processo di sincronizzazione avviene in background. Per tenant di grandi dimensioni con decine di migliaia di oggetti, possono essere necessari anche un paio di giorni perché tutti gli utenti, le appartenenze ai gruppi e le credenziali siano sincronizzati.
> 
> 

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Attività 4: Aggiornare le impostazioni DNS per la rete virtuale di Azure
L'attività di configurazione successiva consiste nell' [aggiornare le impostazioni DNS per la rete virtuale di Azure](active-directory-ds-getting-started-dns.md).




<!--HONumber=Dec16_HO2-->


