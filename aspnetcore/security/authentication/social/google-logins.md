---
title: Programma di installazione di Google account di accesso esterno in ASP.NET Core
author: rick-anderson
description: Questa esercitazione illustra l'integrazione dell'autenticazione di Google account utente in un'applicazione ASP.NET di base esistente.
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: e3d0f0c058dd7395aebf1c97821867bae3af7c54
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/12/2018
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="9d5b0-103">Programma di installazione di Google account di accesso esterno in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d5b0-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="9d5b0-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9d5b0-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9d5b0-105">In questa esercitazione viene illustrato come consentire agli utenti di accedere con l'account di Google + tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="9d5b0-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="9d5b0-106">Iniziamo seguendo il [passaggi ufficiali](https://developers.google.com/identity/sign-in/web/devconsole-project) per creare una nuova app nella Console API Google.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="9d5b0-107">Creare l'app nella Console API di Google</span><span class="sxs-lookup"><span data-stu-id="9d5b0-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="9d5b0-108">Passare a [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="9d5b0-109">Se si dispone già di un account Google, usare **più opzioni** > **[creare account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  collegamento per creare uno:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Console di API di Google](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="9d5b0-111">Si verrà reindirizzati a **libreria API Manager** pagina:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-111">You are redirected to **API Manager Library** page:</span></span>

