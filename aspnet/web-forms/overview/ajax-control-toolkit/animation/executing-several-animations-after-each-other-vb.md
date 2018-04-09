---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: L'esecuzione di numerose animazioni dopo l'altro (VB) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Consente di eseguire severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 700946b9f32c5ed2dcb8586e7c0e84d2238ff103
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="87192-104">L'esecuzione di numerose animazioni dopo l'altro (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="87192-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="87192-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="87192-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="87192-106">[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="87192-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="87192-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="87192-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="87192-108">Consente di eseguire numerose animazioni uno dopo l'altro.</span><span class="sxs-lookup"><span data-stu-id="87192-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="87192-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="87192-109">Overview</span></span>

<span data-ttu-id="87192-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="87192-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="87192-111">Consente di eseguire numerose animazioni uno dopo l'altro.</span><span class="sxs-lookup"><span data-stu-id="87192-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="87192-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="87192-112">Steps</span></span>

<span data-ttu-id="87192-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="87192-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="87192-114">Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="87192-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="87192-115">Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="87192-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="87192-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="87192-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="87192-117">All'interno di `<Animations>` nodo, utilizzare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="87192-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="87192-118">In genere, `<OnLoad>` accetta solo un'animazione.</span><span class="sxs-lookup"><span data-stu-id="87192-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="87192-119">Il framework di animazione consente di unire più animazioni in uno solo tramite il `<Sequence>` elemento.</span><span class="sxs-lookup"><span data-stu-id="87192-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="87192-120">Tutte le animazioni all'interno di `<Sequence>` vengono eseguite una dopo l'altro.</span><span class="sxs-lookup"><span data-stu-id="87192-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="87192-121">Ecco l'un markup possibili per il `AnimationExtender` controllo, effettua prima il pannello più ampio e quindi diminuire l'altezza:</span><span class="sxs-lookup"><span data-stu-id="87192-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="87192-122">Quando si esegue questo script, il pannello riceve più larghi e quindi più piccoli.</span><span class="sxs-lookup"><span data-stu-id="87192-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="87192-123">[![Innanzitutto viene aumentata la larghezza](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="87192-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="87192-124">Innanzitutto viene aumentata la larghezza ([fare clic per visualizzare l'immagine ingrandita](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="87192-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="87192-125">[![Quindi viene diminuita l'altezza](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="87192-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="87192-126">Quindi viene ridotto l'altezza ([fare clic per visualizzare l'immagine ingrandita](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="87192-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="87192-127">[Precedente](executing-several-animations-at-the-same-time-vb.md)
> [Successivo](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="87192-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
