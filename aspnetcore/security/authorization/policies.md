---
title: Autorizzazione basata su criteri in ASP.NET Core
author: rick-anderson
description: Informazioni su come creare e utilizzare i gestori di criteri di autorizzazione per applicare i requisiti di autorizzazione in un'applicazione ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 411fee90bdccfb45c33f5d4ccd7864c83c614e70
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072861"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="c4da3-103">Autorizzazione basata su criteri in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4da3-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="c4da3-104">Nel sistema il [autorizzazione basata sui ruoli](xref:security/authorization/roles) e [autorizzazione basata sulle attestazioni](xref:security/authorization/claims) utilizzano un requisito, un gestore del requisito e un criterio configurato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="c4da3-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="c4da3-105">Questi blocchi predefiniti supportano l'espressione di valutazioni di autorizzazione nel codice.</span><span class="sxs-lookup"><span data-stu-id="c4da3-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="c4da3-106">Il risultato è una struttura di autorizzazioni complete, riutilizzabili e testabili.</span><span class="sxs-lookup"><span data-stu-id="c4da3-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="c4da3-107">Un criterio di autorizzazione è costituito da uno o più requisiti.</span><span class="sxs-lookup"><span data-stu-id="c4da3-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="c4da3-108">È registrato come parte della configurazione del servizio di autorizzazione, il `Startup.ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="c4da3-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="c4da3-109">Nell'esempio precedente, viene creato un criterio di "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="c4da3-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="c4da3-110">Ha un unico requisito&mdash;che di una durata minima, che viene fornita come parametro al requisito.</span><span class="sxs-lookup"><span data-stu-id="c4da3-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="c4da3-111">I criteri vengono applicati tramite il `[Authorize]` attributo con il nome del criterio.</span><span class="sxs-lookup"><span data-stu-id="c4da3-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="c4da3-112">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c4da3-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="c4da3-113">Requisiti</span><span class="sxs-lookup"><span data-stu-id="c4da3-113">Requirements</span></span>

<span data-ttu-id="c4da3-114">Un requisito di autorizzazione è una raccolta di parametri dei dati che un criterio è possibile utilizzare per valutare l'entità utente corrente.</span><span class="sxs-lookup"><span data-stu-id="c4da3-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="c4da3-115">In questo criterio "AtLeast21", il requisito è un singolo parametro&mdash;la data minima.</span><span class="sxs-lookup"><span data-stu-id="c4da3-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="c4da3-116">Implementa un requisito [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), che è un'interfaccia marcatore vuoto.</span><span class="sxs-lookup"><span data-stu-id="c4da3-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="c4da3-117">Un requisito di età minima con parametri potrebbe essere implementato come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c4da3-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="c4da3-118">Non è un requisito a dati o proprietà.</span><span class="sxs-lookup"><span data-stu-id="c4da3-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="c4da3-119">Gestori di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="c4da3-119">Authorization handlers</span></span>

<span data-ttu-id="c4da3-120">Un gestore di autorizzazione è responsabile per la valutazione delle proprietà di un requisito.</span><span class="sxs-lookup"><span data-stu-id="c4da3-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="c4da3-121">Il gestore autorizzazioni valuta i requisiti in un oggetto fornito [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) per determinare se è consentito l'accesso.</span><span class="sxs-lookup"><span data-stu-id="c4da3-121">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="c4da3-122">Può avere un requisito [più gestori](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="c4da3-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="c4da3-123">Può ereditare un gestore [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), dove `TRequirement` è il requisito deve essere gestito.</span><span class="sxs-lookup"><span data-stu-id="c4da3-123">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="c4da3-124">In alternativa, è possibile implementare un gestore [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) per gestire più di un tipo di requisito.</span><span class="sxs-lookup"><span data-stu-id="c4da3-124">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="c4da3-125">Utilizzare un gestore per un requisito</span><span class="sxs-lookup"><span data-stu-id="c4da3-125">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="c4da3-126">Di seguito è riportato un esempio di una relazione uno a uno in cui un gestore della durata minima prevede l'utilizzo di un unico requisito:</span><span class="sxs-lookup"><span data-stu-id="c4da3-126">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="c4da3-127">Il codice precedente determina se l'entità utente corrente dispone di una data di nascita di attestazioni che è stata eseguita da un'autorità emittente attendibile e nota.</span><span class="sxs-lookup"><span data-stu-id="c4da3-127">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="c4da3-128">Autorizzazione non può verificarsi quando la richiesta è manca, nel qual caso viene restituita un'attività completata.</span><span class="sxs-lookup"><span data-stu-id="c4da3-128">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="c4da3-129">Quando un'attestazione è presente, viene calcolato l'età dell'utente.</span><span class="sxs-lookup"><span data-stu-id="c4da3-129">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="c4da3-130">Se l'utente soddisfi il periodo di memorizzazione minimo definito dai requisiti, autorizzazione viene considerata riuscita.</span><span class="sxs-lookup"><span data-stu-id="c4da3-130">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="c4da3-131">Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito come unico parametro soddisfatto.</span><span class="sxs-lookup"><span data-stu-id="c4da3-131">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="c4da3-132">Utilizzare un gestore per i requisiti di più</span><span class="sxs-lookup"><span data-stu-id="c4da3-132">Use a handler for multiple requirements</span></span>

