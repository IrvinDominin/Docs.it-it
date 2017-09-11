---
title: Core di ASP.NET MVC con Entity Framework Core - ordinamento, filtro, Paging - 3 di 10
author: tdykstra
description: "In questa esercitazione si aggiungeranno ordinamento, filtro e paging funzionalità alla pagina utilizzando ASP.NET Core e componenti di base di Entity Framework."
keywords: ASP.NET Core, Entity Framework Core, ordinamento, filtro, il paging, raggruppamento
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: e6c1ff3c-5673-43bf-9c2d-077f6ada1f29
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: bc2896d0eeda7e84cef06ee3f235e637bfe04318
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2017
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="74056-104">Ordinamento, filtro, paging e raggruppamento: EF Core con l'esercitazione di base di ASP.NET MVC (3 di 10)</span><span class="sxs-lookup"><span data-stu-id="74056-104">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="74056-105">Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="74056-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="74056-106">L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="74056-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="74056-107">Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).</span><span class="sxs-lookup"><span data-stu-id="74056-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="74056-108">Nell'esercitazione precedente, l'implementazione è un set di pagine web per operazioni CRUD di base per le entità di studenti.</span><span class="sxs-lookup"><span data-stu-id="74056-108">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="74056-109">In questa esercitazione si aggiungerà ordinamento, filtro e la funzionalità di paging alla pagina di indice di studenti.</span><span class="sxs-lookup"><span data-stu-id="74056-109">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="74056-110">Si creeranno inoltre una pagina che esegue il raggruppamento semplice.</span><span class="sxs-lookup"><span data-stu-id="74056-110">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="74056-111">Nella figura seguente viene illustrato l'aspetto di pagina al termine.</span><span class="sxs-lookup"><span data-stu-id="74056-111">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="74056-112">Le intestazioni di colonna sono link che l'utente può fare clic per ordinare in base alla colonna.</span><span class="sxs-lookup"><span data-stu-id="74056-112">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="74056-113">Fare clic su una colonna con intestazione ripetutamente passa alternativamente da crescente e decrescente.</span><span class="sxs-lookup"><span data-stu-id="74056-113">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Pagina di indice di studenti](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="74056-115">Aggiungere collegamenti di ordinamento di colonna per la pagina di indice di studenti</span><span class="sxs-lookup"><span data-stu-id="74056-115">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="74056-116">Per aggiungere l'ordinamento alla pagina di indice di studenti, sarà necessario impostare il `Index` metodo del controller di studenti e aggiungere codice per la visualizzazione dell'indice di studenti.</span><span class="sxs-lookup"><span data-stu-id="74056-116">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="74056-117">Aggiunta della funzionalità di ordinamento per il metodo di Index</span><span class="sxs-lookup"><span data-stu-id="74056-117">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="74056-118">In *StudentsController.cs*, sostituire il `Index` (metodo) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="74056-118">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

<span data-ttu-id="74056-119">[!code-csharp[Principale](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]</span><span class="sxs-lookup"><span data-stu-id="74056-119">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]</span></span>

<span data-ttu-id="74056-120">Questo codice riceve un `sortOrder` parametro dalla stringa di query nell'URL.</span><span class="sxs-lookup"><span data-stu-id="74056-120">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="74056-121">Il valore di stringa di query viene fornito da ASP.NET MVC di base come parametro al metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="74056-121">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="74056-122">Il parametro sarà una stringa che è "Name" o "Date", seguito facoltativamente da un carattere di sottolineatura e la stringa "desc" per specificare l'ordine decrescente.</span><span class="sxs-lookup"><span data-stu-id="74056-122">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="74056-123">L'ordinamento predefinito è crescente.</span><span class="sxs-lookup"><span data-stu-id="74056-123">The default sort order is ascending.</span></span>

<span data-ttu-id="74056-124">La prima volta che viene richiesta la pagina di indice, viene individuata alcuna stringa di query.</span><span class="sxs-lookup"><span data-stu-id="74056-124">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="74056-125">Gli studenti vengono visualizzati in ordine crescente in base al cognome, il valore predefinito è definito dall'istruzione case nel `switch` istruzione.</span><span class="sxs-lookup"><span data-stu-id="74056-125">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="74056-126">Quando l'utente fa clic hyperlink intestazione di colonna, appropriata `sortOrder` valore viene fornito nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="74056-126">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="74056-127">I due `ViewData` elementi (NameSortParm e DateSortParm) vengono utilizzati dalla vista per configurare i collegamenti ipertestuali sull'intestazione di colonna con i valori di stringa di query appropriata.</span><span class="sxs-lookup"><span data-stu-id="74056-127">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="74056-128">[!code-csharp[Principale](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="74056-128">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]</span></span>

