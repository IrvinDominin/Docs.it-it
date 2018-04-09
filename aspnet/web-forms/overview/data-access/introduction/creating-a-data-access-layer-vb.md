---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
title: Creazione di un livello di accesso ai dati (VB) | Documenti Microsoft
author: rick-anderson
description: In questa esercitazione si sarà avviare fin dall'inizio e creare Data Access Layer (DAL), utilizzando i dataset tipizzati, per accedere alle informazioni in un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: 6227233a-6254-4b6b-9a89-947efef22330
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cf1a430d6fe94174a877beb04b930409bdbf084
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-data-access-layer-vb"></a>Creazione di un livello di accesso ai dati (VB)
====================
da [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare App di esempio](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_1_VB.exe) o [Scarica il PDF](creating-a-data-access-layer-vb/_static/datatutorial01vb1.pdf)

> In questa esercitazione si sarà avviare fin dall'inizio e creare Data Access Layer (DAL), utilizzando i dataset tipizzati, per accedere alle informazioni in un database.


## <a name="introduction"></a>Introduzione

Gli sviluppatori web poco riguardano utilizzo dei dati. È creare database per archiviare i dati, il codice per recuperare e modificare e pagine web per raccogliere e creare riepiloghi. Questa è la prima esercitazione di una serie di lunga durata che esaminerà le tecniche di implementazione di questi modelli comuni di ASP.NET 2.0. Si inizierà con la creazione di un [architettura software](http://en.wikipedia.org/wiki/Software_architecture) composto di un dati Access Layer (DAL) con un livello di logica di Business (BLL) tipizzati che applica le regole di business personalizzata e pagine da un livello di presentazione composto di ASP.NET condividere un layout di pagina comune. Una volta state stabilite questa base di back-end, passiamo nella creazione di report, che illustra come visualizzare, riepilogare, raccogliere e convalidare i dati da un'applicazione web. Queste esercitazioni sono pensate per essere conciso e vengono fornite istruzioni dettagliate con una notevole quantità di schermate guidare il processo in modo visivo. Ogni esercitazione è disponibile in c# e Visual Basic versioni e include il download di codice completo utilizzato. (Prima di questa esercitazione è piuttosto lunga, ma il resto vengono presentati in più blocchi di gruppi).

Per queste esercitazioni verrà usato una versione di Microsoft SQL Server 2005 Express Edition del database Northwind in modalità di `App_Data` directory. Oltre al file di database, il `App_Data` cartella contiene anche gli script SQL per la creazione del database, nel caso in cui si desidera utilizzare una versione di database diverso. Questi script possono anche essere [scaricati direttamente da Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), se si preferisce. Se si utilizza una versione diversa di SQL Server del database Northwind, è necessario aggiornare il `NORTHWNDConnectionString` l'impostazione dell'applicazione `Web.config` file. L'applicazione web è stato compilato utilizzando Visual Studio 2005 Professional Edition come un file di progetto di sito Web basato su sistema. Tuttavia, tutte le esercitazioni funzioneranno correttamente con la versione gratuita di Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).

In questa esercitazione viene avviare fin dall'inizio e Data Access Layer (DAL), seguita dalla creazione di creare il [Business livello Business LOGIC](creating-a-business-logic-layer-vb.md) nel secondo esercitazione e lavora su [pagina layout e la navigazione](master-pages-and-site-navigation-vb.md) nel terzo. Le esercitazioni dopo il terzo verrà compilato al momento la base di cui le prime tre. Abbiamo molte per descriverle tutte in questa esercitazione prima, quindi avviare Visual Studio ed è possibile iniziare subito!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Passaggio 1: Creazione di un progetto Web e la connessione al Database

Prima di poter creare il livello di accesso dati (DAL), è necessario creare un sito web e il database del programma di installazione. Iniziare creando un nuovo file basato su sistema sito web ASP.NET. A tale scopo, tornare al menu File e scegliere Nuovo sito Web, la finestra di dialogo Nuovo sito Web. Scegliere il modello di sito Web ASP.NET, impostare l'elenco di riepilogo a discesa percorso al File System, scegliere una cartella in cui inserire il sito web e impostare la lingua di Visual Basic.


[![Creare un nuovo sistema basato su sito Web di File](creating-a-data-access-layer-vb/_static/image2.png)](creating-a-data-access-layer-vb/_static/image1.png)

**Figura 1**: creare un sito Web New File System-Based ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image3.png))


Verrà creato un nuovo sito web con un `Default.aspx` pagina ASP.NET, un `App_Data` cartella e un `Web.config` file.

Con il sito web è stato creato, il passaggio successivo consiste nell'aggiungere un riferimento al database in Esplora Server di Visual Studio. Tramite l'aggiunta di un database in Esplora Server, è possibile aggiungere tabelle, stored procedure, viste e così via all dall'interno di Visual Studio. È inoltre possibile visualizzare i dati della tabella o creare query personalizzate a mano o graficamente tramite il generatore delle Query. Inoltre, quando si compila il dataset tipizzati per DAL sarà necessario punto Visual Studio per il database da cui deve essere creato il set di dati tipizzato. Mentre è possibile fornire queste informazioni di connessione a questo punto nel tempo, Visual Studio popola automaticamente un elenco a discesa dei database di cui è già registrato in Esplora Server.

I passaggi per aggiungere il database Northwind in Esplora Server variano a seconda se si desidera utilizzare il database di SQL Server 2005 Express Edition nel `App_Data` cartella o se si dispone di un'installazione server di database 2005 che si desidera utilizzare Microsoft SQL Server 2000 In alternativa.

## <a name="using-a-database-in-theappdatafolder"></a>Utilizzo di un Database nel`App_Data`cartella

Se non si dispone di SQL Server 2000 o 2005 server di database a cui connettersi, o semplicemente per evitare di dover aggiungere il database a un server di database, è possibile utilizzare la versione di SQL Server 2005 Express Edition del database Northwind cui si trova il websit scaricato e's `App_Data` cartella (`NORTHWND.MDF`).

Un database in modalità del `App_Data` cartella viene automaticamente aggiunto a Esplora Server. Se che si dispone di SQL Server 2005 Express Edition installata nel computer verrà visualizzato un nodo denominato NORTHWND. MDF in Esplora Server, che è possibile espandere e visualizzare le relative tabelle, viste, stored procedure e così via (vedere la figura 2).

