---
title: "Utilizzo dell'autenticazione Cookie senza identità ASP.NET Core"
author: rick-anderson
description: Una spiegazione dell'utilizzo dell'autenticazione cookie senza ASP.NET Identity Core
manager: wpickett
ms.author: riande
ms.date: 10/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/cookie
ms.openlocfilehash: bbc49a0d3ede66ad07ec3f1dea055cae5fec39ff
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="d67eb-103">Utilizzo dell'autenticazione Cookie senza identità ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d67eb-103">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="d67eb-104">Da [Rick Anderson](https://twitter.com/RickAndMSFT) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d67eb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d67eb-105">Come si è visto negli argomenti precedenti autenticazione [ASP.NET Identity Core](xref:security/authentication/identity) è un provider di autenticazione completa e completa per creare e gestire gli account di accesso.</span><span class="sxs-lookup"><span data-stu-id="d67eb-105">As you've seen in the earlier authentication topics, [ASP.NET Core Identity](xref:security/authentication/identity) is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="d67eb-106">Tuttavia, si consiglia di utilizzare la logica di autenticazione personalizzato con l'autenticazione basata su cookie in alcuni casi.</span><span class="sxs-lookup"><span data-stu-id="d67eb-106">However, you may want to use your own custom authentication logic with cookie-based authentication at times.</span></span> <span data-ttu-id="d67eb-107">È possibile utilizzare l'autenticazione basata su cookie come un provider di autenticazione autonomo senza ASP.NET Identity Core.</span><span class="sxs-lookup"><span data-stu-id="d67eb-107">You can use cookie-based authentication as a standalone authentication provider without ASP.NET Core Identity.</span></span>

