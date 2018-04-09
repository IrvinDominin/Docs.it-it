---
title: Impedire attacchi di reindirizzamento open in ASP.NET Core
author: ardalis
description: Viene illustrato come evitare attacchi di reindirizzamento aprire un'applicazione ASP.NET di base
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/preventing-open-redirects
ms.openlocfilehash: 4a210b8bb8091e7c036d4bc98306e3b3f90d7d46
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>Impedire attacchi di reindirizzamento open in ASP.NET Core

Un'app web che reindirizza a un URL che viene specificato tramite la richiesta, ad esempio i dati di stringa di query o i form può essere alterata potenzialmente per reindirizzare gli utenti a un URL esterno, dannoso. Questo tipo di manomissione viene chiamato un attacco di reindirizzamento aperto.

Ogni volta che la logica dell'applicazione reindirizza a un URL specificato, è necessario verificare che l'URL di reindirizzamento non è stato alterato. ASP.NET Core è una funzionalità integrata per proteggere le app da attacchi di reindirizzamento aprire (anche noto come open reindirizzamento).

## <a name="what-is-an-open-redirect-attack"></a>Che cos'è un attacco di reindirizzamento aprire?

Applicazioni Web spesso reindirizzare gli utenti a una pagina di accesso quando accedono alle risorse che richiedono l'autenticazione. Il reindirizzamento typlically include un `returnUrl` parametro querystring in modo che l'utente può essere restituito all'URL richiesto originariamente dopo che hanno effettuato correttamente l'accesso. Dopo aver autenticato l'utente, viene indirizzato all'URL richiesto originariamente.

Poiché l'URL di destinazione è specificato nella stringa di query della richiesta, un utente malintenzionato può manomettere il parametro querystring. Una stringa di query manomessi potrebbe consentire al sito di reindirizzare l'utente a un sito esterno, dannoso. Questa tecnica viene chiamata un attacco di reindirizzamento (o di reindirizzamento) aperto.

### <a name="an-example-attack"></a>Un attacco di esempio

Un utente malintenzionato potrebbe sviluppare un attacco che consente l'accesso utente non autorizzato a informazioni riservate sull'applicazione o le credenziali di un utente. Per avviare l'attacco, essi convincere l'utente di fare clic su un collegamento alla pagina di accesso del sito, con un `returnUrl` valore querystring aggiunto all'URL. Ad esempio, il [NerdDinner.com](http://nerddinner.com) , scritto per ASP.NET MVC, l'applicazione di esempio include tali una pagina di accesso qui: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``. L'attacco quindi questi passaggi:

1. Utente fa clic su un collegamento a ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (si noti che secondo URL è nerddi**n**er, non nerddi**nn**er).
2. L'utente accede correttamente.
3. L'utente viene reindirizzato (per il sito) ``http://nerddiner.com/Account/LogOn`` (sito dannoso simile sito reale).
4. L'utente accede nuovamente (dando dannoso le proprie credenziali del sito) e viene reindirizzato al sito effettivo.

L'utente verrà probabilmente ritiene che il primo tentativo di accedere non è riuscita e il secondo è stata eseguita correttamente. Molto probabilmente rimarrà anche senza conoscere le credenziali sono stati compromessi.

![Processo di attacchi di reindirizzamento aperto](preventing-open-redirects/_static/open-redirection-attack-process.png)

Oltre alle pagine di accesso, alcuni siti forniscono pagine di reindirizzamento o di endpoint. Si supponga l'app abbia una pagina con un reindirizzamento di aprire, ``/Home/Redirect``. Un utente malintenzionato potrebbe creare ad esempio, un collegamento tramite posta elettronica a ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``. Un utente tipico analizza l'URL e vedere che inizia con il nome del sito. Concessione dell'attendibilità che, essi verranno fare clic sul collegamento. Il reindirizzamento aprire invierebbe quindi l'utente al sito di phishing, identico al proprio, e l'utente potrebbe accedere a essi ritiene è il sito.

## <a name="protecting-against-open-redirect-attacks"></a>Protezione da attacchi di reindirizzamento aperto

Quando si sviluppano applicazioni web, considerare tutti i dati forniti dall'utente come non attendibili. Se l'applicazione dispone di funzionalità che reindirizza l'utente in base al contenuto dell'URL, assicurarsi che tali reindirizzamenti sono solo in locale all'interno dell'app (o a un URL noto, non qualsiasi URL che può essere fornito nella stringa di query).

### <a name="localredirect"></a>LocalRedirect

Utilizzare il ``LocalRedirect`` metodo helper dalla base `Controller` classe:

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

``LocalRedirect`` verrà generata un'eccezione se è stato specificato un URL non locali. In caso contrario, si comporta esattamente come il ``Redirect`` metodo.

### <a name="islocalurl"></a>IsLocalUrl

Utilizzare il [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metodo per testare gli URL prima di reindirizzamento:

Nell'esempio seguente viene illustrato come controllare se un URL è locale prima di reindirizzamento.

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

Il `IsLocalUrl` metodo protegge gli utenti non venga inavvertitamente reindirizzato a un sito dannoso. È possibile registrare i dettagli dell'URL che è stato specificato quando viene fornito un URL non locali in una situazione in cui previsto un URL locale. Registrazione di reindirizzamento URL può essere utile per diagnosticare gli attacchi di reindirizzamento.
