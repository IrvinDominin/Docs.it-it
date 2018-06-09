---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Creare un'app web di ASP.NET MVC 5 sicura con l'accesso, inviare tramite posta elettronica di conferma e reimpostazione della password (c#) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione viene illustrato come compilare un'app web ASP.NET MVC 5 con conferma tramite posta elettronica e la password reimpostata utilizzando il sistema di appartenenze di ASP.NET Identity. Si autorità di certificazione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: bfa5d52019be81374c7a544e255ab7ffb301fa7b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "34452568"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="7f1cc-104">Creare un'app web di ASP.NET MVC 5 sicura con l'accesso, inviare tramite posta elettronica di conferma e reimpostazione della password (c#)</span><span class="sxs-lookup"><span data-stu-id="7f1cc-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="7f1cc-105">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7f1cc-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="7f1cc-106">In questa esercitazione viene illustrato come compilare un'app web ASP.NET MVC 5 con conferma tramite posta elettronica e la password reimpostata utilizzando il sistema di appartenenze di ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="7f1cc-107">È possibile scaricare l'applicazione completata [qui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="7f1cc-108">Il download contiene funzioni di supporto di debug che consentono di testare conferma tramite posta elettronica e SMS senza dover configurare un messaggio di posta elettronica o il provider SMS.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="7f1cc-109">In questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="7f1cc-110">Creare un'applicazione MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7f1cc-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="7f1cc-111">Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="7f1cc-112">Installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="7f1cc-113">Avviso: È necessario installare [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o versione successiva per completare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="7f1cc-114">Creare un nuovo progetto Web ASP.NET e selezionare il modello MVC.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="7f1cc-115">Web Form supporta anche ASP.NET Identity, pertanto è possibile attenersi alla procedura simile in un'app di web form.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="7f1cc-116">Lasciare l'autenticazione predefinita come **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="7f1cc-117">Se si desidera ospitare l'applicazione in Azure, lasciare selezionata la casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="7f1cc-118">Più avanti nell'esercitazione si distribuirà in Azure.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="7f1cc-119">È possibile [aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="7f1cc-120">Impostare il [progetto utilizzare SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="7f1cc-121">Eseguire l'app, fare clic su di **registrare** collegare e registrare un utente.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="7f1cc-122">A questo punto, la convalida sola nel messaggio di posta elettronica è con il [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attributo.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="7f1cc-123">In Esplora Server, passare a **dati Connections\DefaultConnection\Tables\AspNetUsers**fare clic destro del mouse e selezionare **Apri definizione tabella**.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="7f1cc-124">La figura seguente mostra il `AspNetUsers` dello schema:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="7f1cc-125">Fare clic con il pulsante destro sul **AspNetUsers** tabella e selezionare **Mostra dati tabella**.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="7f1cc-126">A questo punto il messaggio di posta elettronica non è stato confermato.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="7f1cc-127">Fare clic sulla riga e selezionare Elimina.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-127">Click on the row and select delete.</span></span> <span data-ttu-id="7f1cc-128">Verranno aggiungere di nuovo questo messaggio di posta elettronica nel passaggio successivo e inviare un messaggio di posta elettronica di conferma.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="7f1cc-129">Conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="7f1cc-129">Email confirmation</span></span>

<span data-ttu-id="7f1cc-130">È consigliabile verificare il messaggio di posta elettronica di una nuova registrazione utente per verificare che non rappresentano un altro utente (vale a dire, è ancora stato registrato con un altro messaggio di posta elettronica).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="7f1cc-131">Si supponga di un forum di discussione, si desidera impedire `"bob@example.com"` tramite la registrazione come `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="7f1cc-132">Senza conferma tramite posta elettronica, `"joe@contoso.com"` Impossibile ricevere la posta elettronica indesiderato dall'app.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="7f1cc-133">Si supponga che Bob accidentalmente registrato come `"bib@example.com"` e non veniva notare, ha non sarebbe in grado di utilizzare password Ripristina perché l'app non ha il suo messaggio di posta elettronica corretto.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="7f1cc-134">Conferma tramite posta elettronica fornisce a livello di protezione limitato da BOT e non offre protezione da spamming determinato, hanno molti alias di posta elettronica di lavoro che possono utilizzare per registrare.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="7f1cc-135">In genere si desidera impedire agli utenti di nuovo di registrazione dei dati del sito web prima che siano stati confermati tramite posta elettronica, un SMS o un altro meccanismo.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="7f1cc-136">Nelle sezioni riportate di seguito, si consentirà conferma tramite posta elettronica e modificare il codice per impedire l'accesso fino a quando non è stato confermato l'accesso alla posta elettronica di utenti appena registrati.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="7f1cc-137">Collegare SendGrid</span><span class="sxs-lookup"><span data-stu-id="7f1cc-137">Hook up SendGrid</span></span>

<span data-ttu-id="7f1cc-138">Sebbene questa esercitazione viene illustrato solo come aggiungere una notifica tramite posta elettronica tramite [SendGrid](http://sendgrid.com/), è possibile inviare tramite posta elettronica tramite SMTP e altri meccanismi (vedere [risorse aggiuntive](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="7f1cc-139">Nella Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="7f1cc-140">Passare al [pagina di iscrizione Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) e registrare un account di SendGrid gratuito.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="7f1cc-141">Configurare SendGrid aggiungendo codice analogo al seguente in *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="7f1cc-142">È necessario aggiungere che include quanto segue:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="7f1cc-143">Per semplificare questo esempio, si saranno archiviare le impostazioni di app nel *Web. config* file:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="7f1cc-144">Sicurezza - archivio mai i dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="7f1cc-145">L'account e le credenziali vengono archiviate in appSetting.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="7f1cc-146">In Azure, è possibile archiviare questi valori in modo sicuro nel **[configura](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** scheda nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="7f1cc-147">Vedere [procedure consigliate per la distribuzione delle password e altri dati sensibili su ASP.NET e in Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="7f1cc-148">Abilitare la conferma tramite posta elettronica nel controller di Account</span><span class="sxs-lookup"><span data-stu-id="7f1cc-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="7f1cc-149">Verificare il *Views\Account\ConfirmEmail.cshtml* file presenta la sintassi razor corretto.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="7f1cc-150">(Il @ carattere nella prima riga potrebbe essere manca.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="7f1cc-151">)</span><span class="sxs-lookup"><span data-stu-id="7f1cc-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="7f1cc-152">Eseguire l'app e fare clic sul collegamento di registro.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-152">Run the app and click the Register link.</span></span> <span data-ttu-id="7f1cc-153">Dopo aver inviato il modulo di registrazione, si è connessi.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="7f1cc-154">Verificare l'account di posta elettronica e fare clic sul collegamento per confermare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="7f1cc-155">Richiedi conferma tramite posta elettronica prima dell'accesso in</span><span class="sxs-lookup"><span data-stu-id="7f1cc-155">Require email confirmation before log in</span></span>

<span data-ttu-id="7f1cc-156">Attualmente una volta che un utente ha completato il modulo di registrazione, vengono registrate.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="7f1cc-157">In genere, è necessario confermare la posta elettronica prima di registrarle.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="7f1cc-158">Nella sezione seguente, si modificherà il codice per richiedere nuovi utenti per disporre di un messaggio di posta elettronica di conferma vengono registrate nel (autenticato).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="7f1cc-159">Aggiornamento di `HttpPost Register` metodo con le seguenti modifiche evidenziate:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="7f1cc-160">Commentando il `SignInAsync` (metodo), l'utente non sarà firmato per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="7f1cc-161">Il `TempData["ViewBagLink"] = callbackUrl;` riga può essere utilizzata per [debug dell'app](#dbg) e registrazione di test senza l'invio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="7f1cc-162">`ViewBag.Message` viene utilizzato per visualizzare le istruzioni di conferma.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="7f1cc-163">Il [Scarica esempio](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene il codice per test conferma tramite posta elettronica senza configurare posta elettronica e consente inoltre il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="7f1cc-164">Creare un `Views\Shared\Info.cshtml` file e aggiungere il markup razor seguente:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="7f1cc-165">Aggiungere il [attributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) per il `Contact` il metodo di azione del controller Home.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="7f1cc-166">È possibile fare clic sui **contatto** collegamento per verificare gli utenti anonimi non hanno accesso e gli utenti autenticati dispone dell'accesso.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="7f1cc-167">È necessario aggiornare anche il `HttpPost Login` metodo di azione:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="7f1cc-168">Aggiornamento di *Views\Shared\Error.cshtml* visualizzazione per visualizzare il messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="7f1cc-169">Eliminare gli account nel **AspNetUsers** la tabella contenente l'alias di posta elettronica che si desidera eseguire il test con.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="7f1cc-170">Eseguire l'app e verificare che non è possibile accedere fino a quando non è confermato l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="7f1cc-171">Dopo aver confermato l'indirizzo di posta elettronica, fare clic su di **contatto** collegamento.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="7f1cc-172">Ripristino o la reimpostazione della password</span><span class="sxs-lookup"><span data-stu-id="7f1cc-172">Password recovery/reset</span></span>

<span data-ttu-id="7f1cc-173">Rimuovere i caratteri di commento dal `HttpPost ForgotPassword` il metodo di azione nel controller di account:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="7f1cc-174">Rimuovere i caratteri di commento dal `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) nel *Views\Account\Login.cshtml* file di visualizzazione razor:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="7f1cc-175">Nella pagina di accesso verrà visualizzati un collegamento per reimpostare la password.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="7f1cc-176">Inviare di nuovo il collegamento di conferma tramite posta elettronica</span><span class="sxs-lookup"><span data-stu-id="7f1cc-176">Resend email confirmation link</span></span>

