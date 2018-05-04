---
title: Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione
author: rick-anderson
description: Informazioni su come creare un'app di pagine Razor con dati dell'utente protetti dall'autorizzazione. Include HTTPS, l'autenticazione, protezione, ASP.NET Identity Core.
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: e42f299efcae7c6a0e3d20b157c591eed98c99d0
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="4e026-104">Crea un'applicazione ASP.NET di base con i dati dell'utente protetti dall'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="4e026-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="4e026-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="4e026-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="4e026-106">In questa esercitazione viene illustrato come creare un'app web ASP.NET di base con i dati utente protetti dall'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="4e026-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="4e026-107">Viene visualizzato un elenco di contatti che autenticati (registrato) creato.</span><span class="sxs-lookup"><span data-stu-id="4e026-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="4e026-108">Sono disponibili tre gruppi di sicurezza:</span><span class="sxs-lookup"><span data-stu-id="4e026-108">There are three security groups:</span></span>

* <span data-ttu-id="4e026-109">**Gli utenti registrati** può visualizzare tutti i dati approvati e possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="4e026-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="4e026-110">**Gestori** possono approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="4e026-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="4e026-111">Solo i contatti approvati sono visibili agli utenti.</span><span class="sxs-lookup"><span data-stu-id="4e026-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="4e026-112">**Gli amministratori** può approvare/rifiutare e tutti i dati di modifica/eliminazione.</span><span class="sxs-lookup"><span data-stu-id="4e026-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="4e026-113">Nella figura seguente, l'utente Rick (`rick@example.com`) ha effettuato l'accesso.</span><span class="sxs-lookup"><span data-stu-id="4e026-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="4e026-114">Rick possono visualizzare solo i contatti approvati e **modifica**/**eliminare**/**Crea nuovo** collegamenti per il suo contatti.</span><span class="sxs-lookup"><span data-stu-id="4e026-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="4e026-115">Solo l'ultimo record, creato da Rick, Visualizza **modifica** e **eliminare** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="4e026-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="4e026-116">Altri utenti vedere dell'ultimo record sarà responsabile o un amministratore modifica lo stato su "Approvata".</span><span class="sxs-lookup"><span data-stu-id="4e026-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![immagine descritta precedente](secure-data/_static/rick.png)

<span data-ttu-id="4e026-118">Nella figura seguente, `manager@contoso.com` l'accesso e il ruolo di Manager:</span><span class="sxs-lookup"><span data-stu-id="4e026-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![immagine descritta precedente](secure-data/_static/manager1.png)

<span data-ttu-id="4e026-120">L'immagine seguente mostra i gestori di visualizzazione dei dettagli di un contatto:</span><span class="sxs-lookup"><span data-stu-id="4e026-120">The following image shows the managers details view of a contact:</span></span>

![immagine descritta precedente](secure-data/_static/manager.png)

<span data-ttu-id="4e026-122">Il **Approva** e **rifiutare** pulsanti vengono visualizzati solo per i gestori e amministratori.</span><span class="sxs-lookup"><span data-stu-id="4e026-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="4e026-123">Nella figura seguente, `admin@contoso.com` l'accesso e al ruolo administrators:</span><span class="sxs-lookup"><span data-stu-id="4e026-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![immagine descritta precedente](secure-data/_static/admin.png)

