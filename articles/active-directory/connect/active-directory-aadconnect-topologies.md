---
title: 'Azure AD Connect: topologie supportate | Microsoft Docs'
description: Questo argomento illustra in modo dettagliato le topologie supportate e non supportate per Azure AD Connect
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 1034c000-59f2-4fc8-8137-2416fa5e4bfe
ms.service: active-directory
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: article
ms.date: 11/01/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 28b5da6098316f8fbe84966e0dac88f5b7d2cb1d
ms.openlocfilehash: e5983496cda061b28418dcf86c38fbe80c611503


---
# <a name="topologies-for-azure-ad-connect"></a>Topologie per Azure AD Connect
L'obiettivo di questo argomento consiste nell'illustrare le diverse topologie locali e di Azure AD con il servizio di sincronizzazione Azure AD Connect come soluzione di integrazione principale. Illustra le configurazioni supportate e non supportate.

Legenda per le immagini del documento:

| Descrizione | Icona |
| --- | --- |
| Foresta locale di Active Directory |![AD](./media/active-directory-aadconnect-topologies/LegendAD1.png) |
| Active Directory con importazione con filtri |![AD](./media/active-directory-aadconnect-topologies/LegendAD2.png) |
| Server di sincronizzazione di Azure AD Connect |![Sincronizzazione](./media/active-directory-aadconnect-topologies/LegendSync1.png) |
| Server di sincronizzazione di Azure AD Connect in "Modalità di gestione temporanea" |![Sincronizzazione](./media/active-directory-aadconnect-topologies/LegendSync2.png) |
| GALSync con FIM2010 o MIM2016 |![Sincronizzazione](./media/active-directory-aadconnect-topologies/LegendSync3.png) |
| Dettagli relativi al server di sincronizzazione di Azure AD Connect |![Sincronizzazione](./media/active-directory-aadconnect-topologies/LegendSync4.png) |
| Directory di Azure AD |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png) |
| Scenario non supportato |![Non supportato](./media/active-directory-aadconnect-topologies/LegendUnsupported.png) |