<span data-ttu-id="7f1cc-177">Una volta che un utente crea un nuovo account locale, essi vengono inviati tramite posta elettronica un collegamento di conferma che devono utilizzare prima di poter accedere.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="7f1cc-178">Se l'utente accidentalmente Elimina il messaggio di posta elettronica di conferma o messaggio di posta elettronica non arriva mai, sarà necessario il collegamento di conferma inviato nuovamente.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="7f1cc-179">Le modifiche al codice seguente viene illustrato come abilitare questa opzione.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="7f1cc-180">Aggiungere il seguente metodo helper a fondo il *Controllers\AccountController.cs* file:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="7f1cc-181">Il metodo Register per utilizzare l'helper di nuovo l'aggiornamento:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="7f1cc-182">Aggiornare il metodo di accesso per inviare di nuovo la password se non è stato confermato l'account utente:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="7f1cc-183">Combinare gli account di accesso social networking e locale</span><span class="sxs-lookup"><span data-stu-id="7f1cc-183">Combine social and local login accounts</span></span>

<span data-ttu-id="7f1cc-184">È possibile combinare gli account locali e sociali facendo clic sul collegamento nel messaggio.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="7f1cc-185">Nella sequenza seguente **RickAndMSFT@gmail.com** viene innanzitutto creato un account di accesso locale, ma è possibile creare l'account come un accesso social prima, quindi aggiungere un account di accesso locale.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="7f1cc-186">Fare clic su di **Gestisci** collegamento.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-186">Click on the **Manage** link.</span></span> <span data-ttu-id="7f1cc-187">Si noti il **account di accesso esterni: 0** associata all'account.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="7f1cc-188">Fare clic sul collegamento a un altro registro nel servizio e accettare le richieste di app.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="7f1cc-189">I due account sono stati combinati, sarà possibile eseguire l'accesso con uno di questi account.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="7f1cc-190">Si consiglia agli utenti di aggiungere gli account locali nel caso in cui il log di social networking nel servizio di autenticazione è inattivo o più probabilmente è più possibile accedere al proprio account di social networking.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="7f1cc-191">Nella figura seguente, Tom è una procedura di accesso social (che si può vedere dal **account di accesso esterni: 1** visualizzato nella pagina).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="7f1cc-192">Facendo clic su **Scegli una password** consente di aggiungere un registro locale in associato lo stesso account.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="7f1cc-193">Conferma tramite posta elettronica in modo più approfondito</span><span class="sxs-lookup"><span data-stu-id="7f1cc-193">Email confirmation in more depth</span></span>