<span data-ttu-id="74056-129">Si tratta di istruzioni ternarie.</span><span class="sxs-lookup"><span data-stu-id="74056-129">These are ternary statements.</span></span> <span data-ttu-id="74056-130">Il primo specifica che se il `sortOrder` parametro è null o vuoto, NameSortParm deve essere impostato su "name_desc"; in caso contrario, deve essere impostato su una stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="74056-130">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="74056-131">Le due istruzioni seguenti consentono la visualizzazione impostare la colonna collegamenti ipertestuali di intestazione come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="74056-131">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="74056-132">Ordinamento corrente</span><span class="sxs-lookup"><span data-stu-id="74056-132">Current sort order</span></span>  | <span data-ttu-id="74056-133">Collegamento ipertestuale cognome</span><span class="sxs-lookup"><span data-stu-id="74056-133">Last Name Hyperlink</span></span> | <span data-ttu-id="74056-134">Collegamento ipertestuale Data</span><span class="sxs-lookup"><span data-stu-id="74056-134">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="74056-135">Ultimo nome (crescente)</span><span class="sxs-lookup"><span data-stu-id="74056-135">Last Name ascending</span></span>  | <span data-ttu-id="74056-136">descending</span><span class="sxs-lookup"><span data-stu-id="74056-136">descending</span></span>          | <span data-ttu-id="74056-137">ascending</span><span class="sxs-lookup"><span data-stu-id="74056-137">ascending</span></span>      |
| <span data-ttu-id="74056-138">Ultimo nome (decrescente)</span><span class="sxs-lookup"><span data-stu-id="74056-138">Last Name descending</span></span> | <span data-ttu-id="74056-139">ascending</span><span class="sxs-lookup"><span data-stu-id="74056-139">ascending</span></span>           | <span data-ttu-id="74056-140">ascending</span><span class="sxs-lookup"><span data-stu-id="74056-140">ascending</span></span>      |
| <span data-ttu-id="74056-141">Data ordine crescente</span><span class="sxs-lookup"><span data-stu-id="74056-141">Date ascending</span></span>       | <span data-ttu-id="74056-142">ascending</span><span class="sxs-lookup"><span data-stu-id="74056-142">ascending</span></span>           | <span data-ttu-id="74056-143">descending</span><span class="sxs-lookup"><span data-stu-id="74056-143">descending</span></span>     |
| <span data-ttu-id="74056-144">Data ordine decrescente</span><span class="sxs-lookup"><span data-stu-id="74056-144">Date descending</span></span>      | <span data-ttu-id="74056-145">ascending</span><span class="sxs-lookup"><span data-stu-id="74056-145">ascending</span></span>           | <span data-ttu-id="74056-146">ascending</span><span class="sxs-lookup"><span data-stu-id="74056-146">ascending</span></span>      |