Il `App_Data` cartella può contenere anche Microsoft Access `.mdb` file, che, analogamente alle relative controparti di SQL Server, vengono aggiunti automaticamente a Esplora Server. Se non si desidera utilizzare una delle opzioni di SQL Server, è sempre possibile [scaricare una versione di Microsoft Access del file di database Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) trascinamento della selezione nel `App_Data` directory. Tenere presente, tuttavia, non i database di Access come ricco di funzionalità di SQL Server e non sono progettati per essere utilizzata negli scenari di sito web. Inoltre, un paio di esercitazioni 35 + utilizzeranno alcune funzionalità a livello di database che non sono supportate da Access.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>La connessione al Database in un Server di Database di Microsoft SQL Server 2000 o 2005

In alternativa, è possibile collegare a un database di Northwind installato in un server di database. Se il server di database non ha già installato il database Northwind, è innanzitutto necessario aggiungerlo al server di database eseguendo lo script di installazione incluso nel download di questa esercitazione o da [scaricare la versione di SQL Server 2000 di Northwind script di installazione e](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) direttamente dal sito web Microsoft.

Dopo aver installato il database, passare a Esplora Server in Visual Studio, fare clic sul nodo Connessioni dati e scegliere Aggiungi connessione. Se non viene visualizzato Esplora Server passare alla visualizzazione / Esplora Server o hit Ctrl + Alt + S. Verrà visualizzata la finestra di dialogo Aggiungi connessione, in cui è possibile specificare il server a cui connettersi, le informazioni di autenticazione e il nome del database. Dopo aver configurato le informazioni di connessione di database e fa clic sul pulsante OK, il database verrà aggiunto come nodo sotto il nodo di connessioni dati. È possibile espandere il nodo del database per esplorare le tabelle, viste, stored procedure e così via.


![Aggiungere una connessione al database Northwind del Server di Database.](creating-a-data-access-layer-vb/_static/image4.png)

**Figura 2**: aggiungere una connessione al database Northwind del Server di Database.


## <a name="step-2-creating-the-data-access-layer"></a>Passaggio 2: Creazione del livello di accesso ai dati

Quando si lavora con dati uno consiste nel non incorporare la logica di specifiche dei dati direttamente nel livello di presentazione (in un'applicazione web, il costituiscono il livello di presentazione nella pagine ASP.NET). L'operazione potrebbe richiedere il modulo di scrittura di codice ADO.NET nella porzione di codice della pagina ASP.NET o utilizzando il controllo SqlDataSource dalla parte di markup. In entrambi i casi, questo approccio è strettamente collegato la logica di accesso ai dati con il livello di presentazione. Si consiglia, invece di separare la logica di accesso ai dati dal livello di presentazione. A questo livello separato viene definito il livello di accesso ai dati, in breve e viene in genere implementato come un progetto libreria di classi separato. I vantaggi di questa architettura a più livelli sono ben documentati (vedere la sezione "Ulteriori letture" alla fine di questa esercitazione per informazioni su questi vantaggi) e costituisce l'approccio verrà usato in questa serie.

Tutto il codice specifico per l'origine dati sottostante, ad esempio la creazione di una connessione al database, il rilascio `SELECT`, `INSERT`, `UPDATE`, e `DELETE` comandi e così via devono trovarsi nello DAL. Il livello di presentazione non deve contenere riferimenti a tale codice di accesso ai dati, ma deve invece di effettuare chiamate in per le richieste di tutti i dati. Livelli di accesso ai dati in genere contengono i metodi per l'accesso ai dati di database sottostante. È ad esempio, il database Northwind, `Products` e `Categories` tabelle che registrano i prodotti in vendita e le categorie a cui appartengono. Nel nostro DAL abbiamo metodi, ad esempio:

- `GetCategories(),` Pertanto restituirà informazioni su tutte le categorie
- `GetProducts()`, che restituirà informazioni su tutti i prodotti
- `GetProductsByCategoryID(categoryID)`, che restituirà tutti i prodotti che appartengono a una categoria specificata
- `GetProductByProductID(productID)`, che restituirà informazioni su un prodotto specifico

Questi metodi, quando richiamata, verranno connettersi al database, eseguire la query appropriata e restituire i risultati. Come restituiamo questi risultati è importante. Questi metodi può semplicemente restituire un set di dati o DataReader popolata dalla query sul database, ma idealmente devono essere restituiti i risultati utilizzando *oggetti fortemente tipizzati*. Un oggetto fortemente tipizzato è quello il cui schema è rigidamente definito in fase di compilazione, mentre il contrario, un oggetto fortemente tipizzato, è uno cui schema non è noto fino al runtime.

Il DataReader e il set di dati (per impostazione predefinita), ad esempio, sono oggetti fortemente tipizzato poiché lo schema è definito per le colonne restituite dalla query sul database utilizzata per popolare le. Accedere a una particolare colonna da un DataTable fortemente tipizzato, è necessario utilizzare una sintassi simile: `DataTable.Rows(index)("columnName")`. Il DataTable tipizzazione in questo esempio viene esposto dal fatto che è necessario accedere al nome di colonna utilizzando una stringa o l'indice ordinale. Un oggetto DataTable fortemente tipizzata, d'altra parte, sarà necessario ciascuna delle relative colonne implementati come proprietà, risultante in codice simile al: `DataTable.Rows(index).columnName`.

Per restituire oggetti fortemente tipizzati, gli sviluppatori possono creare propri oggetti di business personalizzata o utilizzano i dataset tipizzati. Un oggetto business viene implementato dallo sviluppatore come rappresenta una classe le cui proprietà riflettono in genere le colonne della tabella di database sottostante dell'oggetto business. Un set di dati tipizzato è una classe generata automaticamente da Visual Studio in base a uno schema di database e i cui membri sono fortemente tipizzati in base a questo schema. Il DataSet tipizzato stesso è costituito da classi che estendono le classi ADO.NET DataSet, DataTable e DataRow. Oltre a DataTable fortemente tipizzata, dataset tipizzati anche includono ora TableAdapter, che sono classi con metodi per il popolamento DataTable del set di dati e propagazione delle modifiche all'interno di DataTable nuovamente al database.

> [!NOTE]
> Per ulteriori informazioni sui vantaggi e gli svantaggi dell'uso di dataset tipizzati e oggetti business personalizzati, fare riferimento a [la progettazione di componenti di livello dati e il passaggio attraverso i livelli dati](https://msdn.microsoft.com/library/ms978496.aspx).


Verrà utilizzata per l'architettura di queste esercitazioni fortemente tipizzati. La figura 3 illustra il flusso di lavoro tra i diversi livelli di un'applicazione che utilizza i dataset tipizzati.


[![Tutto il codice di accesso ai dati è relegati di DAL](creating-a-data-access-layer-vb/_static/image6.png)](creating-a-data-access-layer-vb/_static/image5.png)

**Figura 3**: tutto il codice di accesso ai dati è relegati DAL ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Creazione di un DataSet tipizzato e un adattatore di tabella

Per iniziare a creare il nostro DAL, iniziamo con l'aggiunta di un DataSet tipizzato al progetto. A tale scopo, fare doppio clic sul nodo del progetto in Esplora soluzioni e scegliere Aggiungi un nuovo elemento. Selezionare l'opzione set di dati dall'elenco dei modelli e denominarlo `Northwind.xsd`.


[![Scegliere di aggiungere un nuovo set di dati al progetto](creating-a-data-access-layer-vb/_static/image9.png)](creating-a-data-access-layer-vb/_static/image8.png)

**Figura 4**: scegliere di aggiungere un nuovo set di dati a un progetto ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image10.png))


