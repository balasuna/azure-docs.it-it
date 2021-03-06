---
title: Ruoli di lavoro ibridi per runbook di Automazione di Azure | Microsoft Docs
description: "Questo articolo fornisce informazioni sull&quot;installazione e l&quot;uso di Hybrid Runbook Workers, una funzionalità di Automazione di Azure che consente di eseguire i runbook nei computer del data center locale."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 06227cda-f3d1-47fe-b3f8-436d2b9d81ee
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 4b7912b48ef37eac3148582d1d635d8a4c4a4b44
ms.openlocfilehash: 8ee5dfea961984bd01a24815b9d3c94f486a57d7


---
# <a name="azure-automation-hybrid-runbook-workers"></a>Funzionalità Hybrid Runbook Workers di Automazione di Azure
I runbook in Automazione di Azure non possono accedere alle risorse nel data center locale perché vengono eseguiti nel cloud di Azure.  La funzionalità Hybrid Runbook Workers di Automazione di Azure consente di eseguire i runbook su computer posizionati nel data center per gestire le risorse locali. I runbook vengono infatti archiviati e gestiti in Automazione di Azure e quindi distribuiti a uno o più computer locali.  

Questa funzionalità è illustrata nell'immagine seguente.<br>  

![Panoramica di Hybrid Runbook Workers](media/automation-hybrid-runbook-worker/automation.png)

Grazie a tale funzionalità, è possibile designare uno o più computer del data center come computer di lavoro runbook ibridi ed eseguire i runbook da Automazione di Azure.  Ogni ruolo di lavoro richiede l'agente di gestione Microsoft con una connessione a Microsoft Operations Management Suite e all'ambiente runbook di Automazione di Azure.  Operations Management Suite viene usato solo per installare e gestire l'agente di gestione e per monitorare la funzionalità del ruolo di lavoro.  La distribuzione dei runbook e l'istruzione per eseguirli vengono gestite mediante Automazione di Azure.

