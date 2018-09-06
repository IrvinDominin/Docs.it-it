---
title: Versione di compatibilità per ASP.NET Core MVC
author: rick-anderson
description: Informazioni su come la classe Startup in ASP.NET Core configura i servizi e la pipeline delle richieste dell'app.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/20/2018
uid: mvc/compatibility-version
ms.openlocfilehash: bedeeba07dcca19176e779f3541c445e94efcd07
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2018
ms.locfileid: "41910073"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="5a498-103">Versione di compatibilità per ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="5a498-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="5a498-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5a498-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5a498-105">Il metodo <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> consente a un'app di acconsentire o rifiutare esplicitamente modifiche potenzialmente importanti del comportamento introdotte in ASP.NET Core MVC 2.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="5a498-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="5a498-106">Queste modifiche potenzialmente importanti del comportamento riguardano in genere il funzionamento del sottosistema MVC e la modalità con cui il **codice dell'utente** viene chiamato dal runtime.</span><span class="sxs-lookup"><span data-stu-id="5a498-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="5a498-107">Se si acconsente esplicitamente, si ottiene il comportamento più recente e il comportamento a lungo termine di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a498-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="5a498-108">Il codice seguente imposta la modalità di compatibilità su ASP.NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="5a498-108">The following code sets the compatibility mode to ASP.NET Core 2.1:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="5a498-109">È consigliabile testare l'app usando la versione più recente (`CompatibilityVersion.Version_2_1`).</span><span class="sxs-lookup"><span data-stu-id="5a498-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_1`).</span></span> <span data-ttu-id="5a498-110">Microsoft prevede che la maggior parte delle app non riscontrerà modifiche importanti del comportamento quando viene usata la versione più recente.</span><span class="sxs-lookup"><span data-stu-id="5a498-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="5a498-111">Le app che chiamano `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` sono protette dalle modifiche potenzialmente importanti del comportamento introdotte in ASP.NET Core 2.1 MVC e versioni 2.x successive.</span><span class="sxs-lookup"><span data-stu-id="5a498-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="5a498-112">La protezione:</span><span class="sxs-lookup"><span data-stu-id="5a498-112">This protection:</span></span>

* <span data-ttu-id="5a498-113">Non è valida per tutte le modifiche 2.1 e successive, ma è destinata alle modifiche potenzialmente importanti del comportamento del runtime ASP.NET Core nel sottosistema MVC.</span><span class="sxs-lookup"><span data-stu-id="5a498-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="5a498-114">Non viene estesa alla versione principale successiva.</span><span class="sxs-lookup"><span data-stu-id="5a498-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="5a498-115">La compatibilità predefinita per le app ASP.NET Core 2.1 e versioni 2.x successive che **non** chiamano `SetCompatibilityVersion` è la compatibilità 2.0.</span><span class="sxs-lookup"><span data-stu-id="5a498-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="5a498-116">In altri termini il fatto di non chiamare `SetCompatibilityVersion` equivale a chiamare `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span><span class="sxs-lookup"><span data-stu-id="5a498-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="5a498-117">Il codice seguente imposta la modalità di compatibilità su ASP.NET Core 2.1, con l'eccezione dei comportamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="5a498-117">The following code sets the compatibility mode to ASP.NET Core 2.1, except for the following behaviors:</span></span>

* [<span data-ttu-id="5a498-118">AllowCombiningAuthorizeFilters</span><span class="sxs-lookup"><span data-stu-id="5a498-118">AllowCombiningAuthorizeFilters</span></span>](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [<span data-ttu-id="5a498-119">InputFormatterExceptionPolicy</span><span class="sxs-lookup"><span data-stu-id="5a498-119">InputFormatterExceptionPolicy</span></span>](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="5a498-120">Per le app che riscontrano modifiche potenzialmente importanti del comportamento, l'uso delle opzioni di compatibilità appropriate:</span><span class="sxs-lookup"><span data-stu-id="5a498-120">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="5a498-121">Consente di usare la versione più recente e di rifiutare esplicitamente modifiche importanti del comportamento specifiche.</span><span class="sxs-lookup"><span data-stu-id="5a498-121">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="5a498-122">Garantisce il tempo necessario per l'aggiornamento dell'app, che in tal modo potrà funzionare con le modifiche più recenti.</span><span class="sxs-lookup"><span data-stu-id="5a498-122">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="5a498-123">I commenti relativi alla classe di origine [MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) offrono una spiegazione chiara delle modifiche e dei motivi per cui queste rappresentano un miglioramento per la maggior parte degli utenti.</span><span class="sxs-lookup"><span data-stu-id="5a498-123">The [MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="5a498-124">In una data futura verrà rilasciata la [versione ASP.NET Core 3.0](https://github.com/aspnet/Home/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="5a498-124">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="5a498-125">I comportamenti precedenti supportati dalle opzioni di compatibilità verranno rimossi nella versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="5a498-125">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="5a498-126">Queste modifiche positive andranno a vantaggio della maggior parte degli utenti.</span><span class="sxs-lookup"><span data-stu-id="5a498-126">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="5a498-127">Se si introducono ora queste modifiche, la maggior parte delle app può trarne vantaggio da subito e sarà disponibile tempo sufficiente per aggiornare le altre applicazioni.</span><span class="sxs-lookup"><span data-stu-id="5a498-127">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>