Dopo aver facendo clic su Aggiungi, quando viene richiesto di aggiungere il set di dati per il `App_Code` cartella, scegliere Sì. Verrà quindi visualizzata la finestra di progettazione per il DataSet tipizzato e la configurazione guidata TableAdapter verrà avviato, consentendo di aggiungere il TableAdapter prima al set di dati tipizzato.

Un set di dati tipizzato funge da una raccolta fortemente tipizzata di dati. composti da istanze DataTable fortemente tipizzata, ognuno dei quali a sua volta è costituito da istanze di DataRow fortemente tipizzata. Verrà creata una tabella di dati fortemente tipizzati per ognuna delle tabelle di database sottostanti che è necessario utilizzare in questa serie di esercitazioni. Iniziamo con la creazione di un oggetto DataTable per la `Products` tabella.

Tenere presente che DataTable fortemente tipizzate non includono le informazioni su come accedere ai dati dalla tabella di database sottostante. Per poter recuperare i dati per compilare il DataTable, si utilizza una classe TableAdapter, che funziona come il livello di accesso ai dati. Per il nostro `Products` DataTable, TableAdapter conterrà i metodi `GetProducts()`, `GetProductByCategoryID(categoryID)`e così via che è possibile richiamare dal livello di presentazione. Ruolo del DataTable è come utilizzati per passare dati tra i livelli di oggetti fortemente tipizzati.

La configurazione guidata TableAdapter inizia da cui viene richiesto di selezionare il database da utilizzare. L'elenco di riepilogo a discesa Mostra i database in Esplora Server. Se non è stato aggiunto il database Northwind in Esplora Server, è possibile fare clic sul pulsante nuova connessione al momento eseguire questa operazione.


[![Scegliere il Database Northwind nell'elenco a discesa](creating-a-data-access-layer-vb/_static/image12.png)](creating-a-data-access-layer-vb/_static/image11.png)

**Figura 5**: scegliere il Northwind Database nell'elenco a discesa ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image13.png))


Dopo aver selezionato il database e fare clic su Avanti, verrà chiesto se si desidera salvare la stringa di connessione nel `Web.config` file. Salvando la stringa di connessione si verrà evita che il disco rigido codificate in classi TableAdapter, che semplifica le cose, se le informazioni sulla stringa di connessione viene modificata in futuro. Se si decide di salvare la stringa di connessione nel file di configurazione viene inserito nella `<connectionStrings>` , che può essere [facoltativamente crittografato](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) per migliore sicurezza o modificate in un secondo momento tramite la nuova pagina delle proprietà ASP.NET 2.0 all'interno di l'IIS Admin strumento GUI, più è ideale per gli amministratori.


[![Salvare la stringa di connessione in Web. config](creating-a-data-access-layer-vb/_static/image15.png)](creating-a-data-access-layer-vb/_static/image14.png)

**Figura 6**: Salva stringa di connessione `Web.config` ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image16.png))


Successivamente, è necessario definire lo schema per il primo DataTable fortemente tipizzati e fornire il primo metodo per l'oggetto TableAdapter da utilizzare durante il popolamento del set di dati fortemente tipizzati. Questi due passaggi, vengono eseguiti contemporaneamente creando una query che restituisce le colonne della tabella che si desidera siano indicati nei nostri DataTable. Al termine della procedura guidata si assegnerà un nome di metodo per la query. Una volta che viene eseguito, questo metodo può essere richiamato da questo livello di presentazione. Il metodo esegue la query definita e popolare un oggetto DataTable fortemente tipizzata.

Per iniziare a definire la query SQL è necessario indicare come si desidera eseguire la query TableAdapter. È possibile utilizzare un'istruzione SQL ad hoc, creare una nuova stored procedure o utilizzare una stored procedure esistente. Per queste esercitazioni si userà le istruzioni SQL ad hoc. Fare riferimento a [Brian Noyes](http://briannoyes.net/)dell'articolo, [compilare un livello di accesso ai dati con Progettazione DataSet di Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) per un esempio di utilizzo di stored procedure.


[![Eseguire query sui dati utilizzando un'istruzione SQL Ad Hoc](creating-a-data-access-layer-vb/_static/image18.png)](creating-a-data-access-layer-vb/_static/image17.png)

**Figura 7**: eseguire Query sui dati utilizzando un'istruzione SQL Ad Hoc ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image19.png))


A questo punto è possibile digitare la query SQL manualmente. Quando si crea il primo metodo in TableAdapter in genere si desidera fare in modo che la query restituisca le colonne che devono essere espressi nel DataTable corrispondente. A questo scopo, è possibile creare una query che restituisce tutte le colonne e tutte le righe dal `Products` tabella:


[![Immettere la Query SQL nella casella di testo](creating-a-data-access-layer-vb/_static/image21.png)](creating-a-data-access-layer-vb/_static/image20.png)

**Figura 8**: immettere la Query SQL in the Textbox ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image22.png))


