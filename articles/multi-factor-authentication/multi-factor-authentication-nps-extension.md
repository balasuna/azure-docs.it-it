---
title: "Usare i server di Server dei criteri di rete esistenti per offrire le funzionalità MFA di Azure | Microsoft Docs"
description: "L&quot;estensione di Server dei criteri di rete per Multi-Factor Authentication di Azure è una soluzione semplice per aggiungere le funzionalità di verifica in due passaggi basata sul cloud nell&quot;infrastruttura di autenticazione esistente."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: c9cf1540c0f8d16b0a5cdbedc78ac58cd5686107
ms.openlocfilehash: 51ebadc241892ebc6fb26b29955e3030ae1bdcb3


---
# <a name="augment-your-existing-authentication-infrastructure-with-the-nps-extension-for-azure-multi-factor-authentication---public-preview"></a>Ampliare l'infrastruttura di autenticazione esistente con l'estensione di Server dei criteri di rete per Multi-Factor Authentication di Azure, anteprima pubblica

L'estensione di Server dei criteri di rete (NPS) per Azure MFA aggiunge funzionalità MFA basate su cloud per l'infrastruttura di autenticazione usando i server esistenti. Con l'estensione di Server dei criteri di rete, è possibile aggiungere la verifica con telefonata, SMS o app telefonica al flusso di autenticazione esistente senza dover installare, configurare e gestire nuovi server. 
 
Quando si usa l'estensione dei criteri di rete per di Azure MFA, il flusso di autenticazione include i componenti seguenti: 

1. Il **Server NAS/VPN** riceve le richieste dei client VPN e le converte in richieste RADIUS per il Server dei criteri di rete. 
2. Il **Server dei criteri di rete** si connette ad Active Directory per eseguire l'autenticazione principale per le richieste RADIUS e, al completamento dell'operazione, passa la richiesta alle estensioni installate.  
3. L'**estensione di Server dei criteri di rete** attiva una richiesta di autenticazione secondaria per Azure MFA. Dopo che l'estensione riceve la risposta e se la richiesta di verifica MFA ha esito positivo, la richiesta di autenticazione viene completata, fornendo al server di Server dei criteri di rete i token di sicurezza che includono un'attestazione MFA, emessa dal servizio token di sicurezza di Azure.  
4. **Azure MFA** comunica con Azure Active Directory per recuperare i dettagli dell'utente ed esegue l'autenticazione secondaria grazie al metodo di verifica configurato per l'utente.

Il diagramma seguente illustra questo flusso di richiesta di autenticazione ad alto livello: 

![Diagramma del flusso di autenticazione](./media/multi-factor-authentication-nps-extension/auth-flow.png)

## <a name="prerequisites"></a>Prerequisiti

L'estensione di Server dei criteri di rete è progettata per funzionare con l'infrastruttura esistente. Prima di iniziare, verificare che i prerequisiti seguenti siano disponibili.

### <a name="licenses"></a>Licenze

L'estensione di Server dei criteri di rete per Azure MFA è disponibile per i clienti dotati di [licenze per Multi-Factor Authentication di Azure](multi-factor-authentication.md) (tra cui una sottoscrizione con Azure AD Premium, EMS o MFA).

### <a name="software"></a>Software

Windows Server 2008 R2 SP1 o versione successiva con il componente Server dei criteri di rete abilitato.

### <a name="libraries"></a>Librerie

L'estensione di Server dei criteri di rete richiede due librerie. Queste vengono installate durante il processo di configurazione:

-   Microsoft Visual Studio 2013 C++ Redistributable (X64)
-   Modulo di Microsoft Azure Active Directory per Windows PowerShell versione 1.1.166

### <a name="azure-active-directory"></a>Azure Active Directory

Per gli utenti che usano l'estensione di Server dei criteri di rete è necessaria la sincronizzazione con Azure Active Directory grazie ad Azure AD Connect e l'abilitazione per MFA.

Quando si installa l'estensione, per il tenant di Azure AD sono necessarie le credenziali di amministrazione e l'ID della directory. L'ID della directory si trova nel [Portale di Azure](https://portal.azure.com). Accedere come amministratore, selezionare l'icona di **Azure Active Directory** sulla sinistra, quindi selezionare **Proprietà**. Copiare il GUID nella casella **ID directory** e salvare.

