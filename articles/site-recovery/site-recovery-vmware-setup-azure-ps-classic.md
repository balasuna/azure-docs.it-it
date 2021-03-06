---
title: " Gestire un server di elaborazione in esecuzione in Azure (versione classica) | Documentazione Microsofts"
description: Questo articolo descrive come configurare un server di elaborazione failback (versione classica) in Azure.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 2/2/2017
ms.author: anoopkv
translationtype: Human Translation
ms.sourcegitcommit: bb24e1c79f6a77ec157fa05a8a212109ce8ae7ad
ms.openlocfilehash: cc2d4d92f7d900f8dc231e11b65acf4b742ed893

---

# <a name="manage-a-process-server-running-in-azure-classic"></a>Gestire un server di elaborazione in esecuzione in Azure (versione classica)
> [!div class="op_single_selector"]
> * [Azure versione classica](./site-recovery-vmware-setup-azure-ps-classic.md)
> * [Gestione risorse](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

Durante il failback è consigliabile distribuire un server di elaborazione in Azure se è presente una latenza elevata tra la rete virtuale di Azure e la rete locale. Questo articolo descrive come impostare, configurare e gestire i server di elaborazione in esecuzione in Azure.

> [!NOTE]
> Questo articolo è utile se è stato usato **Resource Manager** come modello di distribuzione per le macchine virtuali durante il failover. Se è stato usato il modello di distribuzione **classica**, seguire i passaggi indicati in [Come impostare e configurare un server di elaborazione failback (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [site-recovery-vmware-process-server-prereq](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a>Distribuire un server di elaborazione in Azure

1. In Azure Marketplace creare una macchina virtuale usando **Microsoft Azure Site Recovery Process Server V2** </br>
    ![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)
2. Assicurarsi di selezionare il modello di distribuzione **classica** </br>
  ![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)
3. In Creazione guidata macchina virtuale > Impostazioni di base, selezionare la sottoscrizione e la posizione in cui è stato eseguito il failover delle macchine virtuali.</br>
  ![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)
4. Assicurarsi che la macchina virtuale sia connessa alla rete virtuale di Azure a cui è collegata la macchina virtuale della quale è stato eseguito il failover.</br>
  ![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)
5. Dopo aver eseguito il provisioning della macchina virtuale del server di elaborazione è necessario accedere e registrarla con il server di configurazione.

> [!NOTE]
> Per poter usare questo server di elaborazione per il failback è necessario registrarlo con il server di configurazione locale.

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>Registrazione del server di elaborazione in esecuzione in Azure con un server di configurazione locale

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-the-process-server-to-latest-version"></a>Aggiornamento del server di elaborazione alla versione più recente.

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-the-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a>Annullamento della registrazione del server di elaborazione in esecuzione in Azure con un server di configurazione locale

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]



<!--HONumber=Feb17_HO1-->


