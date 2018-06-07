---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Creare MVC 5 App con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione viene illustrato come compilare un'applicazione web ASP.NET MVC 5 che consente agli utenti di accedere con OAuth 2.0 con le credenziali di un authenti esterni...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: aa4c91865f7b720846a5e8deb4281c3ca6933c8e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819097"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="9ec13-103">Crea un'applicazione ASP.NET MVC 5 con Facebook, Twitter, LinkedIn e Google OAuth2 Sign-on (c#)</span><span class="sxs-lookup"><span data-stu-id="9ec13-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="9ec13-104">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9ec13-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="9ec13-105">In questa esercitazione viene illustrato come compilare un'applicazione web ASP.NET MVC 5 che consente agli utenti di accedere utilizzando [OAuth 2.0](http://oauth.net/2/) con le credenziali di un provider di autenticazione esterna, ad esempio Facebook, Twitter, LinkedIn, Microsoft o Google.</span><span class="sxs-lookup"><span data-stu-id="9ec13-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="9ec13-106">Per semplicità, in questa esercitazione è incentrata sull'utilizzo di credenziali Facebook e Google.</span><span class="sxs-lookup"><span data-stu-id="9ec13-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="9ec13-107">Abilitazione di queste credenziali in siti web offre un vantaggio significativo perché milioni di utenti dispongono già di account con tali provider esterni.</span><span class="sxs-lookup"><span data-stu-id="9ec13-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="9ec13-108">Questi utenti potrebbero essere più inclini a effettuare l'iscrizione per il sito se non dispone di creare e ricordare un nuovo insieme di credenziali.</span><span class="sxs-lookup"><span data-stu-id="9ec13-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="9ec13-109">Vedere anche [app ASP.NET MVC 5 con SMS e l'autenticazione a due fattori di posta elettronica](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="9ec13-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="9ec13-110">Anche l'esercitazione viene illustrato come aggiungere dati di profilo per l'utente e come utilizzare l'API di appartenenza per aggiungere ruoli.</span><span class="sxs-lookup"><span data-stu-id="9ec13-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="9ec13-111">In questa esercitazione è stato scritto dal [Rick Anderson](https://blogs.msdn.com/rickAndy) (me seguire su Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="9ec13-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="9ec13-112">Introduzione</span><span class="sxs-lookup"><span data-stu-id="9ec13-112">Getting Started</span></span>

<span data-ttu-id="9ec13-113">Avviare l'installazione ed eseguendo [Visual Studio Express 2013 per Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="9ec13-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="9ec13-114">Installazione di Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="9ec13-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="9ec13-115">Per informazioni su Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, il flusso, Stack di Exchange, Tripit, Twitch, Twitter, Yahoo! e altro ancora, vedere questo [progetto di esempio](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="9ec13-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="9ec13-116">È necessario installare Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o versione successiva per usare Google OAuth 2 e per eseguire il debug in locale senza avvisi SSL.</span><span class="sxs-lookup"><span data-stu-id="9ec13-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="9ec13-117">Fare clic su **nuovo progetto** dal **avviare** pagina oppure è possibile utilizzare il menu e selezionare **File**e quindi **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="9ec13-118">Creazione di un'applicazione</span><span class="sxs-lookup"><span data-stu-id="9ec13-118">Creating Your First Application</span></span>

<span data-ttu-id="9ec13-119">Fare clic su **nuovo progetto**, quindi selezionare **Visual c#** a sinistra, quindi **Web** e quindi selezionare **applicazione Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="9ec13-120">Denominare il progetto "MvcAuth" e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="9ec13-121">Nel **nuovo progetto ASP.NET** finestra di dialogo, fare clic su **MVC**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="9ec13-122">Se l'autenticazione non **singoli account utente di**, fare clic su di **Modifica autenticazione** e selezionare **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="9ec13-123">Controllando **Host nel cloud**, l'app sarà molto semplice ospitare in Azure.</span><span class="sxs-lookup"><span data-stu-id="9ec13-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="9ec13-124">Se si seleziona **Host nel cloud**, completare la finestra di dialogo Configura.</span><span class="sxs-lookup"><span data-stu-id="9ec13-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="9ec13-125">Utilizzare NuGet per aggiornare il middleware OWIN più recente</span><span class="sxs-lookup"><span data-stu-id="9ec13-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="9ec13-126">Usare Gestione pacchetti NuGet per aggiornare il [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="9ec13-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="9ec13-127">Selezionare **aggiornamenti** nel menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="9ec13-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="9ec13-128">È possibile fare clic su di **Aggiorna tutto** pulsante oppure è possibile cercare solo i pacchetti OWIN (illustrati nella figura seguente):</span><span class="sxs-lookup"><span data-stu-id="9ec13-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="9ec13-129">Nell'immagine seguente, vengono visualizzati solo i pacchetti OWIN:</span><span class="sxs-lookup"><span data-stu-id="9ec13-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="9ec13-130">Dal Package Manager Console (PMC), è possibile immettere il `Update-Package` comando, che aggiornerà tutti i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="9ec13-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="9ec13-131">Premere **F5** o **Ctrl + F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9ec13-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="9ec13-132">Nell'immagine seguente, il numero di porta è 1234.</span><span class="sxs-lookup"><span data-stu-id="9ec13-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="9ec13-133">Quando si esegue l'applicazione, verrà visualizzato un numero di porta diverso.</span><span class="sxs-lookup"><span data-stu-id="9ec13-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="9ec13-134">A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di spostamento per visualizzare il **Home**, **su**, **contatto**, **registrare**e **Accedi** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="9ec13-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="9ec13-135">Impostazione di SSL nel progetto</span><span class="sxs-lookup"><span data-stu-id="9ec13-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="9ec13-136">Per connettersi al provider di autenticazione come Google e Facebook, è necessario configurare IIS Express per utilizzare SSL.</span><span class="sxs-lookup"><span data-stu-id="9ec13-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="9ec13-137">È importante mantenere l'utilizzo di SSL dopo l'accesso e drop non HTTP, il cookie di accesso come segreto come nome utente e password e senza l'utilizzo di SSL che si sta inviando il messaggio in testo non crittografato attraverso la rete.</span><span class="sxs-lookup"><span data-stu-id="9ec13-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="9ec13-138">Inoltre, è già stata acquisita il tempo necessario per eseguire l'handshake e proteggere il canale (ovvero la maggior parte di ciò che rende più lenta rispetto all'HTTP a HTTPS) prima dell'esecuzione della pipeline MVC, pertanto reindirizzamento su HTTP dopo che si è connessi non effettuare la richiesta corrente o futuro richieste è molto più veloce.</span><span class="sxs-lookup"><span data-stu-id="9ec13-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="9ec13-139">In **Esplora**, fare clic su di **MvcAuth** progetto.</span><span class="sxs-lookup"><span data-stu-id="9ec13-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="9ec13-140">Premere il tasto F4 per visualizzare le proprietà del progetto.</span><span class="sxs-lookup"><span data-stu-id="9ec13-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="9ec13-141">In alternativa, dal **vista** menu è possibile selezionare **finestra proprietà**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="9ec13-142">Modifica **SSL abilitato** su True.</span><span class="sxs-lookup"><span data-stu-id="9ec13-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="9ec13-143">Copiare l'URL SSL (che sarà `https://localhost:44300/` a meno che non è stato creato altri progetti SSL).</span><span class="sxs-lookup"><span data-stu-id="9ec13-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="9ec13-144">In **Esplora soluzioni**, fare clic destro la **MvcAuth** progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="9ec13-145">Selezionare il **Web** scheda e quindi incollare l'URL SSL nella **Url progetto** casella.</span><span class="sxs-lookup"><span data-stu-id="9ec13-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="9ec13-146">Salvare il file (CTRL + S).</span><span class="sxs-lookup"><span data-stu-id="9ec13-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="9ec13-147">È necessario l'URL per configurare le app di autenticazione di Facebook e Google.</span><span class="sxs-lookup"><span data-stu-id="9ec13-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="9ec13-148">Aggiungere il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attributo la `Home` controller in modo da richiedere tutte le richieste devono utilizzare HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9ec13-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="9ec13-149">Un approccio più sicuro consiste nell'aggiungere il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtro all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9ec13-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="9ec13-150">Vedere la sezione &quot;proteggere l'applicazione con SSL e l'attributo autorizzare&quot; in my tutoral [creare un'applicazione MVC ASP.NET con autenticazione e il database di SQL Server e distribuire in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="9ec13-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="9ec13-151">Di seguito è riportata una parte del controller Home.</span><span class="sxs-lookup"><span data-stu-id="9ec13-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="9ec13-152">Premere CTRL+F5 per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9ec13-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="9ec13-153">Se il certificato è stato installato in precedenza, è possibile ignorare il resto di questa sezione e passare a [creando un'app di Google per OAuth 2 e l'applicazione di connessione al progetto](#goog), in caso contrario, seguire le istruzioni per considerare attendibile autofirmato certificato che ha generato IIS Express.</span><span class="sxs-lookup"><span data-stu-id="9ec13-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="9ec13-154">Lettura di **avviso di sicurezza** finestra di dialogo e quindi fare clic su **Sì** se si desidera installare il certificato che rappresenta localhost.</span><span class="sxs-lookup"><span data-stu-id="9ec13-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="9ec13-155">Internet Explorer mostra il *Home* pagina e nessun avviso SSL.</span><span class="sxs-lookup"><span data-stu-id="9ec13-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="9ec13-156">Google Chrome, inoltre, accetta il certificato e visualizzerà contenuto HTTPS senza un messaggio di avviso.</span><span class="sxs-lookup"><span data-stu-id="9ec13-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="9ec13-157">Firefox Usa il proprio archivio certificati, quindi verrà visualizzato un avviso.</span><span class="sxs-lookup"><span data-stu-id="9ec13-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="9ec13-158">Per l'applicazione è possibile scegliere l'opzione **comprenderne i rischi**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="9ec13-159">Creazione di un'app di Google per OAuth 2 e l'applicazione di connessione al progetto</span><span class="sxs-lookup"><span data-stu-id="9ec13-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="9ec13-160">Per istruzioni di Google OAuth corrente, vedere [l'autenticazione di Google di configurazione in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="9ec13-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="9ec13-161">Passare il [Google Developers Console](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="9ec13-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="9ec13-162">Se è ancora stato creato un progetto, selezionare **credenziali** nel riquadro a sinistra e quindi selezionare **crea**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="9ec13-163">Nella scheda di sinistra, fare clic su **credenziali**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="9ec13-164">Fare clic su **creare credenziali** quindi **ID client OAuth**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="9ec13-165">Nel **Create Client ID** finestra di dialogo, mantenere il valore predefinito **applicazione Web** per il tipo di applicazione.</span><span class="sxs-lookup"><span data-stu-id="9ec13-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="9ec13-166">Impostare il **autorizzato JavaScript** le origini per l'URL SSL utilizzato in precedenza (`https://localhost:44300/` a meno che non è stato creato altri progetti SSL)</span><span class="sxs-lookup"><span data-stu-id="9ec13-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="9ec13-167">Impostare il **URI di reindirizzamento autorizzato** per:</span><span class="sxs-lookup"><span data-stu-id="9ec13-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="9ec13-168">Scegliere la voce di menu consenso OAuth schermata, quindi impostare il nome di prodotto e l'indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="9ec13-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="9ec13-169">Dopo aver completato il modulo di fare clic su **salvare**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="9ec13-170">Scegliere la voce di menu della raccolta, cercare **Google + API**, fare clic su di esso, quindi premere attiva.</span><span class="sxs-lookup"><span data-stu-id="9ec13-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="9ec13-171">L'immagine seguente mostra le API abilitate.</span><span class="sxs-lookup"><span data-stu-id="9ec13-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="9ec13-172">Da Gestione API Google API, visitare il **credenziali** tab per ottenere il **ID Client**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="9ec13-173">Download per salvare un file JSON con segreti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9ec13-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="9ec13-174">Copiare e incollare il **ClientId** e **ClientSecret** nel `UseGoogleAuthentication` trovato nel metodo il *Startup.Auth.cs* file nel *App_Start* cartella.</span><span class="sxs-lookup"><span data-stu-id="9ec13-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="9ec13-175">Il **ClientId** e **ClientSecret** valori riportati di seguito sono riportati alcuni esempi e non funzionano.</span><span class="sxs-lookup"><span data-stu-id="9ec13-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="9ec13-176">Sicurezza - archivio mai i dati sensibili nel codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="9ec13-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="9ec13-177">L'account e le credenziali vengono aggiunti al codice precedente per semplificare il codice di esempio.</span><span class="sxs-lookup"><span data-stu-id="9ec13-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="9ec13-178">Vedere [procedure consigliate per la distribuzione delle password e altri dati sensibili per ASP.NET e servizio App di Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9ec13-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="9ec13-179">Premere **CTRL + F5** per compilare ed eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9ec13-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="9ec13-180">Fare clic su di **Accedi** collegamento.</span><span class="sxs-lookup"><span data-stu-id="9ec13-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="9ec13-181">In **utilizzare un altro servizio per l'accesso**, fare clic su **Google**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="9ec13-182">Se non è disponibile uno dei passaggi precedenti, si otterrà un errore HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="9ec13-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="9ec13-183">Controlla di nuovo i passaggi precedenti.</span><span class="sxs-lookup"><span data-stu-id="9ec13-183">Recheck your steps above.</span></span> <span data-ttu-id="9ec13-184">Se non è disponibile un'impostazione obbligatoria (ad esempio **nome del prodotto**), aggiungere l'elemento mancante e salvare; può richiedere alcuni minuti per l'uso dell'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="9ec13-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="9ec13-185">Si verrà reindirizzati al sito di Google dove inserire le credenziali.</span><span class="sxs-lookup"><span data-stu-id="9ec13-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="9ec13-186">Dopo aver immesso le credenziali, verrà richiesto di concedere le autorizzazioni per l'applicazione web che appena creato:</span><span class="sxs-lookup"><span data-stu-id="9ec13-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="9ec13-187">Fare clic su **accettare**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-187">Click **Accept**.</span></span> <span data-ttu-id="9ec13-188">Si verrà reindirizzati al **registrare** pagina dell'applicazione MvcAuth in cui è possibile registrare l'account Google.</span><span class="sxs-lookup"><span data-stu-id="9ec13-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="9ec13-189">È possibile scegliere di modificare il nome di registrazione di posta elettronica locale utilizzato per l'account Gmail, ma in genere si desidera mantenere l'alias di posta elettronica predefinito (vale a dire quello utilizzato per l'autenticazione).</span><span class="sxs-lookup"><span data-stu-id="9ec13-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="9ec13-190">Fare clic su **Register**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="9ec13-191">Creazione di app in Facebook e l'applicazione di connessione al progetto</span><span class="sxs-lookup"><span data-stu-id="9ec13-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="9ec13-192">Per istruzioni di autenticazione Facebook OAuth2 corrente, vedere [l'autenticazione di configurazione di Facebook](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="9ec13-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="9ec13-193">Per l'autenticazione Facebook OAuth2, è necessario copiare nel progetto alcune impostazioni da un'applicazione creata con Facebook.</span><span class="sxs-lookup"><span data-stu-id="9ec13-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="9ec13-194">Nel browser, passare a [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) e accedere immettendo le credenziali di Facebook.</span><span class="sxs-lookup"><span data-stu-id="9ec13-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="9ec13-195">Se non sono già registrati come uno sviluppatore di Facebook, fare clic su **registrare gli sviluppatori** e seguire le istruzioni per registrare.</span><span class="sxs-lookup"><span data-stu-id="9ec13-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="9ec13-196">Nel **app** scheda, fare clic su **Create New App**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![Crea nuova app](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="9ec13-198">Immettere un **nome App** e **categoria**, quindi fare clic su **creare App**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="9ec13-199">Il <strong>App Namespace</strong> è la parte dell'URL utilizzato per accedere all'applicazione di Facebook per l'autenticazione dell'App (ad esempio, https\://apps.facebook.com/{App Namespace}).</span><span class="sxs-lookup"><span data-stu-id="9ec13-199">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https\://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="9ec13-200">Se non si specifica un <strong>Namespace di App</strong>, <strong>ID App</strong> verrà utilizzato per l'URL.</span><span class="sxs-lookup"><span data-stu-id="9ec13-200">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="9ec13-201">Il <strong>ID App</strong> è un numero lungo generato dal sistema che verrà visualizzato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="9ec13-201">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![Creare una finestra di dialogo Nuova App](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="9ec13-203">Inviare il controllo di sicurezza standard.</span><span class="sxs-lookup"><span data-stu-id="9ec13-203">Submit the standard security check.</span></span>

    ![Controllo di sicurezza](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="9ec13-205">Selezionare **impostazioni** per la barra dei menu a sinistra.![ Barra dei menu per gli sviluppatori di Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="9ec13-205">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="9ec13-206">Nel **base** sezione delle impostazioni della pagina selezionare **aggiungere piattaforma** per specificare che si sta aggiungendo un'applicazione del sito Web.</span><span class="sxs-lookup"><span data-stu-id="9ec13-206">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="9ec13-207">![Impostazioni di base](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="9ec13-207">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="9ec13-208">Selezionare **sito Web** tra le opzioni di piattaforma.</span><span class="sxs-lookup"><span data-stu-id="9ec13-208">Select **Website** from the platform choices.</span></span>  
  
    ![Scelte di piattaforma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="9ec13-210">Annotare il **ID App** e **segreto dell'applicazione** in modo che è possibile aggiungere entrambe nell'applicazione MVC più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9ec13-210">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="9ec13-211">Inoltre, aggiungere l'URL del sito (`https://localhost:44300/`) per testare l'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="9ec13-211">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="9ec13-212">Inoltre, aggiungere un **Contact Email**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-212">Also, add a **Contact Email**.</span></span> <span data-ttu-id="9ec13-213">Selezionare quindi **Salva modifiche**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-213">Then, select **Save Changes**.</span></span>   

    ![Pagina dei dettagli di un'applicazione di base](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="9ec13-215">Si noti che solo sarà in grado di eseguire l'autenticazione utilizzando l'alias di posta elettronica che è stato registrato.</span><span class="sxs-lookup"><span data-stu-id="9ec13-215">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="9ec13-216">Non sarà in grado di registrare altri utenti e account di prova.</span><span class="sxs-lookup"><span data-stu-id="9ec13-216">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="9ec13-217">È possibile concedere l'accesso agli account altri Facebook per l'applicazione su Facebook **ruolo di sviluppatore** scheda.</span><span class="sxs-lookup"><span data-stu-id="9ec13-217">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="9ec13-218">In Visual Studio, aprire *App\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="9ec13-218">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="9ec13-219">Copiare e incollare il **AppId** e **segreto dell'App** nel `UseFacebookAuthentication` metodo.</span><span class="sxs-lookup"><span data-stu-id="9ec13-219">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="9ec13-220">Il **AppId** e **segreto dell'App** valori riportati di seguito sono riportati alcuni esempi e non funzionerà.</span><span class="sxs-lookup"><span data-stu-id="9ec13-220">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="9ec13-221">Fare clic su **salvare modifiche**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-221">Click **Save Changes**.</span></span>
13. <span data-ttu-id="9ec13-222">Premere **CTRL + F5** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9ec13-222">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="9ec13-223">Selezionare **Accedi** per visualizzare la pagina di accesso.</span><span class="sxs-lookup"><span data-stu-id="9ec13-223">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="9ec13-224">Fare clic su **Facebook** in **utilizzare un altro servizio per l'accesso.**</span><span class="sxs-lookup"><span data-stu-id="9ec13-224">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="9ec13-225">Immettere le credenziali di Facebook.</span><span class="sxs-lookup"><span data-stu-id="9ec13-225">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="9ec13-226">Verrà richiesto di concedere l'autorizzazione per l'applicazione accedere al profilo pubblico e un elenco di tipo friend.</span><span class="sxs-lookup"><span data-stu-id="9ec13-226">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Dettagli dell'app Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="9ec13-228">Ora connessi.</span><span class="sxs-lookup"><span data-stu-id="9ec13-228">You are now logged in.</span></span> <span data-ttu-id="9ec13-229">È ora possibile registrare questo account con l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9ec13-229">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="9ec13-230">Quando si registra, viene aggiunta una voce per il *utenti* tabella del database delle appartenenze.</span><span class="sxs-lookup"><span data-stu-id="9ec13-230">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="9ec13-231">Esaminare i dati di appartenenza</span><span class="sxs-lookup"><span data-stu-id="9ec13-231">Examine the Membership Data</span></span>

<span data-ttu-id="9ec13-232">Nel **vista** menu, fare clic su **Esplora Server**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-232">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="9ec13-233">Espandere **DefaultConnection (MvcAuth)**, espandere **tabelle**, fare clic destro **AspNetUsers** e fare clic su **Mostra dati tabella**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-233">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dati della tabella aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="9ec13-235">Aggiunta di dati di profilo per la classe utente</span><span class="sxs-lookup"><span data-stu-id="9ec13-235">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="9ec13-236">In questa sezione si aggiungerà la data di nascita e città natale ai dati utente durante la registrazione, come illustrato nella figura seguente.</span><span class="sxs-lookup"><span data-stu-id="9ec13-236">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg con città natale e Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="9ec13-238">Aprire il *Models\IdentityModels.cs* file e aggiungere le proprietà di città home page e data di nascita:</span><span class="sxs-lookup"><span data-stu-id="9ec13-238">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="9ec13-239">Aprire il *Models\AccountViewModels.cs* file e il set di date e home proprietà città di nascita `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="9ec13-239">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="9ec13-240">Aprire il *Controllers\AccountController.cs* file e aggiungere il codice per città home page e data di nascita nel `ExternalLoginConfirmation` metodo di azione, come illustrato:</span><span class="sxs-lookup"><span data-stu-id="9ec13-240">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="9ec13-241">Aggiungere la data di nascita e città natale per il *Views\Account\ExternalLoginConfirmation.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="9ec13-241">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="9ec13-242">Eliminare il database di appartenenza che consente di registrare l'account di Facebook con l'applicazione e verificare che è possibile aggiungere la nuova data di nascita e le informazioni sul profilo città natale nuovamente.</span><span class="sxs-lookup"><span data-stu-id="9ec13-242">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="9ec13-243">Da **Esplora**, fare clic su di **Mostra tutti i file** icona, quindi fare clic destro *Aggiungi\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;fileconestensionemdf* e fare clic su **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-243">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="9ec13-244">Dal **strumenti** menu, fare clic su **Gestione pacchetti NuGet**, quindi fare clic su **Package Manager Console** (PMC).</span><span class="sxs-lookup"><span data-stu-id="9ec13-244">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="9ec13-245">Immettere i comandi seguenti in PMC.</span><span class="sxs-lookup"><span data-stu-id="9ec13-245">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="9ec13-246">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="9ec13-246">Enable-Migrations</span></span>
2. <span data-ttu-id="9ec13-247">Migrazione aggiungere Init</span><span class="sxs-lookup"><span data-stu-id="9ec13-247">Add-Migration Init</span></span>
3. <span data-ttu-id="9ec13-248">Update-Database</span><span class="sxs-lookup"><span data-stu-id="9ec13-248">Update-Database</span></span>

<span data-ttu-id="9ec13-249">Eseguire l'applicazione e utilizzare FaceBook e Google per accedere e registrare alcuni utenti.</span><span class="sxs-lookup"><span data-stu-id="9ec13-249">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="9ec13-250">Esaminare i dati di appartenenza</span><span class="sxs-lookup"><span data-stu-id="9ec13-250">Examine the Membership Data</span></span>

<span data-ttu-id="9ec13-251">Nel **vista** menu, fare clic su **Esplora Server**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-251">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="9ec13-252">Fare clic destro **AspNetUsers** e fare clic su **Mostra dati tabella**.</span><span class="sxs-lookup"><span data-stu-id="9ec13-252">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="9ec13-253">Il `HomeTown` e `BirthDate` campi sono illustrati di seguito.</span><span class="sxs-lookup"><span data-stu-id="9ec13-253">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="9ec13-254">La disconnessione dell'App e accedere con un altro Account</span><span class="sxs-lookup"><span data-stu-id="9ec13-254">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="9ec13-255">Se si accede all'App con Facebook e quindi disconnettersi e riprovare ad accedere nuovamente con un account Facebook diverso (utilizzando lo stesso browser), verrà immediatamente eseguito per l'account Facebook precedente che è stato utilizzato.</span><span class="sxs-lookup"><span data-stu-id="9ec13-255">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="9ec13-256">Per utilizzare un altro account, è necessario passare a Facebook e la disconnessione a Facebook.</span><span class="sxs-lookup"><span data-stu-id="9ec13-256">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="9ec13-257">La stessa regola si applica a qualsiasi altro 3rd provider di autenticazione entità.</span><span class="sxs-lookup"><span data-stu-id="9ec13-257">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="9ec13-258">In alternativa, è possibile accedere con un altro account usando un browser diverso.</span><span class="sxs-lookup"><span data-stu-id="9ec13-258">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ec13-259">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="9ec13-259">Next Steps</span></span>

<span data-ttu-id="9ec13-260">Vedere [introdurre i provider di sicurezza Yahoo e OAuth di LinkedIn per OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) da Jerrie Pelser per le istruzioni di Yahoo e LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="9ec13-260">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="9ec13-261">Vedere del Jerrie piuttosto pulsanti di accesso social per ASP.NET MVC 5 ottenere l'account di accesso social abilitano i pulsanti.</span><span class="sxs-lookup"><span data-stu-id="9ec13-261">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="9ec13-262">Seguire l'esercitazione [creare un'applicazione MVC ASP.NET con autenticazione e il database di SQL Server e distribuire in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), che continua a questa esercitazione e Mostra le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9ec13-262">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="9ec13-263">Come distribuire l'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="9ec13-263">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="9ec13-264">Come proteggere app con i ruoli.</span><span class="sxs-lookup"><span data-stu-id="9ec13-264">How to secure you app with roles.</span></span>
3. <span data-ttu-id="9ec13-265">Come proteggere le app con il [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) e [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtri.</span><span class="sxs-lookup"><span data-stu-id="9ec13-265">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="9ec13-266">Come utilizzare l'API di appartenenza per aggiungere utenti e ruoli.</span><span class="sxs-lookup"><span data-stu-id="9ec13-266">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="9ec13-267">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione ed è stato possibile apportare miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="9ec13-267">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="9ec13-268">È inoltre possibile richiedere nuovi argomenti in [Mostra Me come con codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="9ec13-268">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="9ec13-269">È anche possibile chiedere e votare nuove caratteristiche per essere aggiunto ad ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9ec13-269">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="9ec13-270">Ad esempio, è possibile votare per uno strumento per [creare e gestire utenti e ruoli.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="9ec13-270">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="9ec13-271">Per una spiegazione chiara del funzionamento di servizi di autenticazione esterno di ASP.NET, vedere di Robert McMurray [servizi di autenticazione esterni](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="9ec13-271">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="9ec13-272">Articolo Paolo inseriti anche nel dettaglio abilitare l'autenticazione di Microsoft e Twitter.</span><span class="sxs-lookup"><span data-stu-id="9ec13-272">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="9ec13-273">Tom Dykstra dell'eccellente [esercitazione EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) viene illustrato l'utilizzo con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9ec13-273">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
