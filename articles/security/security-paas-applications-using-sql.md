---
title: Protezione di applicazioni PaaS Web e per dispositivi mobili usando il database SQL e SQL Data Warehouse | Microsoft Docs
description: " Informazioni sulle procedure consigliate per la protezione delle applicazioni PaaS Web e per dispositivi mobili mediante il database SQL di Azure e SQL Data Warehouse. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/31/2017
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: b3db1281bbd37fcfbec54bdeb998e6338bb08369
ms.openlocfilehash: 39f841c0f3c8bed1bd640dfd644a58a764422aff


---
# <a name="securing-paas-web-and-mobile-applications-using-sql-database-and-sql-data-warehouse"></a>Protezione di applicazioni PaaS Web e per dispositivi mobili usando il database SQL e SQL Data Warehouse

In questo articolo vengono illustrate varie procedure consigliate per la protezione delle applicazioni PaaS Web e per dispositivi mobili mediante il [database SQL di Azure](https://azure.microsoft.com/services/sql-database/) e [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/). Le procedure consigliate si basano sull'esperienza di tecnici e clienti con Azure.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Database SQL di Azure e SQL Data Warehouse
Il [database SQL di Azure](../sql-database/sql-database-technical-overview.md) e [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) forniscono un servizio di database relazionale per le applicazioni basate su Internet. Verranno ora illustrati i servizi che aiutano a proteggere applicazioni e dati quando si usano il database SQL di Azure e SQL Data Warehouse in una distribuzione PaaS:

- Autenticazione di Azure Active Directory (invece di autenticazione di SQL Server)
- Firewall SQL di Azure
- Transparent data encryption (TDE)

## <a name="best-practices"></a>Procedure consigliate

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Usare un repository di identità centralizzato per autenticazione e autorizzazione

È possibile configurare i database SQL affinché usino uno tra due tipi di autenticazione:

- **Autenticazione SQL** usa nome utente e password. Durante la creazione del server logico per il database, è stato specificato un account di accesso "amministratore del server" con un nome utente e una password. Usando queste credenziali, è possibile essere autenticati in qualsiasi database di tale server in qualità di proprietario del database.

- **Autenticazione di Azure Active Directory** usa identità gestite da Azure Active Directory ed è supportata per domini gestiti e integrati. Per usare l'autenticazione di Azure Active Directory, è necessario creare un altro amministratore del server denominato "admin Azure AD," che è autorizzato ad amministrare utenti e gruppi di Azure AD. Questo amministratore può inoltre eseguire tutte le operazioni che un amministratore del server regolare può fare.

[L'autenticazione di Azure Active Directory](../active-directory/develop/active-directory-authentication-scenarios.md) è un meccanismo di connessione al database SQL di Azure e a SQL Data Warehouse tramite le identità di Azure Active Directory (AD). Azure AD rappresenta un'alternativa all'autenticazione di SQL Server, anche per evitare la proliferazione di identità utente in più server di database. Con l'autenticazione di Azure AD è possibile gestire centralmente le identità degli utenti del database e altri servizi Microsoft. La gestione centrale degli ID consente di gestire gli utenti del database da un unico punto e semplifica la gestione delle autorizzazioni.  

Vantaggi dell'uso dell'autenticazione di Azure AD al posto dell'autenticazione SQL:

- Consente la rotazione delle password in un'unica posizione.
- Consente di gestire le autorizzazioni del database tramite gruppi Azure AD esterni.
- Consente di eliminare l'archiviazione delle password abilitando l'autenticazione integrata di Windows e altre forme di autenticazione supportate da Azure AD.
- Usa gli utenti di database indipendente per autenticare le identità a livello di database.
- Supporta l'autenticazione basata su token per le applicazioni che si connettono a database SQL.
- Supporta la federazione dei domini di AD FS o l'autenticazione utente/password nativa per un'istanza locale di Azure AD senza la sincronizzazione del dominio.
- Supporta le connessioni da SQL Server Management Studio che utilizzano l'autenticazione universale di Active Directory, che include l'[MFA (Multi-Factor Authentication)](../multi-factor-authentication/multi-factor-authentication.md). L'MFA include funzionalità avanzate di autenticazione con una serie di semplici opzioni di verifica, tra cui: chiamata telefonica, SMS, smart card con pin o notifica tramite app per dispositivi mobili. Per altre informazioni [Supporto di SQL Server Management Studio (SSMS) per l'autenticazione MFA di Azure AD con il database SQL e SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

Per altre informazioni sull'autenticazione di Azure A, vedere:

- [Connessione al database SQL oppure a SQL Data Warehouse con l'autenticazione di Azure Active Directory](../sql-database/sql-database-aad-authentication.md)
- [Autenticazione in Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Supporto per l'autenticazione basata su token per il database di SQL Azure mediante l'autenticazione di Azure AD](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> Per assicurarsi che Azure Active Directory sia ideale per l'ambiente di riferimento, vedere [le funzionalità e le limitazioni di Azure AD](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), in particolare le considerazioni aggiuntive.
>
>

### <a name="restrict-access-based-on-ip-address"></a>Limitare l'accesso in base all'indirizzo IP
È possibile creare regole del firewall che specificano gli intervalli di indirizzi IP accettabili. Queste regole possono essere destinate a livello di server e database. È consigliabile usare le regole del firewall a livello di database quando è possibile, allo scopo di migliorare la sicurezza e la portabilità del database. Usare le regole del firewall a livello di server per gli amministratori e quando sono disponibili molti database che presentano gli stessi requisiti di accesso ma non si vuole dedicare tempo alla configurazione di ogni singolo database.

Le restrizioni predefinite agli indirizzi IP di origine del database SQL consentono l'accesso da qualsiasi indirizzo di Azure, inclusi altri tenant e sottoscrizioni. È possibile modificare le restrizioni per consentire l'accesso all'istanza solo ai propri indirizzi IP. Anche in presenza del firewall di SQL e delle restrizioni per gli indirizzi IP, è comunque necessario un'autenticazione robusta. Vedere le indicazioni riportate in precedenza in questo articolo.

Per altre informazioni sul firewall di Azure SQL e sulle restrizioni per gli indirizzi IP, vedere:

- [Controllo dell'accesso al database SQL di Azure](../sql-database/sql-database-control-access.md)
- [Configurare le regole del firewall per il database SQL di Azure - Panoramica](../sql-database/sql-database-firewall-configure.md)
- [Configurare una regola firewall a livello di server per il database SQL di Azure tramite il portale di Azure](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>Crittografia dei dati inattivi
La funzione [Transparent Data Encryption (TDE)](https://msdn.microsoft.com/library/azure/bb934049) crittografa file di dati di SQL Server, database SQL di Azure e SQL Data Warehouse di Azure, noti anche come dati inattivi. È possibile adottare diverse precauzioni per proteggere il database, ad esempio la progettazione di un sistema sicuro, la crittografia di asset riservati e la creazione di un firewall che protegga i server di database. Tuttavia, in uno scenario in cui i supporti fisici come unità o nastri di backup dovessero essere rubati, un malintenzionato potrebbe ripristinare o collegare il database e accedere ai dati in esso contenuti. Una soluzione consiste nel crittografare i dati riservati nel database e proteggere con un certificato le chiavi utilizzate per crittografarli. In questo modo, senza disporre delle chiavi nessuno potrà usare i dati. Tuttavia, questo tipo di protezione deve essere pianificato in anticipo.

TDE protegge i dati inattivi, ovvero i file di dati e di log. Offre inoltre la possibilità di conformarsi a diverse leggi, normative e linee guida stabilite in vari settori. Ciò consente agli sviluppatori di software di crittografare i dati usando algoritmi di crittografia standard del settore senza modificare le applicazioni esistenti.

È consigliabile usare la crittografia TDE se le normative la richiedono espressamente. Tenere presente, tuttavia, che un malintenzionato potrà comunque eseguire un attacco tramite il percorso di accesso normale. La crittografia TDE viene usata per proteggersi in quei casi estremamente rari in cui potrebbe essere necessaria una crittografia aggiuntiva a livello di applicazione, ad esempio la crittografia fornita da SQL di Azure per righe e colonne o la crittografia a livello di applicazione.

La crittografia a livello di applicazione deve essere usata anche per dati selettivi. È possibile limitare le problematiche legate alla sovranità dei dati crittografandoli con una chiave da conservare nel paese appropriato. Ciò impedisce che anche un trasferimento dati accidentale possa rappresentare un problema, perché sarà comunque impossibile decrittografarli senza la chiave, presupponendo che venga usato un algoritmo robusto come AES 256.

La crittografia offerta da SQL di Azure per righe e colonne può essere usata solo per consentire l'accesso agli utenti autorizzati ([RBAC](../active-directory/role-based-access-built-in-roles.md)) e impedisce agli utenti con autorizzazioni inferiori di visualizzare colonne o righe.

## <a name="next-steps"></a>Passaggi successivi
In questo articolo sono state illustrate varie procedure consigliate per la protezione delle applicazioni Web PaaS e delle applicazioni per dispositivi mobili mediante il database SQL e SQL Data Warehouse. Per altre informazioni sulla protezione delle distribuzioni PaaS, vedere:

- [Proteggere distribuzioni PaaS](security-paas-deployments.md)
- [Protezione delle applicazioni Web PaaS e delle applicazioni per dispositivi mobili mediante i Servizi app di Azure](security-paas-applications-using-app-services.md)



<!--HONumber=Feb17_HO1-->