<span data-ttu-id="74056-147">Il metodo Usa LINQ to Entities per specificare la colonna da ordinare.</span><span class="sxs-lookup"><span data-stu-id="74056-147">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="74056-148">Il codice crea un `IQueryable` variabile prima dell'istruzione switch, modificarla nell'istruzione switch e chiama il `ToListAsync` metodo dopo il `switch` istruzione.</span><span class="sxs-lookup"><span data-stu-id="74056-148">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="74056-149">Quando si crea e modifica `IQueryable` variabili, nessuna query viene inviata al database.</span><span class="sxs-lookup"><span data-stu-id="74056-149">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="74056-150">La query non viene eseguita fino alla conversione di `IQueryable` oggetto in una raccolta chiamando un metodo, ad esempio `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="74056-150">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="74056-151">Pertanto, questo codice genera una singola query che non viene eseguita finché il `return View` istruzione.</span><span class="sxs-lookup"><span data-stu-id="74056-151">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="74056-152">Questo codice è stato possibile ottenere dettagliato con un numero elevato di colonne.</span><span class="sxs-lookup"><span data-stu-id="74056-152">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="74056-153">[Dell'ultima esercitazione di questa serie](advanced.md#dynamic-linq) viene illustrato come scrivere codice che consente di passare il nome di `OrderBy` colonna in una variabile di stringa.</span><span class="sxs-lookup"><span data-stu-id="74056-153">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="74056-154">Aggiungere collegamenti ipertestuali sull'intestazione di colonna per la visualizzazione dell'indice di Student</span><span class="sxs-lookup"><span data-stu-id="74056-154">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="74056-155">Sostituire il codice in *Views/Students/Index.cshtml*, con il codice seguente per aggiungere collegamenti ipertestuali di intestazione di colonna.</span><span class="sxs-lookup"><span data-stu-id="74056-155">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="74056-156">Le righe modificate vengono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="74056-156">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="74056-157">Questo codice Usa le informazioni contenute in `ViewData` valori di stringa di proprietà per impostare i collegamenti ipertestuali con la query appropriato.</span><span class="sxs-lookup"><span data-stu-id="74056-157">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="74056-158">Eseguire la pagina e fare clic su di **cognome** e **data di registrazione** funziona intestazioni di colonna per verificare che l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="74056-158">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Pagina di indice di studenti nell'ordine dei nomi](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="74056-160">Aggiungere una casella di ricerca alla pagina di indice di studenti</span><span class="sxs-lookup"><span data-stu-id="74056-160">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="74056-161">Per aggiungere filtri alla pagina di indice di studenti, aggiungerai un pulsante di invio e di una casella di testo per la visualizzazione e apportare le modifiche corrispondenti nel `Index` metodo.</span><span class="sxs-lookup"><span data-stu-id="74056-161">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="74056-162">La casella di testo consente di immettere una stringa da cercare nel nome e cognome.</span><span class="sxs-lookup"><span data-stu-id="74056-162">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="74056-163">Aggiungere funzionalità di filtro per il metodo di Index</span><span class="sxs-lookup"><span data-stu-id="74056-163">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="74056-164">In *StudentsController.cs*, sostituire il `Index` (metodo) con il codice seguente (sono evidenziate le modifiche).</span><span class="sxs-lookup"><span data-stu-id="74056-164">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

<span data-ttu-id="74056-165">[!code-csharp[Principale](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]</span><span class="sxs-lookup"><span data-stu-id="74056-165">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]</span></span>

<span data-ttu-id="74056-166">È stato aggiunto un `searchString` parametro per il `Index` (metodo).</span><span class="sxs-lookup"><span data-stu-id="74056-166">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="74056-167">Il valore di stringa di ricerca viene ricevuto da una casella di testo che verrà aggiunto per la visualizzazione dell'indice.</span><span class="sxs-lookup"><span data-stu-id="74056-167">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="74056-168">Inoltre aggiunti all'istruzione LINQ una clausola where clausola che seleziona solo gli studenti il cui nome o cognome contengono la stringa di ricerca.</span><span class="sxs-lookup"><span data-stu-id="74056-168">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="74056-169">L'istruzione che aggiunge where clausola viene eseguita solo se è presente un valore per la ricerca.</span><span class="sxs-lookup"><span data-stu-id="74056-169">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="74056-170">Qui si chiama il `Where` metodo su un `IQueryable` oggetto e il filtro verrà elaborato nel server.</span><span class="sxs-lookup"><span data-stu-id="74056-170">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="74056-171">In alcuni scenari potrebbe essere chiamata la `Where` metodo come metodo di estensione per una raccolta in memoria.</span><span class="sxs-lookup"><span data-stu-id="74056-171">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="74056-172">(Ad esempio, si supponga che si modifica il riferimento a `_context.Students` in modo che anziché un EF `DbSet` fa riferimento a un metodo di repository che restituisce un `IEnumerable` insieme.) Il risultato sarebbe normalmente la stessa, ma in alcuni casi potrebbe essere diverso.</span><span class="sxs-lookup"><span data-stu-id="74056-172">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="74056-173">Ad esempio, l'implementazione di .NET Framework del `Contains` metodo esegue un confronto tra maiuscole e minuscole per impostazione predefinita, ma in SQL Server tale autorizzazione è determinata dall'impostazione delle regole di confronto dell'istanza di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="74056-173">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="74056-174">Tale impostazione predefinita a tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="74056-174">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="74056-175">È possibile chiamare il `ToUpper` metodo per eseguire il test in modo esplicito tra maiuscole e minuscole: *in (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="74056-175">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="74056-176">Assicurarsi che che risultati rimangono invariati se si modifica il codice in un secondo momento per usare un repository che restituisce un `IEnumerable` raccolta invece di un `IQueryable` oggetto.</span><span class="sxs-lookup"><span data-stu-id="74056-176">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="74056-177">(Quando si chiama il `Contains` metodo su un `IEnumerable` raccolta, si ottiene l'implementazione di .NET Framework; quando si chiama su un `IQueryable` dell'oggetto, si ottiene l'implementazione del provider di database.) È tuttavia una riduzione delle prestazioni per questa soluzione.</span><span class="sxs-lookup"><span data-stu-id="74056-177">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there is a performance penalty for this solution.</span></span> <span data-ttu-id="74056-178">Il `ToUpper` codice dovrà inserire una funzione nella clausola WHERE dell'istruzione SELECT TSQL.</span><span class="sxs-lookup"><span data-stu-id="74056-178">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="74056-179">Utilizzo di un indice che impedirebbe l'utilità di ottimizzazione.</span><span class="sxs-lookup"><span data-stu-id="74056-179">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="74056-180">Dato che SQL viene installato per lo più come tra maiuscole e minuscole, è consigliabile evitare di `ToUpper` codice fino a quando non si esegue la migrazione a un archivio dati tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="74056-180">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="74056-181">Aggiungere una casella di ricerca per la visualizzazione dell'indice per studenti</span><span class="sxs-lookup"><span data-stu-id="74056-181">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="74056-182">In *Views/Student/Index.cshtml*, aggiungere il codice evidenziato immediatamente prima dell'apertura tag di tabella per creare una didascalia, una casella di testo e un **ricerca** pulsante.</span><span class="sxs-lookup"><span data-stu-id="74056-182">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="74056-183">Questo codice Usa il `<form>` [helper di tag](https://docs.asp.net/en/latest/mvc/views/tag-helpers/intro.html) per aggiungere la casella di testo di ricerca e un pulsante.</span><span class="sxs-lookup"><span data-stu-id="74056-183">This code uses the `<form>` [tag helper](https://docs.asp.net/en/latest/mvc/views/tag-helpers/intro.html) to add the search text box and button.</span></span> <span data-ttu-id="74056-184">Per impostazione predefinita, il `<form>` helper di tag invia dati con un POST, il che significa che i parametri vengono passati nel corpo del messaggio HTTP e non nell'URL come stringhe di query.</span><span class="sxs-lookup"><span data-stu-id="74056-184">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="74056-185">Quando si specifica di HTTP GET, i dati del form viene passati nell'URL come stringhe di query, che consente agli utenti di segnalibro l'URL.</span><span class="sxs-lookup"><span data-stu-id="74056-185">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="74056-186">Ricevi preferibile W3C linee guida che è necessario utilizzare l'azione non comporta un aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="74056-186">The W3C guidelines recommend that you should use GET when the action does not result in an update.</span></span>

<span data-ttu-id="74056-187">Eseguire la pagina, immettere una stringa di ricerca e fare clic su Cerca per verificare che il filtro sia funzionante.</span><span class="sxs-lookup"><span data-stu-id="74056-187">Run the page, enter a search string, and click Search to verify that filtering is working.</span></span>

![Pagina di indice di studenti di filtro](sort-filter-page/_static/filtering.png)

<span data-ttu-id="74056-189">Si noti che l'URL contiene la stringa di ricerca.</span><span class="sxs-lookup"><span data-stu-id="74056-189">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="74056-190">Se questa pagina, quando si utilizza il segnalibro viene visualizzato l'elenco filtrato.</span><span class="sxs-lookup"><span data-stu-id="74056-190">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="74056-191">Aggiunta di `method="get"` per il `form` tag è ciò che ha determinato la stringa di query da generare.</span><span class="sxs-lookup"><span data-stu-id="74056-191">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="74056-192">In questa fase, se si fa clic su un collegamento di ordinamento di intestazione di colonna si perderanno il valore del filtro immesso nel **ricerca** casella.</span><span class="sxs-lookup"><span data-stu-id="74056-192">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="74056-193">Che potrà essere corretto nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="74056-193">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="74056-194">Aggiungere funzionalità di paging alla pagina di indice di studenti</span><span class="sxs-lookup"><span data-stu-id="74056-194">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="74056-195">Per aggiungere il paging per la pagina di indice di studenti, si creerà un `PaginatedList` classe che utilizza `Skip` e `Take` istruzioni per filtrare i dati sul server invece di recuperare sempre tutte le righe della tabella.</span><span class="sxs-lookup"><span data-stu-id="74056-195">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="74056-196">Quindi è possibile apportare ulteriori modifiche nel `Index` (metodo) e aggiungere i pulsanti di spostamento per la `Index` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="74056-196">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="74056-197">Nella figura seguente mostra i pulsanti di spostamento.</span><span class="sxs-lookup"><span data-stu-id="74056-197">The following illustration shows the paging buttons.</span></span>

![Gli studenti indice pagina con collegamenti di spostamento](sort-filter-page/_static/paging.png)

<span data-ttu-id="74056-199">Nella cartella del progetto, creare `PaginatedList.cs`, quindi sostituire il codice del modello con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="74056-199">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

<span data-ttu-id="74056-200">[!code-csharp[Principale](intro/samples/cu/PaginatedList.cs)]</span><span class="sxs-lookup"><span data-stu-id="74056-200">[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]</span></span>

<span data-ttu-id="74056-201">Il `CreateAsync` metodo in questo codice ha dimensioni di pagina e il numero di pagina e applica appropriata `Skip` e `Take` istruzioni per la `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="74056-201">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="74056-202">Quando `ToListAsync` viene chiamato sul `IQueryable`, verrà restituito un elenco contenente solo la pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="74056-202">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="74056-203">Le proprietà `HasPreviousPage` e `HasNextPage` può essere utilizzato per abilitare o disabilitare **precedente** e **Avanti** pulsanti di spostamento.</span><span class="sxs-lookup"><span data-stu-id="74056-203">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="74056-204">Oggetto `CreateAsync` metodo viene utilizzato invece di un costruttore per creare il `PaginatedList<T>` dell'oggetto poiché i costruttori non possono eseguire codice asincrono.</span><span class="sxs-lookup"><span data-stu-id="74056-204">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="74056-205">Aggiungere funzionalità di paging per il metodo di Index</span><span class="sxs-lookup"><span data-stu-id="74056-205">Add paging functionality to the Index method</span></span>

