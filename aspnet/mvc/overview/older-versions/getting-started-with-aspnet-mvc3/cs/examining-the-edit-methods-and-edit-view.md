---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
title: Esaminare i metodi di modifica e visualizzazione di modifica (c#) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 1d266bf0-a61e-423b-a3d2-13773d7dafe2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 581138bb25b95ef9002a2bd97d1fa28797d96bfa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875812"
---
<a name="examining-the-edit-methods-and-edit-view-c"></a>Esaminare i metodi di modifica e visualizzazione di modifica (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013. È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.
> 
> 
> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con il codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare il [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.


In questa sezione, si esamina i metodi di azione generato e viste per il controller di film. Quindi aggiungere una pagina di ricerca personalizzato.

Eseguire l'applicazione e individuare il `Movies` controller aggiungendo */Movies* per l'URL nella barra degli indirizzi del browser. Posiziona il puntatore del mouse su un **modifica** collegamento per visualizzare l'URL a essa collegate.

[![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image2.png)](examining-the-edit-methods-and-edit-view/_static/image1.png)

Il **modifica** collegamento è stato generato dal `Html.ActionLink` metodo il *Views\Movies\Index.cshtml* Vista:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cshtml)]

[![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image4.png)](examining-the-edit-methods-and-edit-view/_static/image3.png)

Il `Html` oggetto è un helper che viene esposta utilizzando una proprietà sulla `WebViewPage` classe di base. Il `ActionLink` metodo dell'helper semplifica la generazione dinamica di collegamenti ipertestuali HTML che si collegano a metodi di azione nel controller. Il primo argomento per il `ActionLink` metodo è il testo del collegamento per eseguire il rendering (ad esempio, `<a>Edit Me</a>`). Il secondo argomento è il nome del metodo di azione da richiamare. L'argomento finale è un [oggetto anonimo](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) che genera i dati della route (in questo caso, l'ID di 4).

Il collegamento generato illustrato nella figura precedente è `http://localhost:xxxxx/Movies/Edit/4`. La route predefinita ha il modello di URL `{controller}/{action}/{id}`. Pertanto, ASP.NET traduce `http://localhost:xxxxx/Movies/Edit/4` in una richiesta per il `Edit` metodo di azione del `Movies` controller con il parametro `ID` uguale a 4.

È inoltre possibile passare parametri di metodo di azione utilizzando una stringa di query. Ad esempio, l'URL `http://localhost:xxxxx/Movies/Edit?ID=4` passa anche il parametro `ID` 4 al `Edit` il metodo di azione del `Movies` controller.

[![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image6.png)](examining-the-edit-methods-and-edit-view/_static/image5.png)

Aprire il `Movies` controller. I due `Edit` metodi di azione vengono mostrati di seguito.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs)]

Si noti che il secondo metodo di azione `Edit` è preceduto dall'attributo `HttpPost`. Questo attributo specifica che eseguono l'overload di `Edit` metodo può essere richiamato solo per le richieste POST. È possibile applicare il `HttpGet` attributo per il primo metodo di modifica, ma che non è necessario perché è il valore predefinito. (Si farà riferimento ai metodi di azione che vengono assegnati in modo implicito il `HttpGet` attributo `HttpGet` metodi.)

Il `HttpGet` `Edit` metodo accetta il parametro ID film, Cerca il film tramite Entity Framework `Find` (metodo) e restituisce il filmato selezionato per la visualizzazione di modifica. Quando il sistema di scaffolding ha creato la vista Edit, ha esaminato la classe `Movie` e il codice creato per eseguire il rendering degli elementi `<label>` e `<input>` per ogni proprietà della classe. Nell'esempio seguente viene illustrata la visualizzazione di modifica che è stata generata:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

Si noti come il modello di visualizzazione è un `@model MvcMovie.Models.Movie` istruzione all'inizio del file: Specifica che la vista prevede che il modello per il modello di visualizzazione di tipo `Movie`.

Il codice di scaffolding vengono utilizzati diversi *metodi helper* per semplificare il markup HTML. Il [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) helper Visualizza il nome del campo ("Title", "ReleaseDate", "Genre" o "Price"). Il [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) helper Visualizza HTML `<input>` elemento. Il [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) helper Visualizza gli eventuali messaggi di convalida associati alla proprietà.

Eseguire l'applicazione e passare il */Movies* URL. Fare clic su un collegamento **Edit** (Modifica). Nel browser visualizzare l'origine per la pagina. Il codice HTML della pagina è simile alla seguente. (Il markup di menu è stata esclusa per maggiore chiarezza).

[!code-html[Main](examining-the-edit-methods-and-edit-view/samples/sample4.html)]