<span data-ttu-id="c4da3-133">Di seguito è riportato un esempio di una relazione uno-a-molti in cui un gestore autorizzazioni utilizza tre requisiti:</span><span class="sxs-lookup"><span data-stu-id="c4da3-133">The following is an example of a one-to-many relationship in which a permission handler utilizes three requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="c4da3-134">Consente di scorrere il codice precedente [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;una proprietà che contiene i requisiti non contrassegnato come esito positivo.</span><span class="sxs-lookup"><span data-stu-id="c4da3-134">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="c4da3-135">Se l'utente dispone dell'autorizzazione di lettura, che deve essere un proprietario o sponsor per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="c4da3-135">If the user has read permission, he or she must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="c4da3-136">Se l'utente dispone di modifica o Elimina autorizzazione, che deve essere un proprietario per accedere alla risorsa richiesta.</span><span class="sxs-lookup"><span data-stu-id="c4da3-136">If the user has edit or delete permission, he or she must be an owner to access the requested resource.</span></span> <span data-ttu-id="c4da3-137">Quando l'autorizzazione ha esito positivo, `context.Succeed` viene richiamato con il requisito come unico parametro soddisfatto.</span><span class="sxs-lookup"><span data-stu-id="c4da3-137">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="c4da3-138">Registrazione del gestore</span><span class="sxs-lookup"><span data-stu-id="c4da3-138">Handler registration</span></span>

<span data-ttu-id="c4da3-139">Gestori eventi vengono registrati nella raccolta di servizi durante la configurazione.</span><span class="sxs-lookup"><span data-stu-id="c4da3-139">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="c4da3-140">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c4da3-140">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="c4da3-141">Ogni gestore viene aggiunto alla raccolta di servizi richiamando `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="c4da3-141">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="c4da3-142">Cosa deve restituire un gestore?</span><span class="sxs-lookup"><span data-stu-id="c4da3-142">What should a handler return?</span></span>

<span data-ttu-id="c4da3-143">Si noti che il `Handle` metodo il [esempio gestore](#security-authorization-handler-example) non restituisce alcun valore.</span><span class="sxs-lookup"><span data-stu-id="c4da3-143">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="c4da3-144">Come viene riportato lo stato di esito positivo o un errore indicato?</span><span class="sxs-lookup"><span data-stu-id="c4da3-144">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="c4da3-145">Indica l'esito positivo chiamando un gestore `context.Succeed(IAuthorizationRequirement requirement)`, passando il requisito che è stata convalidata.</span><span class="sxs-lookup"><span data-stu-id="c4da3-145">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="c4da3-146">Un gestore non è necessario gestire gli errori in genere, come altri gestori per lo stesso requisito può essere completata.</span><span class="sxs-lookup"><span data-stu-id="c4da3-146">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="c4da3-147">Errore, anche se altri gestori di requisiti di esito positivo, chiamare `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="c4da3-147">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="c4da3-148">Se impostato su `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) proprietà (disponibile in ASP.NET Core 1.1 e versioni successive) provoca un corto circuito l'esecuzione di gestori quando `context.Fail` viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="c4da3-148">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="c4da3-149">`InvokeHandlersAfterFailure` Per impostazione predefinita `true`, nel qual caso tutti i gestori vengono chiamati.</span><span class="sxs-lookup"><span data-stu-id="c4da3-149">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="c4da3-150">In questo modo i requisiti per produrre effetti collaterali, ad esempio la registrazione, che hanno luogo anche se `context.Fail` è stato chiamato in un altro gestore.</span><span class="sxs-lookup"><span data-stu-id="c4da3-150">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="c4da3-151">Perché desiderare più gestori per un requisito?</span><span class="sxs-lookup"><span data-stu-id="c4da3-151">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="c4da3-152">Nei casi in cui si desidera valutazione in un **o** base, implementare più gestori per un unico requisito.</span><span class="sxs-lookup"><span data-stu-id="c4da3-152">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="c4da3-153">Microsoft ha ad esempio, le porte che viene aperta solo con schede di chiave.</span><span class="sxs-lookup"><span data-stu-id="c4da3-153">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="c4da3-154">Se si lascia la smart card a casa, il centralinista Stampa etichetta della custodia temporanea e verrà aperta la porta.</span><span class="sxs-lookup"><span data-stu-id="c4da3-154">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="c4da3-155">In questo scenario, è necessario un unico requisito *BuildingEntry*, ma più gestori, ciascuno di essi un unico requisito di analisi.</span><span class="sxs-lookup"><span data-stu-id="c4da3-155">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="c4da3-156">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="c4da3-156">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="c4da3-157">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c4da3-157">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="c4da3-158">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c4da3-158">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="c4da3-159">Assicurarsi che entrambi i gestori siano [registrato](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="c4da3-159">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="c4da3-160">Valuta se ha esito positivo su gestore, quando un criterio di `BuildingEntryRequirement`, ha esito positivo della valutazione dei criteri.</span><span class="sxs-lookup"><span data-stu-id="c4da3-160">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="c4da3-161">Utilizzo di func per soddisfare i criteri di</span><span class="sxs-lookup"><span data-stu-id="c4da3-161">Using a func to fulfill a policy</span></span>

<span data-ttu-id="c4da3-162">Potrebbero verificarsi situazioni in cui che soddisfano un criterio è semplice da express nel codice.</span><span class="sxs-lookup"><span data-stu-id="c4da3-162">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="c4da3-163">È possibile fornire un `Func<AuthorizationHandlerContext, bool>` quando si configurano i criteri con il `RequireAssertion` generatore di criteri.</span><span class="sxs-lookup"><span data-stu-id="c4da3-163">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="c4da3-164">Ad esempio, il precedente `BadgeEntryHandler` potrebbe essere riscritto come segue:</span><span class="sxs-lookup"><span data-stu-id="c4da3-164">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="c4da3-165">Accesso al contesto di richiesta MVC nei gestori</span><span class="sxs-lookup"><span data-stu-id="c4da3-165">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="c4da3-166">Il `HandleRequirementAsync` metodo è implementato in un gestore di autorizzazione presenta due parametri: un `AuthorizationHandlerContext` e `TRequirement` si sta gestendo.</span><span class="sxs-lookup"><span data-stu-id="c4da3-166">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="c4da3-167">Framework di MVC o Jabbr hanno la possibilità di aggiungere qualsiasi oggetto di `Resource` proprietà il `AuthorizationHandlerContext` per passare informazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="c4da3-167">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="c4da3-168">Ad esempio, MVC passa un'istanza di [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) nel `Resource` proprietà.</span><span class="sxs-lookup"><span data-stu-id="c4da3-168">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="c4da3-169">Questa proprietà fornisce l'accesso a `HttpContext`, `RouteData`e tutto ciò che altrimenti forniti da MVC e pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="c4da3-169">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="c4da3-170">L'utilizzo del `Resource` è di proprietà specifici del framework.</span><span class="sxs-lookup"><span data-stu-id="c4da3-170">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="c4da3-171">Utilizzando le informazioni nel `Resource` proprietà limita i criteri di autorizzazione per un framework specifico.</span><span class="sxs-lookup"><span data-stu-id="c4da3-171">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="c4da3-172">È necessario eseguire il cast di `Resource` proprietà utilizzando il `as` (parola chiave) e quindi confermare il cast ha esito positivo per verificare che il codice non di arresto anomalo con un `InvalidCastException` quando viene eseguito da altri Framework:</span><span class="sxs-lookup"><span data-stu-id="c4da3-172">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