<span data-ttu-id="74056-206">In *StudentsController.cs*, sostituire il `Index` (metodo) con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="74056-206">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

<span data-ttu-id="74056-207">[!code-csharp[Principale](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]</span><span class="sxs-lookup"><span data-stu-id="74056-207">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]</span></span>

<span data-ttu-id="74056-208">Questo codice aggiunge un parametro di numeri di pagina, un parametro di ordine di ordinamento corrente e un parametro di filtro corrente per la firma del metodo.</span><span class="sxs-lookup"><span data-stu-id="74056-208">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="74056-209">La prima volta che viene visualizzata la pagina o se l'utente non è stato selezionato un paging o ordinamento di collegamento, tutti i parametri è null.</span><span class="sxs-lookup"><span data-stu-id="74056-209">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="74056-210">Se si seleziona un collegamento di paging, la variabile di pagina conterrà il numero di pagina da visualizzare.</span><span class="sxs-lookup"><span data-stu-id="74056-210">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="74056-211">Il `ViewData` elemento denominato CurrentSort fornisce la visualizzazione con ordinamento corrente, in quanto ciò deve essere inclusi i collegamenti di spostamento per mantenere la stessa durante il paging dell'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="74056-211">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="74056-212">Il `ViewData` elemento denominato CurrentFilter fornisce la visualizzazione con la stringa di filtro corrente.</span><span class="sxs-lookup"><span data-stu-id="74056-212">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="74056-213">Questo valore deve essere incluso nei collegamenti di spostamento per mantenere le impostazioni di filtro durante il paging, e devono essere ripristinato nella casella di testo quando la pagina viene visualizzata.</span><span class="sxs-lookup"><span data-stu-id="74056-213">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="74056-214">Se la stringa di ricerca viene modificata durante il paging, la pagina deve essere reimpostato su 1, in quanto può comportare dati diversi per visualizzare il nuovo filtro.</span><span class="sxs-lookup"><span data-stu-id="74056-214">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="74056-215">La stringa di ricerca viene modificata quando viene immesso un valore nella casella di testo e viene premuto il pulsante Invia.</span><span class="sxs-lookup"><span data-stu-id="74056-215">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="74056-216">In tal caso, il `searchString` parametro non è null.</span><span class="sxs-lookup"><span data-stu-id="74056-216">In that case, the `searchString` parameter is not null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="74056-217">Alla fine del `Index` (metodo), il `PaginatedList.CreateAsync` metodo converte la query di studenti in una singola pagina degli studenti iscritti a un tipo di insieme che supporta il paging.</span><span class="sxs-lookup"><span data-stu-id="74056-217">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="74056-218">Pagina singola di studenti viene quindi passato alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="74056-218">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="74056-219">Il `PaginatedList.CreateAsync` metodo accetta un numero di pagina.</span><span class="sxs-lookup"><span data-stu-id="74056-219">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="74056-220">Due punti interrogativi rappresenta l'operatore null-coalescing.</span><span class="sxs-lookup"><span data-stu-id="74056-220">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="74056-221">L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. l'espressione `(page ?? 1)` significa restituisca il valore di `page` se ha un valore oppure restituiscono 1 se `page` è null.</span><span class="sxs-lookup"><span data-stu-id="74056-221">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="74056-222">Aggiungere collegamenti di spostamento per la visualizzazione dell'indice per studenti</span><span class="sxs-lookup"><span data-stu-id="74056-222">Add paging links to the Student Index view</span></span>

