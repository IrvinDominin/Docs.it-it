---
title: Usare il modello di progetto per Angular con ASP.NET Core
author: SteveSandersonMS
description: Informazioni su come iniziare a usare il modello di progetto per applicazioni a pagina singola di ASP.NET Core per Angular e l'interfaccia della riga di comando di Angular.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/angular
ms.openlocfilehash: 763b4fff7c64432328af660c66e6ff3f8f697f71
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011358"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>Usare il modello di progetto per Angular con ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Questa documentazione non riguarda il modello di progetto per Angular incluso in ASP.NET 2.0 Core. È relativa al modello per Angular più recente, a cui è possibile eseguire l'aggiornamento manualmente. Il modello è incluso in ASP.NET Core 2.1 per impostazione predefinita.

::: moniker-end

Il modello di progetto aggiornato per Angular fornisce un ottimo punto di partenza per le app ASP.NET Core che usano Angular e l'interfaccia della riga di comando di Angular per implementare un'interfaccia utente avanzata sul lato client.

Il modello è equivalente alla creazione di un progetto ASP.NET Core che opera come un back-end API e un progetto per l'interfaccia della riga di comando di Angular che opera come un'interfaccia utente. Il modello offre la praticità di ospitare entrambi i tipi di progetto in un singolo progetto di app. Di conseguenza, il progetto di app può essere compilato e pubblicato come una singola unità.

## <a name="create-a-new-app"></a>Creare una nuova app

::: moniker range="= aspnetcore-2.0"

Se si usa ASP.NET Core 2.0, assicurarsi di avere [installato il modello di progetto React aggiornato](xref:spa/index#installation).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Se ASP.NET Core 2.1 è installato, non è necessario installare il modello di progetto Angular.

::: moniker-end

Creare un nuovo progetto da un prompt dei comandi usando il comando `dotnet new angular` in una directory vuota. Ad esempio, i comandi seguenti creano l'app in una directory *my-new-app* e passano a tale directory:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Eseguire l'app da Visual Studio o dall'interfaccia della riga di comando di .NET Core:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

Aprire il file con estensione *csproj* generato ed eseguire l'app come di consueto da tale posizione.

Alla prima esecuzione, il processo di compilazione ripristina le dipendenze di npm. Tale operazione può richiedere alcuni minuti. Le compilazioni successive sono molto più veloci.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli/)

Assicurarsi di disporre di una variabile di ambiente denominata `ASPNETCORE_Environment` con valore `Development`. In Windows (in prompt non PowerShell) eseguire `SET ASPNETCORE_Environment=Development`. In Linux o Mac OS eseguire `export ASPNETCORE_Environment=Development`.

Eseguire [dotnet build](/dotnet/core/tools/dotnet-build) per verificare che l'app venga compilata correttamente. Alla prima esecuzione, il processo di compilazione ripristina le dipendenze di npm. Tale operazione può richiedere alcuni minuti. Le compilazioni successive sono molto più veloci.

Eseguire [dotnet run](/dotnet/core/tools/dotnet-run) per avviare l'app. Verrà registrato un messaggio simile al seguente:

```console
Now listening on: http://localhost:<port>
```

Passare a questo URL in un browser.

L'app avvia in background un'istanza del server dell'interfaccia della riga di comando di Angular. Verrà registrato un messaggio simile al seguente: *NG Live Development Server is listening on localhost:&lt;otherport&gt;, open your browser on http://localhost:&lt;otherport&gt;/*. Ignorare questo messaggio: **non** si tratta dell'URL per l'app combinata per ASP.NET Core e l'interfaccia della riga di comando di Angular.

---

Il modello di progetto crea un'app ASP.NET Core e un'app Angular. L'app ASP.NET Core è destinata all'uso per l'accesso ai dati, l'autorizzazione e altri elementi sul lato server. L'app Angular, disponibile nella sottodirectory *ClientApp*, è destinata all'uso per tutti gli aspetti relativi all'interfaccia utente.

