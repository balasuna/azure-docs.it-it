---
title: "Uso di Configurazione dello stato desiderato (DSC) con i set di scalabilità di macchine virtuali | Microsoft Docs"
description: "Utilizzo dei set di scalabilità di macchine virtuali con l&quot;estensione DSC di Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: c0d9ea4f506371ca21a08f6e1d6128c00123cbac


---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Utilizzo dei set di scalabilità di macchine virtuali con l'estensione DSC di Azure
I [set di scalabilità di macchine virtuali (VMSS)](virtual-machine-scale-sets-overview.md) possono essere usati con il gestore dell'estensione [Configurazione dello stato desiderato (DSC) di Azure](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Il set di scalabilità di macchine virtuali consente di distribuire e gestire un numero elevato di macchine virtuali e di aumentare o ridurre la dimensione in risposta al carico. DSC viene usato per la configurazione delle VM mano a mano che sono in linea, in modo che eseguano il software di produzione.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Differenze tra la distribuzione di VM e VMSS
La struttura del modello sottostante alle VMSS è diversa da quella di una singola VM. In particolare, una singola VM distribuisce le estensioni nel nodo "virtualMachines". È presente una voce del tipo "extensions" nella quale DSC viene aggiunto al modello:

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Il nodo VMSS ha una sezione "properties" con gli attributi "VirtualMachineProfile", "extensionProfile". DSC viene aggiunto sotto "extensions":

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>Comportamento di VMSS
Il comportamento di VMSS è identico a quello di una singola macchina virtuale. Quando viene creata una nuova VM, il provisioning viene automaticamente eseguito con l'estensione DSC. Se l'estensione richiede una versione più recente del file WMF, la VM verrà riavviata prima di passare online. Una volta online, il file con estensione zip di configurazione DSC viene scaricato e ne viene eseguito il provisioning nella VM. Altre informazioni sono reperibili nella [panoramica sull'estensione DSC di Azure](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Passaggi successivi
Esaminare il [modello di Azure Resource Manager per l'estensione DSC](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Informazioni su come l' [estensione DSC gestisce in modo sicuro le credenziali](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Per altre informazioni sul gestore dell'estensione DSC, vedere [Introduzione al gestore dell'estensione DSC (Desired State Configuration) di Azure](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Per altre informazioni su PowerShell DSC, [vedere il centro di documentazione di PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 




<!--HONumber=Nov16_HO3-->


