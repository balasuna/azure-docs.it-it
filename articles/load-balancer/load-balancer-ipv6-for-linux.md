---
title: Configurazione di DHCPv6 per VM Linux | Microsoft Docs
description: Come configurare DHCPv6 per VM Linux.
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
keywords: ipv6, azure load balancer, dual stack, ip pubblico, ipv6 nativo, mobili, iot
ms.assetid: b32719b6-00e8-4cd0-ba7f-e60e8146084b
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: cf1eafc7bca5bddeb32f1e1e05e660d6877ed805
ms.openlocfilehash: f14a0473ebcf21e04759cb8eb77eb684c4d9a9cb

---

# <a name="configuring-dhcpv6-for-linux-vms"></a>Configurazione di DHCPv6 per VM Linux

Per alcune delle immagini di macchina virtuale Linux in Azure Marketplace, DHCPv6 non è configurato per impostazione predefinita. Per supportare IPv6, è necessario configurare DHCPv6 all'interno della distribuzione del sistema operativo Linux usato. Le diverse distribuzioni Linux hanno modalità diverse per configurare DHCPv6 perché usano pacchetti differenti.

> [!NOTE]
> Le immagini recenti di SUSE Linux e CoreOS in Azure Marketplace sono state preconfigurate con DHCPv6. Non sono necessarie altre modifiche quando si usano tali immagini.

Questo documento descrive come abilitare DHCPv6 in modo che la macchina virtuale Linux ottenga un indirizzo IPv6.

> [!WARNING]
> La modifica errata dei file di configurazione della rete può causare la perdita dell'accesso alla macchina virtuale. È consigliabile testare le modifiche alla configurazione nei sistemi non di produzione. Le istruzioni riportate in questo articolo sono state testate sulle versioni più recenti delle immagini Linux in Azure Marketplace. Consultare la documentazione per la versione specifica di Linux per istruzioni più dettagliate.

## <a name="ubuntu"></a>Ubuntu

1. Modificare il file `/etc/dhcp/dhclient6.conf` e aggiungere la riga seguente:

        timeout 10;

2. Modificare la configurazione della rete per l'interfaccia eth0 con la configurazione seguente:

   * In **Ubuntu 12.04 e 14.04** modificare il file `/etc/network/interfaces.d/eth0.cfg`
   * In **Ubuntu 16.04**modificare il file `/etc/network/interfaces.d/50-cloud-init.cfg`

        iface eth0 inet6 auto        up sleep 5        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Rinnovare l'indirizzo IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Modificare il file `/etc/dhcp/dhclient6.conf` e aggiungere la riga seguente:

        timeout 10;

2. Modificare il file `/etc/network/interfaces` e aggiungere la configurazione seguente:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Rinnovare l'indirizzo IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Modificare il file `/etc/sysconfig/network` e aggiungere il parametro seguente:

        NETWORKING_IPV6=yes

2. Modificare il file `/etc/sysconfig/network-scripts/ifcfg-eth0` e aggiungere i due parametri seguenti:

        IPV6INIT=yes
        DHCPV6C=yes

3. Rinnovare l'indirizzo IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 e openSUSE 13

Le immagini recenti di SLES e openSUSE in Azure sono state preconfigurate con DHCPv6. Non sono necessarie altre modifiche quando si usano tali immagini. Con una macchina virtuale basata su un'immagine di SUSE personalizzata o precedente, eseguire questa procedura:

1. Installare il pacchetto `dhcp-client` , se necessario:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Modificare il file `/etc/sysconfig/network/ifcfg-eth0` e aggiungere il parametro seguente:

        DHCLIENT6_MODE='managed'

3. Rinnovare l'indirizzo IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 e openSUSE Leap

Le immagini recenti di SLES e openSUSE in Azure sono state preconfigurate con DHCPv6. Non sono necessarie altre modifiche quando si usano tali immagini. Con una macchina virtuale basata su un'immagine di SUSE personalizzata o precedente, eseguire questa procedura:

1. Modificare il file `/etc/sysconfig/network/ifcfg-eth0` e sostituire questo parametro

        #BOOTPROTO='dhcp4'

    con il valore seguente:

        BOOTPROTO='dhcp'

2. Aggiungere il parametro seguente a `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Rinnovare l'indirizzo IPv6:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Le immagini recenti di CoreOS in Azure sono state preconfigurate con DHCPv6. Non sono necessarie altre modifiche quando si usano tali immagini. Con una macchina virtuale basata su un'immagine di CoreOS personalizzata o precedente, eseguire questa procedura:

1. Modificare il file `/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Rinnovare l'indirizzo IPv6:

    ```bash
    sudo systemctl restart systemd-networkd
    ```



<!--HONumber=Nov16_HO3-->