<span data-ttu-id="4e026-125">L'amministratore ha tutti i privilegi.</span><span class="sxs-lookup"><span data-stu-id="4e026-125">The administrator has all privileges.</span></span> <span data-ttu-id="4e026-126">È possibile lettura/modifica/eliminazione di un contatto e modificare lo stato dei contatti.</span><span class="sxs-lookup"><span data-stu-id="4e026-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="4e026-127">L'app è stato creato da [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) seguenti `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="4e026-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="4e026-128">L'esempio include i gestori di autorizzazione seguente:</span><span class="sxs-lookup"><span data-stu-id="4e026-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="4e026-129">`ContactIsOwnerAuthorizationHandler`: Assicura che un utente può modificare solo i dati.</span><span class="sxs-lookup"><span data-stu-id="4e026-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="4e026-130">`ContactManagerAuthorizationHandler`: Consente ai responsabili di approvare o rifiutare i contatti.</span><span class="sxs-lookup"><span data-stu-id="4e026-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="4e026-131">`ContactAdministratorsAuthorizationHandler`: Consente agli amministratori di approvare o rifiutare i contatti e modifica/eliminazione di contatti.</span><span class="sxs-lookup"><span data-stu-id="4e026-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e026-132">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4e026-132">Prerequisites</span></span>

<span data-ttu-id="4e026-133">In questa esercitazione viene anticipata.</span><span class="sxs-lookup"><span data-stu-id="4e026-133">This tutorial is advanced.</span></span> <span data-ttu-id="4e026-134">È necessario avere familiarità con:</span><span class="sxs-lookup"><span data-stu-id="4e026-134">You should be familiar with:</span></span>

* [<span data-ttu-id="4e026-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e026-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="4e026-136">Autenticazione</span><span class="sxs-lookup"><span data-stu-id="4e026-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="4e026-137">Conferma account e recupero password</span><span class="sxs-lookup"><span data-stu-id="4e026-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="4e026-138">Autorizzazione</span><span class="sxs-lookup"><span data-stu-id="4e026-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="4e026-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4e026-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="4e026-140">Vedere [questo file PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) per la versione di ASP.NET MVC di base.</span><span class="sxs-lookup"><span data-stu-id="4e026-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="4e026-141">La versione di ASP.NET 1.1 componenti di base di questa esercitazione è nel [questo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) cartella.</span><span class="sxs-lookup"><span data-stu-id="4e026-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="4e026-142">1.1 esempio ASP.NET Core è il [esempi](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="4e026-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="4e026-143">L'avvio e l'app completata</span><span class="sxs-lookup"><span data-stu-id="4e026-143">The starter and completed app</span></span>

<span data-ttu-id="4e026-144">[Scaricare](xref:tutorials/index#how-to-download-a-sample) il [completato](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span><span class="sxs-lookup"><span data-stu-id="4e026-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="4e026-145">[Test](#test-the-completed-app) app completata in modo da acquisire familiarità con le funzionalità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="4e026-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="4e026-146">L'applicazione di avvio</span><span class="sxs-lookup"><span data-stu-id="4e026-146">The starter app</span></span>

<span data-ttu-id="4e026-147">[Scaricare](xref:tutorials/index#how-to-download-a-sample) il [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span><span class="sxs-lookup"><span data-stu-id="4e026-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="4e026-148">Eseguire l'app, toccare il **ContactManager** collegare e verificare è possibile creare, modificare ed eliminare un contatto.</span><span class="sxs-lookup"><span data-stu-id="4e026-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="4e026-149">Proteggere i dati utente</span><span class="sxs-lookup"><span data-stu-id="4e026-149">Secure user data</span></span>

<span data-ttu-id="4e026-150">Nelle sezioni seguenti sono tutti i passaggi principali per creare l'app di dati utente.</span><span class="sxs-lookup"><span data-stu-id="4e026-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="4e026-151">Può risultare utile per fare riferimento al progetto completato.</span><span class="sxs-lookup"><span data-stu-id="4e026-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="4e026-152">Associare i dati di contatto per l'utente</span><span class="sxs-lookup"><span data-stu-id="4e026-152">Tie the contact data to the user</span></span>

<span data-ttu-id="4e026-153">Utilizzare ASP.NET [identità](xref:security/authentication/identity) ID utente per accertarsi che gli utenti possono modificare i propri dati, ma non altri dati di utenti.</span><span class="sxs-lookup"><span data-stu-id="4e026-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="4e026-154">Aggiungere `OwnerID` e `ContactStatus` per il `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="4e026-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="4e026-155">`OwnerID` è l'ID dell'utente dal `AspNetUser` tabella il [identità](xref:security/authentication/identity) database.</span><span class="sxs-lookup"><span data-stu-id="4e026-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="4e026-156">Il `Status` campo determina se un contatto visibile dagli utenti generale.</span><span class="sxs-lookup"><span data-stu-id="4e026-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="4e026-157">Crea una nuova migrazione e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="4e026-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="4e026-158">HTTPS e gli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="4e026-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="4e026-159">Aggiungere [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) a `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4e026-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="4e026-160">Nel `ConfigureServices` metodo il *Startup.cs* file, aggiungere il [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro di autorizzazione:</span><span class="sxs-lookup"><span data-stu-id="4e026-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="4e026-161">Se si utilizza Visual Studio, abilitare HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e026-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="4e026-162">Per reindirizzare le richieste HTTP a HTTPS, vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="4e026-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="4e026-163">Se si sta utilizzando Visual Studio Code o test in una piattaforma locale che non include un certificato di prova per HTTPS:</span><span class="sxs-lookup"><span data-stu-id="4e026-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="4e026-164">Impostare `"LocalTest:skipSSL": true` nel *appsettings. Developement.JSON* file.</span><span class="sxs-lookup"><span data-stu-id="4e026-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="4e026-165">Richiedere agli utenti autenticati</span><span class="sxs-lookup"><span data-stu-id="4e026-165">Require authenticated users</span></span>