In alternativa, utilizzare il generatore di Query e costruire graficamente la query, come illustrato nella figura 9.


[![Creare la Query in formato grafico tramite l'Editor di Query](creating-a-data-access-layer-vb/_static/image24.png)](creating-a-data-access-layer-vb/_static/image23.png)

**Figura 9**: creare la Query in formato grafico tramite l'Editor di Query ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image25.png))


Dopo la creazione della query, ma prima dello spostamento nella schermata successiva, fare clic sul pulsante Opzioni avanzate. Nei progetti di sito Web "genera Insert, Update e Delete istruzioni" sono l'unica opzione selezionata per impostazione predefinita; avanzata Se si esegue questa procedura guidata da una libreria di classi o un progetto di Windows verrà selezionata anche l'opzione "Usa concorrenza ottimistica". Lasciare deselezionata l'opzione "Usa concorrenza ottimistica" per il momento. Concorrenza ottimistica esamina in esercitazioni future.


[![Selezionare solo le genera istruzioni Insert, Update e Delete istruzioni opzione](creating-a-data-access-layer-vb/_static/image27.png)](creating-a-data-access-layer-vb/_static/image26.png)

**Figura 10**: selezionare solo le genera istruzioni Insert, Update e Delete istruzioni opzione ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image28.png))


Dopo aver verificato le opzioni avanzate, fare clic su Avanti per passare alla schermata finale. Di seguito viene chiesto di selezionare i metodi da aggiungere al TableAdapter. Sono disponibili due modelli per il popolamento dei dati:

- **Riempi un DataTable** con questo approccio viene creato un metodo che accetta un oggetto DataTable come parametro e viene compilato in base ai risultati della query. La classe DataAdapter di ADO.NET, ad esempio, implementa questo modello con il relativo `Fill()` metodo.
- **Restituisci un DataTable** con questo approccio il metodo crea e viene inserito l'oggetto DataTable e lo restituisce come valore restituiscono i metodi.

È possibile avere TableAdapter implementare uno o entrambi questi modelli. È inoltre possibile rinominare i metodi forniti di seguito. Anche se solo utilizzeremo il modello di quest'ultimo in queste esercitazioni, saranno esclusi sia selezionate, le caselle di controllo. Inoltre, rinominare il piuttosto generica `GetData` metodo `GetProducts`.

Se selezionata, la casella di controllo finale, "GenerateDBDirectMethods," Crea `Insert()`, `Update()`, e `Delete()` metodi per l'oggetto TableAdapter. Se si lascia deselezionata questa opzione, tutti gli aggiornamenti dovrà essere eseguita tramite unico dell'oggetto TableAdapter `Update()` metodo che accetta il DataSet tipizzato, un oggetto DataTable, un singolo oggetto DataRow o una matrice di DataRow. (Se è stata deselezionata la "genera Insert, Update e Delete istruzioni" opzione questa casella di controllo le proprietà avanzate nella figura 9 impostazione non avrà alcun effetto.) Consente di lasciare questa casella di controllo selezionata.


[![Modificare il nome del metodo da GetData a GetProducts](creating-a-data-access-layer-vb/_static/image30.png)](creating-a-data-access-layer-vb/_static/image29.png)

**Figura 11**: modificare il nome del metodo da `GetData` alla `GetProducts` ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image31.png))


Completare la procedura guidata, fare clic su Fine. Dopo aver chiuso la procedura guidata è viene restituiti alla finestra di progettazione set di dati che mostra il DataTable che appena creato. È possibile visualizzare l'elenco di colonne di `Products` DataTable (`ProductID`, `ProductName`e così via), nonché i metodi del `ProductsTableAdapter` (`Fill()` e `GetProducts()`).


[![L'oggetto DataTable di prodotti e ProductsTableAdapter sono stati aggiunti al set di dati tipizzato](creating-a-data-access-layer-vb/_static/image33.png)](creating-a-data-access-layer-vb/_static/image32.png)

**Figura 12**: il `Products` DataTable e `ProductsTableAdapter` sono stati aggiunti al set di dati tipizzato ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image34.png))


A questo punto si dispone di un DataSet tipizzato con una sola DataTable (`Northwind.Products`) e una classe di DataAdapter fortemente tipizzata (`NorthwindTableAdapters.ProductsTableAdapter`) con un `GetProducts()` metodo. Questi oggetti consente di accedere a un elenco di tutti i prodotti dal codice, ad esempio:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample1.vb)]

Questo codice non richiede di scrivere un bit di codice di accesso specifico di dati. Non è stato necessario creare un'istanza di tutte le classi ADO.NET, è necessario non fare riferimento a tutte le stringhe di connessione, le query SQL, o le stored procedure. Il TableAdapter invece, fornisce il codice di accesso ai dati di basso livello per noi.

Ogni oggetto utilizzato in questo esempio viene inoltre fortemente tipizzata, consentendo di Visual Studio fornire IntelliSense e il controllo dei tipi in fase di compilazione. E meglio di DataTable restituite dal TableAdapter può essere associato a dati ASP.NET controlli Web, ad esempio GridView, DetailsView, DropDownList, CheckBoxList e molti altri. Nell'esempio seguente viene illustrata l'associazione DataTable restituito dal `GetProducts()` metodo a un controllo GridView in solo un'il tre righe di codice all'interno di `Page_Load` gestore dell'evento.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample2.aspx)]

AllProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample3.vb)]