<span data-ttu-id="74056-223">In *Views/Students/Index.cshtml*, sostituire il codice esistente con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="74056-223">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="74056-224">Le modifiche sono evidenziate.</span><span class="sxs-lookup"><span data-stu-id="74056-224">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="74056-225">Il `@model` istruzione nella parte superiore della pagina specifica che la vista Ottiene ora un `PaginatedList<T>` oggetto anziché un `List<T>` oggetto.</span><span class="sxs-lookup"><span data-stu-id="74056-225">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="74056-226">I collegamenti di intestazione di colonna utilizzano la stringa di query per passare la stringa di ricerca corrente al controller in modo che l'utente può ordinare all'interno dei risultati di filtro:</span><span class="sxs-lookup"><span data-stu-id="74056-226">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="74056-227">Per gli helper di tag vengono visualizzati i pulsanti di paging:</span><span class="sxs-lookup"><span data-stu-id="74056-227">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="74056-228">Eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="74056-228">Run the page.</span></span>

![Gli studenti indice pagina con collegamenti di spostamento](sort-filter-page/_static/paging.png)

<span data-ttu-id="74056-230">Fare clic sui collegamenti di spostamento in diversi tipi di ordinamento per assicurarsi che funziona di paging.</span><span class="sxs-lookup"><span data-stu-id="74056-230">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="74056-231">Quindi immettere una stringa di ricerca e tentare di paging per verificare che il paging funziona inoltre correttamente con ordinamento e filtro.</span><span class="sxs-lookup"><span data-stu-id="74056-231">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="74056-232">Creare una pagina di informazioni che vengono visualizzate le statistiche di Student</span><span class="sxs-lookup"><span data-stu-id="74056-232">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="74056-233">Per il sito Web Contoso University **su** pagina verranno visualizzati il numero di studenti sono registrati per ciascuna data di registrazione.</span><span class="sxs-lookup"><span data-stu-id="74056-233">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="74056-234">Questa operazione richiede i calcoli di raggruppamento e semplici ai gruppi.</span><span class="sxs-lookup"><span data-stu-id="74056-234">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="74056-235">A tale scopo, verranno eseguite le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="74056-235">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="74056-236">Creare una classe di modello di visualizzazione per i dati che è necessario passare alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="74056-236">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="74056-237">Modificare il metodo di informazioni del controller Home.</span><span class="sxs-lookup"><span data-stu-id="74056-237">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="74056-238">Modificare la visualizzazione di informazioni.</span><span class="sxs-lookup"><span data-stu-id="74056-238">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="74056-239">Creare il modello di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="74056-239">Create the view model</span></span>

