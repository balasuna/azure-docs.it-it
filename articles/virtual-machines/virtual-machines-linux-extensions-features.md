---
title: "Estensioni della macchina virtuale e funzionalità per Linux | Documentazione Microsoft"
description: "Informazioni sulle estensioni disponibili per le macchine virtuali di Azure, raggruppate in base a ciò che forniscono o migliorano."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 31c6cb19827279995502c68ed6d86d23ef9eacd0
ms.openlocfilehash: 5d98f086b4ccee300b18c8271f940272f618e93e


---
# <a name="virtual-machine-extensions-and-features-for-linux"></a>Estensioni della macchina virtuale e funzionalità per Linux

Le estensioni della macchina virtuale di Azure sono piccole applicazioni che eseguono attività di configurazione e automazione post-distribuzione nelle macchine virtuali di Azure. Ad esempio, se una macchina virtuale richiede l'installazione di software, la protezione antivirus o la configurazione di Docker, è possibile usare un'estensione di questo tipo per completare queste attività. Le estensioni della macchina virtuale di Azure possono essere eseguite tramite l'interfaccia della riga di comando di Azure, PowerShell, i modelli di Azure Resource Manager e il portale di Azure. Le estensioni possono essere unite in bundle con una nuova distribuzione di macchina virtuale o eseguite su un sistema esistente.

Questo documento fornisce una panoramica delle estensioni della macchina virtuale, i prerequisiti per l'uso di queste estensioni di Azure e indicazioni su come rilevare, gestire e rimuovere le estensioni. Questo documento contiene informazioni generali perché sono disponibili molte estensioni della macchina virtuale, ognuna con una configurazione potenzialmente univoca. I dettagli sono disponibili nel documento specifico della singola estensione.

## <a name="use-cases-and-samples"></a>Casi d'uso ed esempi

Sono disponibili numerose estensioni della macchina virtuale di Azure, ognuna con uno specifico caso d'uso. Di seguito sono riportati alcuni esempi:

