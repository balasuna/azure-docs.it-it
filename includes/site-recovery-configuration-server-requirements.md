| **Hardware** | |
| --- |---|
| Numero di core CPU| 8 core |
| RAM| 12 GB|
| Numero di dischi | **3 dischi** <br> - Disco del sistema operativo<br> - Disco della cache del server di elaborazione<br> - Unità di conservazione (per il failback)|
| Spazio libero su disco (cache del server di elaborazione) | 600 GB
| Spazio libero su disco (disco di conservazione) | 600 GB|
| **Software** | |
| Versione del sistema operativo | Windows Server 2012 R2 |
| Impostazioni locali del sistema operativo | Inglese (en-us)|
| Versione di VMware vSphere PowerCLI | [PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0 "PowerCLI 6.0")|
| Ruoli di Windows Server | Non abilitare i ruoli seguenti <br> **Active Directory Domain Services** <br>**Internet Information Services** <br> **Hyper-V** |
| **Rete** | |
| Tipo di scheda di interfaccia di rete | VMXNET3 |
| Tipo di indirizzo IP | Static |
| Accesso a Internet | Il server deve essere in grado di accedere all'URL seguente direttamente o tramite un server proxy <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi <br> - time.nist.gov <br> - time.windows.com |
| Porte | 443 (orchestrazione del canale di controllo)<br>9443 (trasporto dei dati)|


<!--HONumber=Jan17_HO3-->


