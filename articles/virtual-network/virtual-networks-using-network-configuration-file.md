---
title: Configurare una rete virtuale usando un file di configurazione di rete
description: 'Istruzioni per esportare e importare un file di configurazione di rete nel portale di gestione di Azure per creare o modificare reti virtuali. '
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8a29f94cf0b6f8da70a5a18dde1dcbdca68bf284


---
# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>Configurare una rete virtuale usando un file di configurazione di rete
È possibile configurare una rete virtuale (VNet) tramite il portale di gestione di Azure o utilizzando un file di configurazione di rete.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Creazione e modifica di un file di configurazione di rete
Il modo più semplice per creare un file di configurazione di rete consiste nell'esportare le impostazioni di rete da una configurazione di rete virtuale esistente, quindi modificare il file in modo da contenere le impostazioni che si vogliono configurare per le reti virtuali.

Per modificare il file di configurazione di rete, è sufficiente aprire il file, apportare le modifiche appropriate e salvarlo. È possibile usare qualsiasi editor *xml* per apportare modifiche al file di configurazione di rete. 

È consigliabile attenersi alle istruzioni per le [impostazioni dello schema del file di configurazione](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

Azure considera una subnet con un elemento distribuito come **in uso**. Quando una subnet è in uso, non può essere modificata. Prima di apportare modifiche, spostare qualsiasi elemento distribuito alla subnet in una subnet diversa che non è sottoposta a modifiche.   Vedere [Spostare una macchina virtuale o un'istanza del ruolo in un'altra subnet](virtual-networks-move-vm-role-to-subnet.md).

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>Esportare e importare impostazioni di rete virtuale nel portale di gestione
È possibile importare ed esportare le impostazioni di configurazione di rete contenute nel file di configurazione di rete usando PowerShell o il portale di gestione. Le istruzioni riportate di seguito consentono di eseguire l'esportazione e l'importazione tramite il portale di gestione. 

### <a name="to-export-your-network-settings"></a>Per esportare le impostazioni di rete
Quando si esporta, tutte le impostazioni per le reti virtuali nella sottoscrizione verranno scritte in un file XML. 

1. Accedere al **portale di gestione**.
2. Nel portale di gestione, nella parte inferiore della pagina **Reti**, fare clic su **Esporta**. 
3. Nella finestra **Esporta configurazione di rete** , verificare di aver selezionato la sottoscrizione per la quale si vogliono esportare le impostazioni di rete. Quindi, fare clic sul segno di spunta in basso a destra. 
4. Quando richiesto, salvare il file *NetworkConfig.xml* nel percorso desiderato.

### <a name="to-import-your-network-settings"></a>Per importare le impostazioni di rete
1. Nel **Portale di gestione**, nel riquadro di spostamento in basso a sinistra, fare clic su **Nuovo**.
2. Fare clic su **Servizi di rete** -> **Rete virtuale** -> **Importa configurazione**.
3. Nella pagina **Importa file di configurazione della rete** passare al file di configurazione della rete e quindi fare clic su **Avanti**.
4. Nella pagina **Compilazione della rete** verranno visualizzate le informazioni sulla schermata che mostra quali sezioni della configurazione di rete verranno modificate o create. Se le modifiche sembrano corrette, fare clic sul segno di spunta per continuare con l'aggiornamento o la creazione della rete virtuale. 




<!--HONumber=Nov16_HO3-->