## <a name="single-forest-single-azure-ad-tenant"></a>Foresta singola, tenant singolo di Azure AD
![Foresta singola, tenant singolo](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

La topologia più comune è costituita da una singola foresta locale, con uno o più domini, e un tenant singolo di Azure AD. Per l'autenticazione di Azure AD viene usata la sincronizzazione delle password. L'installazione rapida di Azure AD Connect supporta unicamente questa topologia.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Foresta singola, più server di sincronizzazione a un tenant di Microsoft Azure
![Foresta singola, filtro non supportato](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Non è supportato lo scenario che prevede più server del servizio di sincronizzazione Azure AD Connect connessi allo stesso tenant di Azure AD, ad eccezione di un [server di gestione temporanea](#staging-server). Non è supportato neanche se i server sono configurati per la sincronizzazione di set di oggetti che si escludono a vicenda. Potrebbe essere stato preso in considerazione se non è possibile raggiungere tutti i domini della foresta da un unico server o per distribuire il carico tra server diversi.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Più foreste, tenant singolo di Azure AD
![Più foreste, tenant singolo](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

In molte organizzazioni sono presenti ambienti con più foreste Active Directory locali. La presenza di più foreste Active Directory locali può essere dovuta a vari motivi. Esempi tipici sono costituiti da progettazioni con foreste di account-risorse e dai risultati di fusioni o acquisizioni.

Quando sono presenti più foreste, devono essere tutte raggiungibili da un singolo server del servizio di sincronizzazione Azure AD Connect. Non è necessario aggiungere il server a un dominio. Se è necessario raggiungere tutte le foreste, il server può essere inserito in una rete perimetrale.

L'installazione guidata di Azure AD Connect offre diverse opzioni per consolidare gli utenti rappresentati in più foreste. L'obiettivo è fare in modo che un utente sia rappresentato una sola volta in Azure AD. Nell'installazione guidata sono disponibili alcune topologie comuni che è possibile configurare nel percorso di installazione personalizzato. Selezionare l'opzione corrispondente che rappresenta la topologia nella pagina **Identificazione univoca per gli utenti**. Il consolidamento viene configurato solo per gli utenti. I gruppi duplicati non vengono consolidati con la configurazione predefinita.

Le topologie comuni sono illustrate nella sezione successiva: [Topologie separate](#multiple-forests-separate-topologies), [A maglia completa](#multiple-forests-full-mesh-with-optional-galsync) e [Account-Risorse](#multiple-forests-account-resource-forest).

La configurazione predefinita del servizio di sincronizzazione Azure AD Connect presuppone quanto segue:

1. Gli utenti hanno solo un account abilitato e la foresta in cui si trova l'account viene usata per l'autenticazione dell'utente. Questo presupposto riguarda sia la sincronizzazione delle password che la federazione. userPrincipalName e sourceAnchor o immutableID provengono da questa foresta.
2. Gli utenti dispongono di una sola cassetta postale.
3. La foresta che ospita la cassetta postale di un utente garantisce la migliore qualità dei dati per gli attributi visibili nell'Elenco indirizzi globale di Exchange. Se non è presente una cassetta postale per l'utente, è possibile usare qualsiasi foresta per recuperare questi valori di attributi.
4. Se è disponibile una cassetta postale collegata, è presente anche un altro account in una foresta diversa usato per l'accesso.

Se l'ambiente non soddisfa questi presupposti, si verifica quanto segue:

* Se sono presenti più account attivi o più cassette postali, il motore di sincronizzazione ne seleziona uno e ignora gli altri.
* Una cassetta postale collegata priva di account attivi non viene esportata in Azure AD. L'account utente non è rappresentato come membro in alcun gruppo. Una cassetta postale collegata in DirSync viene sempre rappresentata come cassetta postale normale. Questa modifica introduce un comportamento diverso per migliorare il supporto degli scenari a più foreste.

Per altre informazioni, vedere [Informazioni sulla configurazione predefinita](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Più foreste, più server di sincronizzazione a un tenant di Microsoft Azure
![Più foreste, sincronizzazione multipla non supportata](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Non è consentito connettere più server del servizio di sincronizzazione Azure AD Connect a un tenant singolo di Azure AD. Fa eccezione l'uso di un [server di gestione temporanea](#staging-server).

### <a name="multiple-forests--separate-topologies"></a>Più foreste - Topologie separate
**Gli utenti rappresentati solo una volta in tutte le directory.**

![Più foreste, utenti una sola volta](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Più foreste, topologie separate](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

In questo ambiente tutte le foreste locali sono considerate come entità separate e nessun utente di una foresta può essere incluso in un'altra foresta.
Ogni foresta presenta un'organizzazione Exchange dedicata e non viene effettuata alcuna sincronizzazione dell'elenco di indirizzi globale (GALSync) tra le foreste. Questa topologia può presentarsi dopo una fusione o acquisizione o in un'organizzazione in cui ogni business unit opera in modo isolato rispetto alle altre. In Azure AD queste foreste si trovano nella stessa organizzazione e vengono visualizzate con un elenco di indirizzi globale unificato.
In questa immagine ogni oggetto di ogni foresta viene rappresentato una sola volta nel metaverse e aggregato nel tenant di Azure AD di destinazione.

### <a name="multiple-forests--match-users"></a>Più foreste - Corrispondenze con utenti
**Le identità utenti sono presenti in più directory**

Tutti gli scenari hanno in comune il fatto che i gruppi di distribuzione e di sicurezza possono contenere una combinazione di utenti, contatti ed entità di sicurezza esterne.

Le entità di protezione esterne vengono usate in ADDS per rappresentare i membri di altre foreste in un gruppo di sicurezza. Tutte le entità di sicurezza esterne vengono risolte nell'oggetto reale in Azure AD.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Più foreste - A maglia completa con GALSync facoltativo
**Le identità utenti sono presenti in più directory. Trova la corrispondenza usando: Attributo Mail**

![Più foreste, posta elettronica utenti](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Più foreste, rete completa](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

In una topologia a maglia completa utenti e risorse possono risiedere in qualsiasi foresta e in genere tra le foreste sono presenti trust bidirezionali.

Se Exchange è presente in più di una foresta, potrebbe essere disponibile una soluzione GALSync locale. Ogni utente sarebbe rappresentato come un contatto in tutte le altre foreste. La soluzione GALSync viene comunemente implementata mediante Forefront Identity Manager 2010 o Microsoft Identity Manager 2016. Azure AD Connect non può essere usato per la soluzione GALSync locale.

In questo scenario, gli oggetti relativi all'identità vengono aggiunti tramite l'attributo Mail. Un utente con una cassetta postale in una foresta viene aggiunto ai contatti nelle altre foreste.

### <a name="multiple-forests--account-resource-forest"></a>Più foreste - Foresta di tipo Account-Risorse
**Le identità utenti sono presenti in più directory. Trova la corrispondenza usando: Attributi ObjectSID e msExchMasterAccountSID**

![Più foreste, ObjectSID utenti](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Più foreste, AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

In una topologia di foresta account-risorse, sono presenti una o più foreste di account con account utente attivi. Sono anche presenti una o più foreste risorse con account disabilitati.

In questo scenario una o più **foreste risorse** considerano attendibili tutte le **foreste account**. La foresta risorse presenta in genere uno schema di AD esteso con Exchange e Lync. Tutti i servizi Exchange e Lync e altri servizi condivisi si trovano in questa foresta. Gli utenti hanno un account utente disabilitato in questa foresta e la cassetta postale è collegata alla foresta account.

## <a name="office-365-and-topology-considerations"></a>Considerazioni su Office 365 e sulle topologie
Alcuni carichi di lavoro di Office 365 prevedono determinate restrizioni per le topologie supportate. Se si prevede di farne uso, vedere l'argomento relativo alle topologie supportate per il carico di lavoro.

| Carico di lavoro |
| --- | --- |
| Exchange Online |
| Skype for Business Online |

## <a name="staging-server"></a>server di gestione temporanea
![server di gestione temporanea](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect supporta l'installazione di un secondo server in **modalità di gestione temporanea**. Un server in questa modalità legge dati da tutte le directory connesse, ma non vi esegue operazioni di scrittura. Usa il normale ciclo di sincronizzazione e ha quindi a disposizione una copia aggiornata dei dati di identità. In una situazione di emergenza in cui si verifica un errore nel server primario è possibile eseguire il failover nel server di gestione temporanea. A tale scopo è possibile usare la procedura guidata di Azure AD Connect. È preferibile che il secondo server si trovi in un data center diverso, in modo da non condividere alcuna infrastruttura con il server primario. Eventuali modifiche di configurazione apportate al server primario devono essere copiate manualmente nel secondo server.

È anche possibile usare un server di gestione temporanea per testare una nuova configurazione personalizzata e il relativo effetto sui dati. È possibile visualizzare l'anteprima delle modifiche e perfezionare la configurazione. Dopo aver ottenuto la configurazione ottimale, sarà possibile rendere attivo il server di gestione temporanea e impostare il server attivo precedente in modalità di gestione temporanea.

Questo metodo può anche essere usato per sostituire il server di sincronizzazione attivo. Preparare il nuovo server e impostarlo in modalità di gestione temporanea. Verificarne lo stato di integrità, disabilitare la modalità di gestione temporanea rendendolo attivo e arrestare il server attivo corrente.

Per avere a disposizione più backup in data center diversi è possibile avere più di un server di gestione temporanea.

## <a name="multiple-azure-ad-tenants"></a>Più tenant di Azure AD
È consigliabile che in Azure AD sia presente un tenant singolo per un'organizzazione.
Prima di valutare l'uso di più tenant di Azure AD, esaminare gli scenari comuni illustrati in questi argomenti, che consentono di usare un tenant singolo.

| Argomento |
| --- | --- |
| Delega mediante le unità amministrative |

![Più foreste, più tenant](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Esiste una relazione 1:1 tra un server di sincronizzazione di Azure AD Connect e un tenant di Azure AD. Per ogni tenant di Azure AD è necessaria un'installazione del server del servizio di sincronizzazione Azure AD Connect. Le istanze del tenant di Azure AD sono isolate per impostazione predefinita e gli utenti in un'istanza non possono visualizzare gli utenti in un altro tenant. Se tale separazione è prevista, si tratta di una configurazione supportata. In caso contrario è consigliabile usare il modello a tenant singolo di Azure AD.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Ogni oggetto una sola volta in un tenant di Azure AD
![Foresta singola, filtro applicato](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

In questa topologia un server del servizio di sincronizzazione Azure AD Connect è connesso a ogni tenant di Azure AD. I server del servizio di sincronizzazione Azure AD Connect devono essere configurati per l'applicazione di filtri, in modo che ogni server possa usare un set di oggetti che si escludono a vicenda. Ad esempio, è possibile definire l'ambito di ogni server in modo che corrisponda a un dominio oppure a un'unità organizzativa specifica. Un dominio DNS può essere registrato unicamente in un tenant singolo di Azure AD. Anche gli UPN degli utenti nell'istanza locale di AD devono usare spazi dei nomi separati. Ad esempio, nella figura precedente tre suffissi UPN separati vengono registrati nell'istanza locale di AD: contoso.com, fabrikam.com e wingtiptoys.com. Gli utenti di ogni dominio di AD locale usano uno spazio dei nomi diverso.

Non è presente alcun GALsync tra le istanze del tenant di Azure AD. La rubrica di Exchange Online e Skype for Business mostra solo gli utenti nello stesso tenant.

Questa topologia presenta le restrizioni seguenti in scenari altrimenti supportati:

* Solo uno dei tenant di Azure AD può abilitare la distribuzione ibrida di Exchange con l'istanza di Active Directory locale.
* I dispositivi Windows 10 possono essere associati a un solo tenant di Azure AD.

Il requisito relativo al set di oggetti che si escludono a vicenda si applica anche al writeback. Alcune funzionalità di writeback non sono supportate con questa topologia, perché presuppongono una singola configurazione locale:

* Writeback dei gruppi con la configurazione predefinita
* Writeback dei dispositivi

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Ogni oggetto più volte in un tenant di Azure AD
![Foresta singola, più tenant non supportati](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Foresta singola, più connettori non supportati](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

* Non è supportata la sincronizzazione dello stesso utente in più tenant di Azure AD.
* Non è supportata la modifica della configurazione per fare in modo che gli utenti di un'istanza di Azure AD vengano visualizzati come contatti in un altro tenant di Azure AD.
* Non è supportata la modifica del servizio di sincronizzazione Azure AD Connect per la connessione a più tenant di Azure AD.

### <a name="galsync-by-using-writeback"></a>GALsync tramite l’utilizzo di writeback
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Come previsto da progettazione, i tenant di Azure AD sono isolati.

* Non è supportata la modifica della configurazione del servizio di sincronizzazione Azure AD Connect per la lettura di dati da un altro tenant di Azure AD.
* Non è supportata l'esportazione di utenti come contatti in un'altra istanza locale di AD con il servizio di sincronizzazione Azure AD Connect.

### <a name="galsync-with-on-premises-sync-server"></a>GALsync con server di sincronizzazione locale
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

È supportato l'uso di FIM2010/MIM2016 locale per eseguire la sincronizzazione dell'elenco di indirizzi globale per gli utenti tra due organizzazioni di Exchange. Gli utenti di un'organizzazione vengono visualizzati come utenti/contatti esterni nell'altra organizzazione. Sarà quindi possibile sincronizzare le due istanze locali diverse di AD con i rispettivi tenant di Azure AD.

## <a name="next-steps"></a>Passaggi successivi
Per informazioni su come installare Azure AD Connect per questi scenari, vedere [Installazione personalizzata di Azure Ad Connect](active-directory-aadconnect-get-started-custom.md).

Ulteriori informazioni sulla configurazione della [sincronizzazione di Azure AD Connect](active-directory-aadconnectsync-whatis.md) .

Ulteriori informazioni su [Integrazione delle identità locali con Azure Active Directory](active-directory-aadconnect.md).




<!--HONumber=Dec16_HO3-->