## <a name="add-pages-images-styles-modules-etc"></a>Aggiungere pagine, immagini, stili, moduli e così via.

La directory *ClientApp* contiene un'app standard per l'interfaccia della riga di comando di Angular. Per altre informazioni, vedere la [documentazione ufficiale di Angular](https://github.com/angular/angular-cli/wiki).

Vi sono piccole differenze tra l'app Angular creata tramite questo modello e quella creata tramite l'interfaccia della riga di comando di Angular stessa (mediante `ng new`). Le funzionalità dell'app restano comunque invariate. L'app creata tramite il modello contiene un layout basato su [bootstrap](https://getbootstrap.com/) e un esempio di routing di base.

## <a name="run-ng-commands"></a>Eseguire i comandi ng

In un prompt dei comandi passare alla sottodirectory *ClientApp*:

```console
cd ClientApp
```

Se si dispone dello strumento `ng` installato globalmente, è possibile eseguire i relativi comandi. Ad esempio, è possibile eseguire `ng lint`, `ng test` o qualsiasi altro [comando dell'interfaccia della riga di comando di Angular](https://github.com/angular/angular-cli/wiki#additional-commands). Non è tuttavia necessario eseguire `ng serve`, poiché l'app ASP.NET Core si occupa della gestione sia delle parti sul lato server che di quelle sul lato client dell'app. Internamente, usa `ng serve` in fase di sviluppo.

Se lo strumento `ng` non è installato, eseguire `npm run ng`. Ad esempio, è possibile eseguire `npm run ng lint` o `npm run ng test`.

## <a name="install-npm-packages"></a>Installa nuovi pacchetti npm

Per installare i pacchetti npm di terze parti, usare un prompt dei comandi nella sottodirectory *ClientApp*. Ad esempio:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Pubblicare e distribuire

In fase di sviluppo, l'app viene eseguita in una modalità ottimizzata per gli sviluppatori. Ad esempio, i bundle JavaScript includono il mapping di origine, in modo da poter visualizzare il codice TypeScript originale durante il debug. L'app controlla le modifiche dei file TypeScript, HTML e CSS su disco, quindi esegue automaticamente la ricompilazione e il ricaricamento quando rileva modifiche dei file.

Nell'ambiente di produzione usare una versione dell'app ottimizzata per le prestazioni. Questo comportamento è configurato per l'esecuzione automatica. Quando si esegue la pubblicazione, la configurazione della build genera una compilazione minimizzata AOT (Ahead Of Time) del codice sul lato client. A differenza della build di sviluppo, la build di produzione non richiede l'installazione di Node.js nel server (a meno che non sia stato abilitato il [pre-rendering sul lato server](#server-side-rendering)).

È possibile usare i [metodi standard di hosting e distribuzione di ASP.NET Core](xref:host-and-deploy/index).

## <a name="run-ng-serve-independently"></a>Eseguire "ng serve" in modo indipendente

Il progetto è configurato in modo da avviare in background la propria istanza del server dell'interfaccia della riga di comando di Angular all'avvio dell'app ASP.NET Core in modalità di sviluppo. Ciò risulta utile in quanto evita di dover eseguire manualmente un server distinto.

Questa configurazione predefinita presenta tuttavia uno svantaggio. Ogni volta che si modifica il codice C# ed è necessario riavviare l'app ASP.NET Core, il server dell'interfaccia della riga di comando di Angular viene riavviato. Per avviare il backup sono necessari circa 10 secondi. Se si apportano frequentemente modifiche al codice C# e non si vuole attendere il riavvio dell'interfaccia della riga di comando di Angular, eseguire il server dell'interfaccia della riga di comando di Angular esternamente, in modo indipendente dal processo ASP.NET Core. A tale scopo:

1. In un prompt dei comandi passare alla sottodirectory *ClientApp* e avviare il server di sviluppo dell'interfaccia della riga di comando di Angular:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Usare `npm start` per avviare il server di sviluppo dell'interfaccia della riga di comando di Angular, anziché `ng serve`, in modo che la configurazione in *package.json* venga rispettata. Per passare parametri aggiuntivi al server dell'interfaccia della riga di comando di Angular, aggiungerli alla riga appropriata `scripts` nel file *package.json*.

2. Modificare l'app ASP.NET Core in modo da usare l'istanza dell'interfaccia della riga di comando di Angular esterna anziché avviarne una autonomamente. Nella classe *Startup* sostituire la chiamata `spa.UseAngularCliServer` con quanto segue:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

All'avvio dell'app ASP.NET Core, questa non avvierà un server dell'interfaccia della riga di comando di Angular. Verrà invece usata l'istanza che è stata avviata manualmente. Ciò consente di velocizzare l'avvio e il riavvio. Non è più necessario attendere che l'app client venga ricompilata ogni volta dall'interfaccia della riga di comando di Angular.

## <a name="server-side-rendering"></a>Rendering lato server

Come funzionalità per le prestazioni, è possibile scegliere di eseguire il pre-rendering dell'app Angular sul server, oltre a eseguirla sul client. In tal modo, i browser ricevono il markup HTML che rappresenta l'interfaccia utente iniziale dell'app, visualizzandola ancora prima di scaricare ed eseguire i bundle JavaScript. La maggior parte dell'implementazione di questa caratteristica deriva da una funzionalità di Angular denominata [Angular Universal](https://universal.angular.io/).

> [!TIP]
> L'abilitazione del rendering lato server introduce numerose complicazioni aggiuntive sia durante lo sviluppo che in fase di distribuzione. Consultare le informazioni sugli [svantaggi del rendering lato server](#drawbacks-of-ssr) per determinare se questa funzionalità è adatta per le proprie esigenze.

Per abilitare il rendering lato server, è necessario apportare diverse aggiunte al progetto.

Nella classe *Startup*, *dopo* la riga che configura `spa.Options.SourcePath` e *prima* della chiamata a `UseAngularCliServer` o `UseProxyToSpaDevelopmentServer`, aggiungere quanto segue:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

In modalità di sviluppo, questo codice tenta di compilare il bundle per il rendering lato server eseguendo lo script `build:ssr`, definito in *ClientApp\package.json*. Verrà compilata un'app Angular denominata `ssr`, che non è ancora definita.

Alla fine della matrice `apps` in *ClientApp/.angular-cli.json*, definire un'app aggiuntiva con il nome `ssr`. Usare le opzioni seguenti:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

La configurazione della nuova app abilitata per il rendering lato server richiede altri due file: *tsconfig.server.json* e *main.server.ts*. Il file *tsconfig.server.json* specifica le opzioni di compilazione TypeScript. Il file *main.server.ts* viene usato come punto di ingresso del codice durante il rendering lato server.

Aggiungere un nuovo file denominato *tsconfig.server.json* in *ClientApp/src* (insieme al file *tsconfig.app.json* esistente), contenente quanto segue:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Questo file configura il compilatore AoT di Angular per la ricerca di un modulo denominato `app.server.module`. Aggiungere questo elemento creando un nuovo file in *ClientApp/src/app/app.server.module.ts* (insieme al file *app.module.ts* esistente), contenente quanto segue:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Questo modulo eredita da `app.module` sul lato client e definisce i moduli Angular aggiuntivi disponibili durante il rendering lato server.

Com'è stato accennato, la nuova voce `ssr` in *.angular-cli.json* fa riferimento a un file di punto di ingresso denominato *main.server.ts*. Tale file non è ancora stato aggiunto e a questo punto è necessario eseguire questa operazione. Creare un nuovo file in *ClientApp/src/main.server.ts* (insieme al file *main.ts* esistente), contenente quanto segue:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

Il codice di questo file viene eseguito da ASP.NET Core per ogni richiesta durante l'esecuzione del middleware `UseSpaPrerendering` che è stato aggiunto per la classe *Startup*. Si occupa della ricezione di `params` dal codice .NET (ad esempio l'URL richiesto) e delle chiamate alle API per il rendering lato server di Angular, in modo da ottenere il codice HTML risultante.

In senso stretto, questo è sufficiente per abilitare il rendering lato server in modalità di sviluppo. È essenziale apportare una modifica finale in modo da assicurare il corretto funzionamento dell'app una volta pubblicata. Nel file *.csproj* principale dell'app impostare il valore della proprietà `BuildServerSideRenderer` su `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

In questo modo, il processo di compilazione viene configurato per eseguire `build:ssr` durante la pubblicazione e la distribuzione dei file per il rendering lato server sul server. Se non si abilita questa funzionalità, il rendering lato server avrà esito negativo in produzione.

Quando l'app viene eseguita in modalità di sviluppo o di produzione, il codice Angular esegue il pre-rendering in formato HTML sul server. Il codice sul lato client viene eseguito normalmente.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Passare dati dal codice .NET nel codice TypeScript

Durante il rendering lato server, potrebbe essere necessario passare dati per ogni richiesta dall'app ASP.NET Core nell'app Angular. Ad esempio, è possibile passare informazioni sui cookie o dati letti da un database. A tale scopo, modificare la classe *Startup*. Nel callback per `UseSpaPrerendering`, impostare un valore per `options.SupplyData` come il seguente:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

Il callback `SupplyData` consente di passare dati arbitrari serializzabili con JSON per ogni richiesta, ad esempio stringhe, valori booleani o numeri. Il codice *main.server.ts* riceve questi dati come `params.data`. Ad esempio, l'esempio di codice precedente passa un valore booleano come `params.data.isHttpsRequest` nel callback `createServerRenderer`. È possibile passare questo valore ad altre parti dell'app in qualsiasi modo supportato da Angular. Ad esempio, si osservi come *main.server.ts* passa il valore `BASE_URL` a qualsiasi componente il cui costruttore viene dichiarato per la relativa ricezione.

### <a name="drawbacks-of-ssr"></a>Svantaggi del rendering lato server

Non tutte le app traggono vantaggio dal rendering lato server. Il vantaggio principale è rappresentato dalle prestazioni percepite. I visitatori che raggiungono l'app tramite una connessione di rete lenta o da dispositivi mobili lenti vedono l'interfaccia utente iniziale rapidamente, anche se occorre un certo tempo per recuperare o analizzare i bundle JavaScript. Tuttavia, molte applicazioni a pagina singola sono usate principalmente in reti aziendali interne, su computer veloci in cui l'app viene visualizzata quasi istantaneamente.

Allo stesso tempo, l'abilitazione del rendering lato server presenta alcuni svantaggi significativi. Aumenta la complessità del processo di sviluppo. Il codice deve essere eseguito in due diversi ambienti: lato client e lato server (in un ambiente Node.js richiamato da ASP.NET Core). Di seguito sono illustrati alcuni aspetti da tenere presente:

* Il rendering lato server richiede un'installazione di Node.js nei server di produzione. Ciò avviene automaticamente per alcuni scenari di distribuzione, ad esempio Servizio app di Azure, ma non per altri, come Azure Service Fabric.
* L'abilitazione del flag di compilazione `BuildServerSideRenderer` determina la pubblicazione della directory *node_modules*. Questa cartella contiene oltre 20.000 file, che aumentano il tempo di distribuzione.
* Per eseguire il codice in un ambiente Node.js, non è possibile basarsi sull'esistenza di API JavaScript specifiche del browser, come `window` o `localStorage`. Se il codice (o una libreria di terze parti a cui si fa riferimento) tenta di usare queste API, verrà visualizzato un errore durante il rendering lato server. Ad esempio, non usare jQuery perché fa riferimento ad API specifiche del browser in numerose posizioni. Per evitare errori, è necessario non usare il rendering lato server o evitare API o librerie specifiche del browser. È possibile eseguire il wrapping di tutte le chiamate a tali API nei controlli per assicurarsi che non vengano richiamate durante il rendering lato server. Nel codice JavaScript o TypeScript, ad esempio, usare un controllo simile al seguente:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
