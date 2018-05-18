---
title: Identità Scaffold nei progetti ASP.NET Core
author: rick-anderson
description: Informazioni su come eseguire lo scaffolding di identità in un progetto ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 7527d3c075fd845ac804d4cfd56469a0679ed7e8
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="d8530-103">Identità Scaffold nei progetti ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8530-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="d8530-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d8530-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d8530-105">Fornisce ASP.NET Core 2.1 e versioni successive [ASP.NET Identity Core](xref:security/authentication/identity) come una [libreria di classi Razor](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="d8530-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="d8530-106">Le applicazioni che includono l'identità possono applicare scaffolder per aggiungere in modo selettivo il codice sorgente incluso in Identity Razor classe libreria (RCL).</span><span class="sxs-lookup"><span data-stu-id="d8530-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="d8530-107">È possibile generare codice sorgente, pertanto è possibile modificare il codice e modificare il comportamento.</span><span class="sxs-lookup"><span data-stu-id="d8530-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="d8530-108">Ad esempio, si può indicare scaffolder per generare il codice usato nella registrazione.</span><span class="sxs-lookup"><span data-stu-id="d8530-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="d8530-109">Codice generato ha la precedenza sullo stesso codice in RCL l'identità.</span><span class="sxs-lookup"><span data-stu-id="d8530-109">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="d8530-110">Le applicazioni che hanno **non** includono l'autenticazione è possibile applicare scaffolder per aggiungere il pacchetto RCL identità.</span><span class="sxs-lookup"><span data-stu-id="d8530-110">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="d8530-111">È la possibilità di selezionare il codice di identità da generare.</span><span class="sxs-lookup"><span data-stu-id="d8530-111">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="d8530-112">Sebbene il scaffolder genera la maggior parte del codice necessario, sarà necessario aggiornare il progetto per completare il processo.</span><span class="sxs-lookup"><span data-stu-id="d8530-112">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="d8530-113">Questo documento illustra i passaggi necessari per completare un aggiornamento che lo scaffolding di identità.</span><span class="sxs-lookup"><span data-stu-id="d8530-113">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="d8530-114">Quando viene eseguito il scaffolder Identity, una *ScaffoldingReadme.txt* file viene creato nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="d8530-114">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="d8530-115">Il *ScaffoldingReadme.txt* file contiene istruzioni generali sul cosa è necessario completare l'aggiornamento che lo scaffolding di identità.</span><span class="sxs-lookup"><span data-stu-id="d8530-115">The *ScaffoldingReadme.txt* file contains general instructions on what's need to complete the Identity scaffolding update.</span></span> <span data-ttu-id="d8530-116">Questo documento contiene istruzioni più complete rispetto a lettura il *ScaffoldingReadme.txt* file.</span><span class="sxs-lookup"><span data-stu-id="d8530-116">This document contains more complete instructions than the read the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="d8530-117">È consigliabile utilizzare un sistema di controllo di origine che mostra le differenze tra file e consente di annullare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="d8530-117">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="d8530-118">Controllare le modifiche apportate dopo l'esecuzione di scaffolder di identità.</span><span class="sxs-lookup"><span data-stu-id="d8530-118">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="d8530-119">Lo scaffolding di identità in un progetto vuoto</span><span class="sxs-lookup"><span data-stu-id="d8530-119">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d8530-120">Aggiungere il seguente chiama evidenziato per il `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="d8530-120">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="d8530-121">Lo scaffolding di identità in un progetto Razor senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="d8530-121">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0-rc1-final

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d8530-122">Identità sia configurata nel *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8530-122">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d8530-123">per altre informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="d8530-123">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="d8530-124">Nel `Configure` metodo per il `Startup` classe, chiamare [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="d8530-124">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="d8530-125">Modifiche di layout</span><span class="sxs-lookup"><span data-stu-id="d8530-125">Layout changes</span></span>

<span data-ttu-id="d8530-126">Facoltativo: Aggiungere l'account di accesso parziale (`_LoginPartial`) per il file di layout:</span><span class="sxs-lookup"><span data-stu-id="d8530-126">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-individual-authorization"></a><span data-ttu-id="d8530-127">Lo scaffolding di identità in un progetto Razor con autorizzazione individuale</span><span class="sxs-lookup"><span data-stu-id="d8530-127">Scaffold identity into a Razor project with individual authorization</span></span>

<!--
dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v "2.1.0-rc1-final"
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="d8530-128">Alcune opzioni di identità configurati nel *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8530-128">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d8530-129">Per altre informazioni, vedere [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="d8530-129">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="d8530-130">Lo scaffolding di identità in un progetto MVC senza autorizzazione esistente</span><span class="sxs-lookup"><span data-stu-id="d8530-130">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0-rc1-final

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d8530-131">Facoltativo: Aggiungere l'account di accesso parziale (`_LoginPartial`) per il *Views/Shared/_Layout.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="d8530-131">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="d8530-132">Spostare il *Pages/Shared/_LoginPartial.cshtml* file *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d8530-132">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="d8530-133">Identità sia configurata nel *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8530-133">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d8530-134">Per altre informazioni, vedere IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="d8530-134">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="d8530-135">Chiamare [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dopo `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="d8530-135">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-individual-authorization"></a><span data-ttu-id="d8530-136">Lo scaffolding di identità in un progetto MVC con autorizzazione individuale</span><span class="sxs-lookup"><span data-stu-id="d8530-136">Scaffold identity into an MVC project with individual authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v "2.1.0-rc1-final"
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="d8530-137">Eliminare il *pagine/Shared* cartella e i file in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="d8530-137">Delete the *Pages/Shared* folder and the files in that folder.</span></span>
