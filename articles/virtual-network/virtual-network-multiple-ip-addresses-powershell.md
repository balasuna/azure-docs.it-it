---
title: "Più indirizzi IP per le macchine virtuali - PowerShell | Documentazione di Microsoft"
description: "Informazioni su come assegnare più indirizzi IP a una macchina virtuale usando PowerShell | Resource Manager."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2016
ms.author: jdial;annahar
translationtype: Human Translation
ms.sourcegitcommit: 11e490ee3b2d2f216168e827154125807a61a73f
ms.openlocfilehash: 39b45b276c50922878e9918b225f6fa399d42edf


---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-powershell"></a>Assegnare più indirizzi IP alle macchine virtuali usando PowerShell

> [!div class="op_single_selector"]
> * [Portale](virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
> * [CLI](virtual-network-multiple-ip-addresses-cli.md)
>

Una macchina virtuale di Azure può essere associata a una o più interfacce di rete. A ogni scheda di interfaccia di rete possono essere assegnati uno o più indirizzi IP pubblici o privati, statici e dinamici. L'assegnazione di più indirizzi IP a una VM consente di:

* Ospitare più siti Web o servizi con indirizzi IP e certificati SSL diversi in un singolo server.
* Fungere da appliance virtuale di rete, ad esempio un firewall o un servizio di bilanciamento del carico.
* Aggiungere qualsiasi indirizzo IP per qualsiasi scheda di interfaccia di rete a un pool back-end di Azure Load Balancer. In passato, era possibile aggiungere a un pool di back-end solo gli indirizzi IP primari per la scheda di interfaccia di rete primaria. Per altre informazioni su come bilanciare il carico di più configurazioni IP, leggere l'articolo [Load balancing multiple IP configurations](../load-balancer/load-balancer-multiple-ip.md) (Bilanciamento del carico di più configurazioni IP).

Ogni scheda di interfaccia di rete collegata a una macchina virtuale dispone di una o più configurazioni IP associate. A ogni configurazione viene assegnato un indirizzo IP privato statico o dinamico. Ogni configurazione può anche avere una risorsa di indirizzo IP pubblico associata. Una risorsa indirizzo IP pubblico dispone di un indirizzo IP dinamico o statico assegnato. Per altre informazioni sugli indirizzi IP in Azure, leggere l'articolo sugli [indirizzi IP in Azure](virtual-network-ip-addresses-overview-arm.md).

In questo articolo viene illustrato come usare il portale di Azure per assegnare più indirizzi IP a una VM creata tramite il modello di distribuzione Azure Resource Manager. Non è possibile a assegnare più indirizzi IP alle risorse create tramite il modello di distribuzione classica. Per altre informazioni sui modelli di distribuzione di Azure, leggere l'articolo [Understand Azure deployment models](../resource-manager-deployment-model.md) (Informazioni sui modelli di distribuzione di Azure).

[!INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

## <a name="scenario"></a>Scenario
Una macchina virtuale con una singola scheda di interfaccia di rete viene creata e collegata a una rete virtuale. La macchina virtuale richiede tre diversi indirizzi IP *privati* e due indirizzi IP *pubblici*. Gli indirizzi IP vengono assegnati alle configurazioni IP seguenti:

* **IPConfig-1:** assegna un indirizzo IP privato *dinamico* (predefinito) e un indirizzo IP pubblico *statico*.
* **IPConfig-2:** assegna un indirizzo IP privato *statico* e un indirizzo IP pubblico *statico*.
* **IPConfig-3:** assegna un indirizzo IP privato *dinamico* e nessun indirizzo IP pubblico.
  
    ![Più indirizzi IP](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

Le configurazioni IP vengono associate alla scheda di interfaccia di rete al momento della creazione della stessa, mentre la scheda di interfaccia di rete viene collegata alla macchina virtuale al momento della creazione della macchina virtuale. I tipi di indirizzi IP usati per lo scenario sono a scopo illustrativo. È possibile assegnare qualsiasi tipo di assegnazione e indirizzo IP desiderato.

## <a name="a-name--createacreate-a-vm-with-multiple-ip-addresses"></a><a name = "create"></a>Creare una macchina virtuale con più indirizzi IP

La procedura seguente illustra come creare una macchina virtuale di esempio con più indirizzi IP, come descritto nello scenario. Modificare i nomi delle variabili e i tipi di indirizzi IP come richiesto per l'implementazione.

1. Aprire un prompt dei comandi di PowerShell e completare i passaggi rimanenti in questa sezione in una singola sessione di PowerShell. Se PowerShell non è già installato e configurato, completare la procedura disponibile nell'articolo [Come installare e configurare Azure PowerShell](/powershell/azureps-cmdlets-docs) .
2. Registrarsi all'anteprima inviando un messaggio di posta elettronica a [IP multipli](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) con l'ID sottoscrizione e l'uso previsto. Non completare i passaggi rimanenti:
    - Finché non si riceve un messaggio di posta elettronica che comunica che si è stati accettati nell'anteprima.
    - Senza seguire le istruzioni nel messaggio di posta elettronica ricevuto.
3. Completare i passaggi da 1 a 4 dell'articolo [Creare una macchina Virtuale Windows](../virtual-machines/virtual-machines-windows-ps-create.md). Non completare il passaggio 5 per la creazione di interfaccia di rete e risorse IP pubblico. Se si modificano i nomi delle variabili usate in questo articolo, modificare i nomi delle variabili anche nei rimanenti passaggi. Per creare una macchina virtuale Linux, selezionare un sistema operativo Linux invece di Windows.
4. Creare una variabile per archiviare l'oggetto subnet creato nel passaggio 4 (Creare una rete virtuale) dell'articolo Creare una macchina virtule Windows, digitando il comando seguente:

    ```powershell
    $SubnetName = $mySubnet.Name
    $Subnet = $myVnet.Subnets | Where-Object { $_.Name -eq $SubnetName }
    ```
5. Definire le configurazioni IP da assegnare all'interfaccia di rete. È possibile aggiungere, rimuovere o modificare le configurazioni in base alle esigenze. Nello scenario vengono descritte le configurazioni seguenti:

    **IPConfig-1**

    Immettere i comandi seguenti per creare:
    - Una risorsa indirizzo IP pubblico con un indirizzo IP pubblico statico
    - Una configurazione IP con la risorsa indirizzo IP pubblico e un indirizzo IP privato dinamico

    ```powershell
    $myPublicIp1     = New-AzureRmPublicIpAddress -Name "myPublicIp1" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Static
    $IpConfigName1  = "IPConfig-1"
    $IpConfig1      = New-AzureRmNetworkInterfaceIpConfig -Name $IpConfigName1 -Subnet $Subnet -PublicIpAddress $myPublicIp1 -Primary
    ```

    Si noti che `-Primary` consente di passare al comando precedente. Quando si assegnano più configurazioni IP a un'interfaccia di rete, è necessario assegnare una configurazione a *Primary*.

    > [!NOTE]
    > Per gli indirizzi IP pubblici è prevista una tariffa nominale. Per altre informazioni sui prezzi degli indirizzi IP, vedere la pagina [Prezzi per gli indirizzi IP](https://azure.microsoft.com/pricing/details/ip-addresses) . È previsto un limite per il numero di indirizzi IP pubblici che possono essere usati in una sottoscrizione. Per altre informazioni sui limiti, vedere l'articolo [Limiti di Azure](../azure-subscription-service-limits.md#networking-limits).
    >

    **IPConfig-2**

    Modificare il valore di **$IPAddress** variabile che segue a un indirizzo valido, non disponibile nella subnet creata. Per verificare se l'indirizzo 10.0.0.5 è disponibile nella subnet, immettere il comando `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.5 -VirtualNetwork $myVnet`. Se l'indirizzo è disponibile, l'output restituisce *True*. Se non è disponibile, l'output restituisce *False* e un elenco di indirizzi disponibili. Immettere i comandi seguenti per creare una nuova risorsa indirizzo IP pubblico e una nuova configurazione IP con un indirizzo IP pubblico e un indirizzo IP privato statico:
    
    ```powershell
    $IpConfigName2 = "IPConfig-2"
    $IPAddress     = 10.0.0.5
    $myPublicIp2   = New-AzureRmPublicIpAddress -Name "myPublicIp2" -ResourceGroupName $myResourceGroup `
    -Location $location -AllocationMethod Static
    $IpConfig2     = New-AzureRmNetworkInterfaceIpConfig -Name $IpConfigName2 `
    -Subnet $Subnet -PrivateIpAddress $IPAddress -PublicIpAddress $myPublicIp2
    ```

    **IPConfig-3**

    Immettere i comandi seguenti per creare una configurazione IP con un indirizzo IP privato dinamico e nessun indirizzo IP pubblico:

    ```powershell
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet
    ```
6. Creare l'interfaccia di rete usando le configurazioni IP definite nel passaggio precedente, immettendo il comando seguente:

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name myNIC -ResourceGroupName $myResourceGroup `
    -Location $location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```
    > [!NOTE]
    > Sebbene questo articolo assegna tutte le configurazioni IP a una singola scheda di interfaccia di rete, è possibile anche assegnare più configurazioni IP a qualsiasi scheda di interfaccia di rete in una macchina virtuale. Per informazioni su come creare una VM con più interfacce di rete, leggere l'articolo [Creare una macchina virtuale con più schede di interfaccia di rete usando PowerShell](virtual-network-deploy-multinic-arm-ps.md).

7. Completare il passaggio 6 dell'articolo [Creare una macchina virtuale](../virtual-machines/virtual-machines-windows-ps-create.md). 

    > [!WARNING]
    > Il passaggio 6 nell'articolo Creare una macchina virtuale ha esito negativo se:
    > - Nel passaggio 6 di questo articolo, la variabile denominata $myNIC è stata modificata.
    > - I passaggi precedenti di questo articolo e dell'articolo Creare una macchina virtuale non sono ancora stati completati.
    >
8. Immettere il comando seguente per visualizzare le risorse indirizzo IP privato e indirizzo IP pubblico assegnate alla scheda di interfaccia di rete:

    ```powershell
    $myNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
9. Aggiungere gli indirizzi IP privati al sistema operativo della VM completando i passaggi per il proprio sistema operativo indicati nella sezione [Aggiungere indirizzi IP al sistema operativo di una VM](#os-config) in questo articolo. Non aggiungere gli indirizzi IP pubblici al sistema operativo.

## <a name="a-nameaddaadd-ip-addresses-to-a-vm"></a><a name="add"></a>Aggiungere indirizzi IP a una macchina virtuale

È possibile aggiungere indirizzi IP privati e pubblici a una scheda di interfaccia di rete esistente completando la procedura seguente. Gli esempi delle sezioni seguenti presuppongono che si disponga già di una VM con le tre configurazioni IP descritte nello [scenario](#Scenario) di questo articolo, ma questa condizione non è indispensabile.

1. Aprire un prompt dei comandi di PowerShell e completare i passaggi rimanenti in questa sezione in una singola sessione di PowerShell. Se PowerShell non è già installato e configurato, completare la procedura disponibile nell'articolo [Come installare e configurare Azure PowerShell](/powershell/azureps-cmdlets-docs) .
2. Registrarsi all'anteprima inviando un messaggio di posta elettronica a [IP multipli](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) con l'ID sottoscrizione e l'uso previsto. Non completare i passaggi rimanenti:
    - Finché non si riceve un messaggio di posta elettronica che comunica che si è stati accettati nell'anteprima.
    - Senza seguire le istruzioni nel messaggio di posta elettronica ricevuto.
3. Cambiare i "valori" delle variabili $Variables seguenti specificando il nome dell'interfaccia di rete a cui si vogliono aggiungere l'indirizzo IP, il gruppo di risorse e la località in cui esiste la scheda di interfaccia di rete:

    ```powershell
    $NICname         = "myNIC"
    $myResourceGroup = "myResourceGroup"
    $location        = "westcentralus"
    ```

    Se non si conosce il nome dell'interfaccia di rete da modificare, immettere i comandi seguenti e quindi cambiare i valori delle variabili precedenti:

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
4. Creare una variabile e impostarla sull'interfaccia di rete esistente digitando il comando seguente:

    ```powershell
    $myNIC = Get-AzureRmNetworkInterface -Name $NICname -ResourceGroupName $myResourceGroup
    ```
5. Nei comandi seguenti modificare *myVNet* e *mySubnet* con i nomi della rete virtuale e alla subnet a cui la scheda di interfaccia di rete è connessa. Immettere i comandi per recuperare gli oggetti della rete virtuale e della subnet a cui la scheda di interfaccia di rete è connessa:

    ```powershell
    $myVnet = Get-AzureRMVirtualnetwork -Name myVNet -ResourceGroupName $myResourceGroup
    $Subnet = $myVnet.Subnets | Where-Object { $_.Name -eq "mySubnet" }
    ```
    Se non si conosce il nome di rete virtuale o una subnet che la scheda di interfaccia di rete è connesso, immettere il comando seguente:
    ```powershell
    $mynic.IpConfigurations
    ```
    Cercare un testo simile al seguente nell'output restituito:

        Subnet   : {
                     "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"

    In questo output *myVnet* è la rete virtuale e *mySubnet* è la subnet a cui la scheda di interfaccia di rete è connessa.

6. Completare i passaggi in una delle sezioni seguenti, a seconda delle esigenze:

    **Aggiungere un indirizzo IP privato**

    Per aggiungere un indirizzo IP privato a una scheda di interfaccia di rete, è necessario creare una configurazione IP. Il comando seguente crea una configurazione con un indirizzo IP statico 10.0.0.7. Se si desidera aggiungere un indirizzo IP privato dinamico, rimuovere `-PrivateIpAddress 10.0.0.7` prima di immettere il comando. Quando si specifica un indirizzo IP statico, deve essere un indirizzo non usato per la subnet. Si consiglia di verificare l'indirizzo per assicurarsi che sia disponibile tramite il comando `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet`. Se l'indirizzo IP è disponibile, l'output restituisce *True*. Se non è disponibile, l'output restituisce *False* e un elenco di indirizzi disponibili.

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
     $myNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```

    Creare tutte le configurazioni usando nomi di configurazione univoci e indirizzi IP privati (per le configurazioni con indirizzi IP statici).

    Aggiungere l'indirizzo IP privato al sistema operativo della VM completando i passaggi relativi al sistema operativo indicati nella sezione [Aggiungere indirizzi IP al sistema operativo di una VM](#os-config) di questo articolo.

    **Aggiungere un indirizzo IP pubblico**

    Per aggiungere un indirizzo IP pubblico è necessario associare una risorsa indirizzo IP pubblico a una configurazione IP nuova o esistente. Completare i passaggi in una delle sezioni che seguono, a seconda del caso.

    > [!NOTE]
    > Per gli indirizzi IP pubblici è prevista una tariffa nominale. Per altre informazioni sui prezzi degli indirizzi IP, vedere la pagina [Prezzi per gli indirizzi IP](https://azure.microsoft.com/pricing/details/ip-addresses) . È previsto un limite per il numero di indirizzi IP pubblici che possono essere usati in una sottoscrizione. Per altre informazioni sui limiti, vedere l'articolo [Limiti di Azure](../azure-subscription-service-limits.md#networking-limits).
    >

    **Associare la risorsa indirizzo IP pubblico a una nuova configurazione IP**
    
    Ogni volta che si aggiunge un indirizzo IP pubblico a una nuova configurazione IP, è necessario aggiungere anche un indirizzo IP privato, perché tutte le configurazioni IP devono avere un indirizzo IP privato. È possibile aggiungere una risorsa indirizzo IP pubblico esistente o crearne una nuova. Per crearne una nuova, usare il comando seguente:
    
    ```powershell
    $myPublicIp3   = New-AzureRmPublicIpAddress -Name "myPublicIp3" -ResourceGroupName $myResourceGroup `
    -Location $location -AllocationMethod Static
    ```

    Per creare una nuova configurazione IP con un indirizzo IP privato dinamico e la risorsa indirizzo IP pubblico *myPublicIp3* associata, inserire il comando seguente:

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
     $myNIC -Subnet $Subnet -PublicIpAddress $myPublicIp3
    ```

    **Associare la risorsa indirizzo IP pubblico a una configurazione IP esistente**

    Una risorsa indirizzo IP pubblico può essere associata a una configurazione IP cui non ne sia associata alcuna. È possibile stabilire se una configurazione IP dispone di un indirizzo IP pubblico associato immettendo il comando seguente:

    ```powershell
    $myNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```

    Cercare una riga simile a quella che segue l'output restituito:

        Name       PrivateIpAddress PublicIpAddress                                           Primary
        ----       ---------------- ---------------                                           -------
        IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
        IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
        IpConfig-3 10.0.0.6                                                                     False

    Poiché la colonna **PublicIpAddress** per *IpConfig-3* è vuota, nessuna risorsa di indirizzo IP pubblico è attualmente associata. È possibile aggiungere una risorsa indirizzo IP pubblico esistente a IpConfig-3 o immettere il comando seguente per crearne una:

    ```powershell
    $myPublicIp3   = New-AzureRmPublicIpAddress -Name "myPublicIp3" -ResourceGroupName $myResourceGroup `
    -Location $location -AllocationMethod Static
    ```

    Immettere il comando seguente per associare la risorsa indirizzo IP pubblico alla configurazione IP esistente denominata *IpConfig-3*:
    
    ```powershell
    Set-AzureRmNetworkInterfaceIpConfig -Name IpConfig-3 -NetworkInterface $mynic -Subnet $Subnet -PublicIpAddress $myPublicIp3
    ```

7. Configurare l'interfaccia di rete con la configurazione IP immettendo il comando seguente:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $myNIC
    ```

8. Visualizzare le risorse indirizzo IP privato e indirizzo IP pubblico assegnate alla scheda di interfaccia di rete immettendo il comando seguente:

    ```powershell   
    $myNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
9. Aggiungere l'indirizzo IP privato al sistema operativo della VM completando i passaggi relativi al sistema operativo indicati nella sezione [Aggiungere indirizzi IP al sistema operativo di una VM](#os-config) di questo articolo. Non aggiungere l'indirizzo IP pubblico al sistema operativo.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]


<!--HONumber=Dec16_HO1-->