Non sono previsti requisiti di firewall in ingresso per supportare Hybrid Runbook Workers. L'agente nel computer locale avvia tutte le comunicazioni con Automazione di Azure nel cloud. Quando un runbook viene avviato, Automazione di Azure crea un'istruzione che viene recuperata dall'agente, che esegue quindi il pull del runbook e di tutti i parametri prima dell'esecuzione,  oltre a recuperare da Automazione di Azure gli eventuali [asset](http://msdn.microsoft.com/library/dn939988.aspx) usati dal runbook.

> [!NOTE]
> Per gestire la configurazione dei server che supporta il ruolo di lavoro ibrido per runbook con Desired State Configuration (DSC), è necessario aggiungerli come nodi DSC.  Per informazioni sul caricamento dei server per la gestione con DSC, vedere [Caricamento di computer per la gestione con Automation DSC per Azure](automation-dsc-onboarding.md).           
><br>
>Attualmente se si abilita la [soluzione Gestione aggiornamenti](../operations-management-suite/oms-solution-update-management.md), qualsiasi computer Windows connesso all'area di lavoro OMS verrà automaticamente configurato come ruolo di lavoro ibrido per runbook per supportare i runbook che fanno parte di questa soluzione.  Tuttavia, la soluzione non è registrata con tutti i gruppi del ruolo di lavoro ibrido creati nell'account di Automazione e non è possibile aggiungerla a un gruppo di lavoro ibrido per l'esecuzione dei runbook personalizzati.  Se un computer Windows è già definito come ruolo di lavoro ibrido per runbook e connesso all'area di lavoro OMS, è necessario rimuoverlo dall'area di lavoro OMS prima di aggiungere la soluzione per evitare che i runbook non funzionino come previsto.  


## <a name="hybrid-runbook-worker-groups"></a>Gruppi di computer di lavoro runbook ibridi
Ogni computer di lavoro runbook ibrido è un membro di un gruppo di computer di lavoro runbook ibridi che è possibile specificare quando si installa l'agente.  Un gruppo può includere un solo agente, ma è possibile installarvi più agenti per garantire una disponibilità elevata.

Quando si avvia un runbook in un computer di lavoro runbook ibrido, è necessario specificare il gruppo in cui verrà eseguito.  I membri del gruppo determineranno quale computer di lavoro gestirà la richiesta.  Non è possibile scegliere un computer di lavoro specifico.

## <a name="hybrid-runbook-worker-requirements"></a>Requisiti del ruolo di lavoro ibrido per runbook
È necessario designare almeno un computer locale per l'esecuzione di processi ibridi per runbook.  Il computer deve avere le caratteristiche seguenti:

* Windows Server 2012 o versioni successive
* Windows PowerShell 4.0 o versioni successive.  Si consiglia di installare Windows PowerShell 5.0 sui computer per aumentare l'affidabilità. È possibile scaricare la nuova versione dall'[Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=50395)
* Almeno due core e 4 GB di RAM

Tenere presente quanto segue per i ruoli di lavoro ibridi:

* Designare più ruoli di lavoro ibridi in ogni gruppo per garantire disponibilità elevata.  
* I ruoli di lavoro ibridi possono coesistere con server runbook Service Management Automation o System Center Orchestrator.
* Può essere opportuno usare un computer situato fisicamente all'interno o in prossimità dell'area dell'account di automazione, perché al termine di un processo i relativi dati vengono restituiti ad Automazione di Azure.

### <a name="configure-proxy-and-firewall-settings"></a>Configurare le impostazioni di proxy e firewall
Per connettersi e registrarsi al servizio Microsoft Operations Management Suite (OMS), il ruolo di lavoro ibrido per runbook locale deve avere accesso al numero di porta e agli URL descritti di seguito,  in aggiunta [alle porte e agli URL necessari per la connessione di Microsoft Monitoring Agent](../log-analytics/log-analytics-proxy-firewall.md#configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent) a OMS. Se si usa un server proxy per la comunicazione tra l'agente e il servizio OMS, è necessario verificare che le risorse appropriate siano accessibili. Se si usa un firewall per limitare l'accesso a Internet, è necessario configurare il firewall per consentire l'accesso.

Di seguito sono elencati la porta e gli URL necessari affinché il ruolo di lavoro ibrido per runbook comunichi con Automazione.

* Porta: è necessaria solo la porta TCP 443 per l'accesso a Internet in uscita
* URL globale: *.azure-automation.net

Se si ha un account di automazione definito per un'area specifica e si vuole limitare la comunicazione con il data center di tale area, nella tabella seguente è riportato il record DNS per ogni area.

| **Area** | **Record DNS** |
| --- | --- |
| Stati Uniti centro-meridionali |scus-jobruntimedata-prod-su1.azure-automation.net |
| Stati Uniti orientali 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Europa occidentale |we-jobruntimedata-prod-su1.azure-automation.net |
| Europa settentrionale |ne-jobruntimedata-prod-su1.azure-automation.net |
| Canada centrale |cc-jobruntimedata-prod-su1.azure-automation.net |
| Asia sudorientale |sea-jobruntimedata-prod-su1.azure-automation.net |
| India centrale |cid-jobruntimedata-prod-su1.azure-automation.net |
| Giappone orientale |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australia sud-orientale |ase-jobruntimedata-prod-su1.azure-automation.net |

## <a name="installing-hybrid-runbook-worker"></a>Installazione di Hybrid Runbook Workers

Di seguito vengono descritti due metodi per installare e configurare il ruolo di lavoro ibrido per runbook.  Il primo metodo è uno script di PowerShell che consente di automatizzare tutti i passaggi necessari per configurare il computer Windows. Questo è l'approccio consigliato perché semplifica l'intero processo di distribuzione.  Il secondo metodo prevede l'esecuzione di una procedura dettagliata per installare e configurare il ruolo manualmente.   

### <a name="automated-deployment"></a>Distribuzione automatizzata

Seguire questa procedura per automatizzare l'installazione e configurazione del ruolo di lavoro ibrido.  

1. Scaricare lo script *New-OnPremiseHybridWorker.ps1* da [PowerShell Gallery](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/1.0/DisplayScript) direttamente dal computer che esegue il ruolo di lavoro ibrido per runbook o da un altro computer nell'ambiente e copiarlo nel ruolo di lavoro.  

    Per l'esecuzione dello script *New-OnPremiseHybridWorker.ps1* sono necessari i parametri seguenti:

  * *AutomationAccountName* (obbligatorio) - nome dell'account di Automazione.  
  * *ResourceGroupName* (obbligatorio) - nome del gruppo di risorse associato all'account di Automazione.  
  * *HybridGroupName* (obbligatorio) - nome di un gruppo di ruoli di lavoro ibridi per runbook che verrà specificato come destinazione per i runbook che supportano questo scenario. 
  *  *SubscriptionID* (obbligatorio) - ID della sottoscrizione di Azure in cui è disponibile l'account di Automazione.
  *  *WorkspaceName* (facoltativo) - nome dell'area di lavoro OMS.  Se non è disponibile un'area di lavoro OMS, lo script ne crea e configura una.

    > [!NOTE]
    > Le uniche aree di Automazione supportate per l'integrazione con OMS sono attualmente: **Australia sud-orientale**, **Stati Uniti orientali 2**, **Asia sud-orientale** ed **Europa occidentale**.  Se l'account di Automazione non si trova in una di queste aree, lo script creerà l'area di lavoro OMS ma avviserà l'utente che non è possibile collegarli.  

2. Nel computer in uso avviare **Windows PowerShell** dalla schermata **Start** in modalità amministratore.  
3. Dalla shell della riga di comando di PowerShell passare alla cartella che contiene lo script scaricato ed eseguirlo modificando i valori per i parametri *-AutomationAccountName*, *-ResourceGroupName*, *-HybridGroupName*, *-SubscriptionId* e *-WorkspaceName*.

    > [!NOTE] 
    > Verrà richiesto di autenticarsi con Azure dopo aver eseguito lo script.  È **necessario** accedere con un account membro del ruolo Amministratori della sottoscrizione e coamministratore della sottoscrizione.   
    
        .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> `
        -ResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
        -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfOMSWorkspace>

4. Verrà richiesto di accettare di installare **NuGet** e verrà richiesto di eseguire l'autenticazione con le credenziali di Azure.<br><br> ![Esecuzione dello script New-OnPremiseHybridWorker](media/automation-hybrid-runbook-worker/new-onpremisehybridworker-scriptoutput.png)

5. Dopo aver completato lo script, il pannello Gruppi di ruoli di lavoro ibridi mostrerà il nuovo gruppo e il numero di membri oppure, nel caso di un gruppo esistente, il numero di membri verrà incrementato di conseguenza.  È possibile selezionare il gruppo nell'elenco nel pannello **Gruppi di ruoli di lavoro ibridi** e selezionare il riquadro **Ruoli di lavoro per runbook**.  Nel pannello **Ruoli di lavoro per runbook** verrà visualizzato ogni membro del gruppo elencato.  

### <a name="manual-deployment"></a>Distribuzione manuale 
Eseguire i primi due passaggi una volta per l'ambiente di automazione e quindi ripetere i passaggi rimanenti per ogni computer di lavoro.

#### <a name="1-create-operations-management-suite-workspace"></a>1. Creare l'area di lavoro di Operations Management Suite
Se non si ha ancora un'area di lavoro di Operations Management Suite, crearne una seguendo le istruzioni per [configurare l'area di lavoro](https://technet.microsoft.com/library/mt484119.aspx). Se già si dispone di un'area di lavoro, è possibile usarla.

#### <a name="2-add-automation-solution-to-operations-management-suite-workspace"></a>2. Aggiungere la soluzione di automazione all'area di lavoro di Operations Management Suite
Le soluzioni aggiungono funzionalità a Operations Management Suite.  La soluzione di automazione aggiunge funzionalità per Automazione di Azure, incluso il supporto per il ruolo di lavoro ibrido per runbook.  Quando si aggiunge la soluzione all'area di lavoro, i componenti del ruolo di lavoro vengono automaticamente propagati al computer dell'agente che verrà installato nel passaggio successivo.

Per aggiungere la soluzione [Automazione](../log-analytics/log-analytics-add-solutions.md) all'area di lavoro di Operations Management Suite, seguire le istruzioni contenute nell'articolo relativo a **come aggiungere una soluzione tramite la raccolta soluzioni** .

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. Installare Microsoft Monitoring Agent
Microsoft Monitoring Agent connette i computer a Operations Management Suite.  Quando si installa l'agente nel computer locale e lo si connette all'area di lavoro, viene eseguito automaticamente il download dei componenti necessari per il ruolo di lavoro ibrido per runbook.

Per installare l'agente nel computer locale, seguire le istruzioni contenute in [Connettere computer Windows a Log Analytics](../log-analytics/log-analytics-windows-agents.md).  È possibile ripetere questo processo per più computer per aggiungere più ruoli di lavoro nell'ambiente.

Dopo che l'agente si è connesso a Operations Management Suite, viene elencato nella scheda **Origini connesse** del riquadro **Impostazioni** di Operations Management Suite.  È possibile verificare che l'agente abbia scaricato correttamente la soluzione di automazione se include una cartella **AzureAutomationFiles** in C:\Programmi\Microsoft Monitoring Agent\Agent.  Per verificare la versione della funzionalità dei ruoli di lavoro ibridi per runbook, passare a C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\ e prendere nota della sottocartella \\*version*.   

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Installare l'ambiente runbook e connettersi ad Automazione di Azure
Quando si aggiunge un agente a Operations Management Suite, la soluzione di automazione esegue il push del modulo **HybridRegistration** di PowerShell, che contiene il cmdlet **Add-HybridRunbookWorker**.  È possibile usare questo cmdlet per installare l'ambiente runbook nel computer e registrarlo in Automazione di Azure.

Aprire una sessione di PowerShell in modalità amministratore ed eseguire i comandi seguenti per importare il modulo.

    cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
    Import-Module HybridRegistration.psd1

Eseguire quindi il cmdlet **Add-HybridRunbookWorker** con la sintassi seguente:

    Add-HybridRunbookWorker –Name <String> -EndPoint <Url> -Token <String>

È possibile ottenere le informazioni necessarie per questo cmdlet dal pannello **Gestisci chiavi** del portale di Azure.  Aprire il pannello selezionando l'opzione **Chiavi** nel pannello **Impostazioni** dall'account di Automazione.

![Panoramica di Hybrid Runbook Workers](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **Name** è il nome del gruppo di computer di lavoro runbook ibridi. Se il gruppo già esiste nell'account di automazione, il computer corrente vi verrà aggiunto direttamente.  Se il gruppo ancora non esiste, verrà aggiunto.
* **EndPoint** è il valore del campo **URL** del pannello **Gestisci chiavi**.
* **Token** è il valore **Chiave di accesso primaria** nel pannello **Gestisci chiavi**.  

Usare l'opzione **-Verbose** con **Add-HybridRunbookWorker** per ricevere informazioni dettagliate sull'installazione.

#### <a name="5-install-powershell-modules"></a>5. Installare i moduli di PowerShell
I runbook possono usare tutte le attività e i cmdlet definiti nei moduli installati nell'ambiente di Automazione di Azure.  Questi moduli tuttavia non vengono distribuiti automaticamente nei computer locali, pertanto è necessario installarli manualmente.  L'unica eccezione è rappresentata dal modulo Azure, che viene installato per impostazione predefinita, garantendo l'accesso ai cmdlet per tutte le attività e i servizi di Azure per Automazione di Azure.

Poiché lo scopo principale della funzionalità Hybrid Runbook Workers è gestire le risorse locali, probabilmente sarà necessario installare i moduli che supportano tali risorse.  Per informazioni sull'installazione dei moduli di Windows PowerShell, vedere il [relativo articolo](http://msdn.microsoft.com/library/dd878350.aspx).

## <a name="removing-hybrid-runbook-worker"></a>Rimozione del ruolo di lavoro ibrido per runbook 
È possibile rimuovere uno o più ruoli di lavoro ibrido per runbook da un gruppo o è possibile rimuovere il gruppo, a seconda dei requisiti.  Per rimuovere un ruolo di lavoro ibrido per runbook da un computer locale, seguire questa procedura.

1. Nel portale di Azure passare all'account di Automazione.  
2. Nel pannello **Impostazioni** selezionare **Chiavi** e prendere nota dei valori per il campo **URL** e **Chiave di accesso primaria**.  Queste informazioni saranno necessarie per il passaggio successivo.
3. Aprire una sessione di PowerShell in modalità amministratore ed eseguire il comando seguente - `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Per un log dettagliato del processo di rimozione, usare l'opzione **-Verbose** .

> [!NOTE]
> Questa operazione non rimuove Microsoft Monitoring Agent dal computer, ma solo la funzionalità e la configurazione del ruolo di lavoro ibrido per runbook.  

## <a name="remove-hybrid-worker-groups"></a>Rimuovere gruppi di ruoli di lavoro ibridi
Per rimuovere un gruppo, è innanzitutto necessario rimuovere il ruolo di lavoro ibrido per runbook da ogni computer membro del gruppo usando la procedura descritta in precedenza e quindi attenersi alla procedura seguente per rimuovere il gruppo.  

1. Nel portale di Azure aprire l'account di automazione.
2. Selezionare il riquadro **Gruppi di ruoli di lavoro ibridi** e nel pannello **Gruppi di lavoro ibridi** selezionare il gruppo che si desidera eliminare.  Dopo aver selezionato il gruppo specifico, viene visualizzato il pannello delle proprietà del **Gruppo di lavoro ibrido**.<br> ![Pannello Gruppi di ruoli di lavoro ibridi ](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
3. Nel pannello delle proprietà del gruppo selezionato fare clic su **Elimina**.  Viene visualizzato un messaggio che richiede la conferma dell'azione. Selezionare **Sì** se si è certi che si desidera procedere.<br> ![Finestra di dialogo di conferma dell'eliminazione del gruppo](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> Questo processo può richiedere alcuni secondi. Per tenere traccia dello stato di avanzamento, è possibile usare la voce **Notifiche** nel menu.  

## <a name="starting-runbooks-on-hybrid-runbook-worker"></a>Avvio dei runbook in Hybrid Runbook Workers
[avvio di un runbook in Automazione di Azure](automation-starting-a-runbook.md) illustra diversi modi in cui è possibile eseguire l'avvio dei runbook.  Hybrid Runbook Workers aggiunge un'opzione **RunOn** in cui è possibile specificare il nome di un gruppo di computer di lavoro runbook ibridi.  Se si specifica un gruppo, il runbook verrà recuperato ed eseguito dai computer di lavoro inclusi in tale gruppo.  Se non si specifica l'opzione, verrà eseguito come di consueto in Automazione di Azure.

Quando si avvia un runbook nel portale di Azure, viene visualizzata l'opzione **Esegui in**, che consente di selezionare **Azure** o **Ruolo di lavoro ibrido**.  Se si seleziona **Computer di lavoro ibrido**, sarà quindi possibile selezionare il gruppo da un elenco a discesa.

Usare il parametro **RunOn**. È possibile eseguire il comando seguente per avviare un runbook denominato Test-Runbook in un gruppo di computer di lavoro runbook ibridi denominato MyHybridGroup usando Windows PowerShell.

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> Il parametro **RunOn** è stato aggiunto al cmdlet **Start-AzureAutomationRunbook** nella versione 0.9.1 di Microsoft Azure PowerShell.  È consigliabile [scaricare la versione più recente](https://azure.microsoft.com/downloads/) se la versione installata è precedente.  È sufficiente installare questa versione nella workstation in cui si avvierà il runbook da Windows PowerShell.  Non è necessario installarla nel computer di lavoro, a meno che non si intenda avviare i runbook da tale computer.  Non è attualmente possibile avviare un runbook in un computer di lavoro runbook ibrido da un altro runbook, in quanto nell'account di automazione dovrebbe essere installata la versione più recente di Azure PowerShell.  Tale versione verrà aggiornata automaticamente in Automazione di Azure e verrà presto inviata automaticamente tramite push ai computer di lavoro.
>
>

## <a name="runbook-permissions"></a>Autorizzazioni per i runbook
I runbook eseguiti in un ruolo di lavoro ibrido per runbook non possono usare lo stesso metodo in genere usato per l'autenticazione dei runbook per le risorse di Azure, perché accedono a risorse esterne ad Azure.  Il runbook può fornire la propria autenticazione alle risorse locali oppure è possibile specificare un account RunAs per fornire un contesto utente per tutti i runbook.

### <a name="runbook-authentication"></a>Autenticazione dei runbook
Per impostazione predefinita, i runbook vengono eseguiti nel contesto dell'account di sistema locale nel computer locale, quindi devono autenticarsi per le risorse a cui accederanno.  

Nel proprio runbook è possibile usare asset di tipo [Credenziali](http://msdn.microsoft.com/library/dn940015.aspx) e [Certificato](http://msdn.microsoft.com/library/dn940013.aspx) con cmdlet che consentono di specificare le credenziali per poter eseguire l'autenticazione per risorse diverse.  L'esempio seguente illustra una parte di un runbook che riavvia un computer.  Recupera le credenziali da un asset di tipo credenziale e il nome del computer da un asset di tipo variabile e quindi usa questi valori con il cmdlet Restart-Computer.

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

È anche possibile usare [InlineScript](automation-powershell-workflow.md#inlinescript), che consentirà di eseguire blocchi di codice in un altro computer con le credenziali specificate tramite il [parametro comune PSCredential](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="runas-account"></a>Account RunAs
Per evitare che i runbook debbano autenticarsi per le risorse locali, è possibile specificare un account **RunAs** per un gruppo di ruoli di lavoro ibridi.  Specificare un [asset credenziali](automation-credentials.md) con accesso alle risorse locali. Tutti i runbook useranno queste credenziali durante l'esecuzione in un ruolo di lavoro ibrido per runbook nel gruppo.  

Il nome utente per le credenziali deve essere in uno dei formati seguenti:

* dominio\nome utente
* username@domain
* nome utente (per gli account locali nel computer locale)

Usare la procedura seguente per specificare un account RunAs per un gruppo di lavoro ibrido:

1. Creare un [asset credenziali](automation-credentials.md) con accesso alle risorse locali.
2. Nel portale di Azure aprire l'account di automazione.
3. Selezionare il riquadro **Gruppi di ruoli di lavoro ibridi** e quindi il gruppo.
4. Selezionare **Tutte le impostazioni** e quindi **Impostazioni del gruppo di lavoro ibrido**.
5. Modificare **Esegui come** da **Predefinito** a **Personalizzato**.
6. Selezionare le credenziali e fare clic su **Salva**.

## <a name="creating-runbooks-for-hybrid-runbook-worker"></a>Creazione dei runbook per Hybrid Runbook Workers
Non esiste alcuna differenza nella struttura dei runbook che vengono eseguiti in Automazione di Azure e di quelli eseguiti in Hybrid Runbook Workers. I runbook usati nell'uno o nell'altro caso saranno tuttavia molto diversi perché i runbook per Hybrid Runbook Workers in genere gestiranno le risorse locali nel data center, mentre i runbook in Automazione di Azure di solito gestiscono le risorse nel cloud di Azure.

È possibile modificare un runbook per Hybrid Runbook Workers in Automazione di Azure, ma si potrebbero incontrare difficoltà se si tenta di testare il runbook nell'editor.  I moduli di PowerShell che accedono alle risorse locali potrebbero non essere installati nell'ambiente di Automazione di Azure e in questo caso il test avrebbe esito negativo.  Se si sceglie di installare i moduli necessari, il runbook verrà eseguito, ma non sarà in grado di accedere alle risorse locali per un test completo.

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a>Risoluzione dei problemi relativi ai runbook nel ruolo di lavoro ibrido per runbook
[output e i messaggi di runbook](automation-runbook-output-and-messages.md) vengono inviati ad Automazione di Azure da ruoli di lavoro ibridi nello stesso modo in cui vengono eseguiti i processi per runbook nel cloud.  È anche possibile abilitare i flussi Verbose e Progress come per qualsiasi altro runbook.  

I log vengono archiviati localmente in ogni ruolo di lavoro ibrido in C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes.

Se i runbook non vengono completati correttamente e il riepilogo del processo visualizza lo stato **Sospeso**, vedere l'articolo sulla risoluzione dei problemi [Ruolo di lavoro ibrido per runbook: un processo runbook termina con lo stato Sospeso](automation-troubleshooting-hrw-runbook-terminates-suspended.md).   

## <a name="relationship-to-service-management-automation"></a>Relazione con Service Management Automation
[Service Management Automation (SMA)](https://technet.microsoft.com/library/dn469260.aspx) consente di eseguire gli stessi runbook supportati da Automazione di Azure nel data center locale. SMA viene in genere distribuito insieme a Windows Azure Pack, dal momento che Windows Azure Pack contiene un'interfaccia grafica per la gestione di SMA. A differenza di Automazione di Azure, SMA richiede un'installazione locale che include server Web per ospitare l'API, un database per contenere i runbook e la configurazione SMA e ruoli di lavoro per runbook per eseguire i processi del runbook. Automazione di Azure offre questi servizi nel cloud e richiede solo di gestire i computer di lavoro runbook ibridi nell'ambiente locale.

Gli utenti SMA già esistenti possono spostare i runbook in Automazione di Azure affinché vengano usati con Hybrid Runbook Workers senza alcuna modifica, presupponendo che eseguano la propria autenticazione per le risorse come illustrato in [Creazione dei runbook per Hybrid Runbook Workers](#creating-runbooks-for-hybrid-runbook-worker).  I runbook in SMA vengono eseguiti nel contesto dell'account del servizio nel server di lavoro, che può garantire tale autenticazione per i runbook.

Per determinare se per le proprie esigenze sia più opportuno ricorrere ad Automazione di Azure con Hybrid Runbook Workers o a Service Management Automation, è possibile usare i criteri seguenti.

* Se è necessaria un'interfaccia di gestione grafica, SMA richiede un'installazione locale di componenti sottostanti connessi a Windows Azure Pack. Sono necessarie altre risorse con costi di manutenzione superiori rispetto ad Automazione di Azure, che richiede solo l'installazione di un agente nei ruoli lavoro per runbook locali. Gli agenti vengono gestiti da Operations Management Suite, riducendo ulteriormente i costi di manutenzione.
* Automazione di Azure archivia i runbook nel cloud e li distribuisce ai computer di ruolo di lavoro ibrido per runbook in locale. Se i criteri di sicurezza non consentono questo comportamento, è consigliabile usare SMA.
* SMA è incluso in System Center e richiede pertanto una licenza di System Center 2012 R2. Automazione di Azure si basa su un modello di sottoscrizione a livelli.
* Automazione di Azure offre funzionalità avanzate, tra cui runbook grafici, non disponibili in SMA.

## <a name="next-steps"></a>Passaggi successivi
* Per altre informazioni sui vari modi per avviare un runbook, vedere [Avvio di un runbook in Automazione di Azure](automation-starting-a-runbook.md).  
* Per comprendere le diverse procedure per l'uso di PowerShell e dei runbook del flusso di lavoro di PowerShell in Automazione di Azure con l'editor di testo, vedere [Modifica di runbook testuali in Automazione di Azure](automation-edit-textual-runbook.md)



<!--HONumber=Jan17_HO2-->


