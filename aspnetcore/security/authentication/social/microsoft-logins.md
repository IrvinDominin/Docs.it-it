---
title: Configurazione dell'account di accesso esterno Account Microsoft
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.assetid: 66DB4B94-C78C-4005-BA03-3D982B87C268
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: cc2d119ae2f7cf06ed0ea4b8a91fd8fa15804468
ms.sourcegitcommit: fb518f856f31fe53c09196a13309eacb85b37a22
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/08/2017
---
# <a name="configuring-microsoft-account-authentication"></a><span data-ttu-id="039e2-103">Configurazione dell'autenticazione di Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="039e2-103">Configuring Microsoft Account authentication</span></span>

<a name=security-authentication-microsoft-logins></a>

<span data-ttu-id="039e2-104">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="039e2-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="039e2-105">In questa esercitazione viene illustrato come consentire agli utenti di accedere con l'account di Microsoft tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](index.md).</span><span class="sxs-lookup"><span data-stu-id="039e2-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="039e2-106">Creare l'app nel portale per sviluppatori di Microsoft</span><span class="sxs-lookup"><span data-stu-id="039e2-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="039e2-107">Passare a [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) e creare o accedere a un account Microsoft:</span><span class="sxs-lookup"><span data-stu-id="039e2-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Accedere alla finestra di dialogo](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="039e2-109">Se si dispone già di un account Microsoft, tocca  **[crearne uno.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="039e2-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="039e2-110">Dopo l'accesso, si verrà reindirizzati a **applicazioni personali** pagina:</span><span class="sxs-lookup"><span data-stu-id="039e2-110">After signing in you are redirected to **My applications** page:</span></span>

![Portale per sviluppatori Microsoft aperto in Microsoft Edge](index/_static/MicrosoftDev.png)

