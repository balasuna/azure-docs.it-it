---
title: Introduzione all&quot;hub IoT di Azure per Node.js | Microsoft Docs
description: "Come inviare messaggi da un dispositivo a un hub IoT cloud di Azure tramite gli SDK IoT di Azure per Node.js. È necessario creare un&quot;app di simulazione del dispositivo per inviare messaggi, un&quot;app di servizio per registrare il dispositivo nel registro di identità e un&quot;app di servizio per leggere i messaggi dal dispositivo al cloud nell&quot;hub IoT."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 56618522-9a31-42c6-94bf-55e2233b39ac
ms.service: iot-hub
ms.devlang: javascript
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2e4220bedcb0091342fd9386669d523d4da04d1c
ms.openlocfilehash: 5d005e3259333f79b9b9852e325864745ee54b84


---
# <a name="get-started-with-azure-iot-hub-node"></a>Introduzione all'hub IoT di Azure per Node.js
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Al termine di questa esercitazione si avranno tre app console Node.js:

* **CreateDeviceIdentity.js**, che crea un'identità del dispositivo e la chiave di sicurezza associata per connettere l'app per dispositivo simulato.
* **ReadDeviceToCloudMessages.js**, che visualizza i dati di telemetria inviati dall'app per dispositivo simulato.
* **SimulatedDevice.js**, che si connette all'hub IoT con l'identità del dispositivo creata in precedenza e invia un messaggio di telemetria ogni secondo usando il protocollo MQTT.

> [!NOTE]
> L'articolo [Azure IoT SDK][lnk-hub-sdks] offre informazioni sui vari Azure IoT SDK che è possibile usare per compilare applicazioni da eseguire nei dispositivi e il backend della soluzione.
> 
> 

Per completare l'esercitazione, sono necessari gli elementi seguenti:

* Node.js 0.10.x o versione successiva.
* Un account Azure attivo. Se non si ha un account, è possibile creare un [account gratuito][lnk-free-trial] in pochi minuti.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

L'hub IoT è stato creato. Ora sono disponibili il nome host e la stringa di connessione dell'hub IoT necessari per completare il resto di questa esercitazione.

