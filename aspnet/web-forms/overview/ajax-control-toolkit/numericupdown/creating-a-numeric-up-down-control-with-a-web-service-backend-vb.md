---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Creazione di controllo su/giù numerico con un back-end del servizio Web (VB) | Documenti Microsoft
author: wenz
description: Anziché consentire a un utente di digitare un valore in una casella di controllo, un controllo (che è presente in Windows e altri sistemi operativi) su/giù numerico potrebbe rivelarsi man mano c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 690fd89c552407ec5d77419aae2488e4832efe44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Creazione di un controllo su/giù numerico con un back-end del servizio Web (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> Anziché consentire a un utente di digitare un valore in una casella di controllo, un valore numerico su/giù controllo (che è presente in Windows e altri sistemi operativi) potrebbe rivelarsi man mano familiarità. Per impostazione predefinita, il controllo NumericUpDown sempre aumenta o diminuisce di un valore di 1, ma una maggiore flessibilità di prova di un servizio web.


## <a name="overview"></a>Panoramica

Anziché consentire a un utente di digitare un valore in una casella di controllo, un valore numerico su/giù controllo (che è presente in Windows e altri sistemi operativi) potrebbe rivelarsi man mano familiarità. Per impostazione predefinita, il `NumericUpDown` controllo sempre aumenta o diminuisce di un valore di 1, ma una maggiore flessibilità di prova di un servizio web.

## <a name="steps"></a>Passaggi

ASP.NET AJAX Control Toolkit contiene il `NumericUpDown` extender che aggiunge automaticamente due pulsanti di una casella di testo: uno per aumentare il valore, uno per ridurlo. Tuttavia il controllo supporta una chiamata al servizio web (o una chiamata al metodo pagina). Ogni volta che il viene selezionato verso l'alto o verso il basso pulsante, il codice JavaScript codice si connette al server web ed esegue un metodo non esiste. La firma del metodo è quello riportato di seguito:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

Il `current` argomento è il valore corrente nella casella di testo; il `tag` attributo è dati di un contesto aggiuntivo che possono essere impostati come una proprietà del `NumericUpDown` extender (sebbene non sia obbligatorio).

Per questo esempio, il valore numerico controllo su/giù deve consentire solo potenze di due valori: 1, 2, 4, 8, 16, 32, 64 e così via. Di conseguenza, il metodo eseguito quando l'utente desidera aumentare il valore necessario double il valore precedente. l'altro metodo è necessario dividere valore per due. Ecco quindi il servizio web completo:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Infine, creare una nuova pagina ASP.NET. Come al solito, è necessario un `ScriptManager` (controllo), un `TextBox` controllo e un `NumericUpDownExtender` controllo. Nel secondo caso, è necessario fornire le informazioni sul servizio web:

- `ServiceDownMethod` metodo web nome la freccia giù o la pagina (metodo)
- `ServiceDownPath` percorso del servizio web con il metodo di servizio verso il basso. omettere se si utilizza un metodo di pagina
- `ServiceUpMethod` nome della freccia metodo web o di pagina (metodo)
- `ServiceUpPath` percorso del servizio web con il metodo di servizio aggiornato; omettere se si utilizza un metodo di pagina

Di seguito è riportato il codice completo per la pagina:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Se si esegue la pagina, si noti come il valore nella casella di testo raddoppia sempre quando si fare clic sul pulsante superiore e viene dimezzato quando si fa clic sul pulsante inferiore.


[![Vengono visualizzati solo i numeri una potenza di 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Vengono visualizzati solo i numeri che sono potenza di 2 ([fare clic per visualizzare l'immagine ingrandita](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
