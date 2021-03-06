---
title: Procedure consigliate per il modello di Resource Manager | Microsoft Docs
description: Linee guida per la semplificazione dei modelli di Azure Resource Manager.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 311eeee882e64bc9fb2e7c1ecb4706de351e7753
ms.openlocfilehash: c774a249d37c489ad53665ad6432122c8fdd1c32


---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Procedure consigliate per la creazione di modelli di Azure Resource Manager
Le linee guida seguenti consentono di creare modelli di Resource Manager affidabili e facili da usare e vengono fornite solo come suggerimenti e non requisiti assoluti. Lo scenario potrebbe richiedere variazioni rispetto a queste linee guida.

## <a name="resource-names"></a>Nomi di risorse
In genere vengono usati tre tipi di nomi di risorse:

1. Nomi di risorse che devono essere univoci.
2. Nomi di risorse che non devono necessariamente essere univoci, ma che si desidera identifichino il contesto.
3. Nomi di risorse che possono essere generici.

Per informazioni sulla creazione di una convenzione di denominazione, vedere [Infrastructure naming guidelines](../virtual-machines/virtual-machines-windows-infrastructure-naming-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)(Linee guida di denominazione di un'infrastruttura). Per informazioni sulle restrizioni relative ai nomi di risorse, vedere [Recommended naming conventions for Azure resources](../guidance/guidance-naming-conventions.md)(Convenzioni di denominazione consigliate per le risorse di Azure).

### <a name="unique-resource-names"></a>Nomi di risorse univoci
È necessario fornire un nome univoco per qualsiasi tipo di risorsa con un endpoint di accesso ai dati. Alcuni tipi comuni che richiedono un nome univoco includono:

* Account di archiviazione
* Sito Web
* Server SQL
* Insieme di credenziali delle chiavi
* Cache Redis
* Account Batch
* Gestione traffico
* Servizio di ricerca
* Cluster HDInsight

I nomi di account di archiviazione devono essere formati da lettere minuscole, un massimo di 24 caratteri e non devono includere alcun trattino.

Se si specifica un parametro per questi nomi di risorse, è necessario individuare un nome univoco durante la distribuzione. In alternativa, è possibile creare una variabile che usi la funzione [uniqueString()](resource-group-template-functions.md#uniquestring) per generare un nome. È spesso opportuno aggiungere un prefisso o un suffisso al risultato di **uniqueString** per poter determinare più facilmente il tipo di risorsa in base al nome. È ad esempio possibile generare un nome univoco per un account di archiviazione con la variabile seguente:

    "variables": {
        "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
    }

### <a name="resource-names-for-identification"></a>Nomi di risorse per l'identificazione
Per denominare i tipi di risorse di cui non è necessario garantire l'univocità, è sufficiente fornire un nome che identifichi il contesto e il tipo di risorsa. È opportuno attribuire un nome descrittivo che ne faciliti il riconoscimento in un elenco di nomi di risorse. Se è necessario modificare il nome di risorsa durante le distribuzioni, usare un parametro per il nome:

    "parameters": {
        "vmName": { 
            "type": "string",
            "defaultValue": "demoLinuxVM",
            "metadata": {
                "description": "The name of the VM to create."
            }
        }
    }

Se non è necessario passare un nome durante la distribuzione, usare una variabile: 

    "variables": {
        "vmName": "demoLinuxVM"
    }

In alternativa, un valore hardcoded:

    {
      "type": "Microsoft.Compute/virtualMachines",
      "name": "demoLinuxVM",
      ...
    }

### <a name="generic-resource-names"></a>Nomi di risorse generici
Per i tipi di risorse in gran parte accessibili tramite un'altra risorsa, è possibile usare un nome generico che sia hardcoded nel modello. Ad esempio, probabilmente non si vorrà assegnare un nome personalizzabile per le regole del firewall in SQL Server.

    {
        "type": "firewallrules",
        "name": "AllowAllWindowsAzureIps",
        ...
    }

## <a name="parameters"></a>Parametri
1. Ridurre al minimo i parametri laddove possibile. Se è possibile usare una variabile o un valore letterale, è necessario farlo. Specificare solo parametri per:
   
   * Impostazioni da variare in base all'ambiente (ad esempio SKU, dimensione o capacità).
   * Nomi di risorse da specificare per facilitare l'identificazione.
   * Valori usati spesso per completare altre attività (ad esempio nome utente dell'amministratore).
   * Segreti (ad esempio password)
   * Il numero o la matrice di valori da usare durante la creazione di più istanze di un tipo di risorsa.
2. I nomi dei parametri devono seguire la **notazione Camel**.
3. Fornire una descrizione nei metadati per ogni parametro.
   
        "parameters": {
            "storageAccountType": {
                "type": "string",
                "metadata": {
                    "description": "The type of the new storage account created to store the VM disks"
                }
            }
        }
4. Definire i valori predefiniti per i parametri (ad eccezione delle password e delle chiavi SSH).
   
        "parameters": {
            "storageAccountType": {
                "type": "string",
                "defaultValue": "Standard_GRS",
                "metadata": {
                    "description": "The type of the new storage account created to store the VM disks"
                }
            }
        }
5. Usare **securestring** per tutte le password e i segreti. 
   
        "parameters": {
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        }
6. Quando possibile, evitare di usare un parametro per specificare la **posizione**. Usare invece la proprietà location del gruppo di risorse. Usando l'espressione **resourceGroup().location** per tutte le risorse, le risorse nel modello verranno distribuite nella stessa posizione del gruppo di risorse.
   
        "resources": [
          {
              "name": "[variables('storageAccountName')]",
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2016-01-01",
              "location": "[resourceGroup().location]",
              ...
          }
        ]
   
     Se un tipo di risorsa è supportato solo in un numero limitato di posizioni, provare a specificare una posizione valida direttamente nel modello. Se è necessario usare un parametro location, condividere per quanto possibile il relativo valore con le risorse che potrebbero essere nella stessa posizione. Questo approccio permette di ridurre al minimo gli utenti che devono fornire percorsi per ogni tipo di risorsa.
7. Evitare di usare un parametro o una variabile per la versione dell'API per un tipo di risorsa. I valori e le proprietà delle risorse possono variare in base al numero di versione. Quando la versione dell'API è impostata su un parametro o una variabile, IntelliSense negli editor di codice non può determinare lo schema corretto. Impostare invece la versione dell'API come hardcoded nel modello.

## <a name="variables"></a>Variabili
1. Usare le variabili per i valori da usare più volte in un modello. Se un valore viene usato una sola volta, un valore hardcoded facilita la lettura del modello.
2. Non è possibile usare la funzione [reference](resource-group-template-functions.md#reference) nella sezione delle variabili. La funzione reference deriva il proprio valore dallo stato di runtime della risorsa, ma le variabili vengono risolte durante l'analisi iniziale del modello. Costruire invece valori che richiedono la funzione **reference** direttamente nella sezione **resources** o **outputs** del modello.
3. Includere le variabili per i nomi di risorse che devono essere univoci, come illustrato in [Nomi di risorse](#resource-names).
4. È possibile raggruppare le variabili in oggetti complessi. È possibile fare riferimento a un valore da un oggetto complesso nel formato **variable.subentry**. Il raggruppamento delle variabili consente di tenere traccia delle variabili correlate e migliora la leggibilità del modello.
   
        "variables": {
            "storage": {
                "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
                "type": "Standard_LRS"
            }
        },
        "resources": [
          {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "[variables('storage').name]",
              "apiVersion": "2016-01-01",
              "location": "[resourceGroup().location]",
              "sku": {
                  "name": "[variables('storage').type]"
              },
              ...
          }
        ]
   
   > [!NOTE]
   > Un oggetto complesso non può contenere un'espressione che fa riferimento a un valore da un oggetto complesso. A questo scopo, definire una variabile separata.
   > 
   > 
   
     Per esempi avanzati di uso di oggetti complessi come variabili, vedere [Condivisione dello stato in modelli di Azure Resource Manager](best-practices-resource-manager-state.md).

## <a name="resources"></a>Risorse
1. Specificare **comments** per ogni risorsa nel modello per consentire ad altri collaboratori di comprendere lo scopo della risorsa.
   
        "resources": [
          {
              "name": "[variables('storageAccountName')]",
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2016-01-01",
              "location": "[resourceGroup().location]",
              "comments": "This storage account is used to store the VM disks",
              ...
          }
        ]
2. Usare i tag per aggiungere metadati alle risorse allo scopo di aumentare le informazioni sulle risorse. Ad esempio, è possibile aggiungere metadati a una risorsa quali dettagli di fatturazione. Per altre informazioni, vedere [Uso dei tag per organizzare le risorse di Azure](resource-group-using-tags.md).
3. Se si usa un **endpoint pubblico** nel modello, ad esempio un endpoint pubblico di archiviazione BLOB, **non impostare come hardcoded** lo spazio dei nomi. Usare la funzione **reference** per recuperare lo spazio dei nomi in modo dinamico. Questo approccio consente di distribuire il modello in ambienti diversi dello spazio dei nomi pubblico, senza dover modificare manualmente l'endpoint nel modello. Impostare apiVersion sulla stessa versione in uso per storageAccount nel modello.
   
        "osDisk": {
            "name": "osdisk",
            "vhd": {
                "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
            }
        }
   
     Se l'account di archiviazione viene distribuito nello stesso modello, non è necessario specificare lo spazio dei nomi del provider quando si fa riferimento alla risorsa. La sintassi semplificata è:
   
        "osDisk": {
            "name": "osdisk",
            "vhd": {
                "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
            }
        }
   
     Se nel modello sono presenti altri valori configurati con uno spazio dei nomi pubblico, modificarli in modo da riflettere la stessa funzione reference. Ad esempio, la proprietà storageUri di diagnosticsProfile della macchina virtuale.
   
        "diagnosticsProfile": {
            "bootDiagnostics": {
                "enabled": "true",
                "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
            }
        }
   
     È anche possibile **fare riferimento** a un account di archiviazione in un gruppo di risorse diverso.

        "osDisk": {
            "name": "osdisk", 
            "vhd": {
                "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
            }
        }

1. Assegnare publicIPAddresses a una macchina virtuale solo se richiesto per un'applicazione. Per connettersi a fini amministrativi, di gestione o debug, usare inboundNatRules, virtualNetworkGateways o un jumpbox.
   
     Per altre informazioni sulla connessione alle macchine virtuali, vedere:
   
   * [Esecuzione di VM per un'architettura a più livelli in Azure](../guidance/guidance-compute-n-tier-vm.md)
   * [Configurare l'accesso WinRM per le macchine virtuali in Azure Resource Manager](../virtual-machines/virtual-machines-windows-winrm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
   * [Consentire l'accesso esterno a una VM usando il portale di Azure](../virtual-machines/virtual-machines-windows-nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
   * [Consentire l'accesso esterno alla VM mediante PowerShell](../virtual-machines/virtual-machines-windows-nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
   * [Apertura di porte e di endpoint](../virtual-machines/virtual-machines-linux-nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. La proprietà **domainNameLabel** per publicIPAddresses deve essere univoca, avere una lunghezza compresa tra 3 e 63 caratteri e seguire le regole specificate dall'espressione regolare `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Dato che la funzione uniqueString genera una stringa lunga 13 caratteri, il parametro dnsPrefixString non deve superare 50 caratteri.
   
        "parameters": {
            "dnsPrefixString": {
                "type": "string",
                "maxLength": 50,
                "metadata": {
                    "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
                }
            }
        },
        "variables": {
            "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
        }
3. Quando si aggiunge una password a **customScriptExtension**, usare la proprietà **commandToExecute** in protectedSettings.
   
        "properties": {
            "publisher": "Microsoft.Azure.Extensions",
            "type": "CustomScript",
            "typeHandlerVersion": "2.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "fileUris": [
                    "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
                ]
            },
            "protectedSettings": {
                "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
            }
        }
   
   > [!NOTE]
   > Per garantire che i segreti passati come parametri a virtualMachines/extensions siano crittografati, usare la proprietà protectedSettings delle estensioni pertinenti.
   > 
   > 

## <a name="outputs"></a>Output
Se un modello crea **publicIPAddresses**, deve includere una sezione **outputs** che restituisca i dettagli dell'indirizzo IP e del nome di dominio completo. Questi valori di output consentiranno di recuperare facilmente tali informazioni dopo la distribuzione. Quando si fa riferimento alla risorsa, usare la versione dell'API usata per crearlo. 

```
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-or-nested-templates"></a>Modello singolo o modelli annidati
Per distribuire la soluzione, è possibile usare un modello singolo o un modello principale con più modelli annidati. I modelli annidati sono comuni per scenari più avanzati. I modelli annidati presentano i vantaggi seguenti:

1. È possibile scomporre la soluzione in componenti di destinazione
2. È possibile riusare i modelli annidati con modelli principali diversi

Quando si decide di scomporre la progettazione del modello in più modelli annidati, le linee guida seguenti ne consentono la standardizzazione. Queste linee guida si basano sulla documentazione descritta nell'articolo [Procedure consigliate per la progettazione di modelli di Azure Resource Manager](best-practices-resource-manager-design-templates.md) . La progettazione consigliata è costituita dai modelli seguenti:

* **Modello principale** (azuredeploy.json). Usato per i parametri di input.
* **Modello di risorse condivise**. Distribuisce le risorse condivise usate da tutte le altre risorse (ad esempio, rete virtuale e set di disponibilità). L'espressione dependsOn impone che questo modello venga distribuito prima degli altri.
* **Modello di risorse facoltative**. Distribuisce risorse in modo condizionale in base a un parametro, ad esempio un jumpbox.
* **Modelli di risorse membro**. Ogni tipo di istanza all'interno di un livello di applicazione prevede una propria configurazione. All'interno di un livello è possibile definire diversi tipi di istanza. Ad esempio, la prima istanza crea un cluster, mentre le altre vengono aggiunte al cluster esistente. Ogni tipo di istanza avrà un proprio modello di distribuzione.
* **Script**. Per ogni tipo di istanza sono applicabili script ampiamente riutilizzabili, come ad esempio quelli di inizializzazione e formattazione di dischi aggiuntivi. Gli script personalizzati vengono creati per scopi di personalizzazione specifici e variano in base al tipo di istanza.

![modello annidato](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

Per altre informazioni, vedere [Uso di modelli collegati con Gestione risorse di Azure](resource-group-linked-templates.md).

## <a name="conditionally-link-to-nested-template"></a>Collegarsi in modo condizionale al modello annidato
È possibile collegarsi in modo condizionale ai modelli annidati usando un parametro che diventa parte dell'URI per il modello.

    "parameters": {
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },
    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                }
            }
        }
    ]

## <a name="template-format"></a>Formato del modello
1. È consigliabile passare il modello tramite una convalida JSON per rimuovere virgole, parentesi e parentesi quadre estranee che potrebbero causare un errore durante la distribuzione. Provare [JSONlint](http://jsonlint.com/) o un pacchetto linter per l'ambiente di modifica preferito (Visual Studio Code, Atom, Sublime Text, Visual Studio, ecc.)
2. È anche consigliabile formattare il codice JSON per una migliore leggibilità. È possibile usare un pacchetto formattatore JSON per l'editor locale. In Visual Studio formattare il documento con **Ctrl+K, Ctrl+D**. In VS Code usare **Alt+Shift+F**. Se l'editor locale non formatta il documento, è possibile usare un [formattatore online](https://www.bing.com/search?q=json+formatter).

## <a name="next-steps"></a>Passaggi successivi
* Per indicazioni sull'architettura della soluzione per le macchine virtuali, vedere [Running a Windows VM on Azure](../guidance/guidance-compute-single-vm.md) (Esecuzione di una VM Windows in Azure) e [Running a Linux VM on Azure](../guidance/guidance-compute-single-vm-linux.md) (Esecuzione di una VM Linux in Azure).
* Per indicazioni sulla configurazione di un account di archiviazione, vedere [Elenco di controllo di prestazioni e scalabilità per Archiviazione di Microsoft Azure](../storage/storage-performance-checklist.md).
* Per assistenza con le reti virtuali, vedere [Networking infrastructure guidelines](../virtual-machines/virtual-machines-windows-infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)(Linee guida sull'infrastruttura di rete).
* Per indicazioni su come le aziende possono usare Resource Manager per gestire efficacemente le sottoscrizioni, vedere [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Scaffolding aziendale Azure - Governance prescrittiva per le sottoscrizioni).




<!--HONumber=Dec16_HO2-->