* <span data-ttu-id="039e2-112">Toccare **aggiungere un'app** in alto a destra angolo e immettere il **nome applicazione** e **Contact Email**:</span><span class="sxs-lookup"><span data-stu-id="039e2-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Finestra di dialogo Nuova registrazione dell'applicazione](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="039e2-114">Ai fini di questa esercitazione, cancellare il **impostazione guidata** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="039e2-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="039e2-115">Toccare **crea** per continuare al **registrazione** pagina:</span><span class="sxs-lookup"><span data-stu-id="039e2-115">Tap **Create** to continue to the **Registration** page:</span></span>

![Pagina di registrazione](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="039e2-117">Toccare **aggiungere piattaforma** nel **piattaforme** sezione e selezionare il **Web** piattaforma:</span><span class="sxs-lookup"><span data-stu-id="039e2-117">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Finestra di dialogo piattaforma Aggiungi](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="039e2-119">Nel nuovo **Web** piattaforma, quindi immettere l'URL di sviluppo con */signin-microsoft* aggiunti al **URL di reindirizzamento** campo (ad esempio: `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="039e2-119">In the new **Web** platform section, enter your development URL with */signin-microsoft* appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="039e2-120">Lo schema di autenticazione Microsoft configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel */signin-microsoft* route per implementare il flusso di OAuth:</span><span class="sxs-lookup"><span data-stu-id="039e2-120">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at */signin-microsoft* route to implement the OAuth flow:</span></span>

![Sezione relativa alla piattaforma Web](index/_static/MicrosoftRedirectUri.png)

* <span data-ttu-id="039e2-122">Toccare **Aggiungi URL** per verificare l'URL è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="039e2-122">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="039e2-123">Compilare tutte le altre impostazioni applicazione, se necessario e toccare **salvare** nella parte inferiore della pagina per salvare le modifiche alla configurazione di app.</span><span class="sxs-lookup"><span data-stu-id="039e2-123">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="039e2-124">Quando si distribuisce il sito è necessario rivedere il **registrazione** pagina e impostare un nuovo URL pubblico.</span><span class="sxs-lookup"><span data-stu-id="039e2-124">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="039e2-125">Id applicazione di Microsoft e la Password archiviati</span><span class="sxs-lookup"><span data-stu-id="039e2-125">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="039e2-126">Nota il `Application Id` visualizzata sul **registrazione** pagina.</span><span class="sxs-lookup"><span data-stu-id="039e2-126">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="039e2-127">Toccare **generare una nuova Password** nel **applicazione segreti** sezione.</span><span class="sxs-lookup"><span data-stu-id="039e2-127">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="039e2-128">Verrà visualizzata una casella in cui è possibile copiare la password dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="039e2-128">This displays a box where you can copy the application password:</span></span>

![Finestra di dialogo nuova password generata](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="039e2-130">Collegare le impostazioni sensibili come Microsoft `Application ID` e `Password` all'applicazione mediante configurazione di [Manager segreto](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="039e2-130">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="039e2-131">Ai fini di questa esercitazione, denominare il token `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="039e2-131">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="039e2-132">Configurare l'autenticazione di Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="039e2-132">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="039e2-133">Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) pacchetto è già installato.</span><span class="sxs-lookup"><span data-stu-id="039e2-133">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="039e2-134">Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="039e2-134">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="039e2-135">Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="039e2-135">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="039e2-136">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="039e2-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="039e2-137">Aggiungere il servizio Account Microsoft nel `ConfigureServices` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="039e2-137">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

<span data-ttu-id="039e2-138">Il `AddAuthentication` metodo deve essere chiamato solo una volta quando si aggiunge più provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="039e2-138">The `AddAuthentication` method should only be called once when adding multiple authentication providers.</span></span> <span data-ttu-id="039e2-139">Le chiamate successive a esso hanno la possibilità di cui si esegue l'override di qualsiasi configurati in precedenza [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) proprietà.</span><span class="sxs-lookup"><span data-stu-id="039e2-139">Subsequent calls to it have the potential of overriding any previously configured [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) properties.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="039e2-140">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="039e2-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="039e2-141">Aggiungere il middleware di Account Microsoft nel `Configure` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="039e2-141">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

<span data-ttu-id="039e2-142">Sebbene la terminologia utilizzata nel portale per sviluppatori Microsoft nomi questi token `ApplicationId` e `Password`, gli oggetti vengono esposti come `ClientId` e `ClientSecret` nell'API di configurazione.</span><span class="sxs-lookup"><span data-stu-id="039e2-142">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they are exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="039e2-143">Vedere il [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Account Microsoft.</span><span class="sxs-lookup"><span data-stu-id="039e2-143">See the [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="039e2-144">Questo può essere usato per richiedere informazioni diverse relative all'utente.</span><span class="sxs-lookup"><span data-stu-id="039e2-144">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="039e2-145">Accedi con Account Microsoft</span><span class="sxs-lookup"><span data-stu-id="039e2-145">Sign in with Microsoft Account</span></span>

<span data-ttu-id="039e2-146">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="039e2-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="039e2-147">Viene visualizzata un'opzione per eseguire l'accesso con Microsoft:</span><span class="sxs-lookup"><span data-stu-id="039e2-147">An option to sign in with Microsoft appears:</span></span>

![Log di applicazione nella pagina Web: utente non autenticato](index/_static/DoneMicrosoft.png)

<span data-ttu-id="039e2-149">Quando si fa clic su Microsoft, si viene reindirizzati a Microsoft per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="039e2-149">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="039e2-150">Dopo l'accesso con l'Account Microsoft (se non è già stato effettuato l'accesso) verrà richiesto per consentire all'app di accedere alle tue info:</span><span class="sxs-lookup"><span data-stu-id="039e2-150">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Finestra di dialogo autenticazione Microsoft](index/_static/MicrosoftLogin.png)

<span data-ttu-id="039e2-152">Toccare **Sì** e si verrà reindirizzati al sito web in cui è possibile impostare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="039e2-152">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="039e2-153">A questo punto si è connessi utilizzando le credenziali di Microsoft:</span><span class="sxs-lookup"><span data-stu-id="039e2-153">You are now logged in using your Microsoft credentials:</span></span>

![Applicazione Web: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="039e2-155">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="039e2-155">Troubleshooting</span></span>

* <span data-ttu-id="039e2-156">Se il provider dell'Account di Microsoft si viene reindirizzati a una pagina di errore di accesso, tenere presente l'errore titolo e descrizione parametri stringa di query direttamente dopo il `#` (hashtag) nell'Uri.</span><span class="sxs-lookup"><span data-stu-id="039e2-156">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="039e2-157">Anche se sembra che il messaggio di errore indicano un problema con l'autenticazione di Microsoft, la causa più comune è l'Uri non corrisponde a nessuna delle applicazione il **Redirect URIs** specificato per il **Web** piattaforma .</span><span class="sxs-lookup"><span data-stu-id="039e2-157">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="039e2-158">**ASP.NET Core solo 2. x:** identità se non è configurato per la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="039e2-158">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="039e2-159">Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="039e2-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="039e2-160">Se il database del sito non è stato creato applicando la migrazione iniziale, si otterranno *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="039e2-160">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="039e2-161">Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.</span><span class="sxs-lookup"><span data-stu-id="039e2-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="039e2-162">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="039e2-162">Next steps</span></span>

* <span data-ttu-id="039e2-163">In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="039e2-163">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="039e2-164">È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](index.md).</span><span class="sxs-lookup"><span data-stu-id="039e2-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="039e2-165">Quando si pubblica il sito web all'app web di Azure, è necessario creare un nuovo `Password` nel portale per sviluppatori di Microsoft.</span><span class="sxs-lookup"><span data-stu-id="039e2-165">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="039e2-166">Impostare il `Authentication:Microsoft:ApplicationId` e `Authentication:Microsoft:Password` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="039e2-166">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="039e2-167">Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="039e2-167">The configuration system is set up to read keys from environment variables.</span></span>