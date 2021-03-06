---
title: 'Connettere Raspberry Pi (C) ad Azure IoT: lezione 3: Eseguire l&quot;esempio | Documentazione Microsoft'
description: Distribuire ed eseguire un&quot;applicazione di esempio nel dispositivo Raspberry Pi 3 che invia messaggi all&quot;hub IoT e fa lampeggiare il LED.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: lampeggiamento led cloud pi, lampeggiamento led dal cloud
ms.assetid: e38df29f-f77f-435f-9add-46814297564f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 985ba895bfcad1611bebe80a6c2fc8ffed780da5


---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Eseguire un'applicazione di esempio per inviare messaggi da dispositivo a cloud
## <a name="what-you-will-do"></a>Contenuto dell'esercitazione
Questo articolo illustra come distribuire ed eseguire nel dispositivo Raspberry Pi 3 un'applicazione di esempio che invia messaggi all'hub IoT. In caso di problemi, cercare le soluzioni nella pagina sulla [risoluzione dei problemi](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Contenuto dell'esercitazione
Si apprenderà come usare lo strumento gulp per distribuire ed eseguire l'applicazione Node.js di esempio nel dispositivo Pi.

## <a name="what-you-need"></a>Elementi necessari
* Prima di iniziare questa attività è necessario aver completato [Creare un'app per le funzioni di Azure e un account di archiviazione di Azure per elaborare e archiviare i messaggi dell'hub IoT](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Ottenere le stringhe di connessione dell'hub IoT e del dispositivo
La stringa di connessione del dispositivo viene usata dal dispositivo Pi per connettersi all'hub IoT. La stringa di connessione dell'hub IoT viene usata per connettersi al registro delle identità nell'hub IoT per gestire i dispositivi che possono connettersi all'hub IoT. 

* Elencare tutti gli hub IoT nel gruppo di risorse eseguendo questo comando dell'interfaccia della riga di comando di Azure:

```bash
az iot hub list -g iot-sample --query [].name
```

Usare `iot-sample` come valore di `{resource group name}` se non si è modificato il valore.

* Ottenere la stringa di connessione dell'hub IoT eseguendo il comando seguente dell'interfaccia della riga di comando di Azure:

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

`{my hub name}` è il nome specificato quando è stato creato l'hub IoT ed è stato registrato il dispositivo Pi.

* Ottenere la stringa di connessione del dispositivo usando il comando seguente:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

Usare `myraspberrypi` come valore di `{device id}` se non si è modificato il valore.

## <a name="configure-the-device-connection"></a>Configurare la connessione del dispositivo
1. Inizializzare il file di configurazione usando i comandi seguenti:
   
   ```bash
   npm install
   gulp init
   ```

> [!NOTE]
> Se non è stato fatto nella lezione 1, eseguire anche il comando **gulp install-tools**.

2. Aprire il file di configurazione del dispositivo `config-raspberrypi.json` in Visual Studio Code usando il comando seguente:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
   ![config.json](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. Sostituire i valori seguenti nel file `config-raspberrypi.json`:
   
   * Sostituire **[device hostname or IP address]** con l'indirizzo IP o il nome host del dispositivo ottenuto da `device-discovery-cli` o con il valore ereditato quando è stato configurato il dispositivo.
   * Sostituire **[IoT device connection string]** con il valore di `device connection string` ottenuto.
   * Sostituire **[IoT hub connection string]** con il valore di `iot hub connection string` ottenuto.

> [!NOTE]
> Non è necessario specificare `azure_storage_connection_string` in questo articolo. Mantenerlo invariato.

Aggiornare il file `config-raspberrypi.json` per poter distribuire l'applicazione di esempio dal computer.

## <a name="deploy-and-run-the-sample-application"></a>Distribuire ed eseguire l'applicazione di esempio
Distribuire ed eseguire l'applicazione di esempio nel dispositivo Pi eseguendo questo comando:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a>Verificare il funzionamento dell'applicazione di esempio
Il LED connesso al dispositivo Pi dovrebbe lampeggiare ogni due secondi. Ogni volta che il LED lampeggia, l'applicazione di esempio invia un messaggio all'hub IoT e verifica se il messaggio è stato inviato all'hub IoT. Ogni messaggio ricevuto dall'hub IoT viene stampato nella finestra della console. L'applicazione di esempio termina automaticamente dopo l'invio di 20 messaggi.

![Applicazione di esempio con messaggi inviati e ricevuti](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run_c.png)

## <a name="summary"></a>Riepilogo
È stata distribuita ed eseguita la nuova applicazione di esempio per il lampeggiamento nel dispositivo Pi per l'invio di messaggi da dispositivo a cloud all'hub IoT. È ora possibile monitorare i messaggi mentre vengono scritti nell'account di archiviazione.

## <a name="next-steps"></a>Passaggi successivi
[Leggere i messaggi con salvataggio permanente in Archiviazione di Azure](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md)




<!--HONumber=Jan17_HO4-->