Il `<input>` nell'HTML sono elementi `<form>` elemento il cui `action` attributo è impostato su post per il *filmati/modifica* URL. I dati verranno inviati al server quando il **modifica** si fa clic sul pulsante.

## <a name="processing-the-post-request"></a>Elaborazione della richiesta POST

Nell'elenco seguente viene indicata la versione `HttpPost` del metodo di azione `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs)]

Il raccoglitore di modelli di framework ASP.NET accetta i valori del form inseriti e crea un `Movie` oggetto passato come il `movie` parametro. Il `ModelState.IsValid` controllo nel codice per verificare i dati inviati nel formato possono essere utilizzati per modificare un `Movie` oggetto. Se i dati non validi, il codice salva i dati dei film per il `Movies` insieme il `MovieDBContext` istanza. Il codice quindi Salva i nuovi dati film nel database chiamando il `SaveChanges` metodo `MovieDBContext`, che mantiene le modifiche al database. Dopo aver salvato i dati, il codice reindirizza l'utente per il `Index` metodo di azione della `MoviesController` classe, che fa sì che il film aggiornato da visualizzare nell'elenco di film.

Se i valori registrati non sono validi, essi verranno nuovamente visualizzati nel form. Il `Html.ValidationMessageFor` helper nel *Edit.cshtml* vista modello si occuperà di visualizzazione dei messaggi di errore appropriato.

[![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image8.png)](examining-the-edit-methods-and-edit-view/_static/image7.png)

> **Nota sulle impostazioni locali** se normalmente si lavora con una lingua diversa dall'inglese, è possibile vedere [supporto ASP.NET MVC 3 convalida con le impostazioni locali Non in lingua inglese.](https://msdn.microsoft.com/library/gg674880(VS.98).aspx)


## <a name="making-the-edit-method-more-robust"></a>Rendere più affidabile il metodo di modifica

Il `HttpGet` `Edit` metodo generato dal sistema scaffolding non verifica che l'ID che viene passato al metodo è valido. Se un utente rimuove il segmento ID dall'URL (`http://localhost:xxxxx/Movies/Edit`), viene visualizzato l'errore seguente:

[![Null_ID](examining-the-edit-methods-and-edit-view/_static/image10.png)](examining-the-edit-methods-and-edit-view/_static/image9.png)

Un utente può inoltre passare un ID che non esiste nel database, ad esempio `http://localhost:xxxxx/Movies/Edit/1234`. È possibile apportare due modifiche per il `HttpGet` `Edit` il metodo di azione per risolvere questa limitazione. In primo luogo, modificare il `ID` parametro venga specificato un valore predefinito pari a zero quando un ID non è passato in modo esplicito. È inoltre possibile verificare che il `Find` metodo trovato effettivamente un filmato prima di restituire l'oggetto filmato il modello di visualizzazione. L'aggiornamento `Edit` metodo è illustrato di seguito.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cs)]

Se non viene trovato alcun film, il `HttpNotFound` metodo viene chiamato.

Tutti i `HttpGet` metodi seguono un modello simile. Ricevono un oggetto filmato (o un elenco di oggetti, nel caso di `Index`) e passare alla visualizzazione del modello. Il `Create` metodo passa un oggetto film vuoto per la visualizzazione di creazione. Tutti i metodi che creano, modificano, eliminano o cambiano in altro modo i dati, eseguono questa operazione nell'overload `HttpPost` del metodo. Modifica dei dati in un metodo GET HTTP è un rischio per la sicurezza, come descritto nella voce del post di blog [ASP.NET MVC suggerimento #46 – non utilizzare eliminare collegamenti poiché creano problemi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modifica dei dati in un metodo GET viene violato anche le procedure consigliate HTTP e il modello architetturale REST, che specifica che le richieste GET non dovrebbero modificare lo stato dell'applicazione. In altre parole, l'esecuzione di un'operazione GET deve essere un'operazione sicura che non ha effetti collaterali.

## <a name="adding-a-search-method-and-search-view"></a>Aggiunta di un metodo di ricerca e visualizzazione di ricerca

In questa sezione si aggiungerà un `SearchIndex` metodo di azione che consente di eseguire la ricerca filmati genre o nome. Questo sarà disponibile tramite il *filmati/SearchIndex* URL. La richiesta verrà visualizzato un form HTML che contiene gli elementi di input che è possibile compilare un utente per eseguire la ricerca di un film. Quando un utente invia il form, il metodo di azione otterrà i valori di ricerca registrati dall'utente e utilizzare i valori per eseguire ricerche nel database.

## <a name="displaying-the-searchindex-form"></a>Visualizzazione di Form SearchIndex

