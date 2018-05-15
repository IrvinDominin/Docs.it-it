---
title: Introduzione ad ASP.NET Core 1.1
author: rick-anderson
description: Seguire questa esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core 1.1.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a><span data-ttu-id="b3dda-103">Introduzione ad ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="b3dda-103">Get Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="b3dda-104">Queste istruzioni sono relative ad ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="b3dda-104">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="b3dda-105">Per cercare la versione più recente,</span><span class="sxs-lookup"><span data-stu-id="b3dda-105">Looking for the latest version?</span></span> <span data-ttu-id="b3dda-106">vedere la [versione corrente di questa esercitazione](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="b3dda-106">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="b3dda-107">Installare il **programma di installazione di SDK** per SDK 1.0.4 dalla [pagina di tutti i download di .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="b3dda-107">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="b3dda-108">Creare una cartella per un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3dda-108">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="b3dda-109">In macOS e Linux aprire una finestra del terminale.</span><span class="sxs-lookup"><span data-stu-id="b3dda-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="b3dda-110">In Windows aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="b3dda-110">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="b3dda-111">Se è installata una versione SDK successiva nel computer in uso, creare un file *global.json* per selezionare l'SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="b3dda-111">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="b3dda-112">Creare un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3dda-112">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="b3dda-113">Ripristinare i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="b3dda-113">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="b3dda-114">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="b3dda-114">Run the app.</span></span>

   <span data-ttu-id="b3dda-115">Il comando [dotnet run](/dotnet/core/tools/dotnet-run) compila prima l'app se necessario.</span><span class="sxs-lookup"><span data-stu-id="b3dda-115">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="b3dda-116">Passare a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b3dda-116">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="b3dda-117">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b3dda-117">Next steps</span></span>

<span data-ttu-id="b3dda-118">Per le esercitazioni, introduttive, vedere [Esercitazioni di ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="b3dda-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="b3dda-119">Per un'introduzione ai concetti e all'architettura di ASP.NET Core, vedere [Introduzione ad ASP.NET Core](index.md) e [Nozioni fondamentali di ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="b3dda-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="b3dda-120">Un'app ASP.NET Core può usare la libreria di classi base e il runtime di .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3dda-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="b3dda-121">Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="b3dda-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