<span data-ttu-id="4e026-166">Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato.</span><span class="sxs-lookup"><span data-stu-id="4e026-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="4e026-167">È possibile rifiutare esplicitamente l'autenticazione a livello di metodo di azione, controller o pagina Razor con il `[AllowAnonymous]` attributo.</span><span class="sxs-lookup"><span data-stu-id="4e026-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="4e026-168">L'impostazione del criterio di autenticazione predefinito per richiedere agli utenti di essere autenticato protegge appena aggiunto pagine Razor e controller.</span><span class="sxs-lookup"><span data-stu-id="4e026-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="4e026-169">La presenza di autenticazione richiesta per impostazione predefinita è più sicuro di basarsi su nuovi controller e pagine Razor per includere il `[Authorize]` attributo.</span><span class="sxs-lookup"><span data-stu-id="4e026-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="4e026-170">Con il requisito di tutti gli utenti autenticati, il [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) e [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) chiamate non sono necessari.</span><span class="sxs-lookup"><span data-stu-id="4e026-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="4e026-171">Aggiornamento `ConfigureServices` con le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="4e026-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="4e026-172">Impostare come commento `AuthorizeFolder` e `AuthorizePage`.</span><span class="sxs-lookup"><span data-stu-id="4e026-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="4e026-173">Impostare i criteri di autenticazione predefinito per richiedere agli utenti di essere autenticato.</span><span class="sxs-lookup"><span data-stu-id="4e026-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="4e026-174">Aggiungere [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) all'indice, contattare e sulle pagine in modo gli utenti anonimi possono ottenere informazioni sul sito per la registrazione.</span><span class="sxs-lookup"><span data-stu-id="4e026-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="4e026-175">Aggiungere `[AllowAnonymous]` per il [LoginModel e RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="4e026-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="4e026-176">Configurare l'account di test</span><span class="sxs-lookup"><span data-stu-id="4e026-176">Configure the test account</span></span>

<span data-ttu-id="4e026-177">La `SeedData` classe crea due account: amministratore e gestione.</span><span class="sxs-lookup"><span data-stu-id="4e026-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="4e026-178">Utilizzare il [lo strumento Gestione segreto](xref:security/app-secrets) per impostare una password per questi account.</span><span class="sxs-lookup"><span data-stu-id="4e026-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="4e026-179">Impostare la password della directory del progetto (la directory contenente *Program.cs*):</span><span class="sxs-lookup"><span data-stu-id="4e026-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="4e026-180">Aggiornamento `Main` per utilizzare la password di test:</span><span class="sxs-lookup"><span data-stu-id="4e026-180">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="4e026-181">Creare gli account di test e aggiornare i contatti</span><span class="sxs-lookup"><span data-stu-id="4e026-181">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="4e026-182">Aggiornamento di `Initialize` metodo la `SeedData` classe per creare gli account di test:</span><span class="sxs-lookup"><span data-stu-id="4e026-182">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="4e026-183">Aggiungere l'ID utente di amministratore e `ContactStatus` ai contatti.</span><span class="sxs-lookup"><span data-stu-id="4e026-183">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="4e026-184">Effettuare uno dei contatti "Inviato" e "Rejected" uno.</span><span class="sxs-lookup"><span data-stu-id="4e026-184">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="4e026-185">Aggiungere l'ID utente e lo stato di tutti i contatti.</span><span class="sxs-lookup"><span data-stu-id="4e026-185">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="4e026-186">Viene visualizzato un solo contatto:</span><span class="sxs-lookup"><span data-stu-id="4e026-186">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="4e026-187">Creare i gestori di autorizzazione di amministratore, manager e proprietario</span><span class="sxs-lookup"><span data-stu-id="4e026-187">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="4e026-188">Creare un `ContactIsOwnerAuthorizationHandler` classe il *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="4e026-188">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="4e026-189">Il `ContactIsOwnerAuthorizationHandler` verifica che l'utente che agisce su una risorsa proprietario della risorsa.</span><span class="sxs-lookup"><span data-stu-id="4e026-189">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="4e026-190">Il `ContactIsOwnerAuthorizationHandler` chiamate [contesto. Esito positivo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) se l'utente autenticato corrente è il proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="4e026-190">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="4e026-191">I gestori di autorizzazione in genere:</span><span class="sxs-lookup"><span data-stu-id="4e026-191">Authorization handlers generally:</span></span>

