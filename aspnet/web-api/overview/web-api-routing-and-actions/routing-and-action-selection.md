---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing e la selezione di azione in ASP.NET Web API | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="25152-102">Routing e la selezione di azione in ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="25152-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="25152-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="25152-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="25152-104">In questo articolo viene descritto come ASP.NET Web API invia una richiesta HTTP a una determinata azione su un controller.</span><span class="sxs-lookup"><span data-stu-id="25152-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="25152-105">Per una panoramica del routing, vedere [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="25152-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="25152-106">In questo articolo esamina i dettagli del processo di routing.</span><span class="sxs-lookup"><span data-stu-id="25152-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="25152-107">Se si crea un progetto di Web API e trova che alcune richieste non indirizzate come che previsto, probabilmente consente di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="25152-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="25152-108">Il routing offre tre fasi principali:</span><span class="sxs-lookup"><span data-stu-id="25152-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="25152-109">Corrispondenza dell'URI per un modello di route.</span><span class="sxs-lookup"><span data-stu-id="25152-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="25152-110">Selezione di un controller.</span><span class="sxs-lookup"><span data-stu-id="25152-110">Selecting a controller.</span></span>
3. <span data-ttu-id="25152-111">Selezionando un'azione.</span><span class="sxs-lookup"><span data-stu-id="25152-111">Selecting an action.</span></span>

<span data-ttu-id="25152-112">È possibile sostituire alcune parti del processo con comportamenti personalizzati.</span><span class="sxs-lookup"><span data-stu-id="25152-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="25152-113">In questo articolo, descrivo il comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="25152-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="25152-114">Al termine, nota le posizioni in cui è possibile personalizzare il comportamento.</span><span class="sxs-lookup"><span data-stu-id="25152-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="25152-115">Modelli di route</span><span class="sxs-lookup"><span data-stu-id="25152-115">Route Templates</span></span>

<span data-ttu-id="25152-116">Un modello di route è simile a un percorso URI, ma può avere i valori segnaposto, indicati tra parentesi graffe:</span><span class="sxs-lookup"><span data-stu-id="25152-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="25152-117">Quando si crea una route, è possibile fornire valori predefiniti per alcuni o tutti i segnaposto:</span><span class="sxs-lookup"><span data-stu-id="25152-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="25152-118">È inoltre possibile specificare vincoli, ovvero limitano come un segmento URI può corrispondere a un segnaposto:</span><span class="sxs-lookup"><span data-stu-id="25152-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="25152-119">Il framework tenta di abbinare i segmenti nel percorso dell'URI per il modello.</span><span class="sxs-lookup"><span data-stu-id="25152-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="25152-120">Valori letterali nel modello devono corrispondere esattamente.</span><span class="sxs-lookup"><span data-stu-id="25152-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="25152-121">Un segnaposto corrisponde a qualsiasi valore, a meno che non si specificano vincoli.</span><span class="sxs-lookup"><span data-stu-id="25152-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="25152-122">Il framework non corrisponde a altre parti dell'URI, ad esempio il nome host o i parametri di query.</span><span class="sxs-lookup"><span data-stu-id="25152-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="25152-123">Il framework seleziona la prima route nella tabella di route che corrisponde all'URI specificato.</span><span class="sxs-lookup"><span data-stu-id="25152-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="25152-124">Esistono due segnaposto speciale: "{controller}" e "{action}".</span><span class="sxs-lookup"><span data-stu-id="25152-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="25152-125">"{controller}" fornisce il nome del controller.</span><span class="sxs-lookup"><span data-stu-id="25152-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="25152-126">"{action}" fornisce il nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="25152-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="25152-127">Nell'API Web, la convenzione di solito è omettere "{action}".</span><span class="sxs-lookup"><span data-stu-id="25152-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="25152-128">attività</span><span class="sxs-lookup"><span data-stu-id="25152-128">Defaults</span></span>

<span data-ttu-id="25152-129">Se si specificano valori predefiniti, la route corrisponde un URI a cui mancano i segmenti.</span><span class="sxs-lookup"><span data-stu-id="25152-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="25152-130">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="25152-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="25152-131">L'URI "`http://localhost/api/products`" corrisponde a questa route.</span><span class="sxs-lookup"><span data-stu-id="25152-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="25152-132">Il segmento '{categoria}' è assegnato il valore predefinito "tutti".</span><span class="sxs-lookup"><span data-stu-id="25152-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="25152-133">Dizionario della route</span><span class="sxs-lookup"><span data-stu-id="25152-133">Route Dictionary</span></span>

<span data-ttu-id="25152-134">Se il framework trova una corrispondenza per un URI, viene creato un dizionario che contiene il valore per ogni segnaposto.</span><span class="sxs-lookup"><span data-stu-id="25152-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="25152-135">Le chiavi sono i nomi dei segnaposto, senza includere le parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="25152-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="25152-136">I valori vengono ricavati dal percorso URI o dalle impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="25152-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="25152-137">Il dizionario viene archiviato nel **IHttpRouteData** oggetto.</span><span class="sxs-lookup"><span data-stu-id="25152-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="25152-138">Durante questa fase di corrispondenza di route le speciali "{controller}" segnaposto "{action}" vengano considerati come altri segnaposto.</span><span class="sxs-lookup"><span data-stu-id="25152-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="25152-139">Questi sono semplicemente archiviati nel dizionario con gli altri valori.</span><span class="sxs-lookup"><span data-stu-id="25152-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="25152-140">Valore predefinito può avere il valore speciale **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="25152-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="25152-141">Se questo valore assegnato un segnaposto, il valore non viene aggiunto per il dizionario della route.</span><span class="sxs-lookup"><span data-stu-id="25152-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="25152-142">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="25152-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="25152-143">Per il percorso dell'URI "api/prodotti", conterrà il dizionario della route:</span><span class="sxs-lookup"><span data-stu-id="25152-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="25152-144">controller: "prodotti"</span><span class="sxs-lookup"><span data-stu-id="25152-144">controller: "products"</span></span>
- <span data-ttu-id="25152-145">category: "all"</span><span class="sxs-lookup"><span data-stu-id="25152-145">category: "all"</span></span>

<span data-ttu-id="25152-146">Per "api/prodotti/toys/123", tuttavia, il dizionario della route conterrà:</span><span class="sxs-lookup"><span data-stu-id="25152-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="25152-147">controller: "prodotti"</span><span class="sxs-lookup"><span data-stu-id="25152-147">controller: "products"</span></span>
- <span data-ttu-id="25152-148">category: "toys"</span><span class="sxs-lookup"><span data-stu-id="25152-148">category: "toys"</span></span>
- <span data-ttu-id="25152-149">id: "123"</span><span class="sxs-lookup"><span data-stu-id="25152-149">id: "123"</span></span>

<span data-ttu-id="25152-150">Le impostazioni predefinite possono inoltre includere un valore che non viene visualizzata in qualsiasi punto nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="25152-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="25152-151">Se la route corrisponde, tale valore viene archiviato nel dizionario.</span><span class="sxs-lookup"><span data-stu-id="25152-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="25152-152">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="25152-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="25152-153">Se il percorso dell'URI è "root/api/8", il dizionario contiene due valori:</span><span class="sxs-lookup"><span data-stu-id="25152-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="25152-154">controller: "customers"</span><span class="sxs-lookup"><span data-stu-id="25152-154">controller: "customers"</span></span>
- <span data-ttu-id="25152-155">id: "8"</span><span class="sxs-lookup"><span data-stu-id="25152-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="25152-156">La selezione di un Controller</span><span class="sxs-lookup"><span data-stu-id="25152-156">Selecting a Controller</span></span>

<span data-ttu-id="25152-157">La selezione del controller è gestita dal **IHttpControllerSelector.SelectController** metodo.</span><span class="sxs-lookup"><span data-stu-id="25152-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="25152-158">Questo metodo accetta un **HttpRequestMessage** istanza e restituisce un **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="25152-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="25152-159">L'implementazione predefinita viene eseguito il **DefaultHttpControllerSelector** classe.</span><span class="sxs-lookup"><span data-stu-id="25152-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="25152-160">Questa classe Usa un semplice algoritmo:</span><span class="sxs-lookup"><span data-stu-id="25152-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="25152-161">Esaminare il dizionario della route per la chiave "controller".</span><span class="sxs-lookup"><span data-stu-id="25152-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="25152-162">Accettare il valore per questa chiave e aggiungere la stringa "Controller" per ottenere il nome del tipo di controller.</span><span class="sxs-lookup"><span data-stu-id="25152-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="25152-163">Cercare un controller Web API con questo nome del tipo.</span><span class="sxs-lookup"><span data-stu-id="25152-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="25152-164">Ad esempio, se il dizionario della route contiene il coppia chiave-valore Microsoft "controller" = "prodotti", il tipo di controller è "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="25152-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="25152-165">Se è presente alcun tipo corrispondente, o più corrispondenze, il framework restituisce un errore al client.</span><span class="sxs-lookup"><span data-stu-id="25152-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="25152-166">Per il passaggio 3, **DefaultHttpControllerSelector** utilizza il **IHttpControllerTypeResolver** interfaccia da ottenere l'elenco dei tipi di controller API Web.</span><span class="sxs-lookup"><span data-stu-id="25152-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="25152-167">L'implementazione predefinita di **IHttpControllerTypeResolver** restituisce tutte le classi pubbliche che implementano (a) **IHttpController**, (b) sono non astratta e (c) dispone di un nome che termina con "Controller".</span><span class="sxs-lookup"><span data-stu-id="25152-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="25152-168">Selezione di azione</span><span class="sxs-lookup"><span data-stu-id="25152-168">Action Selection</span></span>

<span data-ttu-id="25152-169">Dopo aver selezionato il controller, il framework seleziona l'azione chiamando il **IHttpActionSelector.SelectAction** metodo.</span><span class="sxs-lookup"><span data-stu-id="25152-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="25152-170">Questo metodo accetta un **HttpControllerContext** e restituisce un **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="25152-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="25152-171">L'implementazione predefinita viene eseguito il **ApiControllerActionSelector** classe.</span><span class="sxs-lookup"><span data-stu-id="25152-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="25152-172">Per selezionare un'azione, esamina le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="25152-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="25152-173">Il metodo HTTP della richiesta.</span><span class="sxs-lookup"><span data-stu-id="25152-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="25152-174">Il segnaposto "{action}" nel modello di route, se presente.</span><span class="sxs-lookup"><span data-stu-id="25152-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="25152-175">I parametri delle operazioni nel controller.</span><span class="sxs-lookup"><span data-stu-id="25152-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="25152-176">Prima di esaminare l'algoritmo di selezione, è necessario comprendere alcuni aspetti di azioni del controller.</span><span class="sxs-lookup"><span data-stu-id="25152-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="25152-177">**I metodi sul controller sono considerati "azioni"?**</span><span class="sxs-lookup"><span data-stu-id="25152-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="25152-178">Quando si seleziona un'azione, il framework esamina solo i metodi di istanza pubblici nel controller.</span><span class="sxs-lookup"><span data-stu-id="25152-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="25152-179">Inoltre, sono esclusi ["nome speciale"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) metodi (costruttori, eventi, gli overload degli operatori e così via) e metodi ereditati dal **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="25152-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="25152-180">**Metodi HTTP.**</span><span class="sxs-lookup"><span data-stu-id="25152-180">**HTTP Methods.**</span></span> <span data-ttu-id="25152-181">Il framework sceglie solo le azioni che corrispondano al metodo HTTP della richiesta, determinata come segue:</span><span class="sxs-lookup"><span data-stu-id="25152-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="25152-182">È possibile specificare il metodo HTTP con un attributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, o **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="25152-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="25152-183">In caso contrario, se il nome del metodo controller inizia con "Get", "Post", "Put", "Delete", "Head", "Opzioni" o "Patch", quindi per convenzione l'azione supporta il metodo HTTP.</span><span class="sxs-lookup"><span data-stu-id="25152-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="25152-184">Se nessuna delle precedenti, il metodo supporta POST.</span><span class="sxs-lookup"><span data-stu-id="25152-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="25152-185">**Associazioni di parametri.**</span><span class="sxs-lookup"><span data-stu-id="25152-185">**Parameter Bindings.**</span></span> <span data-ttu-id="25152-186">Associazione di un parametro è un valore per un parametro di creazione delle API Web.</span><span class="sxs-lookup"><span data-stu-id="25152-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="25152-187">Di seguito è riportata la regola predefinita per l'associazione di parametri:</span><span class="sxs-lookup"><span data-stu-id="25152-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="25152-188">Tipi semplici vengono eseguiti dall'URI.</span><span class="sxs-lookup"><span data-stu-id="25152-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="25152-189">Tipi complessi vengono estratti dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="25152-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="25152-190">I tipi semplici includono tutti il [tipi primitivi di .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), oltre a **DateTime**, **decimale**, **Guid**, **stringa** , e **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="25152-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="25152-191">Per ogni azione, al massimo un parametro può leggere il corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="25152-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="25152-192">È possibile ignorare le regole di associazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="25152-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="25152-193">Vedere [l'associazione di parametri WebAPI dietro le quinte](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="25152-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="25152-194">Con lo sfondo, di seguito è l'algoritmo di selezione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="25152-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="25152-195">Creare un elenco di tutte le azioni sul controller a cui corrisponde il metodo di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="25152-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="25152-196">Se il dizionario della route è una voce "action", rimuovere le azioni il cui nome corrisponde a questo valore.</span><span class="sxs-lookup"><span data-stu-id="25152-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="25152-197">Provare a corrispondere ai parametri azione per l'URI, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="25152-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="25152-198">Per ogni azione, è possibile ottenere un elenco di parametri che sono un tipo semplice, in cui l'associazione ottiene il parametro dall'URI.</span><span class="sxs-lookup"><span data-stu-id="25152-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="25152-199">Escludere i parametri facoltativi.</span><span class="sxs-lookup"><span data-stu-id="25152-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="25152-200">Da questo elenco, provare a trovare una corrispondenza per ogni nome di parametro, il dizionario della route o nella stringa di query dell'URI.</span><span class="sxs-lookup"><span data-stu-id="25152-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="25152-201">Corrispondenze vengono fatta distinzione tra maiuscole e minuscole e non dipendono dall'ordine dei parametri.</span><span class="sxs-lookup"><span data-stu-id="25152-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="25152-202">Selezionare un'azione in cui ogni parametro nell'elenco ha una corrispondenza nell'URI.</span><span class="sxs-lookup"><span data-stu-id="25152-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="25152-203">Se più di un'azione soddisfa questi criteri, selezionare quella con la maggior parte delle corrispondenze di parametro.</span><span class="sxs-lookup"><span data-stu-id="25152-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="25152-204">Ignorare le azioni con il **[NonAction]** attributo.</span><span class="sxs-lookup"><span data-stu-id="25152-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="25152-205">Passaggio 3 # è probabilmente il più intuitive.</span><span class="sxs-lookup"><span data-stu-id="25152-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="25152-206">L'idea di base è che un parametro è possibile ottenere il valore dell'URI, dal corpo della richiesta o da un'associazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="25152-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="25152-207">Per i parametri forniti dall'URI, si desidera assicurarsi che l'URI contiene effettivamente un valore per il parametro, nel percorso (tramite il dizionario della route) o nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="25152-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="25152-208">Ad esempio, si consideri l'azione seguente:</span><span class="sxs-lookup"><span data-stu-id="25152-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="25152-209">Il *id* il parametro viene associato all'URI.</span><span class="sxs-lookup"><span data-stu-id="25152-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="25152-210">Pertanto, questa azione può corrispondere solo un URI che contiene un valore per "id", il dizionario della route o nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="25152-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="25152-211">Parametri facoltativi sono un'eccezione, perché sono facoltativi.</span><span class="sxs-lookup"><span data-stu-id="25152-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="25152-212">Per un parametro facoltativo, è OK se l'associazione non è possibile ottenere il valore dell'URI.</span><span class="sxs-lookup"><span data-stu-id="25152-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="25152-213">I tipi complessi sono un'eccezione per un motivo diverso.</span><span class="sxs-lookup"><span data-stu-id="25152-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="25152-214">Un tipo complesso può associarsi solo a URI tramite un'associazione personalizzata.</span><span class="sxs-lookup"><span data-stu-id="25152-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="25152-215">Ma in tal caso, cui il framework non è possibile sapere in anticipo se il parametro sarebbe associato a un particolare URI.</span><span class="sxs-lookup"><span data-stu-id="25152-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="25152-216">Per individuare, sarebbe necessario richiamare l'associazione.</span><span class="sxs-lookup"><span data-stu-id="25152-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="25152-217">L'obiettivo dell'algoritmo consiste nel selezionare un'azione in base alla descrizione statica, prima di richiamare le associazioni.</span><span class="sxs-lookup"><span data-stu-id="25152-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="25152-218">Di conseguenza, i tipi complessi sono escluse dall'algoritmo di corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="25152-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="25152-219">Dopo aver selezionato l'azione, tutte le associazioni di parametro vengono richiamate.</span><span class="sxs-lookup"><span data-stu-id="25152-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="25152-220">Riepilogo:</span><span class="sxs-lookup"><span data-stu-id="25152-220">Summary:</span></span>

- <span data-ttu-id="25152-221">L'azione deve corrispondere il metodo HTTP della richiesta.</span><span class="sxs-lookup"><span data-stu-id="25152-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="25152-222">Il nome dell'azione deve corrispondere alla voce di "azione" nel dizionario di route, se presente.</span><span class="sxs-lookup"><span data-stu-id="25152-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="25152-223">Per ogni parametro dell'azione, se il parametro non viene eseguito dall'URI, quindi il nome del parametro deve essere trovato nel dizionario della route o nella stringa di query dell'URI.</span><span class="sxs-lookup"><span data-stu-id="25152-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="25152-224">(Parametri facoltativi e parametri con tipi complessi sono esclusi.)</span><span class="sxs-lookup"><span data-stu-id="25152-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="25152-225">Provare a cui corrisponde il maggior numero di parametri.</span><span class="sxs-lookup"><span data-stu-id="25152-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="25152-226">La corrispondenza migliore potrebbe essere un metodo senza parametri.</span><span class="sxs-lookup"><span data-stu-id="25152-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="25152-227">Esempio esteso</span><span class="sxs-lookup"><span data-stu-id="25152-227">Extended Example</span></span>

<span data-ttu-id="25152-228">Route:</span><span class="sxs-lookup"><span data-stu-id="25152-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="25152-229">Controller:</span><span class="sxs-lookup"><span data-stu-id="25152-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="25152-230">Richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="25152-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="25152-231">Corrispondenza di route</span><span class="sxs-lookup"><span data-stu-id="25152-231">Route Matching</span></span>

<span data-ttu-id="25152-232">L'URI corrisponde alla route denominata "DefaultApi".</span><span class="sxs-lookup"><span data-stu-id="25152-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="25152-233">Il dizionario della route contiene le voci seguenti:</span><span class="sxs-lookup"><span data-stu-id="25152-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="25152-234">controller: "prodotti"</span><span class="sxs-lookup"><span data-stu-id="25152-234">controller: "products"</span></span>
- <span data-ttu-id="25152-235">id: "1"</span><span class="sxs-lookup"><span data-stu-id="25152-235">id: "1"</span></span>

<span data-ttu-id="25152-236">Il dizionario della route non contiene parametri di stringa di query "version" e "Dettagli", ma questi verranno ancora considerati durante la selezione di azione.</span><span class="sxs-lookup"><span data-stu-id="25152-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="25152-237">Selezione del controller</span><span class="sxs-lookup"><span data-stu-id="25152-237">Controller Selection</span></span>

<span data-ttu-id="25152-238">Voce "controller" nel dizionario della route, è il tipo di controller `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="25152-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="25152-239">Selezione di azione</span><span class="sxs-lookup"><span data-stu-id="25152-239">Action Selection</span></span>

<span data-ttu-id="25152-240">La richiesta HTTP è una richiesta GET.</span><span class="sxs-lookup"><span data-stu-id="25152-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="25152-241">Le azioni del controller che supportano il recupero sono `GetAll`, `GetById`, e `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="25152-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="25152-242">Il dizionario della route non contiene una voce per "action", è necessario associare il nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="25152-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="25152-243">Successivamente, si tenta di corrispondere i nomi dei parametri per le azioni, esaminare solo le operazioni GET.</span><span class="sxs-lookup"><span data-stu-id="25152-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="25152-244">Operazione</span><span class="sxs-lookup"><span data-stu-id="25152-244">Action</span></span> | <span data-ttu-id="25152-245">Parametri di corrispondenza</span><span class="sxs-lookup"><span data-stu-id="25152-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="25152-246">none</span><span class="sxs-lookup"><span data-stu-id="25152-246">none</span></span> |
| `GetById` | <span data-ttu-id="25152-247">"id"</span><span class="sxs-lookup"><span data-stu-id="25152-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="25152-248">"nome"</span><span class="sxs-lookup"><span data-stu-id="25152-248">"name"</span></span> |

<span data-ttu-id="25152-249">Si noti che il *versione* parametro di `GetById` non viene considerato, perché è un parametro facoltativo.</span><span class="sxs-lookup"><span data-stu-id="25152-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="25152-250">Il `GetAll` metodo corrisponde in modo semplice.</span><span class="sxs-lookup"><span data-stu-id="25152-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="25152-251">Il `GetById` metodo inoltre corrisponde, perché il dizionario della route contiene "id".</span><span class="sxs-lookup"><span data-stu-id="25152-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="25152-252">Il `FindProductsByName` (metodo) non corrisponde.</span><span class="sxs-lookup"><span data-stu-id="25152-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="25152-253">Il `GetById` metodo wins, poiché corrisponde a un parametro, e nessun parametro per `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="25152-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="25152-254">Il metodo viene richiamato con i valori dei parametri seguenti:</span><span class="sxs-lookup"><span data-stu-id="25152-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="25152-255">*id* = 1</span><span class="sxs-lookup"><span data-stu-id="25152-255">*id* = 1</span></span>
- <span data-ttu-id="25152-256">*versione* = 1.5</span><span class="sxs-lookup"><span data-stu-id="25152-256">*version* = 1.5</span></span>

<span data-ttu-id="25152-257">Si noti che anche se *versione* non è stato utilizzato nell'algoritmo di selezione, il valore del parametro proviene dalla stringa di query URI.</span><span class="sxs-lookup"><span data-stu-id="25152-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="25152-258">Punti di estensione</span><span class="sxs-lookup"><span data-stu-id="25152-258">Extension Points</span></span>

<span data-ttu-id="25152-259">API Web fornisce punti di estensione per alcune parti del processo di routing.</span><span class="sxs-lookup"><span data-stu-id="25152-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="25152-260">Interfaccia</span><span class="sxs-lookup"><span data-stu-id="25152-260">Interface</span></span> | <span data-ttu-id="25152-261">Descrizione</span><span class="sxs-lookup"><span data-stu-id="25152-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="25152-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="25152-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="25152-263">Seleziona il controller.</span><span class="sxs-lookup"><span data-stu-id="25152-263">Selects the controller.</span></span> |
| <span data-ttu-id="25152-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="25152-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="25152-265">Ottiene l'elenco dei tipi di controller.</span><span class="sxs-lookup"><span data-stu-id="25152-265">Gets the list of controller types.</span></span> <span data-ttu-id="25152-266">Il **DefaultHttpControllerSelector** sceglie il tipo di controller da questo elenco.</span><span class="sxs-lookup"><span data-stu-id="25152-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="25152-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="25152-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="25152-268">Ottiene l'elenco degli assembly di progetto.</span><span class="sxs-lookup"><span data-stu-id="25152-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="25152-269">Il **IHttpControllerTypeResolver** interfaccia utilizza questo elenco per trovare i tipi di controller.</span><span class="sxs-lookup"><span data-stu-id="25152-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="25152-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="25152-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="25152-271">Crea nuove istanze di controller.</span><span class="sxs-lookup"><span data-stu-id="25152-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="25152-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="25152-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="25152-273">Seleziona l'azione.</span><span class="sxs-lookup"><span data-stu-id="25152-273">Selects the action.</span></span> |
| <span data-ttu-id="25152-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="25152-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="25152-275">Richiama l'azione.</span><span class="sxs-lookup"><span data-stu-id="25152-275">Invokes the action.</span></span> |

<span data-ttu-id="25152-276">Per garantire la propria implementazione di una di queste interfacce, utilizzare il **servizi** insieme il **HttpConfiguration** oggetto:</span><span class="sxs-lookup"><span data-stu-id="25152-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