Per iniziare, aggiungere un `SearchIndex` il metodo di azione esistente `MoviesController` classe. Il metodo restituisce una visualizzazione contenente un form HTML. Ecco il codice:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cs)]

La prima riga del `SearchIndex` metodo vengono creati i seguenti [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) query per selezionare i film:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cs)]

La query viene definita a questo punto, ma non è ancora stata eseguita sull'archivio dati.

Se il `searchString` parametro contiene una stringa, viene modificata la query di filmati per filtrare il valore della stringa di ricerca, tramite il codice seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Le query LINQ non vengono eseguite quando vengono definite o quando vengono modificati chiamando un metodo, ad esempio `Where` o `OrderBy`. In alternativa, esecuzione della query è rinviata, il che significa che la valutazione di un'espressione viene ritardata fino a quando il relativo valore realizzato viene effettivamente eseguita un'iterazione o [ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx) metodo viene chiamato. Nel `SearchIndex` esempio, viene eseguita la query nella vista SearchIndex. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](https://msdn.microsoft.com/library/bb738633.aspx).

Ora è possibile implementare il `SearchIndex` visualizzazione che consente di visualizzare il form per l'utente. Fare doppio clic all'interno di `SearchIndex` (metodo) e quindi fare clic su **Aggiungi visualizzazione**. Nel **Aggiungi visualizzazione** finestra di dialogo, specificare che si desidera passare un `Movie` oggetto per il modello di visualizzazione della relativa classe di modello. Nel **modello di scaffolding** scegliere **elenco**, quindi fare clic su **Aggiungi**.

[![AddSearchView](examining-the-edit-methods-and-edit-view/_static/image12.png)](examining-the-edit-methods-and-edit-view/_static/image11.png)

Quando si fa clic il **Aggiungi** pulsante, il *Views\Movies\SearchIndex.cshtml* viene creato il modello di visualizzazione. Perché è stato selezionato **elenco** nel **modello di scaffolding** elencare, Visual Web Developer generato automaticamente (scaffolding) del contenuto predefinito nella visualizzazione. Lo scaffolding creato un form HTML. Viene esaminato il `Movie` classe e codice creato per il rendering `<label>` gli elementi per ogni proprietà della classe. L'elenco di seguito viene illustrata la visualizzazione di creazione che è stata generata:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Eseguire l'applicazione e passare a *filmati/SearchIndex*. Accodare una stringa di query, ad esempio `?searchString=ghost`, all'URL. Vengono visualizzati i film filtrati.

[![SearchQryStr](examining-the-edit-methods-and-edit-view/_static/image14.png)](examining-the-edit-methods-and-edit-view/_static/image13.png)

Se si modifica la firma del `SearchIndex` metodo contiene un parametro denominato `id`, `id` parametro corrisponderà il `{id}` indirizza i segnaposto per il valore predefinito impostato nel *Global. asax* file.

[!code-json[Main](examining-the-edit-methods-and-edit-view/samples/sample11.json)]

Modificato `SearchIndex` metodo si presenterebbe come segue:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample12.cs)]

È ora possibile passare il titolo della ricerca come dati di route (un segmento di URL), anziché come valore della stringa di query.

[![SearchRouteData](examining-the-edit-methods-and-edit-view/_static/image16.png)](examining-the-edit-methods-and-edit-view/_static/image15.png)

Tuttavia, non è possibile supporre che gli utenti modifichino l'URL ogni volta che desiderano cercare un film. Quindi, ora è aggiungerai dell'interfaccia utente per consentire loro filtrare film. Se è stata modificata la firma del `SearchIndex` metodo per verificare come passare il parametro ID di associazione di route, modificarlo in modo che il `SearchIndex` metodo accetta un parametro di stringa denominato `searchString`:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample13.cs)]

Aprire il *Views\Movies\SearchIndex.cshtml* file e subito dopo `@Html.ActionLink("Create New", "Create")`, aggiungere quanto segue:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample14.cshtml)]

Nell'esempio seguente viene mostrata una parte di *Views\Movies\SearchIndex.cshtml* file con il markup filtro aggiunto.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample15.cshtml)]

Il `Html.BeginForm` helper crea un'apertura `<form>` tag. Il `Html.BeginForm` helper, il form inviare a se stessa quando l'utente invia il form scegliendo il **filtro** pulsante.

Eseguire l'applicazione e provare a cercare un filmato.

[![SearchIndxIE9_title](examining-the-edit-methods-and-edit-view/_static/image18.png)](examining-the-edit-methods-and-edit-view/_static/image17.png)