<span data-ttu-id="7f1cc-194">L'esercitazione [la conferma dell'Account e Password di ripristino con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entra in questo argomento con maggiori dettagli.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="7f1cc-195">Debug dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="7f1cc-195">Debugging the app</span></span>

<span data-ttu-id="7f1cc-196">Se non si ottiene un messaggio di posta elettronica contenente il collegamento:</span><span class="sxs-lookup"><span data-stu-id="7f1cc-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="7f1cc-197">Controllare la cartella della posta indesiderata o posta indesiderata.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="7f1cc-198">Accedere al proprio account di SendGrid e fare clic su di [collegamento dell'attività di posta elettronica](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="7f1cc-199">Per verificare il collegamento di verifica senza messaggio di posta elettronica, scaricare il [esempio completo](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="7f1cc-200">Nella pagina verranno visualizzati il collegamento di conferma e codici di conferma.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="7f1cc-201">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7f1cc-201">Additional Resources</span></span>

- [<span data-ttu-id="7f1cc-202">Collegamenti a ASP.NET Identity consigliato risorse</span><span class="sxs-lookup"><span data-stu-id="7f1cc-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="7f1cc-203">[Account di conferma e il recupero della Password con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) vengono descritti in dettaglio in Conferma password di ripristino e account.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="7f1cc-204">[App di MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) in questa esercitazione viene illustrato come scrivere un'applicazione ASP.NET MVC 5 con autorizzazione di Facebook e Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="7f1cc-205">Viene inoltre illustrato come aggiungere ulteriori dati al database di identità.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="7f1cc-206">[Distribuire un'app protetta ASP.NET MVC con appartenenza, OAuth e il Database SQL in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="7f1cc-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="7f1cc-207">In questa esercitazione aggiunge distribuzione di Azure, come proteggere le app con i ruoli, come utilizzare l'API di appartenenza per aggiungere utenti e ruoli e funzionalità di sicurezza aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="7f1cc-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="7f1cc-208">Creazione di un'app di Google per OAuth 2 e l'applicazione di connessione al progetto</span><span class="sxs-lookup"><span data-stu-id="7f1cc-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="7f1cc-209">Creazione di app in Facebook e l'applicazione di connessione al progetto</span><span class="sxs-lookup"><span data-stu-id="7f1cc-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="7f1cc-210">Impostazione di SSL nel progetto</span><span class="sxs-lookup"><span data-stu-id="7f1cc-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