[![Viene visualizzato l'elenco di prodotti in un controllo GridView.](creating-a-data-access-layer-vb/_static/image36.png)](creating-a-data-access-layer-vb/_static/image35.png)

**Figura 13**: viene visualizzato l'elenco di prodotti in un controllo GridView ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image37.png))


In questo esempio viene richiesto che tre righe di codice è scrivere in una pagina ASP.NET `Page_Load` gestore dell'evento, in futuro verrà esaminato come utilizzare ObjectDataSource per recuperare in modo dichiarativo i dati DAL esercitazioni. Con ObjectDataSource è non sarà necessario scrivere codice e verrà visualizzato anche il supporto di ordinamento e paging!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Passaggio 3: Aggiunta di parametri di metodi a livello di accesso ai dati

A questo punto il nostro `ProductsTableAdapter` classe include un metodo, `GetProducts()`, che restituisce tutti i prodotti nel database. Mentre è in grado di funzionare con tutti i prodotti è sicuramente utile, vi sono casi quando si desidera recuperare informazioni su un prodotto specifico o tutti i prodotti che appartengono a una categoria specifica. Per aggiungere tale funzionalità per il livello di accesso ai dati è possibile aggiungere all'oggetto TableAdapter con i metodi con parametri.

Aggiungere il `GetProductsByCategoryID(categoryID)` metodo. Per aggiungere un nuovo metodo di DAL, torna alla finestra di progettazione set di dati, fare clic del mouse il `ProductsTableAdapter` sezione e scegliere Aggiungi Query.


![Fare clic sul TableAdapter e scegliere Aggiungi Query](creating-a-data-access-layer-vb/_static/image38.png)

**Nella figura 14**: pulsante destro del mouse sul TableAdapter e scegliere Aggiungi Query


Vengono innanzitutto richieste se si desidera accedere al database utilizzando un'istruzione SQL ad hoc o una stored procedure nuova o esistente. Si sceglie di utilizzare un'istruzione SQL ad hoc nuovamente. Successivamente, viene chiesto il tipo di query SQL che si desidera utilizzare. Poiché si desidera restituire tutti i prodotti che appartengono a una categoria specificata, si desidera scrivere un `SELECT` istruzione che restituisce righe.


[![Scegliere di creare un'istruzione SELECT che restituisce righe](creating-a-data-access-layer-vb/_static/image40.png)](creating-a-data-access-layer-vb/_static/image39.png)

**Figura 15**: scegliere di creare un `SELECT` che restituisce le righe di rendiconto ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image41.png))


Il passaggio successivo consiste nel definire la query SQL utilizzata per accedere ai dati. Poiché si desidera restituire solo i prodotti che appartengono a una categoria specifica, utilizzare lo stesso `SELECT` from dell'istruzione `GetProducts()`, ma è aggiungere le seguenti `WHERE` clausola: `WHERE CategoryID = @CategoryID`. Il `@CategoryID` per la configurazione guidata TableAdapter parametro indica che il metodo che si sta creando richiede un parametro di input del tipo corrispondente (vale a dire, un intero che ammette valori null).


[![Immettere una Query per restituire solo i prodotti in una categoria specifica](creating-a-data-access-layer-vb/_static/image43.png)](creating-a-data-access-layer-vb/_static/image42.png)

**Figura 16**: immettere una Query per restituire solo i prodotti in una categoria specificata ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image44.png))


Nel passaggio finale che è possibile scegliere quale modelli da utilizzare, nonché di personalizzare i nomi dei metodi generati di accesso ai dati. Per il motivo di riempimento, modificare il nome da `FillByCategoryID` e per il valore restituito un oggetto DataTable restituito modello (il `GetX` metodi), è possibile utilizzare `GetProductsByCategoryID`.


[![Scegliere i nomi per i metodi TableAdapter](creating-a-data-access-layer-vb/_static/image46.png)](creating-a-data-access-layer-vb/_static/image45.png)

**Figura 17**: scegliere i nomi per i metodi TableAdapter ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image47.png))


Dopo aver completato la procedura guidata, la finestra di progettazione del set di dati include i nuovi metodi TableAdapter.


![I prodotti ora possibile eseguire una query per categoria](creating-a-data-access-layer-vb/_static/image48.png)

**Figura 18**: di prodotti può ora eseguire una query per categoria


È opportuno aggiungere una `GetProductByProductID(productID)` metodo utilizzando la stessa tecnica.

Queste query con parametri possono essere verificate direttamente dalla finestra di Progettazione DataSet. Fare clic sul metodo nell'oggetto TableAdapter e scegliere l'anteprima dei dati. Quindi, immettere i valori da utilizzare per i parametri e fare clic su Anteprima.


[![Vengono visualizzati tali prodotti che appartiene alla categoria delle bevande](creating-a-data-access-layer-vb/_static/image50.png)](creating-a-data-access-layer-vb/_static/image49.png)

**Figura 19**: vengono visualizzati quelli prodotti appartenenti alla categoria Beverages ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image51.png))


Con il `GetProductsByCategoryID(categoryID)` nostri DAL metodo, è ora possibile creare una pagina ASP.NET che consente di visualizzare solo i prodotti in una categoria specificata. L'esempio seguente mostra tutti i prodotti della categoria Beverages, che hanno un `CategoryID` pari a 1.

Beverages.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample4.aspx)]

Beverages.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample5.vb)]


[![Vengono visualizzati i prodotti della categoria Beverages](creating-a-data-access-layer-vb/_static/image53.png)](creating-a-data-access-layer-vb/_static/image52.png)

**Figura 20**: vengono visualizzati quelli prodotti della categoria Beverages ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Passaggio 4: Inserimento, aggiornamento ed eliminazione di dati

Sono disponibili due modelli comunemente utilizzati per l'inserimento, aggiornamento ed eliminazione di dati. Il primo modello, chiamare il modello di database diretto, comporta la creazione di metodi che, quando richiamata, problema un `INSERT`, `UPDATE`, o `DELETE` comando per il database che opera su un solo record di database. Tali metodi vengono in genere passati in una serie di valori scalari (integer, stringhe, valori booleani, valori DateTime e così via) che corrispondono ai valori da inserire, aggiornare o eliminare. Ad esempio, con questo modello per il `Products` tabella il metodo delete richiederebbe un parametro intero, che indica il `ProductID` del record da eliminare, mentre il metodo insert richiederebbe in una stringa per il `ProductName`, un numero decimale per il `UnitPrice`, un numero intero per il `UnitsOnStock`e così via.


[![Ogni istruzione Insert, Update e richiesta di eliminazione viene inviato al Database immediatamente](creating-a-data-access-layer-vb/_static/image56.png)](creating-a-data-access-layer-vb/_static/image55.png)

**Figura 21**: ogni Insert, Update e richiesta di eliminazione viene inviato al Database immediatamente ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image57.png))


Altri modelli, che si farà riferimento come batch di aggiornare il modello, si aggiorna un intero set di dati, DataTable o raccolta di DataRow in una chiamata al metodo. Con questo modello di uno sviluppatore Elimina, inserimenti, modifica DataRows in un oggetto DataTable e quindi passa tali DataRows o DataTable in un metodo di aggiornamento. Questo metodo quindi enumera il DataRow passato, determina o meno è stati modificati, aggiunti o eliminati (tramite il DataRow [proprietà RowState](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) valore) e invia la richiesta di database appropriata per ogni record.


