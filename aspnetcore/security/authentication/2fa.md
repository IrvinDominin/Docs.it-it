---
title: Autenticazione a due fattori con SMS in ASP.NET Core
author: rick-anderson
description: Informazioni su come configurare l'autenticazione a due fattori (2FA) con un'applicazione ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
uid: security/authentication/2fa
ms.openlocfilehash: 335edfd5cd4dfbb9d223ba0ae888a6d2386cd4a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272309"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a>Autenticazione a due fattori con SMS in ASP.NET Core

Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [gli sviluppatori di svizzero](https://github.com/Swiss-Devs)

Vedere [generazione del codice QR abilitare per le app di autenticazione in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) per ASP.NET Core 2.0 e versioni successive.

In questa esercitazione viene illustrato come configurare l'autenticazione a due fattori (2FA) tramite SMS. Sono fornite istruzioni per [twilio](https://www.twilio.com/) e [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ma è possibile usare qualsiasi altro provider SMS. Si consiglia di eseguire [la conferma dell'Account e il recupero della Password](xref:security/authentication/accconfirm) prima di iniziare questa esercitazione.

Visualizzazione di [esempio completo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Come scaricare](xref:tutorials/index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Creare un nuovo progetto ASP.NET Core

Creare una nuova app web ASP.NET Core denominata `Web2FA` con singoli account utente. Seguire le istruzioni in [attivare SSL in un'applicazione ASP.NET Core](xref:security/enforcing-ssl) per impostare e richiedere SSL.

### <a name="create-an-sms-account"></a>Creare un account SMS

Creare un account SMS, ad esempio, da [twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Registrare le credenziali di autenticazione (per twilio: accountSid e authToken, per ASPSMS: userKey specificato e la Password).

#### <a name="figuring-out-sms-provider-credentials"></a>Capire le credenziali del Provider SMS

**Twilio:**  
Dalla scheda Dashboard dell'account di Twilio, copiare il **SID di Account** e **token di autorizzazione**.

**ASPSMS:**  
Le impostazioni dell'account, passare alla **parametri Userkey** e copiarlo in combinazione con il **Password**.

Questi valori con lo strumento di gestione segreto le chiavi in un secondo momento verrà archiviato `SMSAccountIdentification` e `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Specifica l'ID mittente / iniziatore

**Twilio:**  
Nella scheda numeri, copiare il Twilio **il numero di telefono**. 

**ASPSMS:**  
All'interno del Menu dei creatori sbloccare sbloccare una o più mittenti o scegliere un creatore alfanumerico (non supportato da tutte le reti). 

Questo valore con lo strumento di gestione di chiave privata all'interno della chiave in un secondo momento verrà archiviato `SMSAccountFrom`.


### <a name="provide-credentials-for-the-sms-service"></a>Fornire le credenziali per il servizio SMS

Si userà il [modello opzioni](xref:fundamentals/configuration/options) per accedere alle impostazioni di account e la chiave utente. 

   * Creare una classe per recuperare la chiave sicura di SMS. Per questo esempio, il `SMSoptions` classe il *Services/SMSoptions.cs* file.

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Impostare il `SMSAccountIdentification`, `SMSAccountPassword` e `SMSAccountFrom` con il [strumento segreto manager](xref:security/app-secrets). Ad esempio:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* Aggiungere il pacchetto NuGet per il provider SMS. Dal Package Manager Console (PMC) eseguire:

**Twilio:**  
`Install-Package Twilio`

**ASPSMS:**  
`Install-Package ASPSMS`


* Aggiungere il codice nel *Services/MessageServices.cs* file per consentire il SMS. Utilizzare il Twilio o la sezione ASPSMS:


**Twilio:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Configurare l'avvio da utilizzare `SMSoptions`

Aggiungere `SMSoptions` al contenitore del servizio nel `ConfigureServices` metodo il *Startup.cs*:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Abilitare l'autenticazione a due fattori

Aprire il *Views/Manage/Index.cshtml* file di visualizzazione Razor e rimuovere il commento caratteri (pertanto nessun tag è commentato).

## <a name="log-in-with-two-factor-authentication"></a>Eseguire l'accesso con autenticazione a due fattori

* Eseguire l'app e registrare un nuovo utente

![Visualizzazione di registro dell'applicazione Web aperta in Microsoft Edge](2fa/_static/login2fa1.png)

* Toccare il nome utente, che attiva il `Index` il metodo di azione nel controller di gestione. Scegliere il numero di telefono **Aggiungi** collegamento.

![Gestione visualizzazione](2fa/_static/login2fa2.png)

* Aggiungere un numero di telefono che verrà visualizzato il codice di verifica e toccare **invia il codice di verifica**.

![Aggiungi pagina numero di telefono](2fa/_static/login2fa3.png)

* Si riceverà un messaggio di testo con il codice di verifica. L'immissione e toccare **invia**

![Verificare la pagina del numero di telefono](2fa/_static/login2fa4.png)

Se non si ottiene un messaggio di testo, vedere la pagina di log twilio.

* La visualizzazione Gestione mostra che il numero di telefono è stato aggiunto correttamente.

![Gestione visualizzazione](2fa/_static/login2fa5.png)

* Toccare **abilitare** per abilitare l'autenticazione a due fattori.

![Gestione visualizzazione](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Test di autenticazione a due fattori

* Esegue la disconnessione.

* Accedi.

* L'account utente è abilitato l'autenticazione a due fattori, pertanto è necessario specificare il secondo fattore di autenticazione. In questa esercitazione è stata attivata la verifica telefonica. I modelli predefiniti consentono inoltre di impostare la posta elettronica come secondo fattore. È possibile impostare altri fattori secondo per l'autenticazione, ad esempio i codici a matrice. Toccare **inviare**.

![Inviare una visualizzazione di codice di verifica](2fa/_static/login2fa7.png)

* Immettere il codice che è visualizzato nel messaggio SMS.

* Facendo clic su di **ricordare questo browser** casella di controllo sarà esente dalla necessità di utilizzare 2FA per l'accesso quando si usa il dispositivo e il browser. Abilitazione 2FA e facendo clic su **ricordare questo browser** offre una protezione avanzata 2FA da utenti malintenzionati che tentano di accedere all'account, purché non hanno accesso al dispositivo. Tale operazione può essere eseguita su qualsiasi dispositivo privata che vengono utilizzate regolarmente. Impostando **ricordare questo browser**, ottenere le funzionalità di sicurezza di 2FA dai dispositivi che non si utilizza regolarmente e ottenere il vantaggio nel non dover eseguire 2FA nei propri dispositivi.

![Verificare la visualizzazione](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Blocco degli account per la protezione contro gli attacchi di forza bruta

Il blocco dell'account è consigliabile con 2FA. Una volta che un utente accede tramite un account locale o sociali, ogni tentativo non riuscito in 2FA viene archiviato. Se i tentativi di accesso non riusciti massimo viene raggiunto, l'utente è bloccato (impostazione predefinita: blocco di 5 minuti dopo i tentativi di accesso non riusciti 5). Un'autenticazione riuscita Reimposta il numero di tentativi di accesso non riuscito e reimposta l'orologio. Il numero massimo tentativi di accesso e il periodo di blocco può essere impostato con [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) e [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan). Il seguente consente di configurare il blocco degli account per 10 minuti dopo 10 tentativi di accesso non riusciti:

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

Verificare che [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) imposta `lockoutOnFailure` a `true`:

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
