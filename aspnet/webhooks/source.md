---
uid: webhooks/source
title: Codice sorgente Webhook ASP.NET e i pacchetti NuGet | Documenti Microsoft
author: rick-anderson
description: Collegamenti a codice sorgente Webhook ASP.NET e i pacchetti NuGet
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2018
ms.locfileid: "27709970"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Codice sorgente Webhook ASP.NET e i pacchetti NuGet

Microsoft ASP.NET WebHooks fa parte della famiglia Microsoft ASP.NET di moduli ed è ospitato come un [Apri progetto di origine in GitHub](https://github.com/aspnet/WebHooks). Ciò significa che è accettare i contributi ma, esaminare il [linee guida contributo](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) prima di inviare una richiesta pull.

Questa documentazione in linea che si sta leggendo questo punto è ospitata anche come [Open Source su GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e inoltre accetta contributi.

## <a name="nuget-packages"></a>Pacchetti NuGet

Il [pacchetti NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) sono suddivisi in tre parti:

* [Comuni](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): un pacchetto comune che è condiviso tra mittenti e ricevitori.

* [Mittente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): un set di pacchetti che supporta l'invio delle proprie Webhook ad altri utenti. In dettaglio in cui sono descritte le funzionalità per l'invio di Webhook [invio Webhook](sending/index.md).

* [Ricevitori](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): un set di pacchetti che supporta la ricezione di Webhook da altri utenti. La funzionalità per la ricezione di Webhook è descritto in dettaglio in [ricezione Webhook](receiving/index.md).