* <span data-ttu-id="4e026-192">Restituire `context.Succeed` quando vengono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="4e026-192">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="4e026-193">Restituire `Task.CompletedTask` quando non sono soddisfatti i requisiti.</span><span class="sxs-lookup"><span data-stu-id="4e026-193">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="4e026-194">`Task.CompletedTask` è né esito positivo o negativo&mdash;consente altri gestori di autorizzazione per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4e026-194">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="4e026-195">Se è necessario eseguire in modo esplicito, restituire [contesto. Esito negativo](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="4e026-195">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="4e026-196">L'app consente ai proprietari di contatto di modifica, eliminare o creare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="4e026-196">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="4e026-197">`ContactIsOwnerAuthorizationHandler` non è necessario verificare l'operazione passato nel parametro requisito.</span><span class="sxs-lookup"><span data-stu-id="4e026-197">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="4e026-198">Creare un gestore di autorizzazione di gestione</span><span class="sxs-lookup"><span data-stu-id="4e026-198">Create a manager authorization handler</span></span>

<span data-ttu-id="4e026-199">Creare un `ContactManagerAuthorizationHandler` classe il *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="4e026-199">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="4e026-200">Il `ContactManagerAuthorizationHandler` verifica l'utente che opera la risorsa è un responsabile.</span><span class="sxs-lookup"><span data-stu-id="4e026-200">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="4e026-201">Solo i manager possono approvare o rifiutare le modifiche di contenuto (nuove o modificate).</span><span class="sxs-lookup"><span data-stu-id="4e026-201">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="4e026-202">Creare un gestore autorizzazioni di amministratore</span><span class="sxs-lookup"><span data-stu-id="4e026-202">Create an administrator authorization handler</span></span>

<span data-ttu-id="4e026-203">Creare un `ContactAdministratorsAuthorizationHandler` classe il *autorizzazione* cartella.</span><span class="sxs-lookup"><span data-stu-id="4e026-203">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="4e026-204">Il `ContactAdministratorsAuthorizationHandler` verifica l'utente che opera la risorsa è un amministratore.</span><span class="sxs-lookup"><span data-stu-id="4e026-204">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="4e026-205">Amministratore può eseguire tutte le operazioni.</span><span class="sxs-lookup"><span data-stu-id="4e026-205">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="4e026-206">Registrare i gestori di eventi di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="4e026-206">Register the authorization handlers</span></span>

