---
title: Configurazione di account di accesso esterno Facebook in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione di Facebook account utente in un'applicazione ASP.NET di base esistente.
manager: wpickett
ms.author: riande
ms.date: 08/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/facebook-logins
ms.openlocfilehash: cabb5acc6e593c02c20b3403b39c601ce26a4d99
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688983"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Configurazione di account di accesso esterno Facebook in ASP.NET Core

Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa esercitazione viene illustrato come consentire agli utenti di accedere con il proprio account Facebook tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index). Richiede l'autenticazione Facebook il [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacchetto NuGet. Viene innanzitutto creato un ID App Facebook seguendo il [passaggi ufficiali](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Creare l'app in Facebook

* Passare il [sviluppatori Facebook app](https://developers.facebook.com/apps/) pagina ed eseguire l'accesso. Se si dispone già di un account Facebook, utilizzare il **iscriversi a Facebook** collegamento nella pagina di accesso per crearne uno.

* Toccare il **aggiungere una nuova App** pulsante nell'angolo superiore destro per creare un nuovo ID di App.

   ![Facebook per portale sviluppatori aprire in Microsoft Edge](index/_static/FBMyApps.png)

* Compilare il modulo e toccare il **Crea ID App** pulsante.

   ![Creare un nuovo ID di App](index/_static/FBNewAppId.png)

* Nel **selezionare un prodotto** pagina, fare clic su **Set Up** sul **account Facebook** scheda.

   ![Pagina di installazione del prodotto](index/_static/FBProductSetup.png)

* Il **delle Guide rapide** guidata consentirà di avviare con **scegliere una piattaforma** come la prima pagina. Fare clic su ignorare la procedura guidata per il momento di **impostazioni** collegamento nel menu a sinistra:

   ![Guida introduttiva a Skip](index/_static/FBSkipQuickStart.png)

* Viene visualizzata la **impostazioni OAuth Client** pagina:

![Pagina Impostazioni OAuth client](index/_static/FBOAuthSetup.png)

* Immettere l'URI di sviluppo con */signin-facebook* aggiunti al **URI di reindirizzamento di OAuth validi** campo (ad esempio: `https://localhost:44320/signin-facebook`). L'autenticazione di Facebook configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel */signin-facebook* route per implementare il flusso di OAuth.

* Fare clic su **salvare modifiche**.

* Fare clic su di **Dashboard** collegamento nel riquadro di spostamento a sinistra. 

    In questa pagina, annotare il `App ID` e `App Secret`. Nella sezione successiva verranno aggiunti entrambi nell'applicazione ASP.NET di base:

   ![Dashboard dello sviluppatore di Facebook](index/_static/FBDashboard.png)

* Quando si distribuisce il sito è necessario rivedere il **account Facebook** pagina di installazione e registrare un nuovo URI pubblico.

## <a name="store-facebook-app-id-and-app-secret"></a>Archiviare ID App Facebook e segreto dell'applicazione

Collegare le impostazioni sensibili, ad esempio Facebook `App ID` e `App Secret` all'applicazione mediante configurazione di [Manager segreto](xref:security/app-secrets). Ai fini di questa esercitazione, denominare il token `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.

Eseguire i comandi seguenti per archiviare in modo sicuro `App ID` e `App Secret` utilizzando Gestione segreto:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Configurare l'autenticazione di Facebook

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Aggiungere il servizio di Facebook nel `ConfigureServices` metodo il *Startup.cs* file:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Installare il [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacchetto.

* Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.
* Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Aggiungere il middleware di Facebook nel `Configure` metodo *Startup.cs* file:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

Vedere il [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Facebook. Opzioni di configurazione possono essere utilizzate per:

* Richiedere informazioni diverse relative all'utente.
* Aggiungere gli argomenti di stringa di query per personalizzare l'esperienza di accesso.

## <a name="sign-in-with-facebook"></a>Accedere con Facebook

Eseguire l'applicazione e fare clic su **Accedi**. Viene visualizzata un'opzione per accedere con Facebook.

![Applicazione Web: utente non autenticato](index/_static/DoneFacebook.png)

Facendo clic sul **Facebook**, si verrà reindirizzati a Facebook per l'autenticazione:

![Pagina di autenticazione di Facebook](index/_static/FBLogin.png)

Indirizzo di posta elettronica e profilo pubblico le richieste di autenticazione di Facebook per impostazione predefinita:

![Pagina di autenticazione di Facebook](index/_static/FBLoginDone.png)

Dopo aver immesso le credenziali di Facebook, che si verrà reindirizzati al sito in cui è possibile impostare la posta elettronica.

A questo punto si è connessi utilizzando le credenziali di Facebook:

![Applicazione Web: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a>Risoluzione dei problemi

* **ASP.NET Core 2.x solo:** identità se non è configurata tramite la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*. Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.
* Se il database del sito non è stato creato applicando la migrazione iniziale, è possibile ottenere *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore. Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.

## <a name="next-steps"></a>Passaggi successivi

* In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Facebook. È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](xref:security/authentication/social/index).

* Quando si pubblica il sito web all'app web di Azure, è consigliabile reimpostare il `AppSecret` nel portale per sviluppatori di Facebook.

* Impostare il `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` come impostazioni dell'applicazione nel portale di Azure. Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.