![Pagina della libreria di gestione API](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="9d5b0-113">Toccare **crea** e immettere il **nome progetto**:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-113">Tap **Create** and enter your **Project name**:</span></span>

![Finestra di dialogo Nuovo progetto](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="9d5b0-115">Dopo avere accettato la finestra di dialogo, si viene reindirizzati alla pagina libreria che consente di scegliere le funzionalità per la nuova app.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="9d5b0-116">Trovare **Google + API** nell'elenco e fare clic sul relativo collegamento per aggiungere la funzionalità API:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Pagina della libreria di gestione API](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="9d5b0-118">Verrà visualizzata la pagina per l'API appena aggiunto.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="9d5b0-119">Toccare **abilitare** aggiungere Google + segno nella funzionalità all'App:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Pagina di Google + API di gestione API](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="9d5b0-121">Dopo aver abilitato l'API, toccare **creare credenziali** per configurare i segreti:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Pagina di Google + API di gestione API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="9d5b0-123">Scegliere:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-123">Choose:</span></span>
   * <span data-ttu-id="9d5b0-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="9d5b0-124">**Google+ API**</span></span>
   * <span data-ttu-id="9d5b0-125">**Server Web (ad esempio, Node. js, Tomcat)**, e</span><span class="sxs-lookup"><span data-stu-id="9d5b0-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="9d5b0-126">**I dati utente**:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-126">**User data**:</span></span>

![Pagina delle credenziali di gestione API: scoprire quali tipo di credenziali necessario pannello](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="9d5b0-128">Toccare **le credenziali sono necessarie?** che permette di visualizzare il secondo passaggio della configurazione di app, **creare un ID client OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Pagina delle credenziali di gestione API: creazione di un ID client OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="9d5b0-130">Poiché viene creato un progetto di Google + con una sola funzionalità (accesso), è possibile immettere lo stesso **nome** per l'ID client OAuth 2.0 è utilizzato per il progetto.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="9d5b0-131">Immettere l'URI di sviluppo con */signin-google* aggiunti al **autorizzato URI di reindirizzamento** campo (ad esempio: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="9d5b0-131">Enter your development URI with */signin-google* appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="9d5b0-132">L'autenticazione di Google configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel */signin-google* route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-132">The Google authentication configured later in this tutorial will automatically handle requests at */signin-google* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="9d5b0-133">Premere TAB per aggiungere il **autorizzato URI di reindirizzamento** voce.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-133">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="9d5b0-134">Toccare **Create client ID**, che permette di visualizzare il terzo passaggio **impostare la schermata di consenso di OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-134">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Pagina delle credenziali di gestione API: impostare la schermata di consenso di OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="9d5b0-136">Immettere il pubblico **indirizzo di posta elettronica** e **nome prodotto** visualizzato per l'app quando Google + richiesto all'utente di accedere.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-136">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="9d5b0-137">Opzioni aggiuntive sono disponibili in **ulteriori opzioni di personalizzazione**.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-137">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="9d5b0-138">Toccare **continua** per eseguire l'ultimo passaggio, **scaricare le credenziali**:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-138">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Pagina delle credenziali di gestione API: scaricare le credenziali](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="9d5b0-140">Toccare **scaricare** per salvare un file JSON con segreti dell'applicazione, e **eseguita** per completare la creazione della nuova app.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-140">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="9d5b0-141">Quando si distribuisce il sito è necessario rivedere il **Google Console** e registrare un nuovo url pubblico.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-141">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="9d5b0-142">Archivio Google ClientID e ClientSecret</span><span class="sxs-lookup"><span data-stu-id="9d5b0-142">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="9d5b0-143">Collegare le impostazioni sensibili, ad esempio Google `Client ID` e `Client Secret` all'applicazione mediante configurazione di [Manager segreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="9d5b0-143">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="9d5b0-144">Ai fini di questa esercitazione, denominare il token `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-144">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="9d5b0-145">I valori per questi token, vedere il file JSON scaricato nel passaggio precedente in `web.client_id` e `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-145">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="9d5b0-146">Configurare l'autenticazione di Google</span><span class="sxs-lookup"><span data-stu-id="9d5b0-146">Configure Google Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9d5b0-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9d5b0-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9d5b0-148">Aggiungere il servizio Google il `ConfigureServices` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-148">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9d5b0-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9d5b0-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="9d5b0-150">Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) viene installato.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="9d5b0-151">Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="9d5b0-152">Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="9d5b0-153">Aggiungere il middleware di Google nel `Configure` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="9d5b0-154">Vedere il [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione Google.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-154">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="9d5b0-155">Questo può essere usato per richiedere informazioni diverse relative all'utente.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="9d5b0-156">Accedere con Google</span><span class="sxs-lookup"><span data-stu-id="9d5b0-156">Sign in with Google</span></span>

<span data-ttu-id="9d5b0-157">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="9d5b0-158">Viene visualizzata un'opzione per accedere con Google:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-158">An option to sign in with Google appears:</span></span>

![Applicazione Web in esecuzione in Microsoft Edge: utente non autenticato](index/_static/DoneGoogle.png)

<span data-ttu-id="9d5b0-160">Quando si fa clic su Google, si verrà reindirizzati a Google per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Finestra di dialogo autenticazione Google](index/_static/GoogleLogin.png)

<span data-ttu-id="9d5b0-162">Dopo aver immesso le credenziali di Google, quindi si verrà reindirizzati al sito web in cui è possibile impostare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="9d5b0-163">A questo punto si è connessi utilizzando le credenziali di Google:</span><span class="sxs-lookup"><span data-stu-id="9d5b0-163">You are now logged in using your Google credentials:</span></span>

![Applicazione Web in esecuzione in Microsoft Edge: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="9d5b0-165">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="9d5b0-165">Troubleshooting</span></span>

* <span data-ttu-id="9d5b0-166">Se si riceve un `403 (Forbidden)` pagina di errore dall'applicazione durante l'esecuzione in modalità di sviluppo o interruzione nel debugger con lo stesso errore, assicurarsi che **Google + API** è stata abilitata nel **libreria gestione API** seguendo i passaggi elencati [precedenti in questa pagina](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="9d5b0-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="9d5b0-167">Se l'accesso non funziona e non vengono visualizzate eventuali errori, passare alla modalità di sviluppo per rendere più semplice eseguire il debug del problema.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="9d5b0-168">**ASP.NET Core 2.x solo:** identità se non è configurata tramite la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di eseguire l'autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="9d5b0-169">Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="9d5b0-170">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterranno *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="9d5b0-171">Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d5b0-172">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="9d5b0-172">Next steps</span></span>

* <span data-ttu-id="9d5b0-173">In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Google.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="9d5b0-174">È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="9d5b0-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="9d5b0-175">Quando si pubblica il sito web all'app web di Azure, è consigliabile reimpostare il `ClientSecret` nella Console di API di Google.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="9d5b0-176">Impostare il `Authentication:Google:ClientId` e `Authentication:Google:ClientSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="9d5b0-177">Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="9d5b0-177">The configuration system is set up to read keys from environment variables.</span></span>
