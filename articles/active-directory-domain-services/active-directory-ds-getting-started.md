---
title: 'Azure AD Domain Services: Creare il gruppo &quot;AAD DC Administrators&quot; | Microsoft Docs'
description: Introduzione a Servizi di dominio Azure Active Directory
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f6160c560b951c1355bf956a392dda5b41ac9cd7


---
# <a name="get-started-with-azure-ad-domain-services"></a>Introduzione ai servizi di dominio Azure AD
In questo articolo viene illustrata la procedura di configurazione necessaria per abilitare Servizi di dominio Azure AD per il tenant di Azure AD.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Attività 1: Creare il gruppo "AAD DC Administrators"
La prima attività consiste nel creare un gruppo amministrativo nel tenant di Azure Active Directory. Questo gruppo amministrativo speciale è chiamato **AAD DC Administrators**. Ai membri di questo gruppo verranno concessi privilegi di amministratore per i computer aggiunti al dominio gestito di Servizi di dominio Azure AD. Nei computer appartenenti a un dominio viene aggiunto al gruppo "Administrators". Inoltre, i membri di questo gruppo possono usare Desktop remoto per connettersi ai computer del dominio da remoto.  

> [!NOTE]
> Non sarà possibile esercitare i privilegi di amministratore di dominio o amministratore dell'organizzazione all'interno del dominio creato con Servizi di dominio Azure AD. Nei domini gestiti questi privilegi sono riservati dal servizio e non vengono resi disponibili agli utenti all'interno del tenant. Per poter eseguire alcune operazioni con privilegi, sarà tuttavia possibile usare il gruppo di amministrazione speciale creato in questa attività di configurazione. Queste operazioni prevedono l'aggiunta di computer al dominio, l'appartenenza al gruppo degli amministratori su computer aggiunti al dominio, la configurazione di Criteri di gruppo e così via.
> 
> 

In questa attività di configurazione verrà creato il gruppo amministrativo al quale verranno aggiunti uno o più utenti nella directory. Per creare il gruppo amministrativo per Servizi di dominio Azure AD eseguire i passaggi seguenti:

1. Passare al **portale di Azure classico** ([https://manage.windowsazure.com](https://manage.windowsazure.com))
2. Selezionare il nodo **Active Directory** nel riquadro sinistro.
3. Selezionare il tenant di Azure AD (directory) per il quale si desidera abilitare Servizi di dominio Azure AD. È possibile creare solo un dominio per ogni directory di Azure AD.
   
    ![Selezionare una directory di Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Fare clic sulla scheda **Gruppi** .
5. Per aggiungere un gruppo al tenant di Azure AD fare clic su **Aggiungi gruppo** nel riquadro attività nella parte inferiore della pagina.
   
    ![Pulsante Aggiungi gruppo](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. Creare un gruppo denominato **AAD DC Administrators**. Impostare **TIPO GRUPPO** su **Sicurezza**.
   
   > [!WARNING]
   > Per consentire l'accesso a Servizi di dominio Azure AD è necessario creare un gruppo con questo nome esatto.
   > 
   > 
   
    ![Creare un gruppo di amministratori](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. Aggiungere una descrizione per il gruppo in modo che gli altri capiscano che questo gruppo viene usato per concedere privilegi amministrativi all'interno di Servizi di dominio Azure AD.
8. Dopo aver creato il gruppo, fare clic sul nome del gruppo per visualizzarne le proprietà. Per aggiungere utenti come membri di questo gruppo fare clic sul pulsante **Aggiungi membri** nel pannello inferiore.
   
    ![Pulsate Aggiungi membri gruppo](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
9. Nella finestra di dialogo **Aggiungi membri** selezionare gli utenti da includere in questo gruppo e selezionare la casella di controllo al termine.
   
    ![Aggiungere utenti al gruppo di amministratori](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Attività 2: Creare o selezionare una rete virtuale di Azure
L'attività di configurazione successiva consiste nel [creare o selezionare una rete virtuale di Azure](active-directory-ds-getting-started-vnet.md).




<!--HONumber=Feb17_HO2-->