<span data-ttu-id="74056-240">Creare un *SchoolViewModels* cartella la *modelli* cartella.</span><span class="sxs-lookup"><span data-stu-id="74056-240">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="74056-241">Nella nuova cartella, aggiungere un file di classe *EnrollmentDateGroup.cs* e sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="74056-241">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

<span data-ttu-id="74056-242">[!code-csharp[Principale](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]</span><span class="sxs-lookup"><span data-stu-id="74056-242">[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]</span></span>

### <a name="modify-the-home-controller"></a><span data-ttu-id="74056-243">Modificare il Controller Home</span><span class="sxs-lookup"><span data-stu-id="74056-243">Modify the Home Controller</span></span>

<span data-ttu-id="74056-244">In *HomeController.cs*, aggiungere le seguenti istruzioni using all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="74056-244">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

<span data-ttu-id="74056-245">[!code-csharp[Principale](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]</span><span class="sxs-lookup"><span data-stu-id="74056-245">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]</span></span>

<span data-ttu-id="74056-246">Aggiungere una variabile di classe per il contesto del database immediatamente dopo la parentesi graffa di apertura per la classe e ottenere un'istanza del contesto da ASP.NET Core DI:</span><span class="sxs-lookup"><span data-stu-id="74056-246">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

<span data-ttu-id="74056-247">[!code-csharp[Principale](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]</span><span class="sxs-lookup"><span data-stu-id="74056-247">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]</span></span>

<span data-ttu-id="74056-248">Sostituire il metodo `About` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="74056-248">Replace the `About` method with the following code:</span></span>

<span data-ttu-id="74056-249">[!code-csharp[Principale](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]</span><span class="sxs-lookup"><span data-stu-id="74056-249">[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]</span></span>

<span data-ttu-id="74056-250">Raggruppa le entità di studenti per data di registrazione dell'istruzione LINQ, calcola il numero di entità in ciascun gruppo e archivia i risultati in una raccolta di `EnrollmentDateGroup` consente di visualizzare gli oggetti del modello.</span><span class="sxs-lookup"><span data-stu-id="74056-250">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="74056-251">Nella 1.0 versione di Entity Framework Core, l'intero set di risultati viene restituito al client e il raggruppamento avviene sul client.</span><span class="sxs-lookup"><span data-stu-id="74056-251">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="74056-252">In alcuni scenari si potrebbero creare problemi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="74056-252">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="74056-253">Assicurarsi di testare le prestazioni con volumi di produzione di dati e se necessario, utilizzare SQL non elaborato per eseguire il raggruppamento nel server.</span><span class="sxs-lookup"><span data-stu-id="74056-253">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="74056-254">Per informazioni sull'utilizzo di SQL non elaborati, vedere [dell'ultima esercitazione di questa serie](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="74056-254">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="74056-255">Modificare le informazioni sulla visualizzazione</span><span class="sxs-lookup"><span data-stu-id="74056-255">Modify the About View</span></span>

<span data-ttu-id="74056-256">Sostituire il codice di *Views/Home/About.cshtml* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="74056-256">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="74056-257">Eseguire l'app e fare clic su di **su** collegamento.</span><span class="sxs-lookup"><span data-stu-id="74056-257">Run the app and click the **About** link.</span></span> <span data-ttu-id="74056-258">Il numero di studenti per ogni data di registrazione viene visualizzato in una tabella.</span><span class="sxs-lookup"><span data-stu-id="74056-258">The count of students for each enrollment date is displayed in a table.</span></span>

![Informazioni sulla pagina](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="74056-260">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="74056-260">Summary</span></span>

<span data-ttu-id="74056-261">In questa esercitazione è stato spiegato come eseguire l'ordinamento, filtro, paging e raggruppamento.</span><span class="sxs-lookup"><span data-stu-id="74056-261">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="74056-262">Nella prossima esercitazione, si apprenderà come gestire le modifiche al modello di dati utilizzando le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="74056-262">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="74056-263">[Precedente](crud.md)
[Successivo](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="74056-263">[Previous](crud.md)
[Next](migrations.md)</span></span>  