<span data-ttu-id="d67eb-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d67eb-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d67eb-109">Per informazioni sulla migrazione di autenticazione basato su cookie da ASP.NET Core 1. x a 2.0, vedere [la migrazione di autenticazione e identità per ASP.NET 2.0 Core argomento (basato su Cookie di autenticazione)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span><span class="sxs-lookup"><span data-stu-id="d67eb-109">For information on migrating cookie-based authentication from ASP.NET Core 1.x to 2.0, see [Migrating Authentication and Identity to ASP.NET Core 2.0 topic (Cookie-based Authentication)](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication).</span></span>

## <a name="configuration"></a><span data-ttu-id="d67eb-110">Configurazione</span><span class="sxs-lookup"><span data-stu-id="d67eb-110">Configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d67eb-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d67eb-112">Se non si utilizza il [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), installare la versione 2.0 di [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="d67eb-112">If you aren't using the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), install version 2.0+ of the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package.</span></span>

<span data-ttu-id="d67eb-113">Nel `ConfigureServices` (metodo), creare il servizio di Middleware di autenticazione con il `AddAuthentication` e `AddCookie` metodi:</span><span class="sxs-lookup"><span data-stu-id="d67eb-113">In the `ConfigureServices` method, create the Authentication Middleware service with the `AddAuthentication` and `AddCookie` methods:</span></span>

[!code-csharp[](cookie/sample/Startup.cs?name=snippet1)]

<span data-ttu-id="d67eb-114">`AuthenticationScheme` passato a `AddAuthentication` imposta lo schema di autenticazione predefinito per l'app.</span><span class="sxs-lookup"><span data-stu-id="d67eb-114">`AuthenticationScheme` passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="d67eb-115">`AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione dei cookie e si desidera [autorizzazione con uno schema specifico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="d67eb-115">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="d67eb-116">L'impostazione di `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` fornisce un valore pari a "Cookie" per lo schema.</span><span class="sxs-lookup"><span data-stu-id="d67eb-116">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="d67eb-117">È possibile fornire qualsiasi valore stringa che distingue lo schema.</span><span class="sxs-lookup"><span data-stu-id="d67eb-117">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="d67eb-118">Nel `Configure` metodo, utilizzare il `UseAuthentication` metodo da richiamare il Middleware di autenticazione che imposta il `HttpContext.User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="d67eb-118">In the `Configure` method, use the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="d67eb-119">Chiamare il `UseAuthentication` metodo prima di chiamare `UseMvcWithDefaultRoute` o `UseMvc`:</span><span class="sxs-lookup"><span data-stu-id="d67eb-119">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/sample/Startup.cs?name=snippet2)]

<span data-ttu-id="d67eb-120">**Opzioni AddCookie**</span><span class="sxs-lookup"><span data-stu-id="d67eb-120">**AddCookie Options**</span></span>

<span data-ttu-id="d67eb-121">Il [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) classe viene utilizzata per configurare le opzioni del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d67eb-121">The [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="d67eb-122">Opzione</span><span class="sxs-lookup"><span data-stu-id="d67eb-122">Option</span></span> | <span data-ttu-id="d67eb-123">Descrizione</span><span class="sxs-lookup"><span data-stu-id="d67eb-123">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="d67eb-124">AccessDeniedPath</span><span class="sxs-lookup"><span data-stu-id="d67eb-124">AccessDeniedPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-125">Fornisce il percorso di fornire un 302 trovato (reindirizzamento dell'URL) quando sono attivati da `HttpContext.ForbidAsync`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-125">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ForbidAsync`.</span></span> <span data-ttu-id="d67eb-126">Il valore predefinito è `/Account/AccessDenied`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-126">The default value is `/Account/AccessDenied`.</span></span> |
| [<span data-ttu-id="d67eb-127">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="d67eb-127">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-128">L'autorità emittente da utilizzare per il [dell'autorità di certificazione](/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creata dal servizio di autenticazione del cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-128">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication service.</span></span> |
| [<span data-ttu-id="d67eb-129">Cookie.Domain</span><span class="sxs-lookup"><span data-stu-id="d67eb-129">Cookie.Domain</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-130">Il nome di dominio in cui il cookie viene servito.</span><span class="sxs-lookup"><span data-stu-id="d67eb-130">The domain name where the cookie is served.</span></span> <span data-ttu-id="d67eb-131">Per impostazione predefinita, questo è il nome host della richiesta.</span><span class="sxs-lookup"><span data-stu-id="d67eb-131">By default, this is the host name of the request.</span></span> <span data-ttu-id="d67eb-132">Il browser invia solo il cookie nelle richieste inviate a un nome host corrispondente.</span><span class="sxs-lookup"><span data-stu-id="d67eb-132">The browser only sends the cookie in requests to a matching host name.</span></span> <span data-ttu-id="d67eb-133">Si potrebbe voler modificare questa opzione per disporre i cookie disponibili per tutti gli host nel dominio.</span><span class="sxs-lookup"><span data-stu-id="d67eb-133">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="d67eb-134">Ad esempio, impostando il dominio del cookie `.contoso.com` rende disponibili agli `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-134">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="d67eb-135">Cookie.Expiration</span><span class="sxs-lookup"><span data-stu-id="d67eb-135">Cookie.Expiration</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-136">Ottiene o imposta la durata di un cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-136">Gets or sets the lifespan of a cookie.</span></span> <span data-ttu-id="d67eb-137">Attualmente, questa opzione no ops e diventerà obsoleta in ASP.NET Core 2.1 +.</span><span class="sxs-lookup"><span data-stu-id="d67eb-137">Currently, this option no-ops and will become obsolete in ASP.NET Core 2.1+.</span></span> <span data-ttu-id="d67eb-138">Utilizzare il `ExpireTimeSpan` opzione per impostare la scadenza del cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-138">Use the `ExpireTimeSpan` option to set cookie expiration.</span></span> <span data-ttu-id="d67eb-139">Per ulteriori informazioni, vedere [chiarire il comportamento di CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span><span class="sxs-lookup"><span data-stu-id="d67eb-139">For more information, see [Clarify behavior of CookieAuthenticationOptions.Cookie.Expiration](https://github.com/aspnet/Security/issues/1293).</span></span> |
| [<span data-ttu-id="d67eb-140">Cookie.HttpOnly</span><span class="sxs-lookup"><span data-stu-id="d67eb-140">Cookie.HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-141">Flag che indica se il cookie deve essere accessibile solo ai server.</span><span class="sxs-lookup"><span data-stu-id="d67eb-141">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="d67eb-142">Modifica questo valore su `false` consenta script lato client per accedere al cookie e possono aprire l'app al rischio di furto di cookie che l'app deve avere un [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerabilità.</span><span class="sxs-lookup"><span data-stu-id="d67eb-142">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="d67eb-143">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-143">The default value is `true`.</span></span> |
| [<span data-ttu-id="d67eb-144">Cookie.Name</span><span class="sxs-lookup"><span data-stu-id="d67eb-144">Cookie.Name</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-145">Imposta il nome del cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-145">Sets the name of the cookie.</span></span> |
| [<span data-ttu-id="d67eb-146">Cookie.Path</span><span class="sxs-lookup"><span data-stu-id="d67eb-146">Cookie.Path</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-147">Utilizzato per isolare le app eseguono lo stesso nome host.</span><span class="sxs-lookup"><span data-stu-id="d67eb-147">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="d67eb-148">Se si dispone di un'app in esecuzione in `/app1` e desidera limitare i cookie per tale app, impostare il `CookiePath` proprietà `/app1`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-148">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="d67eb-149">In questo modo, il cookie è disponibile solo per le richieste per `/app1` e tutte le app disponibili al suo interno.</span><span class="sxs-lookup"><span data-stu-id="d67eb-149">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="d67eb-150">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="d67eb-150">Cookie.SameSite</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-151">Indica se il browser deve consentire i cookie da collegare alle richieste di nello stesso sito solo (`SameSiteMode.Strict`) o le richieste tra siti tramite i metodi HTTP sicuro e richieste di nello stesso sito (`SameSiteMode.Lax`).</span><span class="sxs-lookup"><span data-stu-id="d67eb-151">Indicates whether the browser should allow the cookie to be attached to same-site requests only (`SameSiteMode.Strict`) or cross-site requests using safe HTTP methods and same-site requests (`SameSiteMode.Lax`).</span></span> <span data-ttu-id="d67eb-152">Se impostato su `SameSiteMode.None`, non è impostato il valore dell'intestazione cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-152">When set to `SameSiteMode.None`, the cookie header value isn't set.</span></span> <span data-ttu-id="d67eb-153">Si noti che [Middleware criteri Cookie](#cookie-policy-middleware) potrebbe sovrascrivere il valore fornito dall'utente.</span><span class="sxs-lookup"><span data-stu-id="d67eb-153">Note that [Cookie Policy Middleware](#cookie-policy-middleware) might overwrite the value that you provide.</span></span> <span data-ttu-id="d67eb-154">Per supportare l'autenticazione OAuth, il valore predefinito è `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-154">To support OAuth authentication, the default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="d67eb-155">Per ulteriori informazioni, vedere [autenticazione OAuth interrotto a causa dei criteri di cookie SameSite](https://github.com/aspnet/Security/issues/1231).</span><span class="sxs-lookup"><span data-stu-id="d67eb-155">For more information, see [OAuth authentication broken due to SameSite cookie policy](https://github.com/aspnet/Security/issues/1231).</span></span> |
| [<span data-ttu-id="d67eb-156">Cookie.SecurePolicy</span><span class="sxs-lookup"><span data-stu-id="d67eb-156">Cookie.SecurePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-157">Flag che indica se il cookie creato deve essere limitato a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o lo stesso protocollo della richiesta (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="d67eb-157">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="d67eb-158">Il valore predefinito è `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-158">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="d67eb-159">DataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="d67eb-159">DataProtectionProvider</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-160">Imposta il `DataProtectionProvider` che viene utilizzato per creare il valore predefinito `TicketDataFormat`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-160">Sets the `DataProtectionProvider` that's used to create the default `TicketDataFormat`.</span></span> <span data-ttu-id="d67eb-161">Se il `TicketDataFormat` è impostata, il `DataProtectionProvider` non viene usata l'opzione.</span><span class="sxs-lookup"><span data-stu-id="d67eb-161">If the `TicketDataFormat` property is set, the `DataProtectionProvider` option isn't used.</span></span> <span data-ttu-id="d67eb-162">Se omesso, viene utilizzato provider di protezione dati predefinito dell'app.</span><span class="sxs-lookup"><span data-stu-id="d67eb-162">If not provided, the app's default data protection provider is used.</span></span> |
| [<span data-ttu-id="d67eb-163">Eventi</span><span class="sxs-lookup"><span data-stu-id="d67eb-163">Events</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-164">Il gestore chiama i metodi nel provider che offrono il controllo di app in alcune fasi di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="d67eb-164">The handler calls methods on the provider that give the app control at certain processing points.</span></span> <span data-ttu-id="d67eb-165">Se `Events` non fornito, viene fornita un'istanza predefinita che non esegue alcuna operazione quando i metodi vengono chiamati.</span><span class="sxs-lookup"><span data-stu-id="d67eb-165">If `Events` aren't provided, a default instance is supplied that does nothing when the methods are called.</span></span> |
| [<span data-ttu-id="d67eb-166">EventsType</span><span class="sxs-lookup"><span data-stu-id="d67eb-166">EventsType</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-167">Utilizzato come tipo di servizio per ottenere il `Events` istanza anziché la proprietà.</span><span class="sxs-lookup"><span data-stu-id="d67eb-167">Used as the service type to get the `Events` instance instead of the property.</span></span> |
| [<span data-ttu-id="d67eb-168">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="d67eb-168">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-169">Il `TimeSpan` dopo che il ticket di autenticazione archiviato il cookie scade.</span><span class="sxs-lookup"><span data-stu-id="d67eb-169">The `TimeSpan` after which the authentication ticket stored inside the cookie expires.</span></span> <span data-ttu-id="d67eb-170">`ExpireTimeSpan` viene aggiunto all'ora corrente per creare l'ora di scadenza per il ticket.</span><span class="sxs-lookup"><span data-stu-id="d67eb-170">`ExpireTimeSpan` is added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="d67eb-171">Il `ExpiredTimeSpan` valore diventa sempre il ticket di autenticazione crittografato verificati dal server.</span><span class="sxs-lookup"><span data-stu-id="d67eb-171">The `ExpiredTimeSpan` value always goes into the encrypted AuthTicket verified by the server.</span></span> <span data-ttu-id="d67eb-172">Può inoltre analizzare le [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) intestazione, ma solo se `IsPersistent` è impostata.</span><span class="sxs-lookup"><span data-stu-id="d67eb-172">It may also go into the [Set-Cookie](https://tools.ietf.org/html/rfc6265#section-4.1) header, but only if `IsPersistent` is set.</span></span> <span data-ttu-id="d67eb-173">Per impostare `IsPersistent` a `true`, configurare il [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passato a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-173">To set `IsPersistent` to `true`, configure the [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties) passed to `SignInAsync`.</span></span> <span data-ttu-id="d67eb-174">Il valore predefinito di `ExpireTimeSpan` è 14 giorni.</span><span class="sxs-lookup"><span data-stu-id="d67eb-174">The default value of `ExpireTimeSpan` is 14 days.</span></span> |
| [<span data-ttu-id="d67eb-175">LoginPath</span><span class="sxs-lookup"><span data-stu-id="d67eb-175">LoginPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-176">Fornisce il percorso di fornire un 302 trovato (reindirizzamento dell'URL) quando sono attivati da `HttpContext.ChallengeAsync`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-176">Provides the path to supply with a 302 Found (URL redirect) when triggered by `HttpContext.ChallengeAsync`.</span></span> <span data-ttu-id="d67eb-177">L'URL corrente che ha generato il codice 401 viene aggiunto per il `LoginPath` come parametro di stringa di query denominato dal `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-177">The current URL that generated the 401 is added to the `LoginPath` as a query string parameter named by the `ReturnUrlParameter`.</span></span> <span data-ttu-id="d67eb-178">Una volta una richiesta per il `LoginPath` concede una nuova identità di accesso, il `ReturnUrlParameter` valore viene utilizzato per reindirizzare il browser all'URL che ha causato il codice di stato unauthorized originale.</span><span class="sxs-lookup"><span data-stu-id="d67eb-178">Once a request to the `LoginPath` grants a new sign-in identity, the `ReturnUrlParameter` value is used to redirect the browser back to the URL that caused the original unauthorized status code.</span></span> <span data-ttu-id="d67eb-179">Il valore predefinito è `/Account/Login`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-179">The default value is `/Account/Login`.</span></span> |
| [<span data-ttu-id="d67eb-180">LogoutPath</span><span class="sxs-lookup"><span data-stu-id="d67eb-180">LogoutPath</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-181">Se il `LogoutPath` viene fornito per il gestore, quindi reindirizza una richiesta per il percorso in base al valore del `ReturnUrlParameter`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-181">If the `LogoutPath` is provided to the handler, then a request to that path redirects based on the value of the `ReturnUrlParameter`.</span></span> <span data-ttu-id="d67eb-182">Il valore predefinito è `/Account/Logout`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-182">The default value is `/Account/Logout`.</span></span> |
| [<span data-ttu-id="d67eb-183">ReturnUrlParameter</span><span class="sxs-lookup"><span data-stu-id="d67eb-183">ReturnUrlParameter</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-184">Determina il nome del parametro della stringa di query che viene aggiunto dal gestore di risposta 302 trovato (reindirizzamento dell'URL).</span><span class="sxs-lookup"><span data-stu-id="d67eb-184">Determines the name of the query string parameter that's appended by the handler for a 302 Found (URL redirect) response.</span></span> <span data-ttu-id="d67eb-185">`ReturnUrlParameter` viene utilizzato quando arriva una richiesta di `LoginPath` o `LogoutPath` per restituire il browser all'URL originale al termine dell'operazione di connessione o disconnessione.</span><span class="sxs-lookup"><span data-stu-id="d67eb-185">`ReturnUrlParameter` is used when a request arrives on the `LoginPath` or `LogoutPath` to return the browser to the original URL after the login or logout action is performed.</span></span> <span data-ttu-id="d67eb-186">Il valore predefinito è `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-186">The default value is `ReturnUrl`.</span></span> |
| [<span data-ttu-id="d67eb-187">SessionStore</span><span class="sxs-lookup"><span data-stu-id="d67eb-187">SessionStore</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-188">Un contenitore facoltativo utilizzato per archiviare l'identità in tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="d67eb-188">An optional container used to store identity across requests.</span></span> <span data-ttu-id="d67eb-189">Se utilizzato, solo un identificatore di sessione viene inviato al client.</span><span class="sxs-lookup"><span data-stu-id="d67eb-189">When used, only a session identifier is sent to the client.</span></span> <span data-ttu-id="d67eb-190">`SessionStore` Consente di attenuare i potenziali problemi con le identità di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="d67eb-190">`SessionStore` can be used to mitigate potential problems with large identities.</span></span> |
| [<span data-ttu-id="d67eb-191">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="d67eb-191">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-192">Un flag che indica se un nuovo cookie con una scadenza aggiornato deve essere visualizzato in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="d67eb-192">A flag indicating if a new cookie with an updated expiration time should be issued dynamically.</span></span> <span data-ttu-id="d67eb-193">Questa situazione può verificarsi in qualsiasi richiesta in cui il periodo di scadenza cookie corrente è superiore al 50% scaduto.</span><span class="sxs-lookup"><span data-stu-id="d67eb-193">This can happen on any request where the current cookie expiration period is more than 50% expired.</span></span> <span data-ttu-id="d67eb-194">La nuova data di scadenza viene spostata in avanti la data corrente più il `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-194">The new expiration date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="d67eb-195">Un [data di scadenza assoluto cookie](xref:security/authentication/cookie#absolute-cookie-expiration) può essere impostato utilizzando il `AuthenticationProperties` classe quando si chiama `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-195">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="d67eb-196">Un'ora di scadenza assoluto può migliorare la sicurezza dell'app, limitando il periodo di tempo che il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="d67eb-196">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="d67eb-197">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-197">The default value is `true`.</span></span> |
| [<span data-ttu-id="d67eb-198">TicketDataFormat</span><span class="sxs-lookup"><span data-stu-id="d67eb-198">TicketDataFormat</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-199">Il `TicketDataFormat` viene utilizzata per proteggere e annullare la protezione dell'identità e altre proprietà che vengono archiviate nel valore del cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-199">The `TicketDataFormat` is used to protect and unprotect the identity and other properties that are stored in the cookie value.</span></span> <span data-ttu-id="d67eb-200">Se non specificato, un `TicketDataFormat` viene creato utilizzando il [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="d67eb-200">If not provided, a `TicketDataFormat` is created using the [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0).</span></span> |
| [<span data-ttu-id="d67eb-201">Convalidare</span><span class="sxs-lookup"><span data-stu-id="d67eb-201">Validate</span></span>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | <span data-ttu-id="d67eb-202">Metodo che verifica che le opzioni sono valide.</span><span class="sxs-lookup"><span data-stu-id="d67eb-202">Method that checks that the options are valid.</span></span> |

<span data-ttu-id="d67eb-203">Impostare `CookieAuthenticationOptions` nella configurazione del servizio per l'autenticazione nel `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="d67eb-203">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d67eb-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d67eb-205">ASP.NET Core 1. x utilizza cookie [middleware](xref:fundamentals/middleware/index) che serializza un'entità utente in un cookie crittografato.</span><span class="sxs-lookup"><span data-stu-id="d67eb-205">ASP.NET Core 1.x uses cookie [middleware](xref:fundamentals/middleware/index) that serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="d67eb-206">Nelle richieste successive, il cookie viene convalidato e ricreato e assegnata l'entità di `HttpContext.User` proprietà.</span><span class="sxs-lookup"><span data-stu-id="d67eb-206">On subsequent requests, the cookie is validated, and the principal is recreated and assigned to the `HttpContext.User` property.</span></span>

<span data-ttu-id="d67eb-207">Installare il [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) pacchetto NuGet nel progetto.</span><span class="sxs-lookup"><span data-stu-id="d67eb-207">Install the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) NuGet package in your project.</span></span> <span data-ttu-id="d67eb-208">Questo pacchetto contiene middleware del cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-208">This package contains the cookie middleware.</span></span>

<span data-ttu-id="d67eb-209">Utilizzare il `UseCookieAuthentication` metodo nel `Configure` metodo i *Startup.cs* file prima di `UseMvc` o `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="d67eb-209">Use the `UseCookieAuthentication` method in the `Configure` method in your *Startup.cs* file before `UseMvc` or `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

<span data-ttu-id="d67eb-210">**Opzioni di CookieAuthenticationOptions**</span><span class="sxs-lookup"><span data-stu-id="d67eb-210">**CookieAuthenticationOptions Options**</span></span>

<span data-ttu-id="d67eb-211">Il [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) classe viene utilizzata per configurare le opzioni del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d67eb-211">The [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1) class is used to configure the authentication provider options.</span></span>

| <span data-ttu-id="d67eb-212">Opzione</span><span class="sxs-lookup"><span data-stu-id="d67eb-212">Option</span></span> | <span data-ttu-id="d67eb-213">Descrizione</span><span class="sxs-lookup"><span data-stu-id="d67eb-213">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="d67eb-214">AuthenticationScheme</span><span class="sxs-lookup"><span data-stu-id="d67eb-214">AuthenticationScheme</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-215">Imposta lo schema di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d67eb-215">Sets the authentication scheme.</span></span> <span data-ttu-id="d67eb-216">`AuthenticationScheme` è utile quando sono presenti più istanze di autenticazione e si desidera [autorizzazione con uno schema specifico](xref:security/authorization/limitingidentitybyscheme).</span><span class="sxs-lookup"><span data-stu-id="d67eb-216">`AuthenticationScheme` is useful when there are multiple instances of authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="d67eb-217">L'impostazione di `AuthenticationScheme` a `CookieAuthenticationDefaults.AuthenticationScheme` fornisce un valore pari a "Cookie" per lo schema.</span><span class="sxs-lookup"><span data-stu-id="d67eb-217">Setting the `AuthenticationScheme` to `CookieAuthenticationDefaults.AuthenticationScheme` provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="d67eb-218">È possibile fornire qualsiasi valore stringa che distingue lo schema.</span><span class="sxs-lookup"><span data-stu-id="d67eb-218">You can supply any string value that distinguishes the scheme.</span></span> |
| [<span data-ttu-id="d67eb-219">AutomaticAuthenticate</span><span class="sxs-lookup"><span data-stu-id="d67eb-219">AutomaticAuthenticate</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-220">Imposta un valore per indicare che l'autenticazione dei cookie di eseguire in ogni richiesta e tenta di convalidare e ricostruire qualsiasi entità serializzato che è creato.</span><span class="sxs-lookup"><span data-stu-id="d67eb-220">Sets a value to indicate that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span> |
| [<span data-ttu-id="d67eb-221">AutomaticChallenge</span><span class="sxs-lookup"><span data-stu-id="d67eb-221">AutomaticChallenge</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-222">Se true, il middleware di autenticazione gestisce sfide automatico.</span><span class="sxs-lookup"><span data-stu-id="d67eb-222">If true, the authentication middleware handles automatic challenges.</span></span> <span data-ttu-id="d67eb-223">Se false, il middleware di autenticazione altera solo le risposte quando richiesto esplicitamente dal `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-223">If false, the authentication middleware only alters responses when explicitly indicated by the `AuthenticationScheme`.</span></span> |
| [<span data-ttu-id="d67eb-224">ClaimsIssuer</span><span class="sxs-lookup"><span data-stu-id="d67eb-224">ClaimsIssuer</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-225">L'autorità emittente da utilizzare per il [dell'autorità di certificazione](/dotnet/api/system.security.claims.claim.issuer) proprietà in tutte le attestazioni creato dal middleware di autenticazione del cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-225">The issuer to use for the [Issuer](/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication middleware.</span></span> |
| [<span data-ttu-id="d67eb-226">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="d67eb-226">CookieDomain</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-227">Il nome di dominio in cui il cookie viene servito.</span><span class="sxs-lookup"><span data-stu-id="d67eb-227">The domain name where the cookie is served.</span></span> <span data-ttu-id="d67eb-228">Per impostazione predefinita, questo è il nome host della richiesta.</span><span class="sxs-lookup"><span data-stu-id="d67eb-228">By default, this is the host name of the request.</span></span> <span data-ttu-id="d67eb-229">Il browser opera solo il cookie a un nome host corrispondente.</span><span class="sxs-lookup"><span data-stu-id="d67eb-229">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="d67eb-230">Si potrebbe voler modificare questa opzione per disporre i cookie disponibili per tutti gli host nel dominio.</span><span class="sxs-lookup"><span data-stu-id="d67eb-230">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="d67eb-231">Ad esempio, impostando il dominio del cookie `.contoso.com` rende disponibili agli `contoso.com`, `www.contoso.com`, e `staging.www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-231">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, and `staging.www.contoso.com`.</span></span> |
| [<span data-ttu-id="d67eb-232">CookieHttpOnly</span><span class="sxs-lookup"><span data-stu-id="d67eb-232">CookieHttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-233">Flag che indica se il cookie deve essere accessibile solo ai server.</span><span class="sxs-lookup"><span data-stu-id="d67eb-233">A flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="d67eb-234">Modifica questo valore su `false` consenta script lato client per accedere al cookie e possono aprire l'app al rischio di furto di cookie che l'app deve avere un [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerabilità.</span><span class="sxs-lookup"><span data-stu-id="d67eb-234">Changing this value to `false` permits client-side scripts to access the cookie and may open your app to cookie theft should your app have a [Cross-site scripting (XSS)](xref:security/cross-site-scripting) vulnerability.</span></span> <span data-ttu-id="d67eb-235">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-235">The default value is `true`.</span></span> |
| [<span data-ttu-id="d67eb-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="d67eb-236">CookiePath</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-237">Utilizzato per isolare le app eseguono lo stesso nome host.</span><span class="sxs-lookup"><span data-stu-id="d67eb-237">Used to isolate apps running on the same host name.</span></span> <span data-ttu-id="d67eb-238">Se si dispone di un'app in esecuzione in `/app1` e desidera limitare i cookie per tale app, impostare il `CookiePath` proprietà `/app1`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-238">If you have an app running at `/app1` and want to restrict cookies to that app, set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="d67eb-239">In questo modo, il cookie è disponibile solo per le richieste per `/app1` e tutte le app disponibili al suo interno.</span><span class="sxs-lookup"><span data-stu-id="d67eb-239">By doing so, the cookie is only available on requests to `/app1` and any app underneath it.</span></span> |
| [<span data-ttu-id="d67eb-240">CookieSecure</span><span class="sxs-lookup"><span data-stu-id="d67eb-240">CookieSecure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-241">Flag che indica se il cookie creato deve essere limitato a HTTPS (`CookieSecurePolicy.Always`), HTTP o HTTPS (`CookieSecurePolicy.None`), o lo stesso protocollo della richiesta (`CookieSecurePolicy.SameAsRequest`).</span><span class="sxs-lookup"><span data-stu-id="d67eb-241">A flag indicating if the cookie created should be limited to HTTPS (`CookieSecurePolicy.Always`), HTTP or HTTPS (`CookieSecurePolicy.None`), or the same protocol as the request (`CookieSecurePolicy.SameAsRequest`).</span></span> <span data-ttu-id="d67eb-242">Il valore predefinito è `CookieSecurePolicy.SameAsRequest`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-242">The default value is `CookieSecurePolicy.SameAsRequest`.</span></span> |
| [<span data-ttu-id="d67eb-243">Descrizione</span><span class="sxs-lookup"><span data-stu-id="d67eb-243">Description</span></span>](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-244">Informazioni aggiuntive sul tipo di autenticazione viene resa disponibile per l'app.</span><span class="sxs-lookup"><span data-stu-id="d67eb-244">Additional information about the authentication type which is made available to the app.</span></span> |
| [<span data-ttu-id="d67eb-245">ExpireTimeSpan</span><span class="sxs-lookup"><span data-stu-id="d67eb-245">ExpireTimeSpan</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-246">Il `TimeSpan` dopo cui scade il ticket di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d67eb-246">The `TimeSpan` after which the authentication ticket expires.</span></span> <span data-ttu-id="d67eb-247">Viene aggiunto all'ora corrente per creare l'ora di scadenza per il ticket.</span><span class="sxs-lookup"><span data-stu-id="d67eb-247">It's added to the current time to create the expiration time for the ticket.</span></span> <span data-ttu-id="d67eb-248">Per utilizzare `ExpireTimeSpan`, è necessario impostare `IsPersistent` a `true` nel `AuthenticationProperties` passato a `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-248">To use `ExpireTimeSpan`, you must set `IsPersistent` to `true` in the `AuthenticationProperties` passed to `SignInAsync`.</span></span> <span data-ttu-id="d67eb-249">Il valore predefinito è 14 giorni.</span><span class="sxs-lookup"><span data-stu-id="d67eb-249">The default value is 14 days.</span></span> |
| [<span data-ttu-id="d67eb-250">SlidingExpiration</span><span class="sxs-lookup"><span data-stu-id="d67eb-250">SlidingExpiration</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | <span data-ttu-id="d67eb-251">Un flag che indica se la data di scadenza del cookie viene reimpostato quando più di semestre di `ExpireTimeSpan` trascorso l'intervallo.</span><span class="sxs-lookup"><span data-stu-id="d67eb-251">A flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="d67eb-252">La nuova ora exipiration viene spostata in avanti la data corrente più il `ExpireTimespan`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-252">The new exipiration time is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="d67eb-253">Un [data di scadenza assoluto cookie](xref:security/authentication/cookie#absolute-cookie-expiration) può essere impostato utilizzando il `AuthenticationProperties` classe quando si chiama `SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-253">An [absolute cookie expiration time](xref:security/authentication/cookie#absolute-cookie-expiration) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="d67eb-254">Un'ora di scadenza assoluto può migliorare la sicurezza dell'app, limitando il periodo di tempo che il cookie di autenticazione è valido.</span><span class="sxs-lookup"><span data-stu-id="d67eb-254">An absolute expiration time can improve the security of your app by limiting the amount of time that the authentication cookie is valid.</span></span> <span data-ttu-id="d67eb-255">Il valore predefinito è `true`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-255">The default value is `true`.</span></span> |

<span data-ttu-id="d67eb-256">Impostare `CookieAuthenticationOptions` per il Middleware di autenticazione nel Cookie di `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="d67eb-256">Set `CookieAuthenticationOptions` for the Cookie Authentication Middleware in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a><span data-ttu-id="d67eb-257">Middleware criteri cookie</span><span class="sxs-lookup"><span data-stu-id="d67eb-257">Cookie Policy Middleware</span></span>

<span data-ttu-id="d67eb-258">[Cookie criteri Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) consente le funzionalità di criteri di cookie in un'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d67eb-258">[Cookie Policy Middleware](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware) enables cookie policy capabilities in an app.</span></span> <span data-ttu-id="d67eb-259">L'aggiunta di middleware alla pipeline di elaborazione app è ordine sensibili; riguarda solo i componenti registrati dopo di esso nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="d67eb-259">Adding the middleware to the app processing pipeline is order sensitive; it only affects components registered after it in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 <span data-ttu-id="d67eb-260">Il [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) fornito per il Middleware di criteri di Cookie consentono di controllare caratteristiche globali del cookie elaborazione e l'hook in elaborazione cookie gestori quando i cookie vengono aggiunti o eliminati.</span><span class="sxs-lookup"><span data-stu-id="d67eb-260">The [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) provided to the Cookie Policy Middleware allow you to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

| <span data-ttu-id="d67eb-261">Proprietà</span><span class="sxs-lookup"><span data-stu-id="d67eb-261">Property</span></span> | <span data-ttu-id="d67eb-262">Descrizione</span><span class="sxs-lookup"><span data-stu-id="d67eb-262">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="d67eb-263">HttpOnly</span><span class="sxs-lookup"><span data-stu-id="d67eb-263">HttpOnly</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | <span data-ttu-id="d67eb-264">Interessa se i cookie devono essere HttpOnly, che è un flag che indica se il cookie deve essere accessibile solo ai server.</span><span class="sxs-lookup"><span data-stu-id="d67eb-264">Affects whether cookies must be HttpOnly, which is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="d67eb-265">Il valore predefinito è `HttpOnlyPolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-265">The default value is `HttpOnlyPolicy.None`.</span></span> |
| [<span data-ttu-id="d67eb-266">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="d67eb-266">MinimumSameSitePolicy</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | <span data-ttu-id="d67eb-267">Interessa attributo nello stesso sito del cookie (vedere sotto).</span><span class="sxs-lookup"><span data-stu-id="d67eb-267">Affects the cookie's same-site attribute (see below).</span></span> <span data-ttu-id="d67eb-268">Il valore predefinito è `SameSiteMode.Lax`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-268">The default value is `SameSiteMode.Lax`.</span></span> <span data-ttu-id="d67eb-269">Questa opzione è disponibile per i componenti di base di ASP.NET 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="d67eb-269">This option is available for ASP.NET Core 2.0+.</span></span> |
| [<span data-ttu-id="d67eb-270">OnAppendCookie</span><span class="sxs-lookup"><span data-stu-id="d67eb-270">OnAppendCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | <span data-ttu-id="d67eb-271">Chiamato quando viene aggiunto un cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-271">Called when a cookie is appended.</span></span> |
| [<span data-ttu-id="d67eb-272">OnDeleteCookie</span><span class="sxs-lookup"><span data-stu-id="d67eb-272">OnDeleteCookie</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | <span data-ttu-id="d67eb-273">Chiamato quando viene eliminato un cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-273">Called when a cookie is deleted.</span></span> |
| [<span data-ttu-id="d67eb-274">Proteggere</span><span class="sxs-lookup"><span data-stu-id="d67eb-274">Secure</span></span>](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | <span data-ttu-id="d67eb-275">Determina se i cookie devono essere protetto.</span><span class="sxs-lookup"><span data-stu-id="d67eb-275">Affects whether cookies must be Secure.</span></span> <span data-ttu-id="d67eb-276">Il valore predefinito è `CookieSecurePolicy.None`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-276">The default value is `CookieSecurePolicy.None`.</span></span> |

<span data-ttu-id="d67eb-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0 + solo)</span><span class="sxs-lookup"><span data-stu-id="d67eb-277">**MinimumSameSitePolicy** (ASP.NET Core 2.0+ only)</span></span>

<span data-ttu-id="d67eb-278">Il valore predefinito `MinimumSameSitePolicy` valore `SameSiteMode.Lax` per consentire l'autenticazione OAuth2.</span><span class="sxs-lookup"><span data-stu-id="d67eb-278">The default `MinimumSameSitePolicy` value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="d67eb-279">Per applicare rigorosamente criteri nello stesso sito di `SameSiteMode.Strict`, impostare il `MinimumSameSitePolicy`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-279">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="d67eb-280">Anche se questa impostazione di interruzioni di OAuth2 e altri schemi di autenticazione tra le origini, eleva il livello di sicurezza dei cookie per altri tipi di App che non richiedono l'elaborazione della richiesta multiorigine.</span><span class="sxs-lookup"><span data-stu-id="d67eb-280">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="d67eb-281">L'impostazione di Middleware di criteri di Cookie per `MinimumSameSitePolicy` possono influenzare l'impostazione di `Cookie.SameSite` in `CookieAuthenticationOptions` impostazioni in base alla matrice di seguito.</span><span class="sxs-lookup"><span data-stu-id="d67eb-281">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect your setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="d67eb-282">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="d67eb-282">MinimumSameSitePolicy</span></span> | <span data-ttu-id="d67eb-283">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="d67eb-283">Cookie.SameSite</span></span> | <span data-ttu-id="d67eb-284">Impostazione Cookie.SameSite risultante</span><span class="sxs-lookup"><span data-stu-id="d67eb-284">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="d67eb-285">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="d67eb-285">SameSiteMode.None</span></span>     | <span data-ttu-id="d67eb-286">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="d67eb-286">SameSiteMode.None</span></span><br><span data-ttu-id="d67eb-287">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d67eb-287">SameSiteMode.Lax</span></span><br><span data-ttu-id="d67eb-288">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d67eb-288">SameSiteMode.Strict</span></span> | <span data-ttu-id="d67eb-289">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="d67eb-289">SameSiteMode.None</span></span><br><span data-ttu-id="d67eb-290">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d67eb-290">SameSiteMode.Lax</span></span><br><span data-ttu-id="d67eb-291">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d67eb-291">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="d67eb-292">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d67eb-292">SameSiteMode.Lax</span></span>      | <span data-ttu-id="d67eb-293">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="d67eb-293">SameSiteMode.None</span></span><br><span data-ttu-id="d67eb-294">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d67eb-294">SameSiteMode.Lax</span></span><br><span data-ttu-id="d67eb-295">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d67eb-295">SameSiteMode.Strict</span></span> | <span data-ttu-id="d67eb-296">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d67eb-296">SameSiteMode.Lax</span></span><br><span data-ttu-id="d67eb-297">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d67eb-297">SameSiteMode.Lax</span></span><br><span data-ttu-id="d67eb-298">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d67eb-298">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="d67eb-299">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d67eb-299">SameSiteMode.Strict</span></span>   | <span data-ttu-id="d67eb-300">SameSiteMode.None</span><span class="sxs-lookup"><span data-stu-id="d67eb-300">SameSiteMode.None</span></span><br><span data-ttu-id="d67eb-301">SameSiteMode.Lax</span><span class="sxs-lookup"><span data-stu-id="d67eb-301">SameSiteMode.Lax</span></span><br><span data-ttu-id="d67eb-302">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d67eb-302">SameSiteMode.Strict</span></span> | <span data-ttu-id="d67eb-303">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d67eb-303">SameSiteMode.Strict</span></span><br><span data-ttu-id="d67eb-304">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d67eb-304">SameSiteMode.Strict</span></span><br><span data-ttu-id="d67eb-305">SameSiteMode.Strict</span><span class="sxs-lookup"><span data-stu-id="d67eb-305">SameSiteMode.Strict</span></span> |

## <a name="creating-an-authentication-cookie"></a><span data-ttu-id="d67eb-306">Creazione di un cookie di autenticazione</span><span class="sxs-lookup"><span data-stu-id="d67eb-306">Creating an authentication cookie</span></span>

<span data-ttu-id="d67eb-307">Per creare un cookie contenente le informazioni sull'utente, è necessario costruire un [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span><span class="sxs-lookup"><span data-stu-id="d67eb-307">To create a cookie holding user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal).</span></span> <span data-ttu-id="d67eb-308">Le informazioni utente vengano serializzate e memorizzate nel cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-308">The user information is serialized and stored in the cookie.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d67eb-309">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d67eb-310">Creare un [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) con le necessarie [attestazione](/dotnet/api/system.security.claims.claim)s e chiamare [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) per l'accesso dell'utente:</span><span class="sxs-lookup"><span data-stu-id="d67eb-310">Create a [ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity) with any required [Claim](/dotnet/api/system.security.claims.claim)s and call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0) to sign in the user:</span></span>

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d67eb-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d67eb-312">Chiamare [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) per l'accesso dell'utente:</span><span class="sxs-lookup"><span data-stu-id="d67eb-312">Call [SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1) to sign in the user:</span></span>

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

<span data-ttu-id="d67eb-313">`SignInAsync` Crea un cookie crittografato e lo aggiunge alla risposta corrente.</span><span class="sxs-lookup"><span data-stu-id="d67eb-313">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="d67eb-314">Se non si specifica un `AuthenticationScheme`, viene utilizzato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="d67eb-314">If you don't specify an `AuthenticationScheme`, the default scheme is used.</span></span>

<span data-ttu-id="d67eb-315">Dietro le quinte, la crittografia utilizzata è ASP.NET Core [la protezione dei dati](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) sistema.</span><span class="sxs-lookup"><span data-stu-id="d67eb-315">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="d67eb-316">Se si ospita l'applicazione in più computer, il bilanciamento del carico tra App o utilizzando una web farm, quindi è necessario [configurare la protezione dati](xref:security/data-protection/configuration/overview) per utilizzare la stessa chiave anello e identificatore dell'app.</span><span class="sxs-lookup"><span data-stu-id="d67eb-316">If you're hosting app on multiple machines, load balancing across apps, or using a web farm, then you must [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="d67eb-317">La disconnessione</span><span class="sxs-lookup"><span data-stu-id="d67eb-317">Signing out</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d67eb-318">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-318">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d67eb-319">Per effettuare la disconnessione dell'utente corrente ed eliminare i cookie, chiamare [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span><span class="sxs-lookup"><span data-stu-id="d67eb-319">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):</span></span>

[!code-csharp[](cookie/sample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d67eb-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d67eb-321">Per effettuare la disconnessione dell'utente corrente ed eliminare i cookie, chiamare [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span><span class="sxs-lookup"><span data-stu-id="d67eb-321">To sign out the current user and delete their cookie, call [SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):</span></span>

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

<span data-ttu-id="d67eb-322">Se non si utilizza `CookieAuthenticationDefaults.AuthenticationScheme` (o "Cookie") per lo schema (ad esempio, "ContosoCookie"), specificare lo schema usato durante la configurazione del provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d67eb-322">If you aren't using `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") as the scheme (for example, "ContosoCookie"), supply the scheme you used when configuring the authentication provider.</span></span> <span data-ttu-id="d67eb-323">In caso contrario, viene utilizzato lo schema predefinito.</span><span class="sxs-lookup"><span data-stu-id="d67eb-323">Otherwise, the default scheme is used.</span></span>

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="d67eb-324">Reazione alle modifiche di back-end</span><span class="sxs-lookup"><span data-stu-id="d67eb-324">Reacting to back-end changes</span></span>

<span data-ttu-id="d67eb-325">Dopo aver creato un cookie, diventa l'unica origine di identità.</span><span class="sxs-lookup"><span data-stu-id="d67eb-325">Once a cookie is created, it becomes the single source of identity.</span></span> <span data-ttu-id="d67eb-326">Anche se si disabilita un utente nei sistemi back-end, il sistema di autenticazione cookie non ha alcuna conoscenza di questo oggetto e un utente rimane connesso, purché i cookie è valido.</span><span class="sxs-lookup"><span data-stu-id="d67eb-326">Even if you disable a user in your back-end systems, the cookie authentication system has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="d67eb-327">Il [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) evento in ASP.NET Core 2. x o [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) metodo in ASP.NET Core 1. x consente di intercettare ed eseguire l'override di convalida dell'identità del cookie.</span><span class="sxs-lookup"><span data-stu-id="d67eb-327">The [ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal) event in ASP.NET Core 2.x or the [ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) method in ASP.NET Core 1.x can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="d67eb-328">Questo approccio riduce il rischio di utenti revocati l'accesso all'app.</span><span class="sxs-lookup"><span data-stu-id="d67eb-328">This approach mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="d67eb-329">Un approccio alla convalida dei cookie è basato su tenere traccia di quando il database utente è stato modificato.</span><span class="sxs-lookup"><span data-stu-id="d67eb-329">One approach to cookie validation is based on keeping track of when the user database has been changed.</span></span> <span data-ttu-id="d67eb-330">Se il database non è stato modificato perché è stato emesso il cookie dell'utente, non è necessario autenticare nuovamente l'utente se il cookie è ancora valido.</span><span class="sxs-lookup"><span data-stu-id="d67eb-330">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="d67eb-331">Implementare questo scenario, il database, che viene implementato in `IUserRepository` per questo esempio, archivia un `LastChanged` valore.</span><span class="sxs-lookup"><span data-stu-id="d67eb-331">To implement this scenario, the database, which is implemented in `IUserRepository` for this example, stores a `LastChanged` value.</span></span> <span data-ttu-id="d67eb-332">Quando un utente viene aggiornato nel database, il `LastChanged` è impostato sull'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="d67eb-332">When any user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="d67eb-333">Per invalidare un cookie quando le modifiche del database basano sul `LastChanged` valore, creare il cookie con una `LastChanged` attestazione contenente corrente `LastChanged` valore dal database:</span><span class="sxs-lookup"><span data-stu-id="d67eb-333">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d67eb-334">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-334">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d67eb-335">Per implementare un override per il `ValidatePrincipal` evento, scrivere un metodo con la firma seguente in una classe derivata da [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span><span class="sxs-lookup"><span data-stu-id="d67eb-335">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that you derive from [CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="d67eb-336">Un esempio è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d67eb-336">An example looks like the following:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="d67eb-337">Registrare l'istanza di eventi durante la registrazione del servizio nel cookie di `ConfigureServices` metodo.</span><span class="sxs-lookup"><span data-stu-id="d67eb-337">Register the events instance during cookie service registration in the `ConfigureServices` method.</span></span> <span data-ttu-id="d67eb-338">Fornire una registrazione di servizio con ambito per il `CustomCookieAuthenticationEvents` classe:</span><span class="sxs-lookup"><span data-stu-id="d67eb-338">Provide a scoped service registration for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d67eb-339">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-339">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d67eb-340">Per implementare un override per il `ValidateAsync` eventi, scrivere un metodo con la firma seguente:</span><span class="sxs-lookup"><span data-stu-id="d67eb-340">To implement an override for the `ValidateAsync` event, write a method with the following signature:</span></span>

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

<span data-ttu-id="d67eb-341">ASP.NET Identity Core implementa questo controllo come parte del relativo [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span><span class="sxs-lookup"><span data-stu-id="d67eb-341">ASP.NET Core Identity implements this check as part of its [SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync).</span></span> <span data-ttu-id="d67eb-342">Un esempio è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d67eb-342">An example looks like the following:</span></span>

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="d67eb-343">Registrare l'evento durante la configurazione dell'autenticazione nel cookie di `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="d67eb-343">Register the event during cookie authentication configuration in the `Configure` method:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

<span data-ttu-id="d67eb-344">Si consideri una situazione in cui viene aggiornato il nome dell'utente &mdash; una decisione che non influisce sulla sicurezza in alcun modo.</span><span class="sxs-lookup"><span data-stu-id="d67eb-344">Consider a situation in which the user's name is updated &mdash; a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="d67eb-345">Se si desidera aggiornare distruttivo l'entità utente, chiamare `context.ReplacePrincipal` e impostare il `context.ShouldRenew` proprietà `true`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-345">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="d67eb-346">L'approccio descritto di seguito viene attivata ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="d67eb-346">The approach described here is triggered on every request.</span></span> <span data-ttu-id="d67eb-347">Ciò può comportare una riduzione delle prestazioni di grandi dimensioni per l'app.</span><span class="sxs-lookup"><span data-stu-id="d67eb-347">This can result in a large performance penalty for the app.</span></span>

## <a name="persistent-cookies"></a><span data-ttu-id="d67eb-348">Cookie permanenti</span><span class="sxs-lookup"><span data-stu-id="d67eb-348">Persistent cookies</span></span>

<span data-ttu-id="d67eb-349">È possibile che il cookie per rendere persistenti le sessioni del browser.</span><span class="sxs-lookup"><span data-stu-id="d67eb-349">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="d67eb-350">Questo tipo di persistenza deve essere abilitata solo con il consenso esplicito utente con un controllo checkbox "Memorizza dati" su account di accesso o un meccanismo simile.</span><span class="sxs-lookup"><span data-stu-id="d67eb-350">This persistence should only be enabled with explicit user consent with a "Remember Me" checkbox on login or a similar mechanism.</span></span> 

<span data-ttu-id="d67eb-351">Il seguente frammento di codice crea un'identità e i cookie corrispondente che resta tramite chiusure browser.</span><span class="sxs-lookup"><span data-stu-id="d67eb-351">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="d67eb-352">Le impostazioni di scadenza variabile configurate in precedenza vengono rispettate.</span><span class="sxs-lookup"><span data-stu-id="d67eb-352">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="d67eb-353">Se il cookie scade mentre il browser viene chiuso, il browser Cancella cookie quando viene riavviato.</span><span class="sxs-lookup"><span data-stu-id="d67eb-353">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d67eb-354">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-354">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="d67eb-355">Il [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) risiede nella classe di `Microsoft.AspNetCore.Authentication` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="d67eb-355">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0) class resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d67eb-356">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-356">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="d67eb-357">Il [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) risiede nella classe di `Microsoft.AspNetCore.Http.Authentication` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="d67eb-357">The [AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1) class resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

## <a name="absolute-cookie-expiration"></a><span data-ttu-id="d67eb-358">Scadenza del cookie assoluto</span><span class="sxs-lookup"><span data-stu-id="d67eb-358">Absolute cookie expiration</span></span>

<span data-ttu-id="d67eb-359">È possibile impostare un'ora di scadenza assoluto con `ExpiresUtc`.</span><span class="sxs-lookup"><span data-stu-id="d67eb-359">You can set an absolute expiration time with `ExpiresUtc`.</span></span> <span data-ttu-id="d67eb-360">È inoltre necessario impostare `IsPersistent`; in caso contrario, `ExpiresUtc` viene ignorato e viene creato un cookie di sessione singola.</span><span class="sxs-lookup"><span data-stu-id="d67eb-360">You must also set `IsPersistent`; otherwise, `ExpiresUtc` is ignored and a single-session cookie is created.</span></span> <span data-ttu-id="d67eb-361">Quando `ExpiresUtc` è impostato su `SignInAsync`, sostituisce il valore del `ExpireTimeSpan` opzione di `CookieAuthenticationOptions`, se impostata.</span><span class="sxs-lookup"><span data-stu-id="d67eb-361">When `ExpiresUtc` is set on `SignInAsync`, it overrides the value of the `ExpireTimeSpan` option of `CookieAuthenticationOptions`, if set.</span></span>

<span data-ttu-id="d67eb-362">Il seguente frammento di codice crea un'identità e i cookie corrispondente che la durata è di 20 minuti.</span><span class="sxs-lookup"><span data-stu-id="d67eb-362">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="d67eb-363">Questo ignora eventuali impostazioni di scadenza variabile configurati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="d67eb-363">This ignores any sliding expiration settings previously configured.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d67eb-364">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-364">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d67eb-365">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d67eb-365">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

## <a name="see-also"></a><span data-ttu-id="d67eb-366">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="d67eb-366">See also</span></span>

* [<span data-ttu-id="d67eb-367">Modifiche di autenticazione 2.0 / migrazione annuncio</span><span class="sxs-lookup"><span data-stu-id="d67eb-367">Auth 2.0 Changes / Migration Announcement</span></span>](https://github.com/aspnet/Announcements/issues/262)
* [<span data-ttu-id="d67eb-368">Limitazione dell'identità tramite schema</span><span class="sxs-lookup"><span data-stu-id="d67eb-368">Limiting identity by scheme</span></span>](xref:security/authorization/limitingidentitybyscheme)
* [<span data-ttu-id="d67eb-369">Autorizzazione basata sulle attestazioni</span><span class="sxs-lookup"><span data-stu-id="d67eb-369">Claims-Based Authorization</span></span>](xref:security/authorization/claims)
* [<span data-ttu-id="d67eb-370">Controlli del ruolo basata su criteri</span><span class="sxs-lookup"><span data-stu-id="d67eb-370">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
