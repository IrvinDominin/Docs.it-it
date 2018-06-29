---
title: Middleware Riscrittura URL in ASP.NET Core
author: guardrex
description: Informazioni sulla riscrittura e il reindirizzamento di URL con il middleware Riscrittura URL nelle applicazioni ASP.NET Core.
ms.author: riande
ms.date: 08/17/2017
uid: fundamentals/url-rewriting
ms.openlocfilehash: d3484e222c4412a427d086c1b71a12b81095ba72
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276347"
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="ce964-103">Middleware Riscrittura URL in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce964-103">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="ce964-104">Di [Luke Latham](https://github.com/guardrex) e [Mikael Mengistu](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="ce964-104">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="ce964-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce964-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ce964-106">La riscrittura URL è l'azione di modificare gli URL di richiesta in base a una o più regole predefinite.</span><span class="sxs-lookup"><span data-stu-id="ce964-106">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="ce964-107">Il processo di riscrittura URL crea un'astrazione tra i percorsi delle risorse e i relativi indirizzi, in modo che i percorsi e gli indirizzi non risultino strettamente collegati.</span><span class="sxs-lookup"><span data-stu-id="ce964-107">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="ce964-108">La riscrittura URL risulta utile in diversi scenari:</span><span class="sxs-lookup"><span data-stu-id="ce964-108">There are several scenarios where URL rewriting is valuable:</span></span>

* <span data-ttu-id="ce964-109">Spostamento o sostituzione temporanea o permanente di risorse server pur mantenendo localizzatori stabili di queste risorse.</span><span class="sxs-lookup"><span data-stu-id="ce964-109">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources.</span></span>
* <span data-ttu-id="ce964-110">Suddivisione dell'elaborazione delle richieste tra diverse app o tra diverse aree di un'unica app.</span><span class="sxs-lookup"><span data-stu-id="ce964-110">Splitting request processing across different apps or across areas of one app.</span></span>
* <span data-ttu-id="ce964-111">Rimozione, aggiunta o riorganizzazione di segmenti URL nelle richieste in ingresso.</span><span class="sxs-lookup"><span data-stu-id="ce964-111">Removing, adding, or reorganizing URL segments on incoming requests.</span></span>
* <span data-ttu-id="ce964-112">Ottimizzazione degli URL pubblici per l'ottimizzazione motore di ricerca (SEO, Search Engine Optimization).</span><span class="sxs-lookup"><span data-stu-id="ce964-112">Optimizing public URLs for Search Engine Optimization (SEO).</span></span>
* <span data-ttu-id="ce964-113">Autorizzazione dell'uso di URL pubblici brevi per consentire agli utenti di prevedere il contenuto trovato seguendo un collegamento.</span><span class="sxs-lookup"><span data-stu-id="ce964-113">Permitting the use of friendly public URLs to help people predict the content they will find by following a link.</span></span>
* <span data-ttu-id="ce964-114">Reindirizzamento delle richieste non protette a endpoint protetti.</span><span class="sxs-lookup"><span data-stu-id="ce964-114">Redirecting insecure requests to secure endpoints.</span></span>
* <span data-ttu-id="ce964-115">Blocco dell'hotlinking di immagini.</span><span class="sxs-lookup"><span data-stu-id="ce964-115">Preventing image hotlinking.</span></span>

<span data-ttu-id="ce964-116">È possibile definire regole per la modifica dell'URL con diverse modalità, tra cui Regex, le regole di modulo Apache mod_rewrite, le regole di modulo IIS Rewrite e l'uso di logica delle regole personalizzata.</span><span class="sxs-lookup"><span data-stu-id="ce964-116">You can define rules for changing the URL in several ways, including Regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="ce964-117">Questo documento presenta la riscrittura URL con istruzioni per l'uso del middleware Riscrittura URL nelle applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce964-117">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="ce964-118">La riscrittura URL può ridurre le prestazioni di un'app.</span><span class="sxs-lookup"><span data-stu-id="ce964-118">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="ce964-119">Ove possibile, limitare il numero e la complessità delle regole.</span><span class="sxs-lookup"><span data-stu-id="ce964-119">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="ce964-120">Reindirizzamento URL e riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="ce964-120">URL redirect and URL rewrite</span></span>

<span data-ttu-id="ce964-121">La differenza tra i termini *reindirizzamento URL* e *riscrittura URL* può apparire minima, ma ha implicazioni importanti nell'offerta di risorse ai clienti.</span><span class="sxs-lookup"><span data-stu-id="ce964-121">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="ce964-122">Il middleware Riscrittura URL di ASP.NET Core è in grado di svolgere entrambe le funzioni.</span><span class="sxs-lookup"><span data-stu-id="ce964-122">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="ce964-123">Un *reindirizzamento URL* è un'operazione lato client, in cui viene richiesto al client di accedere a una risorsa a un altro indirizzo.</span><span class="sxs-lookup"><span data-stu-id="ce964-123">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="ce964-124">Questa operazione richiede una sequenza di andata e ritorno al server.</span><span class="sxs-lookup"><span data-stu-id="ce964-124">This requires a round-trip to the server.</span></span> <span data-ttu-id="ce964-125">L'URL di reindirizzamento restituito al client viene visualizzato nella barra degli indirizzi del browser quando il client effettua una nuova richiesta per la risorsa.</span><span class="sxs-lookup"><span data-stu-id="ce964-125">The redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> 

<span data-ttu-id="ce964-126">Se `/resource` viene *reindirizzato* a `/different-resource` il client richiede `/resource`.</span><span class="sxs-lookup"><span data-stu-id="ce964-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`.</span></span> <span data-ttu-id="ce964-127">Il server risponde che il client deve ottenere la risorsa in `/different-resource` con un codice di stato che indica se il reindirizzamento è temporaneo o permanente.</span><span class="sxs-lookup"><span data-stu-id="ce964-127">The server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="ce964-128">Il client esegue una nuova richiesta per la risorsa all'URL di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="ce964-128">The client executes a new request for the resource at the redirect URL.</span></span>