[![Tutte le modifiche vengono sincronizzate con il Database quando viene richiamato il metodo di aggiornamento](creating-a-data-access-layer-vb/_static/image59.png)](creating-a-data-access-layer-vb/_static/image58.png)

**Figura 22**: tutte le modifiche vengono sincronizzate con il Database quando viene richiamato il metodo di aggiornamento ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image60.png))


TableAdapter Usa il modello di aggiornamento batch per impostazione predefinita, ma supporta anche il modello diretto DB. Poiché è stata selezionata l'opzione "genera Insert, Update e Delete istruzioni" dalle proprietà avanzate quando si crea l'oggetto TableAdapter di `ProductsTableAdapter` contiene un `Update()` (metodo), che implementa il modello di aggiornamento batch. In particolare, l'oggetto TableAdapter contiene un `Update()` metodo che può essere passato del DataSet tipizzato, un oggetto DataTable fortemente tipizzato o DataRows uno o più. Se si lascia la casella di controllo "GenerateDBDirectMethods" selezionata quando prima creazione dell'oggetto TableAdapter il modello diretto DB verrà inoltre implementata mediante `Insert()`, `Update()`, e `Delete()` metodi.

Entrambi i modelli modifica dati utilizzano dell'oggetto TableAdapter `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà per emettere i `INSERT`, `UPDATE`, e `DELETE` comandi al database. È possibile esaminare e modificare il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` proprietà facendo clic sul TableAdapter in Progettazione DataSet e quindi passare alla finestra Proprietà. (Assicurarsi di aver selezionato l'oggetto TableAdapter e che il `ProductsTableAdapter` oggetto corrisponde a quello selezionato nell'elenco a discesa nella finestra Proprietà.)


[![Il TableAdapter è InsertCommand, UpdateCommand e DeleteCommand proprietà](creating-a-data-access-layer-vb/_static/image62.png)](creating-a-data-access-layer-vb/_static/image61.png)

**Nella figura 23**: il TableAdapter ha `InsertCommand`, `UpdateCommand`, e `DeleteCommand` delle proprietà ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image63.png))


Per esaminare o modificare una di queste proprietà di comando di database, fare clic su di `CommandText` sottoproprietà, che verrà visualizzato il generatore delle Query.


[![Configurare l'istruzione INSERT, UPDATE e istruzioni DELETE nel generatore di Query](creating-a-data-access-layer-vb/_static/image65.png)](creating-a-data-access-layer-vb/_static/image64.png)

**Figura 24**: configurare il `INSERT`, `UPDATE`, e `DELETE` istruzioni nel generatore di Query ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image66.png))


Esempio di codice seguente viene illustrato come utilizzare il modello di aggiornamento batch per raddoppiare il prezzo di tutti i prodotti che non sono fuori produzione e che dispongono di 25 unità in magazzino o meno:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample6.vb)]

Il codice riportato di seguito viene illustrato come utilizzare il DB diretto per eliminare un determinato prodotto a livello di codice, quindi aggiornare uno e quindi aggiungere una nuova:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample7.vb)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Creazione personalizzata inserimento, aggiornamento ed eliminazione di metodi

Il `Insert()`, `Update()`, e `Delete()` metodi creati dal metodo diretto DB possono essere poco pratico, soprattutto per le tabelle con molte colonne. Esaminando l'esempio di codice precedente, senza IntelliSense Guida in linea non è particolarmente chiaro cosa `Products` esegue il mapping di colonna della tabella per ogni parametro di input per il `Update()` e `Insert()` metodi. Può accadere quando si vuole inserire solo per aggiornare una singola colonna o due, o un oggetto personalizzato `Insert()` metodo che, ad esempio, verrà restituito il valore del record appena inserito `IDENTITY` campo (incremento automatico).

Per creare un metodo personalizzato di questo tipo, tornare alla finestra di Progettazione DataSet. Fare clic sul TableAdapter e scegliere Aggiungi Query, restituire la configurazione guidata TableAdapter. Nella seconda schermata è possibile indicare il tipo di query da creare. Creare un metodo che aggiunge un nuovo prodotto e quindi restituisce il valore del record appena aggiunto `ProductID`. Pertanto, scegliere di creare un `INSERT` query.


[![Creare un metodo per aggiungere una nuova riga alla tabella Products](creating-a-data-access-layer-vb/_static/image68.png)](creating-a-data-access-layer-vb/_static/image67.png)

**Figura 25**: creare un metodo per aggiungere una nuova riga per il `Products` tabella ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image69.png))


Nella schermata successiva il `InsertCommand`del `CommandText` viene visualizzato. Aumentare la query aggiungendo `SELECT SCOPE_IDENTITY()` alla fine della query, che verrà restituito l'ultimo valore identity inserito in un `IDENTITY` colonna nello stesso ambito. (Vedere il [documentazione tecnica](https://msdn.microsoft.com/library/ms190315.aspx) per ulteriori informazioni su `SCOPE_IDENTITY()` e il motivo per cui è possibile [utilizzare ambito\_IDENTITY() anziché @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Assicurarsi di terminare la `INSERT` istruzione con un punto e virgola prima di aggiungere il `SELECT` istruzione.


[![Aumentare la Query per restituire il valore SCOPE_IDENTITY)](creating-a-data-access-layer-vb/_static/image71.png)](creating-a-data-access-layer-vb/_static/image70.png)

**Figura 26**: aumentare la Query per restituire il `SCOPE_IDENTITY()` valore ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image72.png))


Infine, denominare il nuovo metodo `InsertProduct`.


[![Impostare il nuovo nome del metodo InsertProduct](creating-a-data-access-layer-vb/_static/image74.png)](creating-a-data-access-layer-vb/_static/image73.png)

**Figura 27**: impostare il nuovo nome del metodo `InsertProduct` ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image75.png))


Quando si torna alla finestra di progettazione set di dati si noterà che il `ProductsTableAdapter` contiene un nuovo metodo `InsertProduct`. Se questo nuovo metodo non dispone di un parametro per ogni colonna di `Products` sono probabilità di tabella, non è stato terminato il `INSERT` istruzione con un punto e virgola. Configurare il `InsertProduct` (metodo) e assicurarsi di disporre di un punto e virgola di delimitazione di `INSERT` e `SELECT` istruzioni.

