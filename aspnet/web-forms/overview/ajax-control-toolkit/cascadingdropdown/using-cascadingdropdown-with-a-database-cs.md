---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: Utilizza CascadingDropDown con un Database (c#) | Documenti Microsoft
author: wenz
description: Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 1991c26d408e593999288ea6df0467cea0369457
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879127"
---
<a name="using-cascadingdropdown-with-a-database-c"></a><span data-ttu-id="5e8c2-103">Utilizza CascadingDropDown con un Database (c#)</span><span class="sxs-lookup"><span data-stu-id="5e8c2-103">Using CascadingDropDown with a Database (C#)</span></span>
====================
<span data-ttu-id="5e8c2-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5e8c2-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5e8c2-105">[Scaricare codice](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5e8c2-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span></span>

> <span data-ttu-id="5e8c2-106">Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="5e8c2-107">Affinché il funzionamento, è necessario creare un servizio web speciale.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-107">In order for this to work, a special web service must be created.</span></span>


## <a name="overview"></a><span data-ttu-id="5e8c2-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="5e8c2-108">Overview</span></span>

<span data-ttu-id="5e8c2-109">Il controllo CascadingDropDown AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in una DropDownList carichi associati valori in un altro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="5e8c2-110">(Ad esempio, un unico elenco fornisce un elenco degli stati ed elenco successivo viene riempita con città importanti in tale stato.) Affinché il funzionamento, è necessario creare un servizio web speciale.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) In order for this to work, a special web service must be created.</span></span>

## <a name="steps"></a><span data-ttu-id="5e8c2-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="5e8c2-111">Steps</span></span>

<span data-ttu-id="5e8c2-112">Prima di tutto, un'origine dati è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-112">First of all, a data source is required.</span></span> <span data-ttu-id="5e8c2-113">Questo esempio viene utilizzato il database AdventureWorks e Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="5e8c2-114">Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa express edition) ed è anche disponibile come download separato sotto [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="5e8c2-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="5e8c2-115">Il database AdventureWorks fa parte di database di esempio e gli esempi di SQL Server 2005 (download all'indirizzo [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="5e8c2-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="5e8c2-116">Il modo più semplice per configurare il database consiste nell'utilizzare Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e collegare il `AdventureWorks.mdf` file di database.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="5e8c2-117">In questo esempio, si presuppone che l'istanza di SQL Server 2005 Express Edition è stato chiamato `SQLEXPRESS` e si trova nello stesso computer come server web; questo è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="5e8c2-118">Se il programma di installazione diverso, è necessario adattare le informazioni di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="5e8c2-119">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il &lt; `form` &gt; elemento):</span><span class="sxs-lookup"><span data-stu-id="5e8c2-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

<span data-ttu-id="5e8c2-120">Nel passaggio successivo, sono necessari due controlli DropDownList.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-120">In the next step, two DropDownList controls are required.</span></span> <span data-ttu-id="5e8c2-121">In questo esempio, viene usato il fornitore e informazioni di contatto da AdventureWorks, pertanto è creare un unico elenco per i fornitori disponibili e uno per i contatti disponibili:</span><span class="sxs-lookup"><span data-stu-id="5e8c2-121">In this sample, we use the vendor and contact information from AdventureWorks, thus we create one list for the available vendors and one for the available contacts:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

<span data-ttu-id="5e8c2-122">Quindi, è necessario aggiungere due Extender CascadingDropDown alla pagina.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-122">Then, two CascadingDropDown extenders must be added to the page.</span></span> <span data-ttu-id="5e8c2-123">Uno compilato automaticamente il primo elenco (fornitori), e l'altro nel secondo elenco (contatti).</span><span class="sxs-lookup"><span data-stu-id="5e8c2-123">One fills the first (vendors) list, and the other one fills the second (contacts) list.</span></span> <span data-ttu-id="5e8c2-124">Impostare gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5e8c2-124">The following attributes must be set:</span></span>

- <span data-ttu-id="5e8c2-125">`ServicePath`: URL di un servizio web recapitare le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="5e8c2-125">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="5e8c2-126">`ServiceMethod`: Metodo web recapito le voci dell'elenco</span><span class="sxs-lookup"><span data-stu-id="5e8c2-126">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="5e8c2-127">`TargetControlID`: ID dell'elenco a discesa</span><span class="sxs-lookup"><span data-stu-id="5e8c2-127">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="5e8c2-128">`Category`: Informazioni sulle categorie viene inviati al metodo web quando viene chiamato</span><span class="sxs-lookup"><span data-stu-id="5e8c2-128">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="5e8c2-129">`PromptText`: Testo visualizzato quando si caricano in modo asincrono i dati elenco dal server</span><span class="sxs-lookup"><span data-stu-id="5e8c2-129">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>
- <span data-ttu-id="5e8c2-130">`ParentControlID`: (facoltativo) che il caricamento di trigger dell'elenco corrente di elenco a discesa di padre</span><span class="sxs-lookup"><span data-stu-id="5e8c2-130">`ParentControlID`: (optional) parent dropdown list that triggers loading of the current list</span></span>

<span data-ttu-id="5e8c2-131">A seconda del linguaggio di programmazione utilizzato, viene modificato il nome del servizio web in questione, ma tutti gli altri valori di attributo sono uguali.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-131">Depending on the programming language used, the name of the web service in question changes, but all other attribute values are the same.</span></span> <span data-ttu-id="5e8c2-132">Di seguito è l'elemento CascadingDropDown per il primo elenco a discesa:</span><span class="sxs-lookup"><span data-stu-id="5e8c2-132">Here is the CascadingDropDown element for the first dropdown list:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

<span data-ttu-id="5e8c2-133">È necessario impostare le estensioni di controllo per l'elenco secondo il `ParentControlID` attributo in modo che la selezione di una voce nei trigger elenco fornitori durante il caricamento associati a elementi dell'elenco di contatti.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-133">The control extenders for the second list need to set the `ParentControlID` attribute so that selecting an entry in the vendors list triggers loading associated elements in the contacts list.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

<span data-ttu-id="5e8c2-134">Il lavoro effettivo viene quindi eseguito nel servizio web, che è impostato come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-134">The actual work is then done in the web service, which is set up as follows.</span></span> <span data-ttu-id="5e8c2-135">Si noti che il `[ScriptService]` attributo viene utilizzato, in caso contrario AJAX ASP.NET non è possibile creare il proxy JavaScript per accedere ai metodi web dal codice di script sul lato client.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-135">Note that the `[ScriptService]` attribute is used, otherwise ASP.NET AJAX cannot create the JavaScript proxy to access the web methods from client-side script code.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

<span data-ttu-id="5e8c2-136">La firma dei metodi web chiamato da CascadingDropDown è come segue:</span><span class="sxs-lookup"><span data-stu-id="5e8c2-136">The signature of the web methods called by CascadingDropDown is as follows:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

<span data-ttu-id="5e8c2-137">Pertanto il valore restituito deve essere una matrice di tipo `CascadingDropDownNameValue` definito da Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-137">So the return value must be an array of type `CascadingDropDownNameValue` which is defined by the Control Toolkit.</span></span> <span data-ttu-id="5e8c2-138">Il `GetVendors()` metodo è piuttosto semplice da implementare: il codice si connette al database AdventureWorks e i primi 25 fornitori una query.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-138">The `GetVendors()` method is quite easy to implement: The code connects to the AdventureWorks database and queries the first 25 vendors.</span></span> <span data-ttu-id="5e8c2-139">Il primo parametro di `CascadingDropDownNameValue` costruttore è la didascalia della voce di elenco, il secondo valore (attributo value in del formato HTML &lt; `option` &gt; elemento).</span><span class="sxs-lookup"><span data-stu-id="5e8c2-139">The first parameter in the `CascadingDropDownNameValue` constructor is the caption of the list entry, the second one its value (value attribute in HTML's &lt;`option`&gt; element).</span></span> <span data-ttu-id="5e8c2-140">Ecco il codice:</span><span class="sxs-lookup"><span data-stu-id="5e8c2-140">Here is the code:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

<span data-ttu-id="5e8c2-141">Ottenere i contatti associati per un fornitore (nome del metodo: `GetContactsForVendor()`) è leggermente più complessi.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-141">Getting the associated contacts for a vendor (method name: `GetContactsForVendor()`) is a bit trickier.</span></span> <span data-ttu-id="5e8c2-142">Prima di tutto, è necessario determinare il fornitore a cui è stato selezionato nel primo elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-142">First of all, the vendor which has been selected in the first dropdown list must be determined.</span></span> <span data-ttu-id="5e8c2-143">Control Toolkit definisce un metodo di supporto per l'attività: la `ParseKnownCategoryValuesString()` metodo restituisce un `StringDictionary` elemento con i dati dell'elenco a discesa:</span><span class="sxs-lookup"><span data-stu-id="5e8c2-143">The Control Toolkit defines a helper method for that task: The `ParseKnownCategoryValuesString()` method returns a `StringDictionary` element with the dropdown data:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

<span data-ttu-id="5e8c2-144">Per motivi di sicurezza, questi dati devono essere convalidati prima.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-144">For security reasons, this data must be validated first.</span></span> <span data-ttu-id="5e8c2-145">Pertanto, se è presente una voce del fornitore (perché il `Category` del primo elemento CascadingDropDown è impostata su `"Vendor"`), è possibile recuperare l'ID del fornitore selezionato:</span><span class="sxs-lookup"><span data-stu-id="5e8c2-145">So if there is a Vendor entry (because the `Category` property of the first CascadingDropDown element is set to `"Vendor"`), the ID of the selected vendor may be retrieved:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

<span data-ttu-id="5e8c2-146">Il resto del metodo è piuttosto semplice, quindi.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-146">The rest of the method is fairly straight-forward, then.</span></span> <span data-ttu-id="5e8c2-147">ID del fornitore viene utilizzato come parametro per una query SQL che recupera tutti i contatti associati di tale fornitore.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-147">The vendor's ID is used as a parameter for an SQL query that retrieves all associated contacts for that vendor.</span></span> <span data-ttu-id="5e8c2-148">In questo caso, il metodo restituisce una matrice di tipo `CascadingDropDownNameValue`.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-148">Once again, the method returns an array of type `CascadingDropDownNameValue`.</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

<span data-ttu-id="5e8c2-149">Caricare la pagina ASP.NET e, dopo un breve periodo di tempo nell'elenco di fornitori viene compilato con le voci di 25.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-149">Load the ASP.NET page, and after a short while the vendor list is filled with 25 entries.</span></span> <span data-ttu-id="5e8c2-150">Selezionare una voce e notare come secondo elenco a discesa viene riempita con i dati.</span><span class="sxs-lookup"><span data-stu-id="5e8c2-150">Pick one entry and notice how the second dropdown list is filled with data.</span></span>


<span data-ttu-id="5e8c2-151">[![Nel primo elenco viene compilato automaticamente](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5e8c2-151">[![The first list is filled automatically](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span></span>

<span data-ttu-id="5e8c2-152">Nel primo elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine ingrandita](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5e8c2-152">The first list is filled automatically ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span></span>


<span data-ttu-id="5e8c2-153">[![Nel secondo elenco viene compilato in base della selezione nel primo elenco](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="5e8c2-153">[![The second list is filled according to the selection in the first list](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span></span>

<span data-ttu-id="5e8c2-154">Nel secondo elenco viene compilato in base alla selezione nel primo elenco ([fare clic per visualizzare l'immagine ingrandita](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="5e8c2-154">The second list is filled according to the selection in the first list ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5e8c2-155">[Precedente](filling-a-list-using-cascadingdropdown-cs.md)
> [Successivo](presetting-list-entries-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5e8c2-155">[Previous](filling-a-list-using-cascadingdropdown-cs.md)
[Next](presetting-list-entries-with-cascadingdropdown-cs.md)</span></span>
