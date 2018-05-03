---
title: Compilazione e precompilazione delle visualizzazioni Razor in ASP.NET Core
author: rick-anderson
description: Informazioni su come abilitare la compilazione e precompilazione delle visualizzazioni Razor MVC nelle app ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="88087-103">Compilazione e precompilazione delle visualizzazioni Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88087-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="88087-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="88087-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="88087-105">Le visualizzazioni Razor vengono compilate in fase di esecuzione quando la viene richiamata la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="88087-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="88087-106">ASP.NET Core 1.1.0 e versioni successive possono facoltativamente compilare e distribuire le visualizzazioni Razor usando l'app &mdash;. Questo processo è noto come precompilazione.</span><span class="sxs-lookup"><span data-stu-id="88087-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="88087-107">Per impostazione predefinita, la precompilazione è abilitata nei modelli di progetto ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="88087-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="88087-108">La precompilazione delle visualizzazioni Razor non è attualmente disponibile quando si esegue una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="88087-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="88087-109">La funzionalità sarà disponibile per questo tipo di distribuzione a partire dalla versione 2.1.</span><span class="sxs-lookup"><span data-stu-id="88087-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="88087-110">Per altre informazioni, vedere [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102) (Errore di compilazione delle visualizzazioni durante la cross-compilazione per Linux in Windows.</span><span class="sxs-lookup"><span data-stu-id="88087-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="88087-111">Considerazioni sulla precompilazione:</span><span class="sxs-lookup"><span data-stu-id="88087-111">Precompilation considerations:</span></span>

* <span data-ttu-id="88087-112">La precompilazione delle visualizzazioni consente di ridurre le dimensioni del bundle pubblicato e velocizzare il tempo di avvio.</span><span class="sxs-lookup"><span data-stu-id="88087-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="88087-113">Non è possibile modificare i file Razor dopo aver precompliato le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="88087-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="88087-114">Le visualizzazioni modificate non saranno contenute nel bundle pubblicato.</span><span class="sxs-lookup"><span data-stu-id="88087-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="88087-115">Per distribuire le visualizzazioni precompilate:</span><span class="sxs-lookup"><span data-stu-id="88087-115">To deploy precompiled views:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="88087-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88087-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="88087-117">Se .NET Framework è la destinazione del progetto, includere un riferimento al pacchetto [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="88087-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="88087-118">Se è .NET Core la destinazione del progetto, non sono necessarie modifiche.</span><span class="sxs-lookup"><span data-stu-id="88087-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="88087-119">Per impostazione predefinita, i modelli di progetto ASP.NET Core 2.x impostano in modo implicito `MvcRazorCompileOnPublish` su `true`. In questo modo il nodo può essere rimosso in modo sicuro dal file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="88087-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="88087-120">Se si preferisce la modalità esplicita, è possibile impostare la proprietà `MvcRazorCompileOnPublish` su `true`.</span><span class="sxs-lookup"><span data-stu-id="88087-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="88087-121">Nell'esempio *.csproj* seguente viene evidenziata questa impostazione:</span><span class="sxs-lookup"><span data-stu-id="88087-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="88087-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="88087-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="88087-123">Impostare `MvcRazorCompileOnPublish` su `true`e includere un riferimento al pacchetto `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="88087-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="88087-124">Nell'esempio *.csproj* seguente vengono evidenziate queste impostazioni:</span><span class="sxs-lookup"><span data-stu-id="88087-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

<span data-ttu-id="88087-125">Preparare l'app per una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) con il [comando publish dell'interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="88087-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="88087-126">Ad esempio, eseguire il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="88087-126">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="88087-127">Quando la precompilazione viene completata, si genera un file *<nome_progetto>.PrecompiledViews.dll* contenente le visualizzazioni Razor compilate.</span><span class="sxs-lookup"><span data-stu-id="88087-127">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="88087-128">Ad esempio, lo screenshot seguente illustra il contenuto di *Index.cshtml* in *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="88087-128">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Visualizzazioni Razor in DLL](view-compilation/_static/razor-views-in-dll.png)