![Un endpoint di servizio WebAPI è stato modificato temporaneamente dalla versione 1 (v1) alla versione 2 (v2) nel server.](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="ce964-134">Quando si reindirizzano le richieste a un URL diverso, indicare se il reindirizzamento è temporaneo o permanente.</span><span class="sxs-lookup"><span data-stu-id="ce964-134">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="ce964-135">Il codice di stato 301 (Spostato permanentemente) viene usato se la risorsa ha un nuovo URL definitivo e si vuole indicare al client che tale URL dovrà essere usato per tutte le richieste future della risorsa.</span><span class="sxs-lookup"><span data-stu-id="ce964-135">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="ce964-136">*Quando riceve un codice di stato 301 il client può memorizzare la risposta nella cache.*</span><span class="sxs-lookup"><span data-stu-id="ce964-136">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="ce964-137">Il codice di stato 302 (Trovato) viene usato quando il reindirizzamento è temporaneo o comunque soggetto a modifiche, in modo che il client non dovrà archiviare e riusare l'URL di reindirizzamento in futuro.</span><span class="sxs-lookup"><span data-stu-id="ce964-137">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="ce964-138">Per altre informazioni, vedere [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (RFC 2616: Definizioni dei codici di stato).</span><span class="sxs-lookup"><span data-stu-id="ce964-138">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="ce964-139">Una *riscrittura URL* è un'operazione lato server che rende disponibile una risorsa da un indirizzo risorsa alternativo.</span><span class="sxs-lookup"><span data-stu-id="ce964-139">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="ce964-140">La riscrittura URL non richiede una sequenza di andata e ritorno al server.</span><span class="sxs-lookup"><span data-stu-id="ce964-140">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="ce964-141">L'URL riscritto non viene restituito al client e non viene visualizzato nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="ce964-141">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="ce964-142">Quando `/resource` viene *riscritto* in `/different-resource`, il client richiede `/resource` e il server recupera *internamente* la risorsa in `/different-resource`.</span><span class="sxs-lookup"><span data-stu-id="ce964-142">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="ce964-143">Anche se il client fosse in grado di recuperare la risorsa nell'URL riscritto, non verrà informato dell'esistenza della risorsa nell'URL riscritto quando effettua la richiesta e riceve la risposta.</span><span class="sxs-lookup"><span data-stu-id="ce964-143">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![Un endpoint servizio WebAPI è stato modificato dalla versione 1 (v1) alla versione 2 (v2) sul server.](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="ce964-148">App di esempio per la riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="ce964-148">URL rewriting sample app</span></span>

<span data-ttu-id="ce964-149">È possibile esplorare le funzionalità del middleware Riscrittura URL con l'[app di esempio di riscrittura URL](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span><span class="sxs-lookup"><span data-stu-id="ce964-149">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/sample/).</span></span> <span data-ttu-id="ce964-150">L'app esegue le regole di riscrittura e reindirizzamento e visualizza l'URL riscritto o reindirizzato.</span><span class="sxs-lookup"><span data-stu-id="ce964-150">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="ce964-151">Quando usare il middleware Riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="ce964-151">When to use URL Rewriting Middleware</span></span>

<span data-ttu-id="ce964-152">Usare il middleware Riscrittura URL quando non è possibile usare il modulo [URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) con IIS in Windows Server, il modulo [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/) in Apache Server, la [riscrittura URL in Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/) o quando l'app è ospitata nel [server HTTP.sys](xref:fundamentals/servers/httpsys) (chiamato in precedenza [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="ce964-152">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="ce964-153">I motivi principali per l'uso delle tecnologie di riscrittura URL basate su server in IIS, Apache o Nginx sono il fatto che il middleware non supporta tutte le funzionalità di questi moduli e il fatto che le prestazioni del middleware possono risultare inferiori a quelle dei moduli.</span><span class="sxs-lookup"><span data-stu-id="ce964-153">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="ce964-154">Tuttavia alcune funzionalità dei moduli server non funzionano con i progetti ASP.NET Core, ad esempio i vincoli `IsFile` e `IsDirectory` del modulo IIS Rewrite.</span><span class="sxs-lookup"><span data-stu-id="ce964-154">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="ce964-155">In questi scenari è necessario usare il middleware.</span><span class="sxs-lookup"><span data-stu-id="ce964-155">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="ce964-156">Pacchetto</span><span class="sxs-lookup"><span data-stu-id="ce964-156">Package</span></span>

<span data-ttu-id="ce964-157">Per includere il middleware nel progetto, aggiungere un riferimento al pacchetto [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/).</span><span class="sxs-lookup"><span data-stu-id="ce964-157">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="ce964-158">Questa funzionalità è disponibile per le app destinate ad ASP.NET Core 1.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ce964-158">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="ce964-159">Estensioni e opzioni</span><span class="sxs-lookup"><span data-stu-id="ce964-159">Extension and options</span></span>

<span data-ttu-id="ce964-160">È possibile definire regole di riscrittura e reindirizzamento URL personalizzate creando un'istanza della classe `RewriteOptions` con metodi di estensione per le singole regole.</span><span class="sxs-lookup"><span data-stu-id="ce964-160">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="ce964-161">Concatenare più regole nell'ordine in cui si vuole che vengano elaborate.</span><span class="sxs-lookup"><span data-stu-id="ce964-161">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="ce964-162">Le opzioni `RewriteOptions` vengono passare al middleware Riscrittura URL quando questo viene aggiunto alla pipeline della richiesta con `app.UseRewriter(options);`.</span><span class="sxs-lookup"><span data-stu-id="ce964-162">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce964-163">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce964-163">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce964-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce964-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1")
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true)
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")
        .Add(RedirectXMLRequests)
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

### <a name="url-redirect"></a><span data-ttu-id="ce964-165">Renidirizzamento URL</span><span class="sxs-lookup"><span data-stu-id="ce964-165">URL redirect</span></span>

<span data-ttu-id="ce964-166">Per reindirizzare le richieste, usare `AddRedirect`.</span><span class="sxs-lookup"><span data-stu-id="ce964-166">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="ce964-167">Il primo parametro contiene l'espressione regolare per la corrispondenza in base al percorso dell'URL in ingresso.</span><span class="sxs-lookup"><span data-stu-id="ce964-167">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="ce964-168">Il secondo parametro è la stringa di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="ce964-168">The second parameter is the replacement string.</span></span> <span data-ttu-id="ce964-169">Il terzo parametro, se presente, specifica il codice di stato.</span><span class="sxs-lookup"><span data-stu-id="ce964-169">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="ce964-170">Se il codice di stato non è specificato, assume il valore 302 (Trovato), che indica che la risorsa è stata temporaneamente spostata o sostituita.</span><span class="sxs-lookup"><span data-stu-id="ce964-170">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce964-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce964-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce964-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce964-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirect("redirect-rule/(.*)", "redirected/$1");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="ce964-173">In un browser con gli strumenti di sviluppo abilitati, creare una richiesta all'app di esempio con il percorso `/redirect-rule/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="ce964-173">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="ce964-174">L'espressione regolare definisce la corrispondenza con il percorso di richiesta in `redirect-rule/(.*)` e il percorso viene sostituito con `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="ce964-174">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="ce964-175">L'URL di reindirizzamento viene restituito al client con il codice di stato 302 (Trovato).</span><span class="sxs-lookup"><span data-stu-id="ce964-175">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="ce964-176">Il browser invia una nuova richiesta all'URL di reindirizzamento, che viene visualizzata nella barra degli indirizzi del browser.</span><span class="sxs-lookup"><span data-stu-id="ce964-176">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="ce964-177">Dato che nessuna regola dell'app di esempio presenta una corrispondenza con l'URL di reindirizzamento, la seconda richiesta riceve una risposta 200 (OK) dall'app e il corpo della risposta visualizza l'URL di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="ce964-177">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="ce964-178">Quando un URL viene *reindirizzato* viene eseguita una sequenza di andata e ritorno al server.</span><span class="sxs-lookup"><span data-stu-id="ce964-178">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="ce964-179">Prestare attenzione quando si definiscono le regole di reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="ce964-179">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="ce964-180">Le regole di reindirizzamento vengono valutate in ogni richiesta all'app, anche dopo un reindirizzamento.</span><span class="sxs-lookup"><span data-stu-id="ce964-180">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="ce964-181">È facile creare inavvertitamente un ciclo di reindirizzamento infinito.</span><span class="sxs-lookup"><span data-stu-id="ce964-181">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="ce964-182">Richiesta originale: `/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="ce964-182">Original Request: `/redirect-rule/1234/5678`</span></span>

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="ce964-184">La parte dell'espressione racchiusa tra parentesi è denominata *gruppo Capture*.</span><span class="sxs-lookup"><span data-stu-id="ce964-184">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="ce964-185">Il punto (`.`) dell'espressione significa *corrispondenza con qualsiasi carattere*.</span><span class="sxs-lookup"><span data-stu-id="ce964-185">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="ce964-186">L'asterisco (`*`) indica *corrispondenza con il carattere precedente per zero o più volte*.</span><span class="sxs-lookup"><span data-stu-id="ce964-186">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="ce964-187">Di conseguenza gli ultimi due i segmenti di percorso dell'URL `1234/5678` vengono acquisiti tramite il gruppo Capture `(.*)`.</span><span class="sxs-lookup"><span data-stu-id="ce964-187">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="ce964-188">Qualsiasi valore visualizzato nell'URL della richiesta dopo `redirect-rule/` viene acquisito da questo gruppo Capture.</span><span class="sxs-lookup"><span data-stu-id="ce964-188">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="ce964-189">Nella stringa di sostituzione i gruppi acquisiti vengono inseriti nella stringa con il simbolo di dollaro (`$`) seguito dal numero di sequenza dell'acquisizione.</span><span class="sxs-lookup"><span data-stu-id="ce964-189">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="ce964-190">Il primo valore del gruppo Capture viene ottenuto con `$1`, il secondo con `$2` e la procedura continua in sequenza per i gruppi Capture presenti nell'espressione regolare.</span><span class="sxs-lookup"><span data-stu-id="ce964-190">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="ce964-191">Nell'espressione regolare della regola di reindirizzamento dell'app di esempio è presente un solo gruppo acquisito, pertanto nella stringa di sostituzione è presente un solo gruppo inserito, ovvero `$1`.</span><span class="sxs-lookup"><span data-stu-id="ce964-191">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="ce964-192">Quando viene applicata la regola, l'URL diventa `/redirected/1234/5678`.</span><span class="sxs-lookup"><span data-stu-id="ce964-192">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="ce964-193">Reindirizzamento URL a un endpoint protetto</span><span class="sxs-lookup"><span data-stu-id="ce964-193">URL redirect to a secure endpoint</span></span>

<span data-ttu-id="ce964-194">Usare `AddRedirectToHttps` per reindirizzare le richieste HTTP allo stesso host e allo stesso percorso mediante il protocollo HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="ce964-194">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="ce964-195">Se il codice di stato non viene specificato, il middleware imposta il valore predefinito 302 (Trovato).</span><span class="sxs-lookup"><span data-stu-id="ce964-195">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="ce964-196">Se la porta non viene specificata, il middleware imposta il valore predefinito `null`, che indica che il protocollo diventa `https://` e il client accede alla risorsa sulla porta 443.</span><span class="sxs-lookup"><span data-stu-id="ce964-196">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="ce964-197">L'esempio indica come impostare il codice di stato su 301 (Spostato permanentemente) e come cambiare il numero di porta in 5001.</span><span class="sxs-lookup"><span data-stu-id="ce964-197">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttps(301, 5001);

    app.UseRewriter(options);
}
```

<span data-ttu-id="ce964-198">Usare `AddRedirectToHttpsPermanent` per reindirizzare le richieste non protette allo stesso host e allo stesso percorso con il protocollo di protezione HTTPS (`https://` sulla porta 443).</span><span class="sxs-lookup"><span data-stu-id="ce964-198">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="ce964-199">Il middleware imposta il codice stato su 301 (Spostato permanentemente).</span><span class="sxs-lookup"><span data-stu-id="ce964-199">The middleware sets the status code to 301 (Moved Permanently).</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRedirectToHttpsPermanent();

    app.UseRewriter(options);
}
```

> [!NOTE]
> <span data-ttu-id="ce964-200">Per il reindirizzamento a HTTPS quando non sono richieste regole di reindirizzamento aggiuntive, si consiglia di usare il middleware di reindirizzamento HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ce964-200">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware.</span></span> <span data-ttu-id="ce964-201">Per altre informazioni, vedere l'argomento [Imporre HTTPS](xref:security/enforcing-ssl#require-https).</span><span class="sxs-lookup"><span data-stu-id="ce964-201">For more information, see the [Enforce HTTPS](xref:security/enforcing-ssl#require-https) topic.</span></span>

<span data-ttu-id="ce964-202">L'app di esempio indica come usare `AddRedirectToHttps` o `AddRedirectToHttpsPermanent`.</span><span class="sxs-lookup"><span data-stu-id="ce964-202">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="ce964-203">Aggiungere il metodo di estensione a `RewriteOptions`.</span><span class="sxs-lookup"><span data-stu-id="ce964-203">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="ce964-204">Eseguire una richiesta non sicura all'app su qualsiasi URL.</span><span class="sxs-lookup"><span data-stu-id="ce964-204">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="ce964-205">Ignorare l'avviso di sicurezza del browser indicante che il certificato autofirmato non è attendibile oppure creare un'eccezione per rendere affidabile il certificato.</span><span class="sxs-lookup"><span data-stu-id="ce964-205">Dismiss the browser security warning that the self-signed certificate is untrusted or create an exception to trust the certificate.</span></span>

<span data-ttu-id="ce964-206">Richiesta originale con `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="ce964-206">Original Request using `AddRedirectToHttps(301, 5001)`: `http://localhost:5000/secure`</span></span>

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="ce964-208">Richiesta originale con `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span><span class="sxs-lookup"><span data-stu-id="ce964-208">Original Request using `AddRedirectToHttpsPermanent`: `http://localhost:5000/secure`</span></span>

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="ce964-210">Riscrittura URL</span><span class="sxs-lookup"><span data-stu-id="ce964-210">URL rewrite</span></span>