È presente alcun `HttpPost` overload di `SearchIndex` metodo. Non è necessaria, poiché il metodo non modifica lo stato dell'applicazione, solo il filtraggio dei dati.

È possibile aggiungere il metodo `HttpPost SearchIndex` seguente. In tal caso, corrisponderà a invoker dell'azione di `HttpPost SearchIndex` (metodo) e `HttpPost SearchIndex` metodo verrebbe eseguito come illustrato nell'immagine seguente.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample16.cs)]

[![SearchPostGhost](examining-the-edit-methods-and-edit-view/_static/image20.png)](examining-the-edit-methods-and-edit-view/_static/image19.png)

Tuttavia, anche se si aggiunge questa versione `HttpPost` del metodo `SearchIndex`, esiste una limitazione sul modo sulla relativa implementazione. Si supponga che si desideri usare come segnalibro una ricerca specifica o inviare un collegamento agli amici su cui possono fare clic per visualizzare lo stesso elenco filtrato di film. Si noti che l'URL per la richiesta HTTP POST è lo stesso come URL per la richiesta di recupero (localhost:xxxxx filmati/SearchIndex) - Nessuna informazione di ricerca nell'URL stesso. Diritto a questo punto, le informazioni sulla stringa di ricerca viene inviati al server come un valore del campo modulo. Ciò significa che non è possibile acquisire le informazioni di ricerca per aggiungere un segnalibro o inviare agli amici in un URL.

La soluzione consiste nell'usare un overload di `BeginForm` che specifica che la richiesta POST è necessario aggiungere le informazioni di ricerca per l'URL e che devono essere indirizzati alla versione di HttpGet il `SearchIndex` (metodo). Sostituire le funzioni senza parametri `BeginForm` metodo con le operazioni seguenti:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample17.cshtml)]

[![BeginFormPost_SM](examining-the-edit-methods-and-edit-view/_static/image22.png)](examining-the-edit-methods-and-edit-view/_static/image21.png)

A questo punto quando si invia una ricerca, l'URL contiene una stringa di query di ricerca. La ricerca passerà anche al metodo di azione `HttpGet SearchIndex`, anche se si dispone di un metodo `HttpPost SearchIndex`.

[![SearchIndexWithGetURL](examining-the-edit-methods-and-edit-view/_static/image24.png)](examining-the-edit-methods-and-edit-view/_static/image23.png)

## <a name="adding-search-by-genre"></a>Aggiunta di ricerca per genere

Se è stato aggiunto il `HttpPost` versione il `SearchIndex` metodo elimina ora.

Successivamente, aggiungere una funzionalità per consentire agli utenti di cercare film per genere. Sostituire il metodo `SearchIndex` con il codice seguente:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample18.cs)]

Questa versione di `SearchIndex` metodo accetta un parametro aggiuntivo, vale a dire `movieGenre`. Creano le prime righe di codice un `List` oggetto che contenga generi film dal database.

Il codice seguente è una query LINQ che recupera tutti i generi dal database.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample19.cs)]

Il codice Usa il `AddRange` metodo del tipo generico `List` raccolta da aggiungere all'elenco di tutti i generi distinti. (Senza il `Distinct` modificatore, generi duplicati verrebbero aggiunto, ad esempio, verrebbero aggiunto comici due volte nel nostro esempio). Il codice quindi archivia l'elenco di generi nel `ViewBag` oggetto.

Il codice seguente viene illustrato come controllare il `movieGenre` parametro. Se non è vuota ulteriormente il codice vincola la query di filmati per limitare i film selezionati per il genere specificato.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample20.cs)]

## <a name="adding-markup-to-the-searchindex-view-to-support-search-by-genre"></a>Aggiungere tag alla visualizzazione SearchIndex per supportare la ricerca per genere

Aggiungere un `Html.DropDownList` helper per il *Views\Movies\SearchIndex.cshtml* file, appena prima di `TextBox` helper. Di seguito è riportato il markup completato:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample21.cshtml)]

Eseguire l'applicazione e passare a *filmati/SearchIndex*. Eseguire una ricerca per genere, in base al nome di film e da entrambi i criteri.

In questa sezione sono stati esaminati i metodi di azione CRUD e visualizzazioni generate dal framework. È stato creato un metodo di azione di ricerca e visualizzazione che consentono agli utenti di ricerca in base al genere e titolo del film. Nella sezione successiva verrà esaminato come aggiungere una proprietà per il `Movie` modello e come aggiungere un inizializzatore che verrà creato automaticamente un database di test.

> [!div class="step-by-step"]
> [Precedente](accessing-your-models-data-from-a-controller.md)
> [Successivo](adding-a-new-field.md)
