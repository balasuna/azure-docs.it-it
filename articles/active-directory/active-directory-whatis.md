---
title: Informazioni su Azure Active Directory
description: "Usare Azure Active Directory per estendere le identità locali esistenti nel cloud o sviluppare le applicazioni integrate di Azure AD."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 498820c4-9ebe-42be-bda2-ecf38cc514ca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: b247388bff35fb755c8363eb48695e47f2510574
ms.openlocfilehash: a5f8adfa278b096914239357c675654fd9fc2908


---
# <a name="what-is-azure-active-directory"></a>Informazioni su Azure Active Directory
Azure Active Directory (Azure AD) è il servizio Microsoft di gestione di identità e directory basato sul cloud e multi-tenant.

Azure AD offre agli amministratori IT una soluzione conveniente e facile da usare per fornire a dipendenti e partner commerciali l'accesso Single Sign-On (SSO) a [migliaia di applicazioni SaaS cloud](http://blogs.technet.com/b/ad/archive/2014/09/03/50-saas-apps-now-support-federation-with-azure-ad.aspx) , quali Office365, Salesforce.com, DropBox e Concur.

Azure AD permette agli sviluppatori di applicazioni di concentrarsi sulla creazione dell'applicazione, semplificando e velocizzando l'integrazione con una soluzione di qualità elevata per la gestione delle identità, usata da milioni di organizzazioni in tutto il mondo.

Azure AD include anche un insieme completo di funzionalità per la gestione delle identità, tra cui l'autenticazione a più fattori, la registrazione dei dispositivi, la gestione self-service delle password e dei gruppi, la gestione degli account con privilegi, il controllo degli accessi in base al ruolo, il monitoraggio dell'utilizzo dell'applicazione, il controllo avanzato e il monitoraggio e avvisi relativi alla sicurezza. Queste funzionalità possono contribuire alla protezione di applicazioni basate sul cloud, alla semplificazione dei processi IT, alla riduzione dei costi e alla soddisfazione degli obiettivi aziendali relativi alla conformità.