<span data-ttu-id="ce964-211">Usare `AddRewrite` per creare una regola di riscrittura URL.</span><span class="sxs-lookup"><span data-stu-id="ce964-211">Use `AddRewrite` to create a rule for rewriting URLs.</span></span> <span data-ttu-id="ce964-212">Il primo parametro contiene l'espressione regolare per la corrispondenza in base al percorso dell'URL in ingresso.</span><span class="sxs-lookup"><span data-stu-id="ce964-212">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="ce964-213">Il secondo parametro è la stringa di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="ce964-213">The second parameter is the replacement string.</span></span> <span data-ttu-id="ce964-214">Il terzo parametro, `skipRemainingRules: {true|false}`, indica al middleware se ignorare le regole di riscrittura aggiuntive quando viene applicata la regola corrente.</span><span class="sxs-lookup"><span data-stu-id="ce964-214">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce964-215">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce964-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce964-216">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce964-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", 
            skipRemainingRules: true);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="ce964-217">Richiesta originale: `/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="ce964-217">Original Request: `/rewrite-rule/1234/5678`</span></span>

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richiesta e risposta](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="ce964-219">Il primo elemento che si nota nell'espressione regolare è l'accento circonflesso (`^`) all'inizio dell'espressione.</span><span class="sxs-lookup"><span data-stu-id="ce964-219">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="ce964-220">Questo carattere indica che la corrispondenza inizia all'inizio del percorso dell'URL.</span><span class="sxs-lookup"><span data-stu-id="ce964-220">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="ce964-221">Nell'esempio precedente con la regola di reindirizzamento, `redirect-rule/(.*)`, non è presente un accento circonflesso all'inizio dell'espressione regolare, pertanto nel percorso può essere presente qualsiasi carattere prima di `redirect-rule/` e la corrispondenza viene comunque rilevata.</span><span class="sxs-lookup"><span data-stu-id="ce964-221">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="ce964-222">Path</span><span class="sxs-lookup"><span data-stu-id="ce964-222">Path</span></span>                               | <span data-ttu-id="ce964-223">Corrispondenza con</span><span class="sxs-lookup"><span data-stu-id="ce964-223">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="ce964-224">Yes</span><span class="sxs-lookup"><span data-stu-id="ce964-224">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="ce964-225">Yes</span><span class="sxs-lookup"><span data-stu-id="ce964-225">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="ce964-226">Yes</span><span class="sxs-lookup"><span data-stu-id="ce964-226">Yes</span></span>   |

<span data-ttu-id="ce964-227">La regola di riscrittura, `^rewrite-rule/(\d+)/(\d+)`, rileva la corrispondenza dei percorsi solo se iniziano con `rewrite-rule/`.</span><span class="sxs-lookup"><span data-stu-id="ce964-227">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="ce964-228">Osservare la differenza nella corrispondenza tra la regola di riscrittura in basso e la regola di reindirizzamento in alto.</span><span class="sxs-lookup"><span data-stu-id="ce964-228">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="ce964-229">Path</span><span class="sxs-lookup"><span data-stu-id="ce964-229">Path</span></span>                              | <span data-ttu-id="ce964-230">Corrispondenza con</span><span class="sxs-lookup"><span data-stu-id="ce964-230">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="ce964-231">Yes</span><span class="sxs-lookup"><span data-stu-id="ce964-231">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="ce964-232">No</span><span class="sxs-lookup"><span data-stu-id="ce964-232">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="ce964-233">No</span><span class="sxs-lookup"><span data-stu-id="ce964-233">No</span></span>    |

<span data-ttu-id="ce964-234">Dopo la parte `^rewrite-rule/` dell'espressione sono presenti due gruppi Capture, `(\d+)/(\d+)`.</span><span class="sxs-lookup"><span data-stu-id="ce964-234">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="ce964-235">`\d` indica *corrispondenza con una cifra (numero)*.</span><span class="sxs-lookup"><span data-stu-id="ce964-235">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="ce964-236">Il segno più (`+`) indica *corrispondenza con uno o più dei caratteri precedenti*.</span><span class="sxs-lookup"><span data-stu-id="ce964-236">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="ce964-237">Di conseguenza l'URL deve contenere un numero seguito da una barra seguita a sua volta da un altro numero.</span><span class="sxs-lookup"><span data-stu-id="ce964-237">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="ce964-238">Questi gruppi Capture vengono inseriti nell'URL riscritto come `$1` e `$2`.</span><span class="sxs-lookup"><span data-stu-id="ce964-238">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="ce964-239">La stringa di sostituzione della regola di riscrittura inserisce i gruppi acquisiti nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="ce964-239">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="ce964-240">Il percorso richiesto `/rewrite-rule/1234/5678` viene riscritto per ottenere la risorsa in `/rewritten?var1=1234&var2=5678`.</span><span class="sxs-lookup"><span data-stu-id="ce964-240">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="ce964-241">Se nella richiesta originale è presente una stringa di query, la stringa viene mantenuta quando l'URL viene riscritto.</span><span class="sxs-lookup"><span data-stu-id="ce964-241">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="ce964-242">Non viene eseguita nessuna sequenza di andata e ritorno al server per ottenere la risorsa.</span><span class="sxs-lookup"><span data-stu-id="ce964-242">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="ce964-243">Se la risorsa esiste viene recuperata e restituita al client con il codice stato 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="ce964-243">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="ce964-244">Poiché il client non è reindirizzato, l'URL nella barra degli indirizzi del browser non cambia.</span><span class="sxs-lookup"><span data-stu-id="ce964-244">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="ce964-245">Dal punto di vista del client, l'operazione di riscrittura URL non si è verificata.</span><span class="sxs-lookup"><span data-stu-id="ce964-245">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="ce964-246">Se possibile, usare sempre `skipRemainingRules: true`, perché la corrispondenza tra le regole è un processo dispendioso che rallenta il tempo di risposta dell'app.</span><span class="sxs-lookup"><span data-stu-id="ce964-246">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="ce964-247">Per velocizzare il tempo di risposta dell'app:</span><span class="sxs-lookup"><span data-stu-id="ce964-247">For the fastest app response:</span></span>
> * <span data-ttu-id="ce964-248">Ordinare le regole di riscrittura dalla regola che origina più corrispondenze a quella che origina meno corrispondenze.</span><span class="sxs-lookup"><span data-stu-id="ce964-248">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="ce964-249">Quando viene rilevata una corrispondenza e non sono necessarie altre operazioni di elaborazione delle regole, ignorare l'elaborazione delle regole rimanenti.</span><span class="sxs-lookup"><span data-stu-id="ce964-249">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="ce964-250">Modulo Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="ce964-250">Apache mod_rewrite</span></span>

<span data-ttu-id="ce964-251">Applicare le regole del modulo Apache mod_rewrite con `AddApacheModRewrite`.</span><span class="sxs-lookup"><span data-stu-id="ce964-251">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="ce964-252">Assicurarsi che il file delle regole venga distribuito con l'app.</span><span class="sxs-lookup"><span data-stu-id="ce964-252">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="ce964-253">Per altre informazioni ed esempi di regole mod_rewrite, vedere [Modulo Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span><span class="sxs-lookup"><span data-stu-id="ce964-253">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce964-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce964-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="ce964-255">`StreamReader` viene usato per leggere le regole del file di regole *ApacheModRewrite.txt*.</span><span class="sxs-lookup"><span data-stu-id="ce964-255">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=3-4,12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce964-256">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce964-256">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="ce964-257">Il primo parametro accetta `IFileProvider`, che viene reso disponibile tramite [Dependency Injection](dependency-injection.md) (Inserimento di dipendenze).</span><span class="sxs-lookup"><span data-stu-id="ce964-257">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="ce964-258">`IHostingEnvironment` viene inserito per specificare `ContentRootFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="ce964-258">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="ce964-259">Il secondo parametro è il percorso del file di regole, ovvero *ApacheModRewrite.txt* nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="ce964-259">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="ce964-260">L'app di esempio reindirizza le richieste da `/apache-mod-rules-redirect/(.\*)` a `/redirected?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="ce964-260">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="ce964-261">Il codice di stato della risposta è 302 (Trovato).</span><span class="sxs-lookup"><span data-stu-id="ce964-261">The response status code is 302 (Found).</span></span>