Per impostazione predefinita, inserire i metodi di query non problema metodi, vale a dire che restituiscono il numero di righe interessate. Tuttavia, è necessario il `InsertProduct` per restituire il valore restituito dalla query, non il numero di righe interessate. A tale scopo, modificare il `InsertProduct` del metodo `ExecuteMode` proprietà `Scalar`.


[![Modificare la proprietà ExecuteMode per scalare](creating-a-data-access-layer-vb/_static/image77.png)](creating-a-data-access-layer-vb/_static/image76.png)

**Figura 28**: modifica il `ExecuteMode` proprietà `Scalar` ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image78.png))


Il codice seguente illustra questo nuovo `InsertProduct` metodo di azione:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample8.vb)]

## <a name="step-5-completing-the-data-access-layer"></a>Passaggio 5: Completare il livello di accesso ai dati

Si noti che il `ProductsTableAdapters` classe restituisce il `CategoryID` e `SupplierID` i valori di `Products` tabella, ma non include il `CategoryName` colonna dal `Categories` tabella o `CompanyName` colonna dal `Suppliers` la tabella, anche se si tratta probabilmente le colonne da visualizzare quando vengono visualizzate informazioni sul prodotto. È possibile aumentare il metodo del TableAdapter iniziale, `GetProducts()`, sono inclusi entrambi i `CategoryName` e `CompanyName` valori di colonna, che aggiorneranno il DataTable fortemente tipizzati in modo da includere le nuove colonne.

Questo può costituire un problema, tuttavia, come i metodi dell'oggetto TableAdapter per l'inserimento, aggiornamento, e l'eliminazione di dati sono in base di fuori di questo metodo iniziale. Fortunatamente, i metodi generati automaticamente per l'inserimento, aggiornamento ed eliminazione non sono interessati sottoquery di `SELECT` clausola. Da assicurandosi di aggiungere la query per `Categories` e `Suppliers` come sottoquery, anziché `JOIN` s, si eviterà di dover rielaborare i metodi per la modifica dei dati. Fare clic su di `GetProducts()` metodo il `ProductsTableAdapter` e scegliere Configura. Quindi, modificare il `SELECT` clausola in modo che risulti come:

[!code-sql[Main](creating-a-data-access-layer-vb/samples/sample9.sql)]


[![Aggiornare l'istruzione SELECT per il metodo GetProducts()](creating-a-data-access-layer-vb/_static/image80.png)](creating-a-data-access-layer-vb/_static/image79.png)

**Figura 29**: aggiornamento di `SELECT` istruzione per il `GetProducts()` (metodo) ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image81.png))


Dopo aver aggiornato il `GetProducts()` metodo da usare questa nuova query DataTable includerà due nuove colonne: `CategoryName` e `SupplierName`.


![DataTable prodotti include due nuove colonne](creating-a-data-access-layer-vb/_static/image82.png)

**Figura 30**: il `Products` DataTable include due nuove colonne


È opportuno aggiornare il `SELECT` clausola il `GetProductsByCategoryID(categoryID)` anche metodo.

Se si aggiorna il `GetProducts()` `SELECT` utilizzando `JOIN` sintassi Progettazione DataSet non saranno in grado di generare automaticamente i metodi per l'inserimento, aggiornamento, e l'eliminazione dei dati di database utilizzando il database indirizzare il modello. In alternativa, è necessario crearle manualmente molto come abbiamo visto con il `InsertProduct` metodo precedentemente in questa esercitazione. Inoltre, manualmente è necessario fornire il `InsertCommand`, `UpdateCommand`, e `DeleteCommand` se si desidera utilizzare il modello di aggiornamento in blocco i valori delle proprietà.

## <a name="adding-the-remaining-tableadapters"></a>Aggiungere gli oggetti rimanenti TableAdapter

Fino a questo punto, abbiamo esaminato solo l'utilizzo di un solo oggetto TableAdapter per una singola tabella di database. Tuttavia, il database di Northwind contiene più tabelle correlate che è necessario utilizzare nell'applicazione web. Un set di dati tipizzato può contenere più di DataTable correlati. Per completare il di conseguenza, è necessario aggiungere DataTable per le altre tabelle che verrà usato in queste esercitazioni. Per aggiungere un nuovo TableAdapter a un DataSet tipizzato, aprire la finestra di progettazione del set di dati, fare doppio clic nella finestra di progettazione e scegliere Aggiungi / TableAdapter. Questo crea un nuovo DataTable e TableAdapter e consentono di eseguire la procedura guidata che sono state esaminate in precedenza in questa esercitazione.

Richiedere alcuni minuti per creare i seguenti oggetti TableAdapter e metodi utilizzando le query seguenti. Si noti che le query nel `ProductsTableAdapter` includono una sottoquery per acquisire i nomi di categoria e il fornitore del prodotto. Inoltre, se ci ha seguito, è già stato aggiunto il `ProductsTableAdapter` della classe `GetProducts()` e `GetProductsByCategoryID(categoryID)` metodi.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample21.sql)]


[![Progettazione DataSet dopo che sono stati aggiunti i quattro oggetti TableAdapter](creating-a-data-access-layer-vb/_static/image84.png)](creating-a-data-access-layer-vb/_static/image83.png)

**Figura 31**: il set di dati progettazione dopo il quattro oggetti TableAdapter sono stati aggiunti ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Aggiungere il codice personalizzato per il campo DAL

Il TableAdapter e DataTable aggiunto al set di dati tipizzati sono espressi come file XML Schema Definition (`Northwind.xsd`). È possibile visualizzare queste informazioni sullo schema facendo clic su di `Northwind.xsd` file in Esplora soluzioni e scegliendo Visualizza codice.


[![Il File di definizione (XSD) di XML Schema per l'Employees un DataSet tipizzato.](creating-a-data-access-layer-vb/_static/image87.png)](creating-a-data-access-layer-vb/_static/image86.png)

**Nella figura 32**: File di XML Schema Definition (XSD) per il DataSet tipizzato Employees ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image88.png))


