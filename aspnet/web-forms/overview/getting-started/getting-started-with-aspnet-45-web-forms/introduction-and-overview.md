---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Guida introduttiva a 4.5 Web Form ASP.NET e Visual Studio 2013 | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni dettagliata verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 572b263a5f968b473457771a1dd4075910218c01
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="efd9c-103">Guida introduttiva a 4.5 Web Form ASP.NET e Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="efd9c-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="efd9c-104">da [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="efd9c-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="efd9c-105">[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="efd9c-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="efd9c-106">Questa serie di esercitazioni dettagliata verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web.</span><span class="sxs-lookup"><span data-stu-id="efd9c-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="efd9c-107">Web Form ASP.NET Quiz</span><span class="sxs-lookup"><span data-stu-id="efd9c-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="efd9c-108">Testare la conoscenza e rafforzare i concetti chiave eseguendo il Quiz di ASP.NET Web Form.</span><span class="sxs-lookup"><span data-stu-id="efd9c-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="efd9c-109">Questo quiz è stato progettato in modo specifico dal contenuto di questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="efd9c-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="efd9c-110">Ogni domanda il quiz viene fornita una spiegazione vengono forniti collegamenti a informazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="efd9c-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="efd9c-111">Introduzione</span><span class="sxs-lookup"><span data-stu-id="efd9c-111">Introduction</span></span>

<span data-ttu-id="efd9c-112">Questa serie di esercitazioni in modo semplificato i passaggi necessari per creare un'applicazione Web Form ASP.NET utilizzando Visual Studio Express 2013 per Web e ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="efd9c-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="efd9c-113">L'applicazione che creerai denominato **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="efd9c-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="efd9c-114">È un esempio semplificato di un sito web di front-archivio che vende articoli online.</span><span class="sxs-lookup"><span data-stu-id="efd9c-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="efd9c-115">Questa serie di esercitazioni evidenzia nuove caratteristiche disponibili in ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="efd9c-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="efd9c-116">I commenti sono iniziale e ci accerteremo ogni sforzo per aggiornare questa serie di esercitazioni in base a suggerimenti.</span><span class="sxs-lookup"><span data-stu-id="efd9c-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="efd9c-117">Download completato progetto</span><span class="sxs-lookup"><span data-stu-id="efd9c-117">Download completed project</span></span>

<span data-ttu-id="efd9c-118">È possibile scaricare un progetto c# che contiene l'esercitazione completata.</span><span class="sxs-lookup"><span data-stu-id="efd9c-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="efd9c-119">[Guida introduttiva a 4.5 Web Form ASP.NET e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="efd9c-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="efd9c-120">Esaminare il contenuto effettuando il quiz di Web Form ASP.NET correlato</span><span class="sxs-lookup"><span data-stu-id="efd9c-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="efd9c-121">Dopo aver completato questa esercitazione, testare la conoscenza e rafforzare i concetti chiave eseguendo il [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="efd9c-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="efd9c-122">Questo quiz è stato progettato in modo specifico dal contenuto di questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="efd9c-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="efd9c-123">Ogni domanda il quiz viene fornita una spiegazione vengono forniti collegamenti a informazioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="efd9c-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="efd9c-124">Web Form ASP.NET Quiz</span><span class="sxs-lookup"><span data-stu-id="efd9c-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="efd9c-125">Destinatari</span><span class="sxs-lookup"><span data-stu-id="efd9c-125">Audience</span></span>

<span data-ttu-id="efd9c-126">Il gruppo di destinatari di questa serie di esercitazioni è sviluppatori esperti nuovi utenti di Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="efd9c-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="efd9c-127">Gli sviluppatori interessati a questa serie di esercitazioni devono avere le seguenti aree:</span><span class="sxs-lookup"><span data-stu-id="efd9c-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="efd9c-128">Che hanno familiarità con un oggetto orientata ai servizi di linguaggio di programmazione (OOP)</span><span class="sxs-lookup"><span data-stu-id="efd9c-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="efd9c-129">Familiarità con concetti di sviluppo Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="efd9c-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="efd9c-130">Familiarità con i concetti di database relazionale</span><span class="sxs-lookup"><span data-stu-id="efd9c-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="efd9c-131">Familiarità con concetti dell'architettura a più livelli</span><span class="sxs-lookup"><span data-stu-id="efd9c-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="efd9c-132">Se si desidera esaminare le aree elencate in precedenza, è consigliabile rivedere il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="efd9c-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="efd9c-133">Introduzione a Visual c#</span><span class="sxs-lookup"><span data-stu-id="efd9c-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="efd9c-134">[Sviluppo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="efd9c-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="efd9c-135">Database relazionale</span><span class="sxs-lookup"><span data-stu-id="efd9c-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="efd9c-136">Architettura a più livelli</span><span class="sxs-lookup"><span data-stu-id="efd9c-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="efd9c-137">Funzionalità dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="efd9c-137">Application Features</span></span>

<span data-ttu-id="efd9c-138">Le funzionalità di Web Form ASP.NET presentate in questa serie includono:</span><span class="sxs-lookup"><span data-stu-id="efd9c-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="efd9c-139">Il progetto di applicazione Web (non un progetto di sito Web)</span><span class="sxs-lookup"><span data-stu-id="efd9c-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="efd9c-140">Web Form</span><span class="sxs-lookup"><span data-stu-id="efd9c-140">Web Forms</span></span>
- <span data-ttu-id="efd9c-141">Pagine master, configurazione</span><span class="sxs-lookup"><span data-stu-id="efd9c-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="efd9c-142">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="efd9c-142">Bootstrap</span></span>
- <span data-ttu-id="efd9c-143">Entity Framework prima di tutto, codice del database locale</span><span class="sxs-lookup"><span data-stu-id="efd9c-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="efd9c-144">Convalida della richiesta</span><span class="sxs-lookup"><span data-stu-id="efd9c-144">Request Validation</span></span>
- <span data-ttu-id="efd9c-145">Fortemente tipizzato, i controlli di dati del modello di associazione, le annotazioni dei dati e il valore di provider</span><span class="sxs-lookup"><span data-stu-id="efd9c-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="efd9c-146">SSL e OAuth</span><span class="sxs-lookup"><span data-stu-id="efd9c-146">SSL and OAuth</span></span>
- <span data-ttu-id="efd9c-147">ASP.NET Identity, configurazione e l'autorizzazione</span><span class="sxs-lookup"><span data-stu-id="efd9c-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="efd9c-148">Convalida non intrusiva</span><span class="sxs-lookup"><span data-stu-id="efd9c-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="efd9c-149">Routing</span><span class="sxs-lookup"><span data-stu-id="efd9c-149">Routing</span></span>
- <span data-ttu-id="efd9c-150">Gestione degli errori ASP.NET</span><span class="sxs-lookup"><span data-stu-id="efd9c-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="efd9c-151">Attività e gli scenari di applicazione</span><span class="sxs-lookup"><span data-stu-id="efd9c-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="efd9c-152">Le attività illustrate in questa serie includono:</span><span class="sxs-lookup"><span data-stu-id="efd9c-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="efd9c-153">Creazione, la revisione ed esecuzione del nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="efd9c-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="efd9c-154">Creazione della struttura di database</span><span class="sxs-lookup"><span data-stu-id="efd9c-154">Creating the database structure</span></span>
- <span data-ttu-id="efd9c-155">L'inizializzazione e il seeding del database</span><span class="sxs-lookup"><span data-stu-id="efd9c-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="efd9c-156">Personalizzare l'interfaccia utente utilizzando gli stili, grafica e una pagina master</span><span class="sxs-lookup"><span data-stu-id="efd9c-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="efd9c-157">Aggiunta di pagine e navigazione</span><span class="sxs-lookup"><span data-stu-id="efd9c-157">Adding pages and navigation</span></span>
- <span data-ttu-id="efd9c-158">Visualizzazione dei dettagli di menu e i dati di prodotto</span><span class="sxs-lookup"><span data-stu-id="efd9c-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="efd9c-159">Creazione di un carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="efd9c-159">Creating a shopping cart</span></span>
- <span data-ttu-id="efd9c-160">Supporto SSL aggiunta e OAuth</span><span class="sxs-lookup"><span data-stu-id="efd9c-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="efd9c-161">Aggiunta di un metodo di pagamento</span><span class="sxs-lookup"><span data-stu-id="efd9c-161">Adding a payment method</span></span>
- <span data-ttu-id="efd9c-162">Tra cui un ruolo di amministratore e un utente per l'applicazione</span><span class="sxs-lookup"><span data-stu-id="efd9c-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="efd9c-163">Limitare l'accesso a pagine specifiche e cartella</span><span class="sxs-lookup"><span data-stu-id="efd9c-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="efd9c-164">Caricamento di un file all'applicazione web</span><span class="sxs-lookup"><span data-stu-id="efd9c-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="efd9c-165">Implementazione di convalida dell'input</span><span class="sxs-lookup"><span data-stu-id="efd9c-165">Implementing input validation</span></span>
- <span data-ttu-id="efd9c-166">La registrazione delle route per l'applicazione web</span><span class="sxs-lookup"><span data-stu-id="efd9c-166">Registering routes for the web application</span></span>
- <span data-ttu-id="efd9c-167">Implementazione della gestione degli errori e registrazione degli errori</span><span class="sxs-lookup"><span data-stu-id="efd9c-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="efd9c-168">Panoramica</span><span class="sxs-lookup"><span data-stu-id="efd9c-168">Overview</span></span>

<span data-ttu-id="efd9c-169">Se si ha familiarità con i Web Form ASP.NET ma hanno una certa familiarità con i concetti di programmazione, è necessario l'esercitazione di destra.</span><span class="sxs-lookup"><span data-stu-id="efd9c-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="efd9c-170">Se si ha già familiarità con i Web Form ASP.NET, è possibile trarre vantaggio da questa serie di esercitazioni dalle nuove funzionalità disponibili in ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="efd9c-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="efd9c-171">Se non si ha familiarità con concetti e Web Form ASP.NET di programmazione, vedere le esercitazioni aggiuntive fornite in Web Form [Introduzione](../../../index.md) sezione sul sito Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="efd9c-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="efd9c-172">La specifica **più recente** ASP.NET 4.5 funzionalità fornite in questo Web Form serie di esercitazioni includono quanto segue:</span><span class="sxs-lookup"><span data-stu-id="efd9c-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="efd9c-173">Una semplice interfaccia utente per la creazione di progetti dell'offerta [il supporto per più framework ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Form, MVC e Web API).</span><span class="sxs-lookup"><span data-stu-id="efd9c-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="efd9c-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un framework di layout e i temi che fornisce funzionalità di progettazione e i temi reattiva.</span><span class="sxs-lookup"><span data-stu-id="efd9c-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="efd9c-175">[ASP.NET Identity](../../../../identity/index.md), un nuovo sistema di appartenenze ASP.NET che funziona allo stesso in tutti i framework ASP.NET e funziona con software diversi da IIS di hosting web.</span><span class="sxs-lookup"><span data-stu-id="efd9c-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="efd9c-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), gli oggetti di un aggiornamento a Entity Framework che consente di recuperare e manipolare i dati fortemente tipizzati, accedere ai dati in modo asincrono, gestire errori di connessione temporanei e registrare le istruzioni SQL.</span><span class="sxs-lookup"><span data-stu-id="efd9c-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="efd9c-177">Per un elenco completo delle funzionalità ASP.NET 4.5, vedere [ASP.NET e strumenti Web per note sulla versione di Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="efd9c-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="efd9c-178">L'applicazione di esempio Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="efd9c-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="efd9c-179">Le schermate seguenti forniscono una visualizzazione rapida dell'applicazione Web ASP.NET form che si creerà in questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="efd9c-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="efd9c-180">Quando si esegue l'applicazione da Visual Studio Express 2013 per Web, si visualizzeranno la Home page web seguenti.</span><span class="sxs-lookup"><span data-stu-id="efd9c-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - pagina predefinita](introduction-and-overview/_static/image1.png)

<span data-ttu-id="efd9c-182">È possibile registrare un nuovo account utente o accedere come un utente esistente.</span><span class="sxs-lookup"><span data-stu-id="efd9c-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="efd9c-183">Navigazione viene fornita nella parte superiore per ogni categoria di prodotto tramite il recupero dei prodotti disponibili dal database.</span><span class="sxs-lookup"><span data-stu-id="efd9c-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="efd9c-184">Se si seleziona il collegamento di prodotti, sarà in grado di visualizzare un elenco di tutti i prodotti disponibili.</span><span class="sxs-lookup"><span data-stu-id="efd9c-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - prodotti](introduction-and-overview/_static/image2.png)

<span data-ttu-id="efd9c-186">È inoltre possibile visualizzare i dettagli di singoli prodotti selezionando uno dei prodotti elencati.</span><span class="sxs-lookup"><span data-stu-id="efd9c-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - dettagli sul prodotto](introduction-and-overview/_static/image3.png)

<span data-ttu-id="efd9c-188">Come un utente, è possibile registrare e accedere con la funzionalità predefinita del modello Web Form.</span><span class="sxs-lookup"><span data-stu-id="efd9c-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="efd9c-189">In questa esercitazione viene inoltre illustrato come eseguire l'accesso utilizzando un account Gmail esistente.</span><span class="sxs-lookup"><span data-stu-id="efd9c-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="efd9c-190">Inoltre, può eseguire l'accesso come amministratore di aggiungere e rimuovere i prodotti dal database.</span><span class="sxs-lookup"><span data-stu-id="efd9c-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - Log in](introduction-and-overview/_static/image4.png)

<span data-ttu-id="efd9c-192">Una volta che è connessi come un utente, è possibile aggiungere i prodotti per il carrello acquisti e l'estrazione con PayPal.</span><span class="sxs-lookup"><span data-stu-id="efd9c-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="efd9c-193">Si noti che questa applicazione di esempio è progettata per funzionare con sandbox per sviluppatori di PayPal.</span><span class="sxs-lookup"><span data-stu-id="efd9c-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="efd9c-194">Nessuna transazione money effettivo verrà eseguito.</span><span class="sxs-lookup"><span data-stu-id="efd9c-194">No actual money transaction will take place.</span></span>

![Wingtip Toys - carrello degli acquisti](introduction-and-overview/_static/image5.png)

<span data-ttu-id="efd9c-196">L'account, l'ordine e le informazioni di pagamento PayPal confermare.</span><span class="sxs-lookup"><span data-stu-id="efd9c-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="efd9c-198">Dopo la restituzione da PayPal, è possibile esaminare e completare l'ordine.</span><span class="sxs-lookup"><span data-stu-id="efd9c-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - ordine revisione](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="efd9c-200">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="efd9c-200">Prerequisites</span></span>

<span data-ttu-id="efd9c-201">Prima di iniziare, assicurarsi di aver installato nel computer il software seguente:</span><span class="sxs-lookup"><span data-stu-id="efd9c-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="efd9c-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="efd9c-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="efd9c-203">.NET Framework viene installato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="efd9c-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="efd9c-204">Questa serie di esercitazioni utilizza Microsoft Visual Studio Express 2013 per Web.</span><span class="sxs-lookup"><span data-stu-id="efd9c-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="efd9c-205">Per completare questa serie di esercitazioni, è possibile utilizzare Microsoft Visual Studio Express 2013 per il Web o Microsoft Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="efd9c-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="efd9c-206">Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 per Web verrà essere noto anche come Visual Studio in tutta questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="efd9c-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="efd9c-207">Se è già installata una versione di Visual Studio, il processo di installazione installerà Visual Studio 2013 o Microsoft Visual Studio Express 2013 per Web accanto alla versione esistente.</span><span class="sxs-lookup"><span data-stu-id="efd9c-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="efd9c-208">I siti creati nelle versioni precedenti possono essere aperto in Visual Studio 2013 e continuano a aprire nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="efd9c-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="efd9c-209">Questa procedura dettagliata si presuppone che sia selezionato il *lo sviluppo Web* raccolta di impostazioni la prima volta che viene avviato Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="efd9c-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="efd9c-210">Per ulteriori informazioni, vedere [come: Seleziona impostazioni ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="efd9c-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="efd9c-211">Scaricare l'applicazione di esempio</span><span class="sxs-lookup"><span data-stu-id="efd9c-211">Download the Sample Application</span></span>

<span data-ttu-id="efd9c-212">Dopo aver installato i prerequisiti, si è pronti per iniziare a creare il nuovo progetto Web che viene presentato in questa serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="efd9c-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="efd9c-213">Se si desidera **facoltativamente** eseguire l'applicazione di esempio che crea questa serie di esercitazioni, è possibile scaricarlo dal sito di esempi MSDN.</span><span class="sxs-lookup"><span data-stu-id="efd9c-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="efd9c-214">Questo download contiene quanto segue:</span><span class="sxs-lookup"><span data-stu-id="efd9c-214">This download contains the following:</span></span>

- <span data-ttu-id="efd9c-215">L'applicazione di esempio di *WingtipToys* cartella.</span><span class="sxs-lookup"><span data-stu-id="efd9c-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="efd9c-216">Le risorse utilizzate per creare l'applicazione di esempio nella *WingtipToys asset* cartella la *WingtipToys* cartella.</span><span class="sxs-lookup"><span data-stu-id="efd9c-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="efd9c-217">Scaricare il file dal sito di esempi MSDN:</span><span class="sxs-lookup"><span data-stu-id="efd9c-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="efd9c-218">[Guida introduttiva a 4.5 Web Form ASP.NET e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)</span><span class="sxs-lookup"><span data-stu-id="efd9c-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="efd9c-219">Il download è un <em>zip</em> file.</span><span class="sxs-lookup"><span data-stu-id="efd9c-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="efd9c-220">Per visualizzare il progetto completato crea questa serie di esercitazioni, trovare e selezionare il <em>c#</em>cartella la <em>zip</em> file.</span><span class="sxs-lookup"><span data-stu-id="efd9c-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="efd9c-221">Salvare il <em>c#</em> la cartella in cartella consentono di lavorare con progetti di Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="efd9c-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="efd9c-222">Per impostazione predefinita, la cartella di progetti di Visual Studio 2013 è la seguente:</span><span class="sxs-lookup"><span data-stu-id="efd9c-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="efd9c-223"><strong>C:\Users\</ strong ><strong><em>&lt;nomeutente&gt;</em></strong><strong>\Documents\Visual 2013\Projects Studio</strong></span><span class="sxs-lookup"><span data-stu-id="efd9c-223"><strong>C:\Users\</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="efd9c-224">Rinominare il ***c#*** cartella ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="efd9c-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="efd9c-225">Se si dispone già di una cartella denominata *WingtipToys* nella cartella dei progetti, rinominare temporaneamente tale cartella esistente prima di rinominare il *c#* cartella *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="efd9c-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="efd9c-226">Per eseguire il progetto completato, aprire il *WingtipToys* cartella e fare doppio clic il *WingtipToys.sln* file.</span><span class="sxs-lookup"><span data-stu-id="efd9c-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="efd9c-227">Visual Studio 2013 verrà aperto il progetto.</span><span class="sxs-lookup"><span data-stu-id="efd9c-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="efd9c-228">Successivamente, fare doppio clic su di *Default.aspx* file nella finestra Esplora soluzioni e fare clic su Visualizza nel Browser dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="efd9c-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="efd9c-229">I commenti e supporto dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="efd9c-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="efd9c-230">Utilizzare la sezione Domande e risposte, inclusa il [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) esempio (c#) per eventuali domande o commenti.</span><span class="sxs-lookup"><span data-stu-id="efd9c-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="efd9c-231">I commenti in questa serie di esercitazioni sono, e quando viene aggiornata la serie di esercitazioni sarà ogni sforzo per tener conto correzioni o suggerimenti per i miglioramenti forniti nei commenti dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="efd9c-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="efd9c-232">Quando si verifica un errore durante lo sviluppo oppure se il sito Web non viene eseguito correttamente, i messaggi di errore possono fornire indicazioni complesse per l'origine del problema o non potrebbero spiegare come correggerlo.</span><span class="sxs-lookup"><span data-stu-id="efd9c-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="efd9c-233">Per alcuni scenari comuni di problema, è inoltre possibile utilizzare il [forum ASP.NET](https://forums.asp.net/) o nella sezione delle domande e risposte incluso con il [Introduzione a Web Form ASP.NET 4.5 e Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ( C#) esempio.</span><span class="sxs-lookup"><span data-stu-id="efd9c-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="efd9c-234">Se viene visualizzato un messaggio di errore o non funzioni come eseguire le esercitazioni, assicurarsi di controllare nei percorsi indicati.</span><span class="sxs-lookup"><span data-stu-id="efd9c-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="efd9c-235">avanti</span><span class="sxs-lookup"><span data-stu-id="efd9c-235">Next</span></span>](create-the-project.md)