[!code[](url-rewriting/sample/ApacheModRewrite.txt)]

<span data-ttu-id="ce964-262">Richiesta originale: `/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="ce964-262">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="ce964-264">Variabili server supportate</span><span class="sxs-lookup"><span data-stu-id="ce964-264">Supported server variables</span></span>

<span data-ttu-id="ce964-265">Il middleware supporta le seguenti variabili server del modulo Apache mod_rewrite:</span><span class="sxs-lookup"><span data-stu-id="ce964-265">The middleware supports the following Apache mod_rewrite server variables:</span></span>

* <span data-ttu-id="ce964-266">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="ce964-266">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="ce964-267">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="ce964-267">HTTP_ACCEPT</span></span>
* <span data-ttu-id="ce964-268">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="ce964-268">HTTP_CONNECTION</span></span>
* <span data-ttu-id="ce964-269">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="ce964-269">HTTP_COOKIE</span></span>
* <span data-ttu-id="ce964-270">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="ce964-270">HTTP_FORWARDED</span></span>
* <span data-ttu-id="ce964-271">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="ce964-271">HTTP_HOST</span></span>
* <span data-ttu-id="ce964-272">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="ce964-272">HTTP_REFERER</span></span>
* <span data-ttu-id="ce964-273">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="ce964-273">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="ce964-274">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ce964-274">HTTPS</span></span>
* <span data-ttu-id="ce964-275">IPV6</span><span class="sxs-lookup"><span data-stu-id="ce964-275">IPV6</span></span>
* <span data-ttu-id="ce964-276">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="ce964-276">QUERY_STRING</span></span>
* <span data-ttu-id="ce964-277">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="ce964-277">REMOTE_ADDR</span></span>
* <span data-ttu-id="ce964-278">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="ce964-278">REMOTE_PORT</span></span>
* <span data-ttu-id="ce964-279">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="ce964-279">REQUEST_FILENAME</span></span>
* <span data-ttu-id="ce964-280">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="ce964-280">REQUEST_METHOD</span></span>
* <span data-ttu-id="ce964-281">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="ce964-281">REQUEST_SCHEME</span></span>
* <span data-ttu-id="ce964-282">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="ce964-282">REQUEST_URI</span></span>
* <span data-ttu-id="ce964-283">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="ce964-283">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="ce964-284">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="ce964-284">SERVER_ADDR</span></span>
* <span data-ttu-id="ce964-285">SERVER_PORT</span><span class="sxs-lookup"><span data-stu-id="ce964-285">SERVER_PORT</span></span>
* <span data-ttu-id="ce964-286">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="ce964-286">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="ce964-287">TIME</span><span class="sxs-lookup"><span data-stu-id="ce964-287">TIME</span></span>
* <span data-ttu-id="ce964-288">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="ce964-288">TIME_DAY</span></span>
* <span data-ttu-id="ce964-289">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="ce964-289">TIME_HOUR</span></span>
* <span data-ttu-id="ce964-290">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="ce964-290">TIME_MIN</span></span>
* <span data-ttu-id="ce964-291">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="ce964-291">TIME_MON</span></span>
* <span data-ttu-id="ce964-292">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="ce964-292">TIME_SEC</span></span>
* <span data-ttu-id="ce964-293">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="ce964-293">TIME_WDAY</span></span>
* <span data-ttu-id="ce964-294">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="ce964-294">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="ce964-295">Regole di IIS URL Rewrite Module</span><span class="sxs-lookup"><span data-stu-id="ce964-295">IIS URL Rewrite Module rules</span></span>

<span data-ttu-id="ce964-296">Per usare regole valide per IIS URL Rewrite Module, usare `AddIISUrlRewrite`.</span><span class="sxs-lookup"><span data-stu-id="ce964-296">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="ce964-297">Assicurarsi che il file delle regole venga distribuito con l'app.</span><span class="sxs-lookup"><span data-stu-id="ce964-297">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="ce964-298">Non indirizzare il middleware all'uso del file *web.config* personalizzato quando è in esecuzione in Windows Server IIS.</span><span class="sxs-lookup"><span data-stu-id="ce964-298">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="ce964-299">Con IIS queste regole devono essere archiviate fuori dal file *web.config*, per evitare conflitti con il modulo IIS Rewrite.</span><span class="sxs-lookup"><span data-stu-id="ce964-299">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="ce964-300">Per altre informazioni ed esempi di regole di IIS URL Rewrite Module, vedere [Using URL Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (Uso di URL Rewrite Module 2.0) e [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (Guida di riferimento per la configurazione di URL Rewrite Module).</span><span class="sxs-lookup"><span data-stu-id="ce964-300">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce964-301">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce964-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="ce964-302">Un elemento `StreamReader` viene usato per leggere le regole del file di regole *IISUrlRewrite.xml*.</span><span class="sxs-lookup"><span data-stu-id="ce964-302">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=5-6,13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce964-303">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce964-303">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="ce964-304">Il primo parametro accetta `IFileProvider` e il secondo parametro è il percorso del file di regole XML, ovvero *IISUrlRewrite.xml* nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="ce964-304">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    var options = new RewriteOptions()
        .AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml");

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="ce964-305">L'app di esempio riscrive le richieste da `/iis-rules-rewrite/(.*)` a `/rewritten?id=$1`.</span><span class="sxs-lookup"><span data-stu-id="ce964-305">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="ce964-306">La risposta viene inviata al client con un codice di stato 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="ce964-306">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[](url-rewriting/sample/IISUrlRewrite.xml)]

<span data-ttu-id="ce964-307">Richiesta originale: `/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="ce964-307">Original Request: `/iis-rules-rewrite/1234`</span></span>

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richiesta e risposta](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="ce964-309">Se è presente un modulo IIS Rewrite attivo con regole di livello server configurate che possono avere effetti indesiderati nell'app, è possibile disabilitare il modulo IIS Rewrite per un'app.</span><span class="sxs-lookup"><span data-stu-id="ce964-309">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="ce964-310">Per altre informazioni, vedere [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules) (Disabilitazione di moduli IIS).</span><span class="sxs-lookup"><span data-stu-id="ce964-310">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="ce964-311">Funzionalità non supportate</span><span class="sxs-lookup"><span data-stu-id="ce964-311">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce964-312">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce964-312">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ce964-313">Il middleware rilasciato con ASP.NET Core 2.x non supporta le seguenti funzionalità di IIS URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="ce964-313">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="ce964-314">Regole in uscita</span><span class="sxs-lookup"><span data-stu-id="ce964-314">Outbound Rules</span></span>
* <span data-ttu-id="ce964-315">Variabili server personalizzate</span><span class="sxs-lookup"><span data-stu-id="ce964-315">Custom Server Variables</span></span>
* <span data-ttu-id="ce964-316">Caratteri jolly</span><span class="sxs-lookup"><span data-stu-id="ce964-316">Wildcards</span></span>
* <span data-ttu-id="ce964-317">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="ce964-317">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce964-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce964-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ce964-319">Il middleware rilasciato con ASP.NET Core 1.x non supporta le seguenti funzionalità di IIS URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="ce964-319">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>

* <span data-ttu-id="ce964-320">Regole globali</span><span class="sxs-lookup"><span data-stu-id="ce964-320">Global Rules</span></span>
* <span data-ttu-id="ce964-321">Regole in uscita</span><span class="sxs-lookup"><span data-stu-id="ce964-321">Outbound Rules</span></span>
* <span data-ttu-id="ce964-322">Mapping di riscrittura</span><span class="sxs-lookup"><span data-stu-id="ce964-322">Rewrite Maps</span></span>
* <span data-ttu-id="ce964-323">Azione CustomResponse</span><span class="sxs-lookup"><span data-stu-id="ce964-323">CustomResponse Action</span></span>
* <span data-ttu-id="ce964-324">Variabili server personalizzate</span><span class="sxs-lookup"><span data-stu-id="ce964-324">Custom Server Variables</span></span>
* <span data-ttu-id="ce964-325">Caratteri jolly</span><span class="sxs-lookup"><span data-stu-id="ce964-325">Wildcards</span></span>
* <span data-ttu-id="ce964-326">Action:CustomResponse</span><span class="sxs-lookup"><span data-stu-id="ce964-326">Action:CustomResponse</span></span>
* <span data-ttu-id="ce964-327">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="ce964-327">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="ce964-328">Variabili server supportate</span><span class="sxs-lookup"><span data-stu-id="ce964-328">Supported server variables</span></span>

<span data-ttu-id="ce964-329">Il middleware supporta le seguenti variabili server di IIS URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="ce964-329">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>

* <span data-ttu-id="ce964-330">CONTENT_LENGTH</span><span class="sxs-lookup"><span data-stu-id="ce964-330">CONTENT_LENGTH</span></span>
* <span data-ttu-id="ce964-331">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="ce964-331">CONTENT_TYPE</span></span>
* <span data-ttu-id="ce964-332">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="ce964-332">HTTP_ACCEPT</span></span>
* <span data-ttu-id="ce964-333">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="ce964-333">HTTP_CONNECTION</span></span>
* <span data-ttu-id="ce964-334">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="ce964-334">HTTP_COOKIE</span></span>
* <span data-ttu-id="ce964-335">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="ce964-335">HTTP_HOST</span></span>
* <span data-ttu-id="ce964-336">HTTP_REFERER</span><span class="sxs-lookup"><span data-stu-id="ce964-336">HTTP_REFERER</span></span>
* <span data-ttu-id="ce964-337">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="ce964-337">HTTP_URL</span></span>
* <span data-ttu-id="ce964-338">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="ce964-338">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="ce964-339">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ce964-339">HTTPS</span></span>
* <span data-ttu-id="ce964-340">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="ce964-340">LOCAL_ADDR</span></span>
* <span data-ttu-id="ce964-341">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="ce964-341">QUERY_STRING</span></span>
* <span data-ttu-id="ce964-342">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="ce964-342">REMOTE_ADDR</span></span>
* <span data-ttu-id="ce964-343">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="ce964-343">REMOTE_PORT</span></span>
* <span data-ttu-id="ce964-344">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="ce964-344">REQUEST_FILENAME</span></span>
* <span data-ttu-id="ce964-345">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="ce964-345">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="ce964-346">È anche possibile ottenere un `IFileProvider` tramite un `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="ce964-346">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="ce964-347">Questo approccio può garantire maggior flessibilità per la posizione dei file delle regole di riscrittura.</span><span class="sxs-lookup"><span data-stu-id="ce964-347">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="ce964-348">Assicurarsi che i file di regole di riscrittura vengano distribuiti nel server in corrispondenza del percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="ce964-348">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="ce964-349">Regola basata su metodo</span><span class="sxs-lookup"><span data-stu-id="ce964-349">Method-based rule</span></span>

<span data-ttu-id="ce964-350">Usare `Add(Action<RewriteContext> applyRule)` per implementare logica della regola personalizzata in un metodo.</span><span class="sxs-lookup"><span data-stu-id="ce964-350">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="ce964-351">`RewriteContext` espone `HttpContext` per l'uso nel metodo.</span><span class="sxs-lookup"><span data-stu-id="ce964-351">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="ce964-352">`context.Result` determina la modalità di gestione dell'elaborazione pipeline aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="ce964-352">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="ce964-353">context.Result</span><span class="sxs-lookup"><span data-stu-id="ce964-353">context.Result</span></span>                       | <span data-ttu-id="ce964-354">Operazione</span><span class="sxs-lookup"><span data-stu-id="ce964-354">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="ce964-355">`RuleResult.ContinueRules` (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="ce964-355">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="ce964-356">Continuare ad applicare le regole</span><span class="sxs-lookup"><span data-stu-id="ce964-356">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="ce964-357">Interrompere l'applicazione delle regole e inviare la risposta</span><span class="sxs-lookup"><span data-stu-id="ce964-357">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="ce964-358">Interrompere l'applicazione delle regole e inviare il contesto al middleware seguente</span><span class="sxs-lookup"><span data-stu-id="ce964-358">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce964-359">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce964-359">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce964-360">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce964-360">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(RedirectXMLRequests);

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="ce964-361">L'app di esempio visualizza un metodo che reindirizza le richieste per i percorsi che terminano con *.xml*.</span><span class="sxs-lookup"><span data-stu-id="ce964-361">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="ce964-362">Se si esegue una richiesta per `/file.xml`, questa viene reindirizzata a `/xmlfiles/file.xml`.</span><span class="sxs-lookup"><span data-stu-id="ce964-362">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="ce964-363">Il codice di stato è 301 (Spostato permanentemente).</span><span class="sxs-lookup"><span data-stu-id="ce964-363">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="ce964-364">Per un reindirizzamento è necessario impostare in modo esplicito il codice di stato della risposta; in caso contrario viene restituito il codice di stato 200 (OK) e il reindirizzamento non viene eseguito sul client.</span><span class="sxs-lookup"><span data-stu-id="ce964-364">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet1)]

<span data-ttu-id="ce964-365">Richiesta originale: `/file.xml`</span><span class="sxs-lookup"><span data-stu-id="ce964-365">Original Request: `/file.xml`</span></span>

![Finestra del browser con strumenti di sviluppo per il rilevamento di richieste e risposte per file.xml](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="ce964-367">Regola basata su IRule</span><span class="sxs-lookup"><span data-stu-id="ce964-367">IRule-based rule</span></span>

<span data-ttu-id="ce964-368">Usare `Add(IRule)` per implementare logica della regola personalizzata in una classe derivata da `IRule`.</span><span class="sxs-lookup"><span data-stu-id="ce964-368">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="ce964-369">L'uso di un elemento `IRule` garantisce maggiore flessibilità rispetto all'approccio con una regola basata su un metodo.</span><span class="sxs-lookup"><span data-stu-id="ce964-369">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="ce964-370">La classe derivata può includere un costruttore, in cui è possibile passare parametri per il metodo `ApplyRule`.</span><span class="sxs-lookup"><span data-stu-id="ce964-370">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce964-371">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce964-371">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](url-rewriting/sample/Startup.cs?name=snippet1&highlight=15-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce964-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce964-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
public void Configure(IApplicationBuilder app)
{
    var options = new RewriteOptions()
        .Add(new RedirectImageRequests(".png", "/png-images"))
        .Add(new RedirectImageRequests(".jpg", "/jpg-images"));

    app.UseRewriter(options);
}
```

---

<span data-ttu-id="ce964-373">I valori dei parametri nell'app di esempio per `extension` e `newPath` vengono verificati e devono soddisfare diverse condizioni.</span><span class="sxs-lookup"><span data-stu-id="ce964-373">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="ce964-374">`extension` deve contenere un valore e tale valore deve essere *.png*, *.jpg* o *.gif*.</span><span class="sxs-lookup"><span data-stu-id="ce964-374">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="ce964-375">Se `newPath` non è valido, viene generato un evento `ArgumentException`.</span><span class="sxs-lookup"><span data-stu-id="ce964-375">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="ce964-376">Se si esegue una richiesta per *image.png*, questa viene reindirizzata a `/png-images/image.png`.</span><span class="sxs-lookup"><span data-stu-id="ce964-376">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="ce964-377">Se si esegue una richiesta per *image.jpg*, questa viene reindirizzata a `/jpg-images/image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="ce964-377">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="ce964-378">Il codice di stato viene impostato su 301 (Spostato permanentemente) e l'elemento `context.Result` viene impostato per l'interruzione dell'elaborazione delle regole e l'invio della risposta.</span><span class="sxs-lookup"><span data-stu-id="ce964-378">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

[!code-csharp[](url-rewriting/sample/RewriteRules.cs?name=snippet2)]

<span data-ttu-id="ce964-379">Richiesta originale: `/image.png`</span><span class="sxs-lookup"><span data-stu-id="ce964-379">Original Request: `/image.png`</span></span>

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte per image.png](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="ce964-381">Richiesta originale: `/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="ce964-381">Original Request: `/image.jpg`</span></span>

![Finestra del browser con gli strumenti di sviluppo per il rilevamento di richieste e risposte per image.jpg](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="ce964-383">Esempi di espressioni regolari</span><span class="sxs-lookup"><span data-stu-id="ce964-383">Regex examples</span></span>

| <span data-ttu-id="ce964-384">Obiettivo</span><span class="sxs-lookup"><span data-stu-id="ce964-384">Goal</span></span> | <span data-ttu-id="ce964-385">Esempio di stringa espressione regolare</span><span class="sxs-lookup"><span data-stu-id="ce964-385">Regex String &</span></span><br><span data-ttu-id="ce964-386">e corrispondenza</span><span class="sxs-lookup"><span data-stu-id="ce964-386">Match Example</span></span> | <span data-ttu-id="ce964-387">Esempio di stringa di sostituzione</span><span class="sxs-lookup"><span data-stu-id="ce964-387">Replacement String &</span></span><br><span data-ttu-id="ce964-388">e output</span><span class="sxs-lookup"><span data-stu-id="ce964-388">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="ce964-389">Riscrivere il percorso nella stringa di query</span><span class="sxs-lookup"><span data-stu-id="ce964-389">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="ce964-390">Rimuovere la barra finale</span><span class="sxs-lookup"><span data-stu-id="ce964-390">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="ce964-391">Applicare la barra finale</span><span class="sxs-lookup"><span data-stu-id="ce964-391">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="ce964-392">Evitare la riscrittura di richieste specifiche</span><span class="sxs-lookup"><span data-stu-id="ce964-392">Avoid rewriting specific requests</span></span> | <span data-ttu-id="ce964-393">`^(.*)(?<!\.axd)$` o `^(?!.*\.axd$)(.*)$`</span><span class="sxs-lookup"><span data-stu-id="ce964-393">`^(.*)(?<!\.axd)$` or `^(?!.*\.axd$)(.*)$`</span></span><br><span data-ttu-id="ce964-394">Sì: `/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="ce964-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="ce964-395">No: `/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="ce964-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="ce964-396">Ridisporre i segmenti di URL</span><span class="sxs-lookup"><span data-stu-id="ce964-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="ce964-397">Sostituire un segmento di URL</span><span class="sxs-lookup"><span data-stu-id="ce964-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="ce964-398">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ce964-398">Additional resources</span></span>

* [<span data-ttu-id="ce964-399">Avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="ce964-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="ce964-400">Middleware</span><span class="sxs-lookup"><span data-stu-id="ce964-400">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="ce964-401">Espressioni regolari in .NET</span><span class="sxs-lookup"><span data-stu-id="ce964-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="ce964-402">Linguaggio di espressioni regolari - Riferimento rapido</span><span class="sxs-lookup"><span data-stu-id="ce964-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="ce964-403">Modulo Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="ce964-403">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* <span data-ttu-id="ce964-404">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) (Uso di URL Rewrite Module 2.0 per IIS)</span><span class="sxs-lookup"><span data-stu-id="ce964-404">[Using Url Rewrite Module 2.0 (for IIS)](/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)</span></span>
* <span data-ttu-id="ce964-405">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference) (Guida di riferimento per la configurazione di URL Rewrite Module)</span><span class="sxs-lookup"><span data-stu-id="ce964-405">[URL Rewrite Module Configuration Reference](/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)</span></span>
* [<span data-ttu-id="ce964-406">Forum di IIS URL Rewrite Module</span><span class="sxs-lookup"><span data-stu-id="ce964-406">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* <span data-ttu-id="ce964-407">[Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en) (Mantenere una struttura URL semplice)</span><span class="sxs-lookup"><span data-stu-id="ce964-407">[Keep a simple URL structure](https://support.google.com/webmasters/answer/76329?hl=en)</span></span>
* <span data-ttu-id="ce964-408">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/) (10 suggerimenti e consigli per la riscrittura URL)</span><span class="sxs-lookup"><span data-stu-id="ce964-408">[10 URL Rewriting Tips and Tricks](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)</span></span>
* <span data-ttu-id="ce964-409">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html) (Uso del carattere barra)</span><span class="sxs-lookup"><span data-stu-id="ce964-409">[To slash or not to slash](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)</span></span>
