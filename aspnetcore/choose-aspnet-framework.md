---
title: Scegliere tra ASP.NET e ASP.NET Core
author: rick-anderson
description: Informazioni su come scegliere tra ASP.NET e ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 232e82ed66ff2363230ff09d435db1074c02b53b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="8ac5d-103">Scegliere tra ASP.NET e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac5d-103">Choose between ASP.NET and ASP.NET Core</span></span> 

<span data-ttu-id="8ac5d-104">Indipendentemente dall'applicazione web che si sta creando, ASP.NET dispone di una soluzione per l'utente: da applicazioni web aziendali destinate a Windows Server, a piccoli contenitori di Linux destinati ai microservizi, passando per tutto ciò che è compreso tra i due elementi.</span><span class="sxs-lookup"><span data-stu-id="8ac5d-104">No matter the web application you are creating, ASP.NET has a solution for you: from enterprise web applications targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="8ac5d-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac5d-105">ASP.NET Core</span></span>

<span data-ttu-id="8ac5d-106">ASP.NET Core è un framework open source, multipiattaforma per la compilazione di moderne applicazioni web basate su cloud in Windows, Mac OS o Linux.</span><span class="sxs-lookup"><span data-stu-id="8ac5d-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web applications on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="8ac5d-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ac5d-107">ASP.NET</span></span>

<span data-ttu-id="8ac5d-108">ASP.NET è un framework consolidato che fornisce tutti i servizi necessari per la compilazione di applicazioni Web basate su server di livello aziendale su Windows.</span><span class="sxs-lookup"><span data-stu-id="8ac5d-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web applications on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="8ac5d-109">Quale è adatta alle mie esigenze?</span><span class="sxs-lookup"><span data-stu-id="8ac5d-109">Which one is right for me?</span></span>

| <span data-ttu-id="8ac5d-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac5d-110">ASP.NET Core</span></span> | <span data-ttu-id="8ac5d-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ac5d-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="8ac5d-112">Compilare per Windows, Mac OS o Linux</span><span class="sxs-lookup"><span data-stu-id="8ac5d-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="8ac5d-113">Compilare per Windows</span><span class="sxs-lookup"><span data-stu-id="8ac5d-113">Build for Windows</span></span>|
|<span data-ttu-id="8ac5d-114">[Razor Pages](xref:mvc/razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web con ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="8ac5d-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span> <span data-ttu-id="8ac5d-115">Vedere anche [MVC](xref:mvc/overview) e [Web API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="8ac5d-115">See also [MVC](xref:mvc/overview) and [Web API](xref:tutorials/first-web-api)</span></span>|<span data-ttu-id="8ac5d-116">Usare [Web Form](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), o [Pagine Web](https://docs.microsoft.com/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="8ac5d-116">Use [Web Forms](https://docs.microsoft.com/aspnet/web-forms), [SignalR](https://docs.microsoft.com/aspnet/signalr), [MVC](https://docs.microsoft.com/aspnet/mvc), [Web API](https://docs.microsoft.com/aspnet/web-api/), or [Web Pages](https://docs.microsoft.com/aspnet/web-pages)</span></span>|
|<span data-ttu-id="8ac5d-117">Più versioni per computer</span><span class="sxs-lookup"><span data-stu-id="8ac5d-117">Multiple versions per machine</span></span>|<span data-ttu-id="8ac5d-118">Una versione per computer</span><span class="sxs-lookup"><span data-stu-id="8ac5d-118">One version per machine</span></span>|
|<span data-ttu-id="8ac5d-119">Sviluppare con Visual Studio, [Visual Studio per Mac](https://www.visualstudio.com/vs/visual-studio-mac/), o [Visual Studio Code](https://code.visualstudio.com/) tramite C# o F#</span><span class="sxs-lookup"><span data-stu-id="8ac5d-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="8ac5d-120">Sviluppare con Visual Studio tramite C#, VB o F#</span><span class="sxs-lookup"><span data-stu-id="8ac5d-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="8ac5d-121">Prestazioni più elevate rispetto ad ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ac5d-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="8ac5d-122">Buone prestazioni</span><span class="sxs-lookup"><span data-stu-id="8ac5d-122">Good performance</span></span>|
|[<span data-ttu-id="8ac5d-123">Scegliere .NET Framework o Runtime di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac5d-123">Choose .NET Framework or .NET Core runtime</span></span>](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="8ac5d-124">Usare runtime .NET Framework</span><span class="sxs-lookup"><span data-stu-id="8ac5d-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="8ac5d-125">Scenari ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac5d-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="8ac5d-126">[Razor Pages](xref:mvc/razor-pages/index) è l'approccio consigliato per la creazione di un'interfaccia utente Web con ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="8ac5d-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI with ASP.NET Core 2.0.</span></span>
* [<span data-ttu-id="8ac5d-127">Siti Web</span><span class="sxs-lookup"><span data-stu-id="8ac5d-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="8ac5d-128">API</span><span class="sxs-lookup"><span data-stu-id="8ac5d-128">APIs</span></span>](xref:tutorials/first-web-api)

## <a name="aspnet-scenarios"></a><span data-ttu-id="8ac5d-129">Scenari ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ac5d-129">ASP.NET scenarios</span></span>

* [<span data-ttu-id="8ac5d-130">Siti Web</span><span class="sxs-lookup"><span data-stu-id="8ac5d-130">Websites</span></span>](https://docs.microsoft.com/aspnet/mvc)
* [<span data-ttu-id="8ac5d-131">API</span><span class="sxs-lookup"><span data-stu-id="8ac5d-131">APIs</span></span>](https://docs.microsoft.com/aspnet/web-api)
* [<span data-ttu-id="8ac5d-132">In tempo reale</span><span class="sxs-lookup"><span data-stu-id="8ac5d-132">Real-time</span></span>](https://docs.microsoft.com/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="8ac5d-133">Risorse</span><span class="sxs-lookup"><span data-stu-id="8ac5d-133">Resources</span></span>

* [<span data-ttu-id="8ac5d-134">Introduzione ad ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8ac5d-134">Introduction to ASP.NET</span></span>](https://docs.microsoft.com/aspnet/overview)
* [<span data-ttu-id="8ac5d-135">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ac5d-135">Introduction to ASP.NET Core</span></span>](xref:index)
