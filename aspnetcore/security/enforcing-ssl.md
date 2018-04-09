---
title: Applicare HTTPS in una base di ASP.NET
author: rick-anderson
description: Viene illustrato come richiedere HTTPS/TLS in un Core di ASP.NET web app.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2ebb975e1ea17698cee13ca00d3f5df4a5135e38
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a>Applicare HTTPS in una base di ASP.NET

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo documento illustra come:

- Utilizzare HTTPS per tutte le richieste.
- Reindirizzare tutte le richieste HTTP a HTTPS.

> [!WARNING]
> Eseguire **non** utilizzare `RequireHttpsAttribute` sulle API Web che ricevono informazioni riservate. `RequireHttpsAttribute` Usa i codici di stato HTTP per il reindirizzamento dei browser da HTTP a HTTPS. Client dell'API non possono comprendere o rispettare i reindirizzamenti da HTTP a HTTPS. Tali client possono inviare informazioni su HTTP. API Web dovrebbero:
>
>* È in ascolto su HTTP.
>* Chiudere la connessione con il codice di stato 400 (Bad Request) e non rispondere alla richiesta.

## <a name="require-https"></a>Utilizzo di HTTPS

Il [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) viene utilizzato per richiedere HTTPS. `[RequireHttpsAttribute]` può decorare controller o i metodi, oppure può essere applicata a livello globale. Per applicare l'attributo a livello globale, aggiungere il seguente codice al `ConfigureServices` in `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Il codice evidenziato precedente richiede l'utilizzano di tutte le richieste `HTTPS`; pertanto, le richieste HTTP vengono ignorate. Il seguente codice evidenziato reindirizza tutte le richieste HTTP a HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Per ulteriori informazioni, vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting).

Che richiedono HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una procedura consigliata. L'applicazione di `[RequireHttps]` attributo a tutte le pagine controller/Razor non è considerata sicura come che richiedono HTTPS a livello globale. Non è possibile garantire il `[RequireHttps]` attributo viene applicato quando vengono aggiunti nuovi controller e le pagine Razor.
