---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: Associazione dati per Accordion (VB) | Documenti Microsoft
author: wenz
description: Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono in genere dichiarati w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: 0739e4ad263eb83f844a937eae4aa845df2f2593
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="databinding-to-an-accordion-vb"></a><span data-ttu-id="84005-104">Associazione dati per Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="84005-104">Databinding to an Accordion (VB)</span></span>
====================
<span data-ttu-id="84005-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="84005-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="84005-106">[Scaricare codice](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="84005-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span></span>

> <span data-ttu-id="84005-107">Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta.</span><span class="sxs-lookup"><span data-stu-id="84005-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="84005-108">I pannelli vengono in genere dichiarati all'interno della pagina, ma l'associazione a un'origine dati offre una maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="84005-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="84005-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="84005-109">Overview</span></span>

<span data-ttu-id="84005-110">Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta.</span><span class="sxs-lookup"><span data-stu-id="84005-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="84005-111">I pannelli vengono in genere dichiarati all'interno della pagina, ma l'associazione a un'origine dati offre una maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="84005-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="84005-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="84005-112">Steps</span></span>

<span data-ttu-id="84005-113">Prima di tutto, un'origine dati è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="84005-113">First of all, a data source is required.</span></span> <span data-ttu-id="84005-114">Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="84005-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="84005-115">Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="84005-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="84005-116">Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="84005-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="84005-117">Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.</span><span class="sxs-lookup"><span data-stu-id="84005-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="84005-118">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="84005-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="84005-119">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="84005-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="84005-120">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="84005-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

<span data-ttu-id="84005-121">Quindi, aggiungere un'origine dati per la pagina.</span><span class="sxs-lookup"><span data-stu-id="84005-121">Then, add a data source to the page.</span></span> <span data-ttu-id="84005-122">Per utilizzare una quantità limitata di dati, selezionare solo le prime cinque voci della tabella Vendor del database AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="84005-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="84005-123">Se si utilizza l'Assistente per Visual Studio per creare l'origine dati, è importante che un bug nella versione corrente come prefisso il nome della tabella (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="84005-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="84005-124">Il markup seguente viene illustrata la sintassi corretta:</span><span class="sxs-lookup"><span data-stu-id="84005-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

<span data-ttu-id="84005-125">Ricordare il nome (ID) dell'origine dati.</span><span class="sxs-lookup"><span data-stu-id="84005-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="84005-126">Questa identificazione molto deve quindi essere utilizzata nel `DataSourceID` proprietà del controllo Accordion:</span><span class="sxs-lookup"><span data-stu-id="84005-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

<span data-ttu-id="84005-127">All'interno del controllo Accordion, è possibile fornire modelli per varie parti del controllo, compresa l'intestazione (`<HeaderTemplate>`) e il contenuto (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="84005-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="84005-128">All'interno di questi elementi, restituire solo i dati dai dati di origine, utilizzando il `DataBinder.Eval()` metodo:</span><span class="sxs-lookup"><span data-stu-id="84005-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

<span data-ttu-id="84005-129">Quando viene caricata la pagina, l'origine dati deve essere associato al accordion con questo codice lato server:</span><span class="sxs-lookup"><span data-stu-id="84005-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

<span data-ttu-id="84005-130">Per concludere in questo esempio, è necessario definire le due classi CSS a cui fa riferimento nel controllo Accordion (nelle proprietà `HeaderCssClass` e `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="84005-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="84005-131">Inserire il markup seguente nel `<head>` sezione della pagina:</span><span class="sxs-lookup"><span data-stu-id="84005-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


<span data-ttu-id="84005-132">[![I dati nella accordion derivano direttamente dall'origine dati](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="84005-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span></span>

<span data-ttu-id="84005-133">I dati di accordion provengono direttamente dall'origine dati ([fare clic per visualizzare l'immagine ingrandita](databinding-to-an-accordion-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="84005-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="84005-134">[Precedente](dynamically-adding-an-accordion-pane-cs.md)
> [Successivo](dynamically-adding-an-accordion-pane-vb.md)</span><span class="sxs-lookup"><span data-stu-id="84005-134">[Previous](dynamically-adding-an-accordion-pane-cs.md)
[Next](dynamically-adding-an-accordion-pane-vb.md)</span></span>