Queste informazioni sullo schema viene tradotta in codice c# o Visual Basic in fase di progettazione quando viene compilato o in fase di esecuzione (se necessario), a quel punto è possibile eseguire tramite di esso con il debugger. Per visualizzare questo codice generato automaticamente Vai a Visualizzazione classi e drill down per le classi TableAdapter e DataSet tipizzato. Se non è disponibile la classe sullo schermo, passare al menu Visualizza e selezionarlo o premere Ctrl + Maiusc + C. Da Visualizzazione classi è possibile visualizzare le proprietà, metodi ed eventi delle classi tipizzate DataSet e TableAdapter. Per visualizzare il codice per un metodo specifico, fare doppio clic sul nome del metodo in visualizzazione classi o destro del mouse su di esso e scegliere Vai a definizione.


![Esaminare il codice generato automaticamente scegliendo Vai a definizione da Visualizzazione classi](creating-a-data-access-layer-vb/_static/image89.png)

**Nella figura 33**: esaminare il codice generato automaticamente scegliendo Vai a definizione da Visualizzazione classi


Codice generato automaticamente può essere un risparmiare molto tempo, il codice è spesso molto generico e deve essere personalizzato per soddisfare le esigenze di un'applicazione. Il rischio di estendere il codice generato automaticamente, tuttavia, è che lo strumento che ha generato il codice potrebbe decidere di che è possibile "Rigenera" e sovrascrivere le personalizzazioni. Con nuovo concetto classe parziale di .NET 2.0, è facile suddividere una classe in più file. Ciò consente di aggiungere i propri metodi, proprietà ed eventi per le classi generate automaticamente senza doversi preoccupare di sovrascrivere la personalizzazione di Visual Studio.

Per illustrare come personalizzare DAL, aggiungere un `GetProducts()` metodo per la `SuppliersRow` classe. Il `SuppliersRow` classe rappresenta un singolo record di `Suppliers` tabella; ogni provider può fornitore zero a molti prodotti, in modo `GetProducts()` restituirà i prodotti del fornitore specificato. Per eseguire questo crea un nuovo file di classe nel `App_Code` cartella denominata `SuppliersRow.vb` e aggiungere il codice seguente:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample22.vb)]

Questa classe parziale indica al compilatore che quando la creazione di `Northwind.SuppliersRow` classe per includere il `GetProducts()` metodo definita. Se si compila il progetto e quindi tornare alla visualizzazione classe vedrai `GetProducts()` elencato come un metodo di `Northwind.SuppliersRow`.


![Il metodo GetProducts() fa ora parte della classe Northwind.SuppliersRow](creating-a-data-access-layer-vb/_static/image90.png)

**Figura 34**: il `GetProducts()` metodo fa ora parte di `Northwind.SuppliersRow` (classe)


Il `GetProducts()` metodo ora può essere utilizzato per enumerare il set di prodotti per un particolare fornitore, come illustrato nel codice seguente:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample23.vb)]

Questo tipo di dati può essere visualizzato anche in una qualsiasi di ASP. I controlli Web i dati della rete. La pagina seguente utilizza un controllo GridView con due campi:

- Un BoundField che visualizza il nome di ogni fornitore, e
- Un TemplateField contenente un controllo BulletedList ai risultati restituiti da cui è associato il `GetProducts()` metodo per ogni fornitore.

Esamina la modalità di visualizzazione di tali report master-Details in esercitazioni future. Per il momento, questo esempio è progettato per illustrare l'utilizzo del metodo personalizzato aggiunto la `Northwind.SuppliersRow` classe.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample24.aspx)]

SuppliersAndProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample25.vb)]


[![Nome della società del fornitore è elencato nella colonna sinistra, destra dei prodotti](creating-a-data-access-layer-vb/_static/image92.png)](creating-a-data-access-layer-vb/_static/image91.png)

**Nella figura 35**: nome del fornitore il della società è elencato nella colonna sinistra, destra relativi prodotti ([fare clic per visualizzare l'immagine ingrandita](creating-a-data-access-layer-vb/_static/image93.png))


## <a name="summary"></a>Riepilogo

Quando la creazione di un'applicazione web creazione DAL deve essere uno dei primi passaggi, che si verificano prima di iniziare a creare il livello di presentazione. Con Visual Studio, creazione di DAL basato su dataset tipizzati è un'attività che può essere eseguita in 10-15 minuti senza scrivere una riga di codice. Le esercitazioni in futuro verranno si basano su DAL. Nel [esercitazione successiva](creating-a-business-logic-layer-vb.md) verrà definito un numero di regole business e come implementarli in un livello di logica di Business separato.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere le risorse seguenti:

- [La creazione di DAL utilizzando TableAdapter con fortemente tipizzati e DataTable in Visual Studio 2005 e ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [La progettazione di componenti di livello dati e il passaggio di dati tramite i livelli](https://msdn.microsoft.com/library/ms978496.aspx)
- [Creare un livello di accesso ai dati con Progettazione DataSet di Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [La crittografia delle informazioni di configurazione in ASP.NET 2.0 applicazioni](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Panoramica degli oggetti TableAdapter](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Utilizzo di un DataSet tipizzato](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Tramite l'accesso ai dati fortemente tipizzati in Visual Studio 2005 e ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Come estendere i metodi TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Il recupero di dati scalare da una Stored Procedure](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Video di formazione su argomenti contenuti in questa esercitazione

- [Livelli di accesso ai dati nelle applicazioni ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Come associare manualmente un set di dati a un controllo Datagrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Come lavorare con i set di dati e i filtri di un'applicazione ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), l'autore di sette libri e fondatore di [4GuysFromRolla](http://www.4guysfromrolla.com), ha lavorato con tecnologie Web di Microsoft dal 1998. Scott funziona come un consulente trainer e writer. Il suo ultimo libro è [ *SAM insegna manualmente ASP.NET 2.0 nelle 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Egli può essere raggiunto al [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o sul suo blog, cui è reperibile in [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Ringraziamenti speciali

Questa serie di esercitazioni è stata esaminata da diversi validi revisori. Lead revisori per questa esercitazione sono stati Ron Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, etichetta Gomez e Santos Carlos. Se si è interessati my prossimi articoli MSDN? In caso affermativo, Inviami una riga alla [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](master-pages-and-site-navigation-cs.md)
> [Successivo](creating-a-business-logic-layer-vb.md)