## <a name="create-a-device-identity"></a>Creare un'identità del dispositivo
In questa sezione si scriverà un'app console Node.js che crea un'identità del dispositivo nel registro delle identità dell'hub IoT. Un dispositivo non può connettersi all'hub IoT a meno che non abbia una voce nel registro di identità. Per altre informazioni, vedere la sezione **Registro di identità** della [Guida per gli sviluppatori dell'hub IoT][lnk-devguide-identity]. Quando si esegue questa app console vengono generati un ID dispositivo univoco e una chiave con cui il dispositivo può identificarsi quando invia messaggi da dispositivo a cloud all'hub IoT.

1. Creare una nuova cartella vuota denominata **createdeviceidentity**. Nella cartella **createdeviceidentity** creare un file package.json eseguendo questo comando al prompt dei comandi. Accettare tutte le impostazioni predefinite:
   
    ```
    npm init
    ```
2. Eseguire questo comando al prompt dei comandi nella cartella **createdeviceidentity** per installare il pacchetto SDK del servizio **azure-iothub**:
   
    ```
    npm install azure-iothub --save
    ```
3. Con un editor di testo creare un file **CreateDeviceIdentity.js** nella cartella **createdeviceidentity**.
4. Aggiungere l'istruzione `require` seguente all'inizio del file **CreateDeviceIdentity.js** :
   
    ```
    'use strict';
   
    var iothub = require('azure-iothub');
    ```
5. Aggiungere il codice seguente al file **CreateDeviceIdentity.js** e sostituire il valore del segnaposto con la stringa di connessione dell'hub IoT creato nella sezione precedente: 
   
    ```
    var connectionString = '{iothub connection string}';
   
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```
6. Aggiungere il codice seguente per creare una definizione di dispositivo nel registro di identità dell'hub IoT. .Se l'ID del dispositivo non esiste nel registro delle identità dei dispositivi il codice crea un dispositivo, in caso contrario restituisce la chiave di quello esistente:
   
    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });
   
    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device ID: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.symmetricKey.primaryKey);
      }
    }
    ```
7. Salvare e chiudere il file **CreateDeviceIdentity.js** .
8. Per eseguire l'applicazione **createdeviceidentity** , eseguire questo comando al prompt dei comandi nella cartella createdeviceidentity:
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. Prendere nota dei valori di **Device ID** e **Device key**, che saranno necessari più avanti quando si creerà un'applicazione che si connette all'hub IoT come dispositivo.

> [!NOTE]
> Il registro di identità dell'hub IoT archivia solo le identità del dispositivo per abilitare l'accesso sicuro all'hub. Archivia le chiavi e gli ID dispositivo da usare come credenziali di sicurezza e un flag di abilitazione/disabilitazione che consente di disabilitare l'accesso per un singolo dispositivo. Se l'applicazione deve archiviare altri metadati specifici del dispositivo, dovrà usare un archivio specifico dell'applicazione. Per altre informazioni, vedere la [Guida per gli sviluppatori dell'hub IoT][lnk-devguide-identity].
> 
> 

## <a name="receive-device-to-cloud-messages"></a>Ricezione di messaggi da dispositivo a cloud
In questa sezione si creerà un'app console di Node.js che legge i messaggi da dispositivo a cloud dall'hub IoT. L'hub IoT espone un endpoint compatibile con l'[hub eventi][lnk-event-hubs-overview] per abilitare la lettura dei messaggi dispositivo a cloud. Per semplicità, questa esercitazione crea un lettore di base non adatto per una distribuzione con velocità effettiva elevata. L'esercitazione [Elaborare messaggi dispositivo a cloud][lnk-process-d2c-tutorial] illustra come elaborare i messaggi dispositivo a cloud su vasta scala. L'esercitazione [Introduzione all'hub eventi][lnk-eventhubs-tutorial] offre altre informazioni su come elaborare i messaggi da hub eventi ed è applicabile agli endpoint compatibili con l'hub eventi dell'hub IoT.

> [!NOTE]
> L'endpoint compatibile con Hub eventi per la lettura di messaggi da dispositivo a cloud usa sempre il protocollo AMQP.
> 
> 

1. Creare una cartella vuota denominata **readdevicetocloudmessages**. Nella cartella **readdevicetocloudmessages** creare un file package.json immettendo il comando seguente al prompt dei comandi. Accettare tutte le impostazioni predefinite:
   
    ```
    npm init
    ```
2. Eseguire questo comando al prompt dei comandi nella cartella **readdevicetocloudmessages** per installare il pacchetto **azure-event-hubs**:
   
    ```
    npm install azure-event-hubs --save
    ```
3. Con un editor di testo creare un file **ReadDeviceToCloudMessages.js** nella cartella **readdevicetocloudmessages**.
4. Aggiungere le istruzioni `require` seguenti all'inizio del file **ReadDeviceToCloudMessages.js** :
   
    ```
    'use strict';
   
    var EventHubClient = require('azure-event-hubs').Client;
    ```
5. Aggiungere la dichiarazione di variabile seguente e sostituire il valore del segnaposto con la stringa di connessione per l'hub IoT:
   
    ```
    var connectionString = '{iothub connection string}';
    ```
6. Aggiungere le due funzioni seguenti che stampano l'output nella console:
   
    ```
    var printError = function (err) {
      console.log(err.message);
    };
   
    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```
7. Aggiungere il codice seguente per creare **EventHubClient**, aprire la connessione all'hub IoT e creare un ricevitore per ogni partizione. Questa applicazione usa un filtro quando crea un ricevitore, in modo che il ricevitore legga i messaggi inviati all'hub IoT soltanto dopo l'avvio dell'esecuzione del ricevitore. Questo filtro è utile in un ambiente di test perché consente di visualizzare solo il set di messaggi corrente. In un ambiente di produzione, il codice deve verificare di avere elaborato tutti i messaggi. Per altre informazioni, vedere l'esercitazione [Elaborare messaggi da dispositivo a cloud dell'hub IoT][lnk-process-d2c-tutorial]:
   
    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```
