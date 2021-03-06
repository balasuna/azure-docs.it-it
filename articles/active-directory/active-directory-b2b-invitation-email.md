---
title: Elementi del messaggio di posta elettronica di invito per la collaborazione B2B di Azure Active Directory | Documentazione Microsoft
description: Modello di messaggio di posta elettronica di invito per la collaborazione B2B di Azure Active Directory
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/02/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 9543ac09eacceaf1d740b2e2be25c85924d1fad4
ms.openlocfilehash: ea2d41002cf20fd3ee93f05650cc2e47e1dfb20e


---


# <a name="the-elements-of-the-b2b-collaboration-invitation-email"></a>Elementi del messaggio di posta elettronica di invito per la collaborazione B2B

I messaggi di posta elettronica di invito sono strumenti fondamentali per entrare in contatto con i partner e introdurli alla funzionalità di collaborazione B2B di Azure AD. L'obiettivo principale è quello di aumentare il livello di fiducia nel destinatario e aggiungere elementi di legittimità e riprova sociale al messaggio di posta elettronica, per assicurarsi che il destinatario scelga di selezionare con tranquillità il pulsante **Get Started** (Inizia) per accettare l'invito. Si tratta di un elemento fondamentale per ridurre i problemi relativi alla condivisione. Ovviamente, è anche importante che il messaggio di posta elettronica abbia un aspetto straordinario!

![Messaggio di posta elettronica di invito per la collaborazione B2B](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-the-email"></a>Descrizione del messaggio di posta elettronica
Per sfruttare al meglio le potenzialità del messaggio di posta elettronica, di seguito ne vengono descritti alcuni elementi.

### <a name="subject"></a>Oggetto
Il contenuto dell'oggetto del messaggio di posta elettronica è basato su questo modello: Ti invitiamo all'organizzazione &lt;nometenant&gt;

### <a name="from-address"></a>Indirizzo del mittente.
Per l'indirizzo del mittente si userà un modello simile a quello LinkedIn. L'obiettivo consiste nel rendere il più chiaro possibile chi è il mittente dell'invito e a quale azienda appartiene, oltre a mettere in evidenza che il messaggio proviene da un indirizzo di posta elettronica Microsoft. Il formato è: &lt;nome visualizzato del mittente dell'invito&gt; da &lt;nometenant&gt; (tramite Microsoft)<invites@microsoft.com&gt;

### <a name="reply-to"></a>Rispondi a
Se disponibile, viene impostato l'indirizzo di posta elettronica del mittente dell'invito. Un'eventuale risposta all'invito verrà pertanto inviata al mittente.

### <a name="branding"></a>Personalizzazione
Nei messaggi di posta elettronica di invito vengono usate le informazioni personalizzate distintive dell'azienda eventualmente impostate per il tenant. Se si vuole sfruttare i vantaggi offerti dalla personalizzazione, [qui](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) sono disponibili i dettagli su come eseguirne la configurazione. Il logo del banner verrà visualizzato nel messaggio di posta elettronica. Per ottenere risultati ottimali, seguire le istruzioni relative a qualità e dimensioni dell'immagine, disponibili [qui](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal). Il nome dell'azienda viene anche visualizzato nella chiamata all'azione.

### <a name="call-to-action"></a>Chiamata all'azione
La chiamata all'azione è costituita da due parti: la spiegazione del motivo per cui il destinatario ha ricevuto il messaggio e l'indicazione delle successive azioni richieste al destinatario.
- La sezione relativa al "motivo" può essere basata sul seguente modello: Hai ricevuto l'invito per accedere alle applicazioni nell'organizzazione &lt;nometenant&gt;

- La sezione relativa alle "successive azioni richieste al destinatario" è indicata dalla presenza del pulsante **Get Started** (Inizia). Se per aggiungere il destinatario non è stato necessario inviare inviti, questo pulsante non viene visualizzato.

### <a name="inviters-information"></a>Informazioni sul mittente dell'invito
Il nome visualizzato del mittente dell'invito verrà incluso nel messaggio di posta elettronica. Nel messaggio di invito verrà inclusa anche l'immagine del profilo per l'account di Azure AD, qualora sia stata impostata. Entrambi tali elementi consentono di rendere il messaggio di posta elettronica più attendibile per il destinatario.

Se il mittente dell'invito non ha ancora impostato un'immagine del profilo, Azure AD crea un'icona con le iniziali del mittente dell'invito e la inserisce al posto dell'immagine, come illustrato:

  ![visualizzazione iniziali del mittente dell'invito](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a>Corpo
Questa sezione contiene il messaggio digitato dal mittente dell'invito oppure passato tramite l'API di invito. Si tratta di una semplice casella di testo, che non supporta l'elaborazione dei tag HTML per motivi di sicurezza.

### <a name="footer-section"></a>Sezione piè di pagina
Il piè di pagina contiene il marchio aziendale di Microsoft e indica al destinatario se il messaggio di posta elettronica è stato inviato da un alias non monitorato. Casi speciali:

- Il mittente dell'invito non dispone di un indirizzo di posta elettronica nella tenancy di invito

  ![l'immagine del profilo del mittente dell'invito non è associata a un indirizzo di posta elettronica nella tenancy di invito](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- Il destinatario non deve riscattare l'invito

  ![caso in cui il destinatario non deve riscattare l'invito](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>Passaggi successivi

Vedere gli altri articoli su Azure AD B2B Collaboration.

* [Che cos'è la collaborazione B2B di Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Procedura per aggiungere utenti di Collaborazione B2B ad Azure Active Directory da parte degli amministratori](active-directory-b2b-admin-add-users.md)
* [Procedura per aggiungere utenti di Collaborazione B2B da parte di Information Worker](active-directory-b2b-how-it-works.md)
* [Riscatto dell'invito di Collaborazione B2B](active-directory-b2b-redemption-experience.md)
* [Licenze per la Collaborazione B2B di Azure AD](active-directory-b2b-licensing.md)
* [Risoluzione dei problemi di Collaborazione B2B di Azure Active Directory](active-directory-b2b-troubleshooting.md)
* [Domande frequenti su Collaborazione B2B di Azure Active Directory](active-directory-b2b-faq.md)
* [API e personalizzazione per Collaborazione B2B di Azure Active Directory](active-directory-b2b-api.md)
* [Autenticazione a più fattori per utenti di Collaborazione B2B](active-directory-b2b-mfa-instructions.md)
* [Indice di articoli per la gestione di applicazioni in Azure Active Directory](active-directory-apps-index.md)



<!--HONumber=Feb17_HO1-->