Con soli [quattro clic](http://blogs.technet.com/b/ad/archive/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect.aspx), inoltre, è possibile integrare Azure AD con un'istanza esistente di Windows Server Active Directory, permettendo alle organizzazioni di sfruttare gli investimenti locali esistenti a livello di gestione delle identità per gestire l'accesso alle applicazioni SaaS basate sul cloud.

Se si è clienti di Office365, Azure o Dynamics CRM Online, è possibile che si stia già usando Azure AD, senza esserne consapevoli. Ogni tenant di Office365, Azure e Dynamics CRM è in effetti già un tenant di Azure AD. Quando lo si desidera, è possibile iniziare a usare tale tenant per gestire l'accesso a migliaia di altre applicazioni cloud integrate con Azure AD.

![Stack di Azure AD Connect](./media/active-directory-whatis/Azure_Active_Directory.png)

## <a name="how-reliable-is-azure-ad"></a>Affidabilità di Azure AD
La progettazione multi-tenant, geograficamente distribuita e a disponibilità elevata di Azure AD offre l'affidabilità adeguata per le esigenze aziendali più importanti. Grazie all'esecuzione in 28 data center in tutto il mondo e al failover automatizzato, Azure AD offre la certezza di un'affidabilità elevata. Anche in caso di problemi in un data center, le copie dei dati delle directory saranno sempre disponibili in almeno due data center dislocati in aree geografiche diverse, consentendo l'accesso immediato.

Per altre informazioni, vedere [Contratti di servizio](https://azure.microsoft.com/support/legal/sla/).

## <a name="what-are-the-benefits-of-azure-ad"></a>Vantaggi di Azure AD
L'organizzazione può usare Azure AD per migliorare la produttività dei dipendenti, semplificare i processi IT, migliorare la sicurezza e ridurre i costi in molti modi:

* Adozione rapida dei servizi cloud, per offrire a dipendenti e partner un'esperienza di accesso Single-Sign-On semplificata basata sulle capacità completamente automatizzate di Azure AD per la gestione dell'accesso alle app SaaS e sulle capacità dei servizi di provisioning.
* Possibilità di offrire ai dipendenti l'accesso ad app cloud di qualità elevata e le capacità self-service ovunque si trovino e con qualsiasi dispositivo.
* Gestione semplice e sicura dell'accesso di dipendenti e fornitori agli account aziendali per social media.
* Miglioramento della sicurezza delle applicazioni grazie all'autenticazione a più fattori e all'accesso condizionale di Azure AD.
* Implementazione di una gestione coerente e self-service degli accessi alle applicazioni, in modo da permettere ai titolari di aziende di mantenere la rapidità di azione, riducendo al tempo stesso costi IT e costi generali.
* Monitoraggio dell'utilizzo delle applicazioni e protezione dell'attività da minacce avanzate, grazie alla creazione di report e al monitoraggio della sicurezza.
* Accesso mobile (remoto) sicuro alle applicazioni locali.

## <a name="how-does-azure-ad-compare-to-on-premises-active-directory-domain-services-ad-ds"></a>Quali sono le analogie e le differenze tra Azure AD e Active Directory Domain Services (AD DS)?

Sia Azure Active Directory (Azure AD) che Active Directory locale (Active Directory Domain Services o AD DS) sono sistemi che archiviano i dati nella directory e gestiscono le comunicazioni tra utenti e risorse, compresi processi di accesso degli utenti, autenticazione e ricerche nelle directory.

AD DS è un ruolo server in Windows Server; ciò significa che può essere distribuito in macchine virtuali o fisiche. Dispone di una struttura gerarchica basata su X.500. Usa DNS per l'individuazione degli oggetti, supporta interazioni tramite LDAP e usa principalmente Kerberos per l'autenticazione. Active Directory abilita unità organizzative e oggetti Criteri di gruppo, oltre ad aggiungere computer al dominio; inoltre vengono create relazioni di trust tra i domini.

Azure AD è un servizio directory pubblico a più clienti, il che significa che in Azure AD è possibile creare un tenant per i server cloud e le applicazioni come Office 365. Utenti e gruppi vengono creati in una struttura piatta senza unità organizzative o oggetti Criteri di gruppo. L'autenticazione viene eseguita tramite protocolli, come SAML, WS-Federation e OAuth. È possibile eseguire query in Azure AD, tuttavia invece di usare LDAP è necessario usare un'API REST chiamata API Graph AD. Tutte queste operazioni possono essere eseguite su HTTP e HTTPS.

È possibile usare Azure AD Connect per sincronizzare le identità locali con Azure AD.

---

### <a name="authentication-and-authorization-details"></a>Dettagli relativi ad autenticazione e autorizzazione

#### <a name="azure-ad"></a>Azure AD
`SAML`, `WS-Federation`, Interattivo con credenziali supportate, OAuth 2.0, OpenID Connect 

#### <a name="on-premises-ad-ds"></a>AD DS locale
`SAML`, `WS-Federation`, NTLM, Kerberos, MD5, Basic

---

### <a name="object-repository-details"></a>Dettagli dei repository di oggetti

#### <a name="azure-ad"></a>Azure AD
Accesso tramite Azure AD Graph e Microsoft Graph 

#### <a name="on-premises-ad-ds"></a>AD DS locale
X.500 LDAP  

---


### <a name="programmatic-access-details"></a>Dettagli dell'accesso programmatico

#### <a name="azure-ad"></a>Azure AD
API REST di MS/Azure AD Graph 

#### <a name="on-premises-ad-ds"></a>AD DS locale
LDAP 

---

### <a name="sso-to-applications-details"></a>SSO per i dettagli delle applicazioni

#### <a name="azure-ad"></a>Azure AD
`OpenID Connect`, `SAML` 

#### <a name="on-premises-ad-ds"></a>AD DS locale
`Open-ID Connect`, `SAML`, WS-Fed 

---

### <a name="access-management-details"></a>Dettagli della gestione degli accessi

#### <a name="azure-ad"></a>Azure AD
Ambito definito a livello di risorse e controllo degli accessi in base al ruolo, autorizzazioni delegate definite a livello di client e dell'applicazione, Consent Framework (forza il consento utente/amministratore appropriato in base a quanto definito/richiesto dalla risorsa/dal client) 

Tramite il ruolo applicazione può essere applicato individualmente o tramite gruppi. Supporta la gestione da parte dell'amministratore, l'accesso self-service all'applicazione e il consento dell'utente

#### <a name="on-premises-ad-ds"></a>AD DS locale
Tramite ACL, può essere applicato individualmente o tramite gruppi e supporta la gestione da parte dell'amministratore 

---

### <a name="group-management-details"></a>Dettagli della gestione dei gruppi

#### <a name="azure-ad"></a>Azure AD
`Admin managed`, gestione dinamica/in base a regole, gestione self-service dei gruppi 

#### <a name="on-premises-ad-ds"></a>AD DS locale
`Admin managed`, Sistema esterno (FIM o altro) necessario per la gestione dinamica/in base a regole |

---

### <a name="supported-credentials-details"></a>Dettagli credenziali supportate

#### <a name="azure-ad"></a>Azure AD
`Username + password`, `Smartcard` 

#### <a name="on-premises-ad-ds"></a>AD DS locale
`Username + password`, `Smartcard` 
 
---







## <a name="how-can-i-get-started"></a>Operazioni iniziali

**Amministratori IT:**

* [provarlo,](https://azure.microsoft.com/trial/get-started-active-directory/) - è possibile ottenere subito gratuitamente una versione di valutazione di 30 giorni e distribuire la prima soluzione cloud in meno di 5 minuti, usando il collegamento seguente

* Per suggerimenti relativi a come attivare ed eseguire rapidamente un tenant di Azure AD, vedere "Introduzione ad Azure AD".

**Sviluppatori:**
 
* Vedere la [Guida per gli sviluppatori](active-directory-developers-guide.md) per Azure Active Directory

* [Iniziare a usare una versione di valutazione](https://azure.microsoft.com/trial/get-started-active-directory/): è possibile ottenere subito gratuitamente una versione di valutazione di 30 giorni e iniziare a integrare la propria app con Azure AD.

## <a name="where-can-i-learn-more"></a>Altre informazioni
Sono disponibili moltissime risorse utili online, che forniscono tutte le informazioni necessarie su Azure AD. Gli articoli seguenti forniscono informazioni introduttive:

* [Abilitazione della directory per la gestione ibrida con Azure AD Connect](active-directory-aadconnect.md)
* [Sicurezza aggiuntiva per un mondo sempre connesso](../multi-factor-authentication/multi-factor-authentication.md)
* [Automatizzare il provisioning e il deprovisioning utenti in applicazioni SaaS con Azure Active Directory](active-directory-saas-app-provisioning.md)
* [Introduzione ad Azure AD Report](active-directory-reporting-getting-started.md)
* [Gestire le password da qualsiasi posizione](active-directory-passwords.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [Automatizzare il provisioning e il deprovisioning utenti in applicazioni SaaS con Azure Active Directory](active-directory-saas-app-provisioning.md)
* [Come fornire l'accesso remoto sicuro alle applicazioni locali](active-directory-application-proxy-get-started.md)
* [Gestione dell'accesso alle risorse tramite i gruppi di Azure Active Directory](active-directory-manage-groups.md)
* [Che cosa sono le licenze di Microsoft Azure Active Directory?](active-directory-licensing-what-is.md)
* [Come individuare app cloud non autorizzate usate nell'organizzazione](active-directory-cloudappdiscovery-whatis.md)



<!--HONumber=Dec16_HO3-->