8. Salvare e chiudere il file **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Creare un'app di dispositivo simulato
In questa sezione si creerà un'app console Node.js che simula un dispositivo che invia messaggi da dispositivo a cloud a un hub IoT.

1. Creare una cartella vuota denominata **simulateddevice**. Nella cartella **simulateddevice** creare un file package.json eseguendo questo comando al prompt dei comandi. Accettare tutte le impostazioni predefinite:
   
    ```
    npm init
    ```
2. Eseguire questo comando al prompt dei comandi nella cartella **simulateddevice** per installare il pacchetto SDK per dispositivi **azure-iot-device** e il pacchetto **azure-iot-device-mqtt**:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Con un editor di testo creare un file **SimulatedDevice.js** nella cartella **simulateddevice**.
4. Aggiungere le istruzioni `require` seguenti all'inizio del file **SimulatedDevice.js** :
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. Aggiungere una variabile **connectionString** e usarla per creare un'istanza **Client**. Sostituire il valore di **{youriothostname}** con il nome dell'hub IoT creato nella sezione *Creare un hub IoT* . Sostituire **{yourdevicekey}** con il valore della chiave del dispositivo generato nella sezione *Creare un'identità del dispositivo* :
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```
6. Aggiungere la funzione seguente per visualizzare l'output dell'applicazione:
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Creare un callback e usare la funzione **setInterval** per inviare un messaggio all'hub IoT ogni secondo:
   
    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
   
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
8. Aprire la connessione all'hub IoT e iniziare a inviare messaggi:
   
    ```
    client.open(connectCallback);
    ```
9. Salvare e chiudere il file **SimulatedDevice.js** .

> [!NOTE]
> Per semplicità, in questa esercitazione non si implementa alcun criterio di ripetizione dei tentativi. Nel codice di produzione è consigliabile implementare criteri per i tentativi, ad esempio un backoff esponenziale, come illustrato nell'articolo di MSDN [Transient Fault Handling][lnk-transient-faults] (Gestione degli errori temporanei).
> 
> 

## <a name="run-the-apps"></a>Eseguire le app
A questo punto è possibile eseguire le app.

1. Eseguire questo comando al prompt dei comandi nella cartella **readdevicetocloudmessages** per iniziare il monitoraggio dell'hub IoT:
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![App del servizio hub IoT per Node.js per monitorare i messaggi dispositivo a cloud][7]
2. Eseguire questo comando al prompt dei comandi nella cartella **simulateddevice** per iniziare a inviare dati di telemetria all'hub IoT:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![App del dispositivo hub IoT per Node.js per inviare i messaggi da dispositivo a cloud][8]
3. Il riquadro **Utilizzo** nel [portale di Azure][lnk-portal] mostra il numero di messaggi inviati all'hub IoT:
   
    ![Riquadro Utilizzo del portale di Azure con il numero dei messaggi inviati all'hub IoT][43]

## <a name="next-steps"></a>Passaggi successivi
In questa esercitazione è stato configurato un nuovo hub IoT nel Portale di Azure ed è stata quindi creata un'identità del dispositivo nel registro di identità dell'hub IoT. Questa identità del dispositivo è stata usata per consentire all'app per dispositivo simulato di inviare all'hub IoT messaggi dispositivo a cloud. È stata anche creata un'app che visualizza i messaggi ricevuti dall'hub IoT. 

Per altre informazioni sulle attività iniziali con l'hub IoT e per esplorare altri scenari IoT, vedere:

* [Connessione del dispositivo][lnk-connect-device]
* [Introduzione alla gestione dei dispositivi][lnk-device-management]
* [Introduzione all'IoT SDK per gateway][lnk-gateway-SDK]

Per informazioni sull'estensione della soluzione IoT e l'elaborazione di messaggi dispositivo a cloud su vasta scala, vedere l'esercitazione [Elaborare messaggi dispositivo a cloud][lnk-process-d2c-tutorial].

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Dec16_HO3-->