<span data-ttu-id="4e026-207">Deve essere registrata con Entity Framework Core Services [inserimento di dipendenze](xref:fundamentals/dependency-injection) utilizzando [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="4e026-207">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="4e026-208">Il `ContactIsOwnerAuthorizationHandler` utilizza ASP.NET Core [identità](xref:security/authentication/identity), che si basa su Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4e026-208">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="4e026-209">Registrare i gestori con la raccolta di servizio in modo che siano disponibili per il `ContactsController` tramite [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4e026-209">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="4e026-210">Aggiungere il codice seguente alla fine di `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4e026-210">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="4e026-211">`ContactAdministratorsAuthorizationHandler` e `ContactManagerAuthorizationHandler` vengono aggiunti come singleton.</span><span class="sxs-lookup"><span data-stu-id="4e026-211">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="4e026-212">Perché non usano Entity Framework e tutte le informazioni necessarie, ma sono singleton di `Context` parametro del `HandleRequirementAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="4e026-212">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="4e026-213">Autorizzazione di supporto</span><span class="sxs-lookup"><span data-stu-id="4e026-213">Support authorization</span></span>

<span data-ttu-id="4e026-214">In questa sezione aggiornare le pagine Razor e aggiungere una classe di requisiti di operazioni.</span><span class="sxs-lookup"><span data-stu-id="4e026-214">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="4e026-215">Esaminare la classe di requisiti di operazioni di contatto</span><span class="sxs-lookup"><span data-stu-id="4e026-215">Review the contact operations requirements class</span></span>

<span data-ttu-id="4e026-216">Esaminare la `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="4e026-216">Review the `ContactOperations` class.</span></span> <span data-ttu-id="4e026-217">Questa classe contiene i requisiti di supporta l'app:</span><span class="sxs-lookup"><span data-stu-id="4e026-217">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="4e026-218">Creare una classe base per le pagine Razor</span><span class="sxs-lookup"><span data-stu-id="4e026-218">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="4e026-219">Creare una classe di base che contiene i servizi usati nei contatti pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="4e026-219">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="4e026-220">La classe di base inserisce il codice di inizializzazione in un'unica posizione:</span><span class="sxs-lookup"><span data-stu-id="4e026-220">The base class puts that initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="4e026-221">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="4e026-221">The preceding code:</span></span>

* <span data-ttu-id="4e026-222">Aggiunge il `IAuthorizationService` accesso ai gestori di autorizzazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="4e026-222">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="4e026-223">Aggiunge l'identità `UserManager` servizio.</span><span class="sxs-lookup"><span data-stu-id="4e026-223">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="4e026-224">Aggiungere l'oggetto `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="4e026-224">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="4e026-225">Aggiornare il CreateModel</span><span class="sxs-lookup"><span data-stu-id="4e026-225">Update the CreateModel</span></span>

<span data-ttu-id="4e026-226">Aggiornare il costruttore di modello della pagina Crea per utilizzare il `DI_BasePageModel` classe di base:</span><span class="sxs-lookup"><span data-stu-id="4e026-226">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="4e026-227">Aggiornamento di `CreateModel.OnPostAsync` metodo:</span><span class="sxs-lookup"><span data-stu-id="4e026-227">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="4e026-228">Aggiungere l'ID utente di `Contact` modello.</span><span class="sxs-lookup"><span data-stu-id="4e026-228">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="4e026-229">Chiamare il gestore dell'autorizzazione per verificare che l'utente disponga delle autorizzazioni per creare i contatti.</span><span class="sxs-lookup"><span data-stu-id="4e026-229">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="4e026-230">Aggiornare il IndexModel</span><span class="sxs-lookup"><span data-stu-id="4e026-230">Update the IndexModel</span></span>

<span data-ttu-id="4e026-231">Aggiornamento di `OnGetAsync` metodo pertanto solo i contatti approvati vengono visualizzati per gli utenti:</span><span class="sxs-lookup"><span data-stu-id="4e026-231">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="4e026-232">Aggiornare il EditModel</span><span class="sxs-lookup"><span data-stu-id="4e026-232">Update the EditModel</span></span>

<span data-ttu-id="4e026-233">Aggiungere un gestore di autorizzazione per verificare che l'utente proprietario del contatto.</span><span class="sxs-lookup"><span data-stu-id="4e026-233">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="4e026-234">Poiché l'autorizzazione per le risorse in fase di convalida, il `[Authorize]` attributo non è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="4e026-234">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="4e026-235">L'app non dispone dell'accesso alla risorsa quando vengono valutati gli attributi.</span><span class="sxs-lookup"><span data-stu-id="4e026-235">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="4e026-236">Autorizzazione basata sulle risorse deve essere imperativo.</span><span class="sxs-lookup"><span data-stu-id="4e026-236">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="4e026-237">Controlli devono essere eseguiti una volta che l'app ha accesso alla risorsa, è possibile caricarlo nel modello di pagina oppure caricarlo all'interno del gestore stesso.</span><span class="sxs-lookup"><span data-stu-id="4e026-237">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="4e026-238">Si accede di frequente la risorsa passando la chiave della risorsa.</span><span class="sxs-lookup"><span data-stu-id="4e026-238">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="4e026-239">Aggiornare il DeleteModel</span><span class="sxs-lookup"><span data-stu-id="4e026-239">Update the DeleteModel</span></span>

<span data-ttu-id="4e026-240">Aggiornare il modello di pagina di eliminazione per utilizzare il gestore dell'autorizzazione per verificare che l'utente disponga dell'autorizzazione di eliminazione per il contatto.</span><span class="sxs-lookup"><span data-stu-id="4e026-240">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="4e026-241">Inserire il servizio di autorizzazione nelle viste</span><span class="sxs-lookup"><span data-stu-id="4e026-241">Inject the authorization service into the views</span></span>

<span data-ttu-id="4e026-242">Attualmente, illustrato nell'interfaccia utente modifica ed elimina i collegamenti per l'utente non è possibile modificare i dati.</span><span class="sxs-lookup"><span data-stu-id="4e026-242">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="4e026-243">L'interfaccia utente è stato risolto applicando il gestore dell'autorizzazione per le viste.</span><span class="sxs-lookup"><span data-stu-id="4e026-243">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="4e026-244">Inserire il servizio di autorizzazione nel *Views/_ViewImports.cshtml* file in modo che sia disponibile per tutte le viste:</span><span class="sxs-lookup"><span data-stu-id="4e026-244">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="4e026-245">Il codice precedente aggiunge diverse `using` istruzioni.</span><span class="sxs-lookup"><span data-stu-id="4e026-245">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="4e026-246">Aggiornamento di **modifica** e **eliminare** di collegamenti in *Pages/Contacts/Index.cshtml* in modo vengono sottoposti a rendering solo per gli utenti con le autorizzazioni appropriate:</span><span class="sxs-lookup"><span data-stu-id="4e026-246">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="4e026-247">Nascondere i collegamenti da utenti che non dispone dell'autorizzazione per modificare i dati non proteggere l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4e026-247">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="4e026-248">Nascondere i collegamenti rende più semplici l'app visualizzando i collegamenti sono validi.</span><span class="sxs-lookup"><span data-stu-id="4e026-248">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="4e026-249">Gli utenti possono hack gli URL generati per richiamare modifica ed eliminazione di operazioni sui dati che non possiedono.</span><span class="sxs-lookup"><span data-stu-id="4e026-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="4e026-250">Pagina Razor o del controller deve applicare controlli di accesso per proteggere i dati.</span><span class="sxs-lookup"><span data-stu-id="4e026-250">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="4e026-251">L'aggiornamento dei dettagli</span><span class="sxs-lookup"><span data-stu-id="4e026-251">Update Details</span></span>

<span data-ttu-id="4e026-252">Aggiornare la visualizzazione dei dettagli in modo che i responsabili possano approvare o rifiutare contatti:</span><span class="sxs-lookup"><span data-stu-id="4e026-252">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="4e026-253">Aggiornare il modello di pagina dei dettagli:</span><span class="sxs-lookup"><span data-stu-id="4e026-253">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="4e026-254">Testare l'app completata</span><span class="sxs-lookup"><span data-stu-id="4e026-254">Test the completed app</span></span>

<span data-ttu-id="4e026-255">Se si sta utilizzando Visual Studio Code o test in una piattaforma locale che non include un certificato di prova per HTTPS:</span><span class="sxs-lookup"><span data-stu-id="4e026-255">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="4e026-256">Impostare `"LocalTest:skipSSL": true` nel *appsettings. Developement.JSON* file da ignorare il requisito HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e026-256">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="4e026-257">HTTPS Skip solo su un computer di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="4e026-257">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="4e026-258">Se l'applicazione dispone di contatti:</span><span class="sxs-lookup"><span data-stu-id="4e026-258">If the app has contacts:</span></span>

* <span data-ttu-id="4e026-259">Eliminare tutti i record di `Contact` tabella.</span><span class="sxs-lookup"><span data-stu-id="4e026-259">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="4e026-260">Riavviare l'app per l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="4e026-260">Restart the app to seed the database.</span></span>

<span data-ttu-id="4e026-261">Registrazione di un utente per esplorare i contatti.</span><span class="sxs-lookup"><span data-stu-id="4e026-261">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="4e026-262">Un modo semplice per testare app completata consiste nell'avviare tre diversi browser (o incognito/InPrivate versioni).</span><span class="sxs-lookup"><span data-stu-id="4e026-262">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="4e026-263">In un browser, registrare un nuovo utente (ad esempio, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="4e026-263">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="4e026-264">Accedere a ogni browser con un altro utente.</span><span class="sxs-lookup"><span data-stu-id="4e026-264">Sign in to each browser with a different user.</span></span> <span data-ttu-id="4e026-265">Verificare che le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4e026-265">Verify the following operations:</span></span>

* <span data-ttu-id="4e026-266">Gli utenti registrati possono visualizzare tutti i dati di contatto approvati.</span><span class="sxs-lookup"><span data-stu-id="4e026-266">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="4e026-267">Gli utenti registrati possono modificare/eliminare i propri dati.</span><span class="sxs-lookup"><span data-stu-id="4e026-267">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="4e026-268">Responsabili possano approvare o rifiutare i dati di contatto.</span><span class="sxs-lookup"><span data-stu-id="4e026-268">Managers can approve or reject contact data.</span></span> <span data-ttu-id="4e026-269">Il `Details` vista mostra **Approva** e **rifiutare** pulsanti.</span><span class="sxs-lookup"><span data-stu-id="4e026-269">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="4e026-270">Gli amministratori possono approvare/rifiutare e tutti i dati di modifica/eliminazione.</span><span class="sxs-lookup"><span data-stu-id="4e026-270">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="4e026-271">Utente</span><span class="sxs-lookup"><span data-stu-id="4e026-271">User</span></span>| <span data-ttu-id="4e026-272">Opzioni</span><span class="sxs-lookup"><span data-stu-id="4e026-272">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="4e026-273">Può modificare/eliminare dati</span><span class="sxs-lookup"><span data-stu-id="4e026-273">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="4e026-274">Può approvare/rifiutare e modifica/eliminazione disporre di dati</span><span class="sxs-lookup"><span data-stu-id="4e026-274">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="4e026-275">Possibile modifica/eliminazione e approvare/rifiutare tutti i dati</span><span class="sxs-lookup"><span data-stu-id="4e026-275">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="4e026-276">Creare un contatto nel browser dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="4e026-276">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="4e026-277">Copiare l'URL per l'eliminazione e modificare il contatto dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="4e026-277">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="4e026-278">Incollare questi collegamenti nel browser dell'utente test per verificare che l'utente test non è possibile eseguire queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="4e026-278">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="4e026-279">Creare l'app starter</span><span class="sxs-lookup"><span data-stu-id="4e026-279">Create the starter app</span></span>

* <span data-ttu-id="4e026-280">Creare un'applicazione di pagine Razor denominata "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="4e026-280">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="4e026-281">Creare l'app con **singoli account utente**.</span><span class="sxs-lookup"><span data-stu-id="4e026-281">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="4e026-282">Il nome "ContactManager" in modo lo spazio dei nomi utilizzato nell'esempio corrisponde a uno spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="4e026-282">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="4e026-283">`-uld` Specifica del database locale anziché SQLite</span><span class="sxs-lookup"><span data-stu-id="4e026-283">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="4e026-284">Aggiungere il seguente `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="4e026-284">Add the following `Contact` model:</span></span>

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="4e026-285">Lo scaffolding di `Contact` modello:</span><span class="sxs-lookup"><span data-stu-id="4e026-285">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="4e026-286">Aggiornamento di **ContactManager** di ancoraggio nel *Pages/_Layout.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="4e026-286">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="4e026-287">Lo scaffolding della migrazione iniziale e aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="4e026-287">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="4e026-288">Testare l'app per la creazione, modifica ed eliminazione di un contatto</span><span class="sxs-lookup"><span data-stu-id="4e026-288">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="4e026-289">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="4e026-289">Seed the database</span></span>

<span data-ttu-id="4e026-290">Aggiungere il `SeedData` classe per il *dati* cartella.</span><span class="sxs-lookup"><span data-stu-id="4e026-290">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="4e026-291">Se è stato scaricato l'esempio, è possibile copiare il *SeedData.cs* file per il *dati* cartella del progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="4e026-291">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="4e026-292">Chiamare `SeedData.Initialize` da `Main`:</span><span class="sxs-lookup"><span data-stu-id="4e026-292">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="4e026-293">Verificare che l'app effettuato il seeding del database.</span><span class="sxs-lookup"><span data-stu-id="4e026-293">Test that the app seeded the database.</span></span> <span data-ttu-id="4e026-294">Se sono presenti righe nel database di contatto, il metodo di inizializzazione non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="4e026-294">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="4e026-295">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4e026-295">Additional resources</span></span>

* <span data-ttu-id="4e026-296">[Lab autorizzazione ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="4e026-296">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="4e026-297">Questa esercitazione vengono descritti in dettaglio le funzionalità di sicurezza introdotte in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="4e026-297">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="4e026-298">Autorizzazione in ASP.NET Core: semplice, ruolo, basata sulle attestazioni e personalizzato</span><span class="sxs-lookup"><span data-stu-id="4e026-298">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="4e026-299">Autorizzazione personalizzata basata su criteri</span><span class="sxs-lookup"><span data-stu-id="4e026-299">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