![L'ID directory si trova nelle proprietà di Azure Active Directory](./media/multi-factor-authentication-nps-extension/find-directory-id.png)

## <a name="install-the-nps-extension"></a>Installare l'estensione di Server dei criteri di rete

Per installare l'estensione di Server dei criteri di rete per Azure MFA:

1.  [Scaricare l'estensione di Server dei criteri di rete](https://aka.ms/npsmfa) dall'Area download Microsoft
2.  Copiare il file binario nel Server dei criteri di rete da configurare
3.  Eseguire *setup.exe* e seguire le istruzioni di installazione

Dopo aver completato l'installazione, il programma di installazione crea uno script di PowerShell in questa posizione: `C:\Program Files\Microsoft\AzureMfa\Config` (dove C:\ è l'unità di installazione). Lo script di PowerShell esegue le azioni seguenti:

-   Creare un certificato autofirmato.
-   Associare la chiave pubblica del certificato all'entità servizio su Azure AD.
-   Archiviare il certificato nell'archivio certificati del computer locale.
-   Concedere l'accesso alla chiave privata del certificato all'utente di rete.
-   Riavviare il Server dei criteri di rete.

A meno che non si desideri utilizzare i propri certificati (invece dei certificati autofirmati generati dallo script di PowerShell), eseguire lo script di PowerShell per completare l'installazione.

## <a name="configure-your-nps-extension"></a>Configurare l'estensione di Server dei criteri di rete

In questa sezione sono disponibili considerazioni e suggerimenti sulla progettazione per una corretta distribuzione dell'estensione di Server dei criteri di rete.

### <a name="configurations-limitations"></a>Limitazioni delle configurazioni

- L'estensione di Server dei criteri di rete è pensata per la distribuzione esistente e non per le nuove distribuzioni. Per questo motivo, l'estensione di Server dei criteri di rete per Azure MFA non include strumenti per la migrazione degli utenti e impostazioni dal Server MFA al cloud.

- L'estensione di Server dei criteri di rete usa UPN dell'Active Directory locale per identificare l'utente in Azure MFA che deve eseguire l'autenticazione secondaria. L'estensione non può essere configurata per usare un identificatore come ID di accesso alternativo o campo AD personalizzato diverso dall'UPN.  

### <a name="control-radius-clients-that-require-mfa"></a>Client RADIUS di controllo che richiedono MFA

Dopo aver abilitato MFA per un client RADIUS utilizzando l'estensione di Server dei criteri di rete, tutte le autenticazioni per questo client devono eseguire MFA. Se si desidera abilitare MFA solo per alcuni client RADIUS, è possibile configurare due server di Server dei criteri di rete e installare l'estensione solo su uno di questi. Configurare i client RADIUS per cui si vuole fare in modo che MFA invii richieste al server di Server dei criteri di rete configurato con l'estensione e gli altri client RADIUS al server di Server dei criteri di rete senza configurazione per l'estensione.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>Impostazioni per gli utenti che non sono registrati per MFA

Se sono presenti utenti che non sono registrati per MFA, è possibile stabilire cosa succede quando questi tentano di eseguire l'autenticazione. Usare l'impostazione del registro di sistema *REQUIRE_USER_MATCH* nel percorso del registro di sistema *HKLM\Software\Microsoft\AzureMFA* per controllare il comportamento della funzionalità. Questa impostazione non ha un'unica opzione di configurazione:

| Chiave | Valore | Default |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | VERO/FALSO | Non impostato (equivalente a VERO) |

Lo scopo di questa impostazione è stabilire cosa fare quando un utente non è registrato per MFA. Quando la chiave non esiste, non è impostata o è impostata su VERO e l'utente non è registrato, allora l'estensione non esegue correttamente la richiesta di verifica MFA. Quando la chiave è impostata su FALSO e l'utente non è registrato, l'autenticazione procede senza eseguire MFA.

È possibile scegliere di creare questa chiave e impostarla su FALSO durante il caricamento dell'utente. Poiché l'impostazione della chiave consente agli utenti che non sono registrati per MFA di accedere senza una richiesta di verifica, è necessario rimuovere la chiave prima di passare all'ambiente di produzione.

## <a name="troubleshooting"></a>Risoluzione dei problemi

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>Come verificare che il certificato client sia installato come previsto?

Cercare il certificato autofirmato creato dal programma di installazione nell'archivio dei certificati e verificare che la chiave privata disponga delle autorizzazioni concesse all'utente **Servizio di rete**. Il certificato avrà un nome oggetto **CN \<tenantid\>, OU = Estensione di Server dei criteri di rete Microsoft**

-------------------------------------------------------------

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>Come verificare che il certificato client sia associato al tenant in Azure Active Directory?

Aprire il prompt dei comandi di PowerShell ed eseguire i comandi seguenti:

```
> import-module MSOnline
> Connect-MsolService
> Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 
```

Questi comandi consentono di stampare tutti i certificati associando il tenant con l'istanza dell'estensione di Server dei criteri di rete nella sessione di PowerShell. Cercare il certificato esportando il certificato client come file "Codificato Base&64; X.509 (.CER)" senza la chiave privata e confrontarlo con l'elenco di PowerShell.

I timbri data/ora Valido-dal e Valido-fino al, che sono in formato leggibile, possono essere usati per filtrare i risultati errati se il comando restituisce più di un certificato.

-------------------------------------------------------------

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>Perché le richieste hanno esito negativo con errore di token ADAL?

Questo errore potrebbe essere dovuto a diverse ragioni. Usare la procedura seguente per la risoluzione:

1. Riavviare il server di Server dei criteri di rete.
2. Verificare che il certificato client sia installato come previsto.
3. Verificare che il certificato sia associato al tenant in Azure AD.
4. Verificare che https://login.windows.new/ sia accessibile dal server che esegue l'estensione.

-------------------------------------------------------------

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Perché l'autenticazione ha esito negativo e restituisce un errore nei log HTTP che indica che l'utente non è stato trovato?

Verificare che AD Connect sia in esecuzione e che l'utente sia presente sia in Active Directory di Windows sia in Azure Active Directory.

------------------------------------------------------------

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Perché vengono visualizzati errori di connessione HTTP nei log che contengono le autenticazioni non riuscite?

Verificare che https://adnotifications.windowsazure.com sia raggiungibile dal server che esegue l'estensione di Server dei criteri di rete.

## <a name="next-steps"></a>Passaggi successivi

Consultare gli articoli su come integrare Azure MFA con [Active Directory](multi-factor-authentication-get-started-server-dirint.md), [l'autenticazione RADIUS](multi-factor-authentication-get-started-server-radius.md) e [l'autenticazione LDAP](multi-factor-authentication-get-started-server-ldap.md).



<!--HONumber=Feb17_HO1-->


