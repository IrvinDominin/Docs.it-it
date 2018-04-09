---
title: Limitare la durata di payload protetto in ASP.NET Core
author: rick-anderson
description: Informazioni su come limitare la durata di un payload protetto utilizzando le API di protezione dei dati di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 324887b3d29de989ad855c4e78fd5a235fdb560e
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a><span data-ttu-id="274a9-103">Limitare la durata di payload protetto in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="274a9-103">Limit the lifetime of protected payloads in ASP.NET Core</span></span>

<span data-ttu-id="274a9-104">Esistono scenari in cui lo sviluppatore dell'applicazione desidera creare un payload protetto che scade dopo un determinato periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="274a9-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="274a9-105">Ad esempio, il payload protetto potrebbe rappresentare un token di reimpostazione della password che devono solo essere valido per un'ora.</span><span class="sxs-lookup"><span data-stu-id="274a9-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="274a9-106">È certamente possibile per gli sviluppatori di creare i propri formato di payload che contiene una data di scadenza incorporati e gli sviluppatori avanzati si consiglia di eseguire questa operazione comunque, ma per la maggior parte degli sviluppatori di gestire queste scadenze può crescere noiosa.</span><span class="sxs-lookup"><span data-stu-id="274a9-106">It's certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="274a9-107">Per semplificare questa operazione per il pubblico di sviluppatore, il pacchetto [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contiene API dell'utilità per la creazione di payload che scadrà automaticamente dopo un determinato periodo di tempo.</span><span class="sxs-lookup"><span data-stu-id="274a9-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="274a9-108">Queste API sporgere all'esterno del `ITimeLimitedDataProtector` tipo.</span><span class="sxs-lookup"><span data-stu-id="274a9-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="274a9-109">Utilizzo delle API</span><span class="sxs-lookup"><span data-stu-id="274a9-109">API usage</span></span>

<span data-ttu-id="274a9-110">Il `ITimeLimitedDataProtector` è l'interfaccia di base per la protezione e la rimozione della protezione di payload di tempo limitato / temporanee.</span><span class="sxs-lookup"><span data-stu-id="274a9-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="274a9-111">Per creare un'istanza di un `ITimeLimitedDataProtector`, è necessario innanzitutto un'istanza di una normale [oggetto IDataProtector](xref:security/data-protection/consumer-apis/overview) costruito con uno scopo specifico.</span><span class="sxs-lookup"><span data-stu-id="274a9-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](xref:security/data-protection/consumer-apis/overview) constructed with a specific purpose.</span></span> <span data-ttu-id="274a9-112">Una volta il `IDataProtector` istanza è disponibile, chiamare il `IDataProtector.ToTimeLimitedDataProtector` metodo di estensione per ottenere una protezione con le funzionalità di scadenza predefinita.</span><span class="sxs-lookup"><span data-stu-id="274a9-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="274a9-113">`ITimeLimitedDataProtector` espone i metodi di estensione e di area API seguenti:</span><span class="sxs-lookup"><span data-stu-id="274a9-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="274a9-114">CreateProtector (scopo stringa): ITimeLimitedDataProtector - questa API è simile al `IDataProtectionProvider.CreateProtector` in quanto può essere utilizzato per creare [allo scopo di catene](xref:security/data-protection/consumer-apis/purpose-strings) da una protezione con durata limitata di radice.</span><span class="sxs-lookup"><span data-stu-id="274a9-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](xref:security/data-protection/consumer-apis/purpose-strings) from a root time-limited protector.</span></span>

* <span data-ttu-id="274a9-115">Proteggere (byte [] testo normale, scadenza DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="274a9-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="274a9-116">Proteggere (testo normale di byte [], durata intervallo di tempo): byte]</span><span class="sxs-lookup"><span data-stu-id="274a9-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="274a9-117">Proteggere (testo crittografato [byte): byte]</span><span class="sxs-lookup"><span data-stu-id="274a9-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="274a9-118">Proteggere (stringa testo normale, scadenza DateTimeOffset): stringa</span><span class="sxs-lookup"><span data-stu-id="274a9-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="274a9-119">Proteggere (testo normale di stringa, durata intervallo di tempo): stringa</span><span class="sxs-lookup"><span data-stu-id="274a9-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="274a9-120">Proteggere (testo normale stringa): stringa</span><span class="sxs-lookup"><span data-stu-id="274a9-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="274a9-121">Oltre alle principali `Protect` metodi che accettano solo il testo non crittografato, sono disponibili nuovi overload che consentono di specificare la data di scadenza del payload.</span><span class="sxs-lookup"><span data-stu-id="274a9-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="274a9-122">La data di scadenza può essere specificata come una data assoluta (tramite un `DateTimeOffset`) o come un tempo relativi (dal sistema corrente time, tramite un `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="274a9-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="274a9-123">Se viene chiamato un overload che non avranno una scadenza, il payload presuppone scada mai.</span><span class="sxs-lookup"><span data-stu-id="274a9-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="274a9-124">Rimuovere la protezione (protectedData [] byte, out scadenza DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="274a9-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="274a9-125">Rimuovere la protezione (byte [] protectedData): byte]</span><span class="sxs-lookup"><span data-stu-id="274a9-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="274a9-126">Rimuovere la protezione (stringa protectedData, out scadenza DateTimeOffset): stringa</span><span class="sxs-lookup"><span data-stu-id="274a9-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="274a9-127">Rimuovere la protezione (stringa protectedData): stringa</span><span class="sxs-lookup"><span data-stu-id="274a9-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="274a9-128">Il `Unprotect` i metodi restituiscono i dati originali non protetti.</span><span class="sxs-lookup"><span data-stu-id="274a9-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="274a9-129">Se il payload non è ancora scaduto, la scadenza assoluta viene restituita come parametro insieme ai dati protetti originali out facoltativa.</span><span class="sxs-lookup"><span data-stu-id="274a9-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="274a9-130">Se il payload è scaduto, tutti gli overload del metodo Unprotect genererà CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="274a9-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="274a9-131">Ha non è consigliabile usare queste API per proteggere i payload che richiedono la persistenza a lungo termine o indefinita.</span><span class="sxs-lookup"><span data-stu-id="274a9-131">It's not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="274a9-132">"È concedere per il payload protetto essere recuperati in modo permanente dopo un mese?"</span><span class="sxs-lookup"><span data-stu-id="274a9-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="274a9-133">può essere utilizzato come una buona regola pratica; Se la risposta non sviluppatori quindi considerare API alternative.</span><span class="sxs-lookup"><span data-stu-id="274a9-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="274a9-134">L'esempio seguente viene utilizzato il [i percorsi del codice non DI](xref:security/data-protection/configuration/non-di-scenarios) per creare un'istanza di sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="274a9-134">The sample below uses the [non-DI code paths](xref:security/data-protection/configuration/non-di-scenarios) for instantiating the data protection system.</span></span> <span data-ttu-id="274a9-135">Per eseguire questo esempio, verificare che è stato innanzitutto aggiunto un riferimento al pacchetto Microsoft.AspNetCore.DataProtection.Extensions.</span><span class="sxs-lookup"><span data-stu-id="274a9-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
