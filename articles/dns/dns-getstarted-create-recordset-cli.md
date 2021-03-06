---
title: Creare record DNS usando l&quot;interfaccia della riga di comando di Azure | Documentazione Microsoft
description: Come creare i record host per DNS di Azure. Configurare i set di record e i record usando CLI
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 02b897d3-e83b-4257-b96d-5c29aa59e843
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 18a21cdc0f9641356dfaf6f6d93edfcac11af210
ms.openlocfilehash: 790af1544ed86155f5f864f3914b5fd1c4f42f4b

---

# <a name="create-dns-records-using-the-azure-cli"></a>Creare record DNS usando l'interfaccia della riga di comando di Azure

> [!div class="op_single_selector"]
> * [Portale di Azure](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Interfaccia della riga di comando di Azure](dns-getstarted-create-recordset-cli.md)

Questo articolo illustra la creazione di record e set di record usando l'interfaccia della riga di comando di Azure.

## <a name="introduction"></a>Introduzione

Prima di creare record DNS nel servizio DNS di Azure, è necessario comprendere il modo in cui quest'ultimo organizza i record DNS nei set di record DNS.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Per altre informazioni sui record DNS nel servizio DNS di Azure, vedere [Zone e record DNS](dns-zones-records.md).

## <a name="create-a-record-set-and-record"></a>Creare un set di record e un record

In questa sezione viene descritto come creare record DNS in DNS di Azure. Negli esempi si suppone che l'utente abbia già [installato l'interfaccia della riga di comando di Azure, effettuato l'accesso e creato una zona DNS](dns-getstarted-create-dnszone-cli.md).

Gli esempi in questa pagina usano tutti il tipo di record DNS "A". Per altri tipi di record e maggiori informazioni su come gestire i record DNS e i set di record, vedere [Gestire record e set di record DNS con l'interfaccia della riga di comando di Azure](dns-operations-recordsets-cli.md).

## <a name="create-a-dns-record"></a>Creare un record DNS

Per creare un record DNS, usare il comando `azure network dns record-set add-record`. Per altre informazioni, vedere `azure network dns record-set add-record -h`.

Quando si crea un record è necessario specificare il nome del gruppo di risorse, della zona e del set di record, il tipo di record e i dettagli del record da creare.

Il comando crea il set di record, se non esiste già. In caso contrario, il comando aggiunge il record specificato al set di record esistente. 

Se viene creato un nuovo set di record, per la durata (TTL) viene usato un valore predefinito di 3600. Per istruzioni sull'uso di durate diverse, vedere [Gestire record e set di record DNS con l'interfaccia della riga di comando](dns-operations-recordsets-cli.md).

L'esempio seguente mostra come creare un record "A" denominato *www* nella zona *contoso.com* nel gruppo di risorse *MyResourceGroup*. L'indirizzo IP del record "A" è *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Per creare un set di record nel dominio radice, in questo caso "contoso.com", usare il nome record "@",, incluse le virgolette:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

I parametri usati per specificare i dati del record variano a seconda del tipo di record. Per un record di tipo "A", ad esempio, specificare l'indirizzo IPv4 con il parametro `-a <IPv4 address>`. Vedere `azure network dns record-set add-record -h` per elencare i parametri per altri tipi di record. Per esempi sui singoli tipi di record, vedere [Gestire record e set di record DNS con l'interfaccia della riga di comando](dns-operations-recordsets-cli.md).


## <a name="verify-name-resolution"></a>Verificare la risoluzione dei nomi

È possibile verificare che i record DNS siano presenti sui server di nomi DNS di Azure usando gli strumenti DNS come nslookup, DIG o il [cmdlet di PowerShell Resolve-DnsName](https://technet.microsoft.com/library/jj590781.aspx).

Se non è stato ancora delegato il dominio per usare la nuova zona in DNS di Azure, sarà necessario [indirizzare la query DNS direttamente a uno dei server dei nomi per la zona](dns-getstarted-create-dnszone.md#test-name-servers). Assicurarsi di sostituire i valori corretti per la propria zona di record nel comando seguente.

```
nslookup
> set type=A
> server ns1-01.azure-dns.com
> www.contoso.com

Server:  ns1-01.azure-dns.com
Address:  40.90.4.1

Name:    www.contoso.com
Address:  1.2.3.4
```

## <a name="next-steps"></a>Passaggi successivi

Informazioni su come [delegare il nome di dominio ai server dei nomi DNS di Azure](dns-domain-delegation.md)

Informazioni su come [gestire le zone DNS usando l'interfaccia della riga di comando di Azure](dns-operations-dnszones-cli.md).

Informazioni su come [gestire i record e i set di record DNS usando l'interfaccia della riga di comando di Azure](dns-operations-recordsets-cli.md).




<!--HONumber=Dec16_HO3-->


