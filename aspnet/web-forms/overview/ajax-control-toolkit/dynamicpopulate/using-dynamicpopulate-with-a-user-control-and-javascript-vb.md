---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Utilizzo di DynamicPopulate con un controllo utente e JavaScript (VB) | Documenti Microsoft
author: wenz
description: Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e inserisce il valore risultante in un controllo di destinazione t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 715973ef4923e635ec2a860d00d55f13a5f8b2d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="85dbe-103">Utilizzo di DynamicPopulate con un controllo utente e JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="85dbe-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>
====================
<span data-ttu-id="85dbe-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="85dbe-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="85dbe-105">[Scaricare codice](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="85dbe-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="85dbe-106">Il controllo DynamicPopulate in ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e riempie il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="85dbe-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="85dbe-107">È inoltre possibile attivare la popolazione utilizzando codice JavaScript sul lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="85dbe-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="85dbe-108">Tuttavia particolare attenzione deve essere eseguita quando il programma di estensione si trova in un controllo utente.</span><span class="sxs-lookup"><span data-stu-id="85dbe-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="85dbe-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="85dbe-109">Overview</span></span>

<span data-ttu-id="85dbe-110">Il `DynamicPopulate` controllo ASP.NET AJAX Control Toolkit chiama un servizio web (o un metodo di pagina) e riempie il valore risultante in un controllo di destinazione nella pagina senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="85dbe-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="85dbe-111">È inoltre possibile attivare la popolazione utilizzando codice JavaScript sul lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="85dbe-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="85dbe-112">Tuttavia particolare attenzione deve essere eseguita quando il programma di estensione si trova in un controllo utente.</span><span class="sxs-lookup"><span data-stu-id="85dbe-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="85dbe-113">Passaggi</span><span class="sxs-lookup"><span data-stu-id="85dbe-113">Steps</span></span>

<span data-ttu-id="85dbe-114">Prima di tutto, è necessario un servizio Web ASP.NET che implementa il metodo da chiamare `DynamicPopulateExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="85dbe-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="85dbe-115">Il servizio web implementa il metodo `getDate()` che prevede un argomento di tipo string, denominato `contextKey`, poiché il `DynamicPopulate` controllo invia una parte delle informazioni di contesto con ogni chiamata al servizio web.</span><span class="sxs-lookup"><span data-stu-id="85dbe-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="85dbe-116">Ecco il codice (file `DynamicPopulate.vb.asmx`) che consente di recuperare la data corrente in uno dei tre formati:</span><span class="sxs-lookup"><span data-stu-id="85dbe-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="85dbe-117">Nel passaggio successivo, creare un nuovo controllo utente (`.ascx` file), indicati dalla dichiarazione seguente nella prima riga:</span><span class="sxs-lookup"><span data-stu-id="85dbe-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="85dbe-118">Oggetto &lt; `label` &gt; elemento verrà utilizzato per visualizzare i dati provenienti dal server.</span><span class="sxs-lookup"><span data-stu-id="85dbe-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="85dbe-119">Anche nel file di controllo utente, si utilizzerà tre pulsanti di opzione, ogni uno che rappresenta uno dei tre formati di data supportati dal servizio web.</span><span class="sxs-lookup"><span data-stu-id="85dbe-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="85dbe-120">Quando l'utente fa clic su uno dei pulsanti di opzione, il browser verrà eseguito il codice JavaScript che è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="85dbe-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="85dbe-121">Questo codice consente di accedere il `DynamicPopulateExtender` (non è necessario l'ID strano ancora, questo argomento verrà descritto più avanti) e attiva la popolazione con dati dinamica.</span><span class="sxs-lookup"><span data-stu-id="85dbe-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="85dbe-122">Nel contesto del pulsante di opzione corrente `this.value` fa riferimento al valore, ovvero `format1`, `format2` o `format3` esattamente ciò che prevede il metodo web.</span><span class="sxs-lookup"><span data-stu-id="85dbe-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="85dbe-123">È l'unico elemento mancante nel controllo utente ancora il `DynamicPopulateExtender` controllo che collega i pulsanti di opzione per il servizio web.</span><span class="sxs-lookup"><span data-stu-id="85dbe-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="85dbe-124">Nuovo si può notare l'ID strano utilizzata per il controllo: `mcd1$myDate` anziché `myDate`.</span><span class="sxs-lookup"><span data-stu-id="85dbe-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="85dbe-125">In precedenza, il codice JavaScript utilizzato `mcd1_dpe1` per l'accesso di `DynamicPopulateExtender` anziché `dpe1`. Questa strategia di denominazione è necessario quando si utilizza `DynamicPopulateExtender` all'interno di un controllo utente.</span><span class="sxs-lookup"><span data-stu-id="85dbe-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="85dbe-126">Inoltre, è necessario incorporare il controllo utente in modo specifico per il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="85dbe-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="85dbe-127">Creare una nuova pagina ASP.NET e registrare un prefisso di tag per il controllo utente che appena implementato:</span><span class="sxs-lookup"><span data-stu-id="85dbe-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="85dbe-128">Quindi, includere ASP.NET AJAX `ScriptManager` controllo nella nuova pagina:</span><span class="sxs-lookup"><span data-stu-id="85dbe-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="85dbe-129">Infine, aggiungere il controllo utente alla pagina.</span><span class="sxs-lookup"><span data-stu-id="85dbe-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="85dbe-130">È necessario impostare il relativo `ID` attributo (e `runat="server"`, naturalmente), ma è anche necessario impostare un nome specifico: `mcd1` poiché si tratta del prefisso utilizzato all'interno del controllo utente per l'accesso utilizzando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="85dbe-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="85dbe-131">L'operazione è ora completata.</span><span class="sxs-lookup"><span data-stu-id="85dbe-131">And that's it!</span></span> <span data-ttu-id="85dbe-132">La pagina si comporti come previsto: un utente fa clic su uno dei pulsanti di opzione, il controllo in Toolkit chiama il servizio web e Visualizza la data corrente nel formato desiderato.</span><span class="sxs-lookup"><span data-stu-id="85dbe-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="85dbe-133">[![I pulsanti di opzione si trovano in un controllo utente](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="85dbe-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="85dbe-134">I pulsanti di opzione si trovano in un controllo utente ([fare clic per visualizzare l'immagine ingrandita](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="85dbe-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="85dbe-135">Precedente</span><span class="sxs-lookup"><span data-stu-id="85dbe-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