- Applicare le configurazioni dello stato desiderato tramite PowerShell a una macchina virtuale usando l'estensione DSC per Linux. Per altre informazioni, vedere l'argomento relativo all'[Introduzione al gestore dell'estensione DSC (Desired State Configuration) di Azure](https://github.com/Azure/azure-linux-extensions/tree/master/DSC).
- Configurare il monitoraggio di una macchina virtuale con l'estensione macchina virtuale di Microsoft Monitoring Agent. Per altre informazioni, vedere [Abilitare o disabilitare il monitoraggio delle VM di Azure](virtual-machines-linux-vm-monitoring.md).
- Configurare il monitoraggio dell'infrastruttura di Azure con l'estensione Datadog. Per altre informazioni, vedere il [blog Datadog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Configurare un host Docker in una macchina virtuale di Azure usando l'estensione macchina virtuale Docker. Per altre informazioni, vedere [Estensione macchina virtuale per Docker](virtual-machines-linux-dockerextension.md).

Oltre alle estensioni specifiche del processo, è disponibile un'estensione dello script personalizzata per le macchine virtuali Linux e Windows. L'estensione dello script personalizzata per Linux consente l'esecuzione di qualsiasi script Bash su una macchina virtuale. È utile per la progettazione di distribuzioni di Azure che richiedono una configurazione, in aggiunta a quella offerta dagli strumenti nativi di Azure. Per altre informazioni, vedere [Estensione di script personalizzata per le VM Linux](virtual-machines-linux-extensions-customscript.md).

Per illustrare un esempio d'uso di un'estensione macchina virtuale in una distribuzione di applicazioni end-to-end, vedere [Automazione della distribuzione di applicazioni nelle macchine virtuali di Azure](virtual-machines-linux-dotnet-core-1-landing.md).

## <a name="prerequisites"></a>Prerequisiti

Ogni estensione macchina virtuale può avere un insieme specifico di prerequisiti. Ad esempio, un prerequisito dell'estensione macchina virtuale per Docker è una distribuzione Linux supportata. La documentazione specifica dell'estensione illustra in dettaglio i requisiti delle singole estensioni.

### <a name="azure-vm-agent"></a>Agente VM di Azure

L'agente VM di Azure gestisce le interazioni tra una macchina virtuale di Azure e il controller di infrastruttura di Azure. L'agente VM è responsabile di molti aspetti funzionali della distribuzione e della gestione delle macchine virtuali di Azure, tra cui l'esecuzione delle estensioni della macchina virtuale. L'agente VM di Azure è preinstallato nelle immagini di Azure Marketplace e può essere installato manualmente nei sistemi operativi supportati.

Per informazioni sui sistemi operativi supportati e per le istruzioni di installazione, vedere l'[agente VM di Azure](virtual-machines-linux-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Individuare le estensioni della macchina virtuale

Molte estensioni della macchina virtuale di diverso tipo sono disponibili per l'uso con le macchine virtuali di Azure. Per visualizzare un elenco completo, eseguire il comando seguente con l'interfaccia della riga di comando di Azure, sostituendo il percorso di esempio con quello scelto.

```azurecli
azure vm extension-image list westus
```

## <a name="run-vm-extensions"></a>Eseguire le estensioni della macchina virtuale

Le estensioni della macchina virtuale di Azure possono essere eseguite nelle macchine virtuali esistenti e sono utili quando è necessario apportare modifiche alla configurazione o ripristinare la connettività in una VM già distribuita. Le estensioni della macchina virtuale possono essere anche unite in bundle con le distribuzioni di modelli di Azure Resource Manager. L'uso delle estensioni con i modelli di Resource Manager consente di distribuire e configurare le macchine virtuali di Azure senza l'intervento post-distribuzione.

Per eseguire un'estensione in una macchina virtuale esistente, è possibile usare i metodi seguenti.

### <a name="azure-cli"></a>Interfaccia della riga di comando di Azure

Le estensioni della macchina virtuale di Azure possono essere eseguite in una macchina virtuale esistente usando il comando `azure vm extension set`. Questo esempio esegue l'estensione script personalizzata in una macchina virtuale.

```azurecli
azure vm extension set myResourceGroup myVM CustomScript Microsoft.Azure.Extensions 2.0 \
  --auto-upgrade-minor-version \
  --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

L'output ha un aspetto simile al testo seguente:

```azurecli
info:    Executing command vm extension set
+ Looking up the VM "myVM"
+ Installing extension "CustomScript", VM: "mvVM"
info:    vm extension set command OK
```

### <a name="azure-portal"></a>Portale di Azure

Le estensioni della macchina virtuale possono essere applicate a una macchina virtuale esistente tramite il portale di Azure. A tale scopo, selezionare la macchina virtuale, scegliere **Estensioni**, fare clic su **Aggiungi**. Viene fornito un elenco di estensioni disponibili. Selezionare quella desiderata e seguire le istruzioni nella procedura guidata.

L'immagine seguente illustra l'installazione dell'estensione di script personalizzata per Linux dal portale di Azure.

![Estensione di script personalizzati](./media/virtual-machines-linux-extensions-features/script-extension-linux.jpg)

### <a name="azure-resource-manager-templates"></a>Modelli di Gestione risorse di Azure

Le estensioni della macchina virtuale possono essere aggiunte a un modello di Azure Resource Manager ed eseguite con la distribuzione del modello. Quando si distribuisce un'estensione con un modello, è possibile creare distribuzioni di Azure completamente configurate. Ad esempio, il codice JSON seguente viene eseguito da un modello di Resource Manager che consente di distribuire un set di macchine virtuali con carico bilanciato e un database SQL di Azure, quindi installa un'applicazione .NET Core in ogni VM. L'estensione macchina virtuale gestisce l'installazione del software.

Per altre informazioni, vedere il [modello di Resource Manager](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux) completo.

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
    }
}
```

Per altre informazioni, vedere [Creazione di modelli di Azure Resource Manager con le estensioni della macchina virtuale per Linux](virtual-machines-linux-extensions-authoring-templates.md).

## <a name="secure-vm-extension-data"></a>Proteggere i dati dell'estensione macchina virtuale

Quando si esegue un'estensione macchina virtuale, potrebbe essere necessario includere informazioni riservate, ad esempio le credenziali, i nomi degli account di archiviazione e le chiavi di accesso dell'account di archiviazione. Molte estensioni della macchina virtuale includono una configurazione protetta che crittografa i dati e li decrittografa solo all'interno della macchina virtuale di destinazione. Ogni estensione dispone di uno schema di configurazione protetta specifico, descritto in dettaglio nella documentazione specifica dell'estensione.

L'esempio seguente illustra un'istanza dell'estensione script personalizzata per Linux. Si noti che il comando da eseguire include un insieme di credenziali. In questo esempio, il comando da eseguire non verrà crittografato.


```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ],
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Lo spostamento della proprietà del **comando da eseguire** nella configurazione **protetta** consente di proteggere la stringa di esecuzione.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

## <a name="troubleshoot-vm-extensions"></a>Risoluzione dei problemi relativi alle estensioni della macchina virtuale

Ogni estensione macchina virtuale può richiedere passaggi per la risoluzione dei problemi specifici per l'estensione. Ad esempio, quando si usa l'estensione di script personalizzata, i dettagli sull'esecuzione dello script sono disponibili in locale nella macchina virtuale in cui è stata eseguita l'estensione. Tutti i passaggi per la risoluzione dei problemi specifici dell'estensione sono descritti in dettaglio nella documentazione specifica dell'estensione.

I passaggi per la risoluzione dei problemi seguenti si applicano a tutte le estensioni macchina virtuale.

### <a name="view-extension-status"></a>Visualizzare lo stato dell'estensione

Dopo l'esecuzione di un'estensione macchina virtuale in una macchina virtuale, usare il comando dell'interfaccia della riga di comando seguente di Azure per ottenere lo stato dell'estensione. Sostituire i nomi dei parametri di esempio con i valori desiderati.

```azurecli
azure vm extension get myResourceGroup myVM
```

L'output ha un aspetto simile al testo seguente:

```azurecli
info:    Executing command vm extension get
+ Looking up the VM "myVM"
data:    Publisher                   Name             Version  State
data:    --------------------------  ---------------  -------  ---------
data:    Microsoft.Azure.Extensions  DockerExtension  1.0      Succeeded
info:    vm extension get command OK         :
```

Lo stato di esecuzione dell'estensione è disponibile anche nel portale di Azure. Per visualizzare lo stato di un'estensione, selezionare la macchina virtuale, scegliere **Estensioni** e selezionare l'estensione desiderata.

### <a name="rerun-a-vm-extension"></a>Rieseguire un'estensione macchina virtuale

In alcuni casi potrebbe essere necessario rieseguire un'estensione macchina virtuale. È possibile farlo rimuovendo l'estensione e quindi eseguendo di nuovo l'estensione con il metodo di esecuzione scelto. Per rimuovere un'estensione, eseguire il comando seguente con l'interfaccia della riga di comando di Azure. Sostituire i nomi dei parametri di esempio con i valori desiderati.

```azurecli
azure vm extension set myResourceGroup myVM --uninstall CustomScript Microsoft.Azure.Extensions 2.0
```

È possibile rimuovere un'estensione usando i passaggi seguenti nel portale di Azure:

1. Selezionare una macchina virtuale.
2. Scegliere **Estensioni**.
3. Selezionare l'estensione desiderata.
4. Scegliere **Disinstalla**.

## <a name="common-vm-extension-reference"></a>Riferimento alle estensioni della macchina virtuale comuni
| Nome estensione | Descrizione | Altre informazioni |
| --- | --- | --- |
| Estensione script personalizzata per Linux |Eseguire gli script in una macchina virtuale di Azure |[Estensione script personalizzata per Linux](virtual-machines-linux-extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Estensione per Docker |Installare il daemon Docker per supportare i comandi Docker remoti. |[Estensione macchina virtuale per Docker](virtual-machines-linux-dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Estensione dell'accesso alle macchine virtuali |Ripristinare l'accesso a una macchina virtuale di Azure |[Estensione dell'accesso alle macchine virtuali](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Estensione di Diagnostica di Azure |Gestisce Diagnostica di Azure. |[Estensione di Diagnostica di Azure](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Estensione dell'accesso alla VM di Azure |Gestire gli utenti e le credenziali |[Estensione dell'accesso alla VM per Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |



<!--HONumber=Dec16_HO3-->


