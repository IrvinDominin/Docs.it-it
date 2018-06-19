---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Aggiunta di un nuovo campo al modello di film e di tabella (c#) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: f6364e438bbb7e128945255a5150e1e84e593ac4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871558"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a><span data-ttu-id="9c300-103">Aggiunta di un nuovo campo al modello di film e di tabella (c#)</span><span class="sxs-lookup"><span data-stu-id="9c300-103">Adding a New Field to the Movie Model and Table (C#)</span></span>
====================
<span data-ttu-id="9c300-104">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9c300-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="9c300-105">È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="9c300-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="9c300-106">È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.</span><span class="sxs-lookup"><span data-stu-id="9c300-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="9c300-107">In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9c300-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="9c300-108">Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="9c300-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="9c300-109">È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="9c300-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="9c300-110">In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="9c300-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="9c300-111">Prerequisiti di Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="9c300-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="9c300-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="9c300-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="9c300-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)</span><span class="sxs-lookup"><span data-stu-id="9c300-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="9c300-114">Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="9c300-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="9c300-115">Un progetto di Visual Web Developer con il codice sorgente c# è disponibile a complemento di questo argomento.</span><span class="sxs-lookup"><span data-stu-id="9c300-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="9c300-116">[Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="9c300-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="9c300-117">Se si preferisce Visual Basic, passare il [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9c300-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="9c300-118">In questa sezione viene apportare alcune modifiche per le classi di modello e informazioni su come è possibile aggiornare lo schema di database affinché corrisponda le modifiche del modello.</span><span class="sxs-lookup"><span data-stu-id="9c300-118">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="9c300-119">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="9c300-119">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="9c300-120">Per iniziare, aggiungere un nuovo `Rating` proprietà esistente `Movie` classe.</span><span class="sxs-lookup"><span data-stu-id="9c300-120">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="9c300-121">Aprire il *Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="9c300-121">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="9c300-122">L'intero `Movie` classe ora ha un aspetto simile al codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9c300-122">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

<span data-ttu-id="9c300-123">Ricompilare l'applicazione utilizzando il **Debug** &gt; **compilare film** comando di menu.</span><span class="sxs-lookup"><span data-stu-id="9c300-123">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="9c300-124">Ora che è stato aggiornato il `Model` (classe), è anche necessario aggiornare il *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* consente di visualizzare i modelli per supportare la nuova `Rating`proprietà.</span><span class="sxs-lookup"><span data-stu-id="9c300-124">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="9c300-125">Aprire il *\Views\Movies\Index.cshtml* file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il **prezzo** colonna.</span><span class="sxs-lookup"><span data-stu-id="9c300-125">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="9c300-126">Aggiungere quindi una `<td>` colonna verso la fine del modello per il rendering di `@item.Rating` valore.</span><span class="sxs-lookup"><span data-stu-id="9c300-126">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="9c300-127">Di seguito è riportato l'aggiornato *cshtml* modello di visualizzazione è simile a:</span><span class="sxs-lookup"><span data-stu-id="9c300-127">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

<span data-ttu-id="9c300-128">Aprire quindi il *\Views\Movies\Create.cshtml* file e aggiungere il markup seguente verso la fine del form.</span><span class="sxs-lookup"><span data-stu-id="9c300-128">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="9c300-129">Si esegue il rendering di una casella di testo in modo che quando viene creato un nuovo filmato, è possibile specificare una classificazione.</span><span class="sxs-lookup"><span data-stu-id="9c300-129">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="9c300-130">Modello di gestione e le differenze dello Schema di Database</span><span class="sxs-lookup"><span data-stu-id="9c300-130">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="9c300-131">Ora è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.</span><span class="sxs-lookup"><span data-stu-id="9c300-131">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="9c300-132">Eseguire l'applicazione e passare al */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="9c300-132">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="9c300-133">Quando si esegue questa operazione, tuttavia, verrà visualizzato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="9c300-133">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="9c300-134">Viene visualizzato questo errore perché l'aggiornamento `Movie` classe del modello nell'applicazione è diverso rispetto allo schema di ora il `Movie` tabella del database esistente.</span><span class="sxs-lookup"><span data-stu-id="9c300-134">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="9c300-135">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="9c300-135">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="9c300-136">Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come in precedenza in questa esercitazione, il primo codice aggiunge una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi di modello da che è stato generato.</span><span class="sxs-lookup"><span data-stu-id="9c300-136">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="9c300-137">Se non sono sincronizzati, Entity Framework genera un errore.</span><span class="sxs-lookup"><span data-stu-id="9c300-137">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="9c300-138">Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbe essere in caso contrario solo (da errori oscuri) in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9c300-138">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="9c300-139">La funzionalità di controllo di sincronizzazione è quello che determina il messaggio di errore da visualizzare appena individuato.</span><span class="sxs-lookup"><span data-stu-id="9c300-139">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="9c300-140">Esistono due approcci per la risoluzione dell'errore:</span><span class="sxs-lookup"><span data-stu-id="9c300-140">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="9c300-141">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="9c300-141">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="9c300-142">Questo approccio è molto utile durante la fase di sviluppo attivo in un database di test, in quanto consente rapidamente evolversi insieme lo schema del modello e il database.</span><span class="sxs-lookup"><span data-stu-id="9c300-142">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="9c300-143">Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo si *non* per usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="9c300-143">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="9c300-144">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="9c300-144">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="9c300-145">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="9c300-145">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="9c300-146">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="9c300-146">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="9c300-147">Per questa esercitazione, verrà utilizzato il primo approccio, è necessario Entity Framework Code First automaticamente ricreare il database ogni volta che viene modificato il modello.</span><span class="sxs-lookup"><span data-stu-id="9c300-147">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="9c300-148">Ricreare il Database nel modello modifiche automaticamente</span><span class="sxs-lookup"><span data-stu-id="9c300-148">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="9c300-149">Di seguito, aggiornare l'applicazione in modo che il primo codice automaticamente eliminato e ricreato il database in qualsiasi momento si modifica il modello per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9c300-149">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9c300-150">**Avviso** è consigliabile abilitare questo approccio di automaticamente eliminando e ricreando il database solo quando si utilizza un database di sviluppo o test, e *mai* in un database di produzione che contiene dati reali.</span><span class="sxs-lookup"><span data-stu-id="9c300-150">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="9c300-151">Utilizzarla in un server di produzione può causare la perdita di dati.</span><span class="sxs-lookup"><span data-stu-id="9c300-151">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="9c300-152">In **Esplora soluzioni**, fare clic destro la *modelli* cartella, selezionare **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="9c300-152">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="9c300-153">Nome della classe "MovieInitializer".</span><span class="sxs-lookup"><span data-stu-id="9c300-153">Name the class "MovieInitializer".</span></span> <span data-ttu-id="9c300-154">Aggiornamento di `MovieInitializer` classe destinata a contenere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9c300-154">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="9c300-155">La `MovieInitializer` classe specifica che il database utilizzato dal modello deve essere eliminato e ricreato automaticamente se le classi modello cambiano.</span><span class="sxs-lookup"><span data-stu-id="9c300-155">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="9c300-156">Il codice include un `Seed` per specificare alcuni dati predefiniti per aggiungere automaticamente al database una volta che ha creato (o ricreato).</span><span class="sxs-lookup"><span data-stu-id="9c300-156">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="9c300-157">Ciò fornisce un modo utile per popolare il database con alcuni dati di esempio, senza che sia necessario compilare manualmente ogni volta che si applica un modello di modifica.</span><span class="sxs-lookup"><span data-stu-id="9c300-157">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="9c300-158">Ora che è stato definito il `MovieInitializer` (classe), sarà necessario associare gli in modo che ogni volta che viene eseguita l'applicazione, controlla se le classi di modello sono diverse dallo schema del database.</span><span class="sxs-lookup"><span data-stu-id="9c300-158">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="9c300-159">In tal caso, è possibile eseguire l'inizializzatore per ricreare il database per la corrispondenza del modello e quindi popolare il database con i dati di esempio.</span><span class="sxs-lookup"><span data-stu-id="9c300-159">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="9c300-160">Aprire il *Global. asax* file che si trova nella radice del `MvcMovies` progetto:</span><span class="sxs-lookup"><span data-stu-id="9c300-160">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

<span data-ttu-id="9c300-161">Il *Global. asax* file contiene la classe che definisce l'intera applicazione per il progetto e contiene un `Application_Start` gestore dell'evento che viene eseguito quando il primo avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9c300-161">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="9c300-162">Aggiungere due utilizzando le istruzioni nella parte superiore del file.</span><span class="sxs-lookup"><span data-stu-id="9c300-162">Let's add two using statements to the top of the file.</span></span> <span data-ttu-id="9c300-163">Il primo fa riferimento lo spazio dei nomi di Entity Framework e il secondo fa riferimento lo spazio dei nomi in cui il nostro `MovieInitializer` classe vita:</span><span class="sxs-lookup"><span data-stu-id="9c300-163">The first references the Entity Framework namespace, and the second references the namespace where our `MovieInitializer` class lives:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

<span data-ttu-id="9c300-164">Individuare il `Application_Start` (metodo) e aggiungere una chiamata a `Database.SetInitializer` all'inizio del metodo, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="9c300-164">Then find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

<span data-ttu-id="9c300-165">Il `Database.SetInitializer` appena aggiunto istruzione indica che il database utilizzando il `MovieDBContext` istanza deve essere automaticamente eliminata e ricreata se lo schema e il database non corrispondono.</span><span class="sxs-lookup"><span data-stu-id="9c300-165">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="9c300-166">E come si è visto, anche popolerà il database con i dati di esempio specificato nella `MovieInitializer` classe.</span><span class="sxs-lookup"><span data-stu-id="9c300-166">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="9c300-167">Chiudi il *Global. asax* file.</span><span class="sxs-lookup"><span data-stu-id="9c300-167">Close the *Global.asax* file.</span></span>

<span data-ttu-id="9c300-168">Eseguire nuovamente l'applicazione e passare il */Movies* URL.</span><span class="sxs-lookup"><span data-stu-id="9c300-168">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="9c300-169">All'avvio dell'applicazione, viene rilevato che la struttura del modello non corrisponde più lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="9c300-169">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="9c300-170">Ricrea il database in modo che corrisponda alla struttura del modello nuovo automaticamente e consente di popolare il database con i film di esempio:</span><span class="sxs-lookup"><span data-stu-id="9c300-170">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

<span data-ttu-id="9c300-172">Fare clic su di **Crea nuovo** collegamento per aggiungere un nuovo film.</span><span class="sxs-lookup"><span data-stu-id="9c300-172">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="9c300-173">Si noti che è possibile aggiungere una classificazione.</span><span class="sxs-lookup"><span data-stu-id="9c300-173">Note that you can add a rating.</span></span>

<span data-ttu-id="9c300-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="9c300-174">[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)</span></span>

<span data-ttu-id="9c300-175">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="9c300-175">Click **Create**.</span></span> <span data-ttu-id="9c300-176">Il nuovo filmato, tra cui la valutazione, viene inserito nei film elenco:</span><span class="sxs-lookup"><span data-stu-id="9c300-176">The new movie, including the rating, now shows up in the movies listing:</span></span>

<span data-ttu-id="9c300-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="9c300-177">[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)</span></span>

<span data-ttu-id="9c300-178">In questa sezione è stato illustrato come è possibile modificare gli oggetti del modello e sincronizzare il database con le modifiche.</span><span class="sxs-lookup"><span data-stu-id="9c300-178">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="9c300-179">È stato inoltre un modo per popolare un database appena creato con dati di esempio in modo è possibile provare gli scenari.</span><span class="sxs-lookup"><span data-stu-id="9c300-179">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="9c300-180">Successivamente, si esaminerà come è possibile aggiungere la logica di convalida più completa per le classi di modello e abilitare alcune regole di business da applicare.</span><span class="sxs-lookup"><span data-stu-id="9c300-180">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c300-181">[Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="9c300-181">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
