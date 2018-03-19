---
title: Configurazione in ASP.NET Core
author: rick-anderson
description: "È possibile usare l'API di configurazione per configurare un'app ASP.NET Core in diversi modi."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 7c41621db835b452c9aad9463a9ffccdf0c06484
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="fdc1f-103">Configurare un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdc1f-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="fdc1f-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fdc1f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fdc1f-105">L'API di configurazione fornisce un modo per configurare un'app Web ASP.NET Core in base a un elenco di coppie nome-valore.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="fdc1f-106">La configurazione viene letta in fase di esecuzione da più origini.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="fdc1f-107">È possibile raggruppare le coppie nome-valore in una gerarchia a più livelli.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="fdc1f-108">Esistono provider di configurazione per:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-108">There are configuration providers for:</span></span>

* <span data-ttu-id="fdc1f-109">Formati di file (INI, JSON e XML).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="fdc1f-110">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-110">Command-line arguments.</span></span>
* <span data-ttu-id="fdc1f-111">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-111">Environment variables.</span></span>
* <span data-ttu-id="fdc1f-112">Oggetti .NET in memoria.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="fdc1f-113">Archiviazione con [Secret Manager](xref:security/app-secrets) senza crittografia.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="fdc1f-114">Un archivio utente non crittografato, come [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="fdc1f-115">Provider personalizzati (installati o creati).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="fdc1f-116">Ogni valore di configurazione è associato a una chiave di stringa.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="fdc1f-117">È disponibile il supporto di associazione incorporato per deserializzare le impostazioni in un oggetto personalizzato [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) (una classe .NET semplice con proprietà).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="fdc1f-118">Il modello di opzioni usa le classi di opzioni per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="fdc1f-119">Per altre informazioni sull'uso del modello di opzioni, vedere l'argomento [Opzioni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="fdc1f-120">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fdc1f-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="fdc1f-121">Configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="fdc1f-121">JSON configuration</span></span>

<span data-ttu-id="fdc1f-122">La seguente app di console usa il provider di configurazione JSON:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="fdc1f-123">L'app legge e visualizza le impostazioni di configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="fdc1f-124">La configurazione è costituita da un elenco gerarchico di coppie nome-valore in cui i nodi sono separati dai due punti.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="fdc1f-125">Per recuperare un valore, accedere all'indicizzatore `Configuration` con la chiave dell'elemento corrispondente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="fdc1f-126">Per lavorare con le matrici nelle origini di configurazione in formato JSON, usare un indice di matrice come parte della stringa separata da due punti.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="fdc1f-127">L'esempio seguente recupera il nome del primo elemento della matrice precedente `wizards`:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="fdc1f-128">Le coppie nome-valore scritte nei provider predefiniti [Configurazione](/dotnet/api/microsoft.extensions.configuration) **non** sono salvate in modo permanente.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="fdc1f-129">Tuttavia è possibile creare un provider personalizzato che salva i valori.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="fdc1f-130">Vedere [provider di configurazione personalizzati](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="fdc1f-131">L'esempio precedente usa l'indicizzatore di configurazione per leggere i valori.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="fdc1f-132">Per accedere alla configurazione fuori da `Startup`, usare il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="fdc1f-133">Per altre informazioni, vedere l'argomento [Opzioni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="fdc1f-134">Configurazione XML</span><span class="sxs-lookup"><span data-stu-id="fdc1f-134">XML configuration</span></span>

<span data-ttu-id="fdc1f-135">Per usare le matrici nelle origini di configurazione in formato XML, specificare un indice `name` in ogni elemento.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="fdc1f-136">Usare l'indice per accedere ai valori:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-136">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="fdc1f-137">Configurazione per ambiente</span><span class="sxs-lookup"><span data-stu-id="fdc1f-137">Configuration by environment</span></span>

<span data-ttu-id="fdc1f-138">Spesso si hanno diverse impostazioni di configurazione per ambienti diversi, ad esempio per sviluppo, test e produzione.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="fdc1f-139">Il metodo di estensione `CreateDefaultBuilder` in un'app ASP.NET Core 2.x (o l'uso di `AddJsonFile` e `AddEnvironmentVariables` direttamente in un'app ASP.NET Core 1.x) aggiunge dei provider di configurazione per la lettura dei file JSON e le origini configurazione del sistema:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="fdc1f-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="fdc1f-140">*appsettings.json*</span></span>
* <span data-ttu-id="fdc1f-141">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="fdc1f-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="fdc1f-142">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="fdc1f-142">Environment variables</span></span>

<span data-ttu-id="fdc1f-143">Le app di ASP.NET Core 1.x devono chiamare `AddJsonFile` e [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="fdc1f-144">Vedere [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) per una spiegazione dei parametri.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="fdc1f-145">`reloadOnChange` è supportato solo in ASP.NET Core 1.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="fdc1f-146">Le origini di configurazione vengono lette nell'ordine in cui sono specificate.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="fdc1f-147">Nel codice precedente, le variabili di ambiente vengono lette per ultime.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="fdc1f-148">I valori di configurazione impostati tramite l'ambiente sostituiscono quelli impostati nei due provider precedenti.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="fdc1f-149">Considerare il file seguente *appsettings. Staging.JSON*:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="fdc1f-150">Quando l'ambiente è impostato su `Staging`, il metodo `Configure` seguente legge il valore di `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-150">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="fdc1f-151">L'ambiente è in genere impostato su `Development`, `Staging` o `Production`.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="fdc1f-152">Per altre informazioni, vedere [Working with multiple environments](xref:fundamentals/environments) (Utilizzo con più ambienti).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-152">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="fdc1f-153">Considerazioni sulla configurazione:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-153">Configuration considerations:</span></span>

* <span data-ttu-id="fdc1f-154">`IOptionsSnapshot` può ricaricare i dati di configurazione in caso di modifiche.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-154">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="fdc1f-155">Per altre informazioni, vedere [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-155">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="fdc1f-156">Le chiavi di configurazione **non** rispettano la distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-156">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="fdc1f-157">Non archiviare **mai** la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-157">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="fdc1f-158">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-158">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="fdc1f-159">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="fdc1f-160">Altre informazioni sull'[uso di più ambienti](xref:fundamentals/environments) e sulla gestione dell'[archiviazione sicura dei segreti delle app durante lo sviluppo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-160">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="fdc1f-161">Se non è possibile usare i due punti (`:`) nelle variabili di ambiente nel sistema, sostituire i due punti (`:`) con un doppio carattere di sottolineatura (`__`).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-161">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="fdc1f-162">Provider in memoria e associazione a una classe POCO</span><span class="sxs-lookup"><span data-stu-id="fdc1f-162">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="fdc1f-163">L'esempio seguente illustra come usare il provider in memoria e associarlo a una classe:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-163">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="fdc1f-164">I valori di configurazione vengono restituiti come stringhe, ma l'associazione consente la costruzione di oggetti.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-164">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="fdc1f-165">L'associazione consente il recupero di oggetti POCO o di interi grafici degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-165">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="fdc1f-166">GetValue</span><span class="sxs-lookup"><span data-stu-id="fdc1f-166">GetValue</span></span>

<span data-ttu-id="fdc1f-167">Nell'esempio seguente viene illustrato il metodo di estensione [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):</span><span class="sxs-lookup"><span data-stu-id="fdc1f-167">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="fdc1f-168">Il metodo `GetValue<T>` di ConfigurationBinder consente la specifica di un valore predefinito (80 nell'esempio).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-168">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="fdc1f-169">`GetValue<T>` è per scenari semplici e non viene associato a intere sezioni.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-169">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="fdc1f-170">`GetValue<T>` ottiene valori scalari da `GetSection(key).Value` convertiti in un tipo specifico.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-170">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="fdc1f-171">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="fdc1f-171">Bind to an object graph</span></span>

<span data-ttu-id="fdc1f-172">È possibile impostare associazioni ricorsive a ogni oggetto di una classe.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-172">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="fdc1f-173">Si consideri la classe `AppSettings` seguente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-173">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="fdc1f-174">L'esempio seguente è associato alla classe `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-174">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="fdc1f-175">**ASP.NET Core 1.1** e le versioni successive possono usare `Get<T>`, che funziona con intere sezioni.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-175">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="fdc1f-176">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-176">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="fdc1f-177">L'esempio di codice seguente illustra come usare il metodo `Get<T>` con l'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-177">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="fdc1f-178">Tramite il file *appSettings.json* seguente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-178">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="fdc1f-179">Il programma visualizza `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-179">The program displays `Height 11`.</span></span>

<span data-ttu-id="fdc1f-180">Il codice seguente consente di seguire un unit test della configurazione:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-180">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="fdc1f-181">Creare un provider personalizzato di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="fdc1f-181">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="fdc1f-182">In questa sezione viene creato un provider di configurazione di base che legge le coppie nome-valore da un database tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-182">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="fdc1f-183">Definire un'entità `ConfigurationValue` per l'archiviazione dei valori di configurazione nel database:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-183">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="fdc1f-184">Aggiungere `ConfigurationContext` per archiviare e accedere ai valori configurati:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-184">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="fdc1f-185">Creare una classe che implementi [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="fdc1f-185">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="fdc1f-186">Creare il provider di configurazione personalizzato ereditando da [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-186">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="fdc1f-187">Il provider di configurazione inizializza il database quando è vuoto:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-187">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="fdc1f-188">Quando l'esempio viene eseguito, vengono visualizzati i valori evidenziati del database ("value_from_ef_1" e "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="fdc1f-188">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="fdc1f-189">È possibile usare un metodo di estensione `EFConfigSource` per aggiungere l'origine di configurazione:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-189">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="fdc1f-190">L'esempio di codice seguente illustra come usare il metodo `EFConfigProvider` personalizzato:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-190">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="fdc1f-191">Notare che l'esempio aggiunge l'oggetto personalizzato `EFConfigProvider` dopo il provider JSON, pertanto le impostazioni del database sostituiranno le impostazioni del file *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-191">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="fdc1f-192">Tramite il file *appSettings.json* seguente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-192">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="fdc1f-193">Verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-193">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="fdc1f-194">Provider di configurazione CommandLine</span><span class="sxs-lookup"><span data-stu-id="fdc1f-194">CommandLine configuration provider</span></span>

<span data-ttu-id="fdc1f-195">Il [provider di configurazione CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) riceve coppie chiave-valore di argomenti della riga di comando per la configurazione in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-195">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="fdc1f-196">Visualizzare o scaricare l'esempio di configurazione CommandLine</span><span class="sxs-lookup"><span data-stu-id="fdc1f-196">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="fdc1f-197">Impostazione e uso del provider di configurazione CommandLine</span><span class="sxs-lookup"><span data-stu-id="fdc1f-197">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="fdc1f-198">Configurazione di base</span><span class="sxs-lookup"><span data-stu-id="fdc1f-198">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="fdc1f-199">Per attivare la configurazione della riga di comando, chiamare il metodo di estensione `AddCommandLine` su un'istanza di [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="fdc1f-199">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="fdc1f-200">Eseguendo il codice, verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-200">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="fdc1f-201">Passando coppie chiave-valore degli argomenti nella riga di comando si modificano i valori di `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-201">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="fdc1f-202">Nella finestra della console viene visualizzato:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-202">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="fdc1f-203">Per eseguire l'override della configurazione fornita da altri provider di configurazione con la configurazione della riga di comando, chiamare `AddCommandLine` per ultimo su `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-203">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fdc1f-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fdc1f-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fdc1f-205">Le tipiche app ASP.NET Core 2.x usano il metodo statico `CreateDefaultBuilder` per compilare l'host:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-205">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="fdc1f-206">`CreateDefaultBuilder` carica la configurazione facoltativa da *appSettings.json*, *appsettings.{Environment}.json*, [i segreti utente](xref:security/app-secrets) (nell'ambiente `Development`), le variabili di ambiente e gli argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-206">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="fdc1f-207">Il provider di configurazione CommandLine è chiamato per ultimo.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-207">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="fdc1f-208">Chiamando il provider per ultimo, gli argomenti della riga di comando passati in fase di esecuzione possono eseguire l'override della configurazione impostata da altri provider di configurazione chiamati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-208">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="fdc1f-209">Per i file *appsettings* in cui:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-209">For *appsettings* files where:</span></span>

* <span data-ttu-id="fdc1f-210">`reloadOnChange` è abilitato.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-210">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="fdc1f-211">Contiene la stessa impostazione negli argomenti della riga di comando e un file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-211">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="fdc1f-212">Il file *appsettings* contenente l'argomento della riga di comando corrispondente è stato modificato dopo l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-212">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="fdc1f-213">Se tutte le condizioni precedenti sono vere, gli argomenti della riga di comando vengono sottoposti a override.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-213">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="fdc1f-214">L'app ASP.NET Core 2.x può usare [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) anziché `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-214">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="fdc1f-215">Quando si usa `WebHostBuilder`, impostare manualmente la configurazione con [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-215">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="fdc1f-216">Per altre informazioni, vedere la scheda ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-216">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fdc1f-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fdc1f-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fdc1f-218">Creare un oggetto [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chiamare il metodo `AddCommandLine` per usare il provider di configurazione CommandLine.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-218">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="fdc1f-219">Chiamando il provider per ultimo, gli argomenti della riga di comando passati in fase di esecuzione possono eseguire l'override della configurazione impostata da altri provider di configurazione chiamati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-219">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="fdc1f-220">Applicare la configurazione a [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con il metodo `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-220">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="fdc1f-221">Argomenti</span><span class="sxs-lookup"><span data-stu-id="fdc1f-221">Arguments</span></span>

<span data-ttu-id="fdc1f-222">Gli argomenti passati alla riga di comando devono essere conformi a uno dei due formati indicati nella tabella seguente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-222">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="fdc1f-223">Formato dell'argomento</span><span class="sxs-lookup"><span data-stu-id="fdc1f-223">Argument format</span></span>                                                     | <span data-ttu-id="fdc1f-224">Esempio</span><span class="sxs-lookup"><span data-stu-id="fdc1f-224">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="fdc1f-225">Singolo argomento: una coppia chiave-valore separata da un segno di uguale (`=`)</span><span class="sxs-lookup"><span data-stu-id="fdc1f-225">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="fdc1f-226">Sequenza di due argomenti: una coppia chiave-valore separata da uno spazio</span><span class="sxs-lookup"><span data-stu-id="fdc1f-226">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="fdc1f-227">**Singolo argomento**</span><span class="sxs-lookup"><span data-stu-id="fdc1f-227">**Single argument**</span></span>

<span data-ttu-id="fdc1f-228">Il valore deve seguire un segno di uguale (`=`).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-228">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="fdc1f-229">Il valore può essere null (ad esempio `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-229">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="fdc1f-230">La chiave può avere un prefisso.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-230">The key may have a prefix.</span></span>

| <span data-ttu-id="fdc1f-231">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="fdc1f-231">Key prefix</span></span>               | <span data-ttu-id="fdc1f-232">Esempio</span><span class="sxs-lookup"><span data-stu-id="fdc1f-232">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="fdc1f-233">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="fdc1f-233">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="fdc1f-234">Trattino singolo (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="fdc1f-234">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="fdc1f-235">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="fdc1f-235">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="fdc1f-236">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="fdc1f-236">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="fdc1f-237">&#8224;Una chiave con un prefisso con trattino singolo (`-`) deve essere fornita nei [mapping di sostituzione](#switch-mappings), come descritto di seguito.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-237">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="fdc1f-238">Comando di esempio:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-238">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="fdc1f-239">Nota: se `-key1` non è presente nei [mapping di sostituzione](#switch-mappings) forniti al provider di configurazione, viene generata un'eccezione `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-239">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="fdc1f-240">**Sequenza di due argomenti**</span><span class="sxs-lookup"><span data-stu-id="fdc1f-240">**Sequence of two arguments**</span></span>

<span data-ttu-id="fdc1f-241">Il valore non può essere null e deve seguire la chiave separata da uno spazio.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-241">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="fdc1f-242">La chiave deve avere un prefisso.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-242">The key must have a prefix.</span></span>

| <span data-ttu-id="fdc1f-243">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="fdc1f-243">Key prefix</span></span>               | <span data-ttu-id="fdc1f-244">Esempio</span><span class="sxs-lookup"><span data-stu-id="fdc1f-244">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="fdc1f-245">Trattino singolo (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="fdc1f-245">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="fdc1f-246">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="fdc1f-246">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="fdc1f-247">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="fdc1f-247">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="fdc1f-248">&#8224;Una chiave con un prefisso con trattino singolo (`-`) deve essere fornita nei [mapping di sostituzione](#switch-mappings), come descritto di seguito.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-248">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="fdc1f-249">Comando di esempio:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-249">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="fdc1f-250">Nota: se `-key1` non è presente nei [mapping di sostituzione](#switch-mappings) forniti al provider di configurazione, viene generata un'eccezione `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-250">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="fdc1f-251">Chiavi duplicate</span><span class="sxs-lookup"><span data-stu-id="fdc1f-251">Duplicate keys</span></span>

<span data-ttu-id="fdc1f-252">Se vengono fornite chiavi duplicate, viene usata l'ultima coppia chiave-valore.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-252">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="fdc1f-253">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="fdc1f-253">Switch mappings</span></span>

<span data-ttu-id="fdc1f-254">Quando si compila manualmente la configurazione con `ConfigurationBuilder`, è possibile aggiungere un dizionario di mapping di sostituzione al metodo `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-254">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="fdc1f-255">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-255">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="fdc1f-256">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-256">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="fdc1f-257">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-257">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="fdc1f-258">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-258">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="fdc1f-259">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-259">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="fdc1f-260">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-260">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="fdc1f-261">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-261">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="fdc1f-262">Nell'esempio seguente il metodo `GetSwitchMappings` consente agli argomenti della riga di comando di usare un prefisso di chiave con trattino singolo (`-`) ed evitare i prefissi iniziali nelle sottochiavi.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-262">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="fdc1f-263">Senza fornire argomenti della riga di comando, il dizionario per `AddInMemoryCollection` imposta i valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-263">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="fdc1f-264">Eseguire l'app con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-264">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="fdc1f-265">Nella finestra della console viene visualizzato:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-265">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="fdc1f-266">Per passare le impostazioni di configurazione, usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-266">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="fdc1f-267">Nella finestra della console viene visualizzato:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-267">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="fdc1f-268">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-268">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="fdc1f-269">Chiave</span><span class="sxs-lookup"><span data-stu-id="fdc1f-269">Key</span></span>            | <span data-ttu-id="fdc1f-270">Valore</span><span class="sxs-lookup"><span data-stu-id="fdc1f-270">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="fdc1f-271">Per illustrare la sostituzione della chiave tramite il dizionario, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-271">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="fdc1f-272">Le chiavi della riga di comando vengono scambiate.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-272">The command-line keys are swapped.</span></span> <span data-ttu-id="fdc1f-273">Nella finestra della console vengono visualizzati i valori di configurazione per `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-273">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="fdc1f-274">File web.config</span><span class="sxs-lookup"><span data-stu-id="fdc1f-274">web.config file</span></span>

<span data-ttu-id="fdc1f-275">Un file *web.config* è obbligatorio per l'hosting di app in IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-275">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="fdc1f-276">Le impostazioni in *web.config* consentono al [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) di avviare l'app e configurare altre impostazioni e moduli di IIS.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-276">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="fdc1f-277">Se il file *web.config* file non è presente e il file di progetto include `<Project Sdk="Microsoft.NET.Sdk.Web">`, la pubblicazione del progetto crea un file *web.config* nell'output pubblicato (cartella *publish*).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-277">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="fdc1f-278">Per altre informazioni, vedere [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file) (Host ASP.NET Core in Windows con IIS).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-278">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="fdc1f-279">Accesso alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="fdc1f-279">Accessing configuration during startup</span></span>

<span data-ttu-id="fdc1f-280">Per accedere alla configurazione in `ConfigureServices` o `Configure` durante l'avvio, vedere gli esempi nell'argomento [Avvio dell'applicazione](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="fdc1f-280">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="fdc1f-281">Note aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fdc1f-281">Additional notes</span></span>

* <span data-ttu-id="fdc1f-282">L'inserimento delle dipendenze non viene impostato fino quando non viene richiamato `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-282">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="fdc1f-283">Il sistema di configurazione non riconosce l'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-283">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="fdc1f-284">`IConfiguration` ha due specializzazioni:</span><span class="sxs-lookup"><span data-stu-id="fdc1f-284">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="fdc1f-285">`IConfigurationRoot` Usata per il nodo radice.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-285">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="fdc1f-286">Può attivare un ricaricamento.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-286">Can trigger a reload.</span></span>
  * <span data-ttu-id="fdc1f-287">`IConfigurationSection` Rappresenta una sezione dei valori di configurazione.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-287">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="fdc1f-288">I metodi `GetSection` e `GetChildren` restituiscono un oggetto `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-288">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="fdc1f-289">Usare [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) durante il ricaricamento della configurazione o per l'accesso a ogni provider.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-289">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="fdc1f-290">Non si tratta di situazioni comuni.</span><span class="sxs-lookup"><span data-stu-id="fdc1f-290">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fdc1f-291">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fdc1f-291">Additional resources</span></span>

* [<span data-ttu-id="fdc1f-292">Opzioni</span><span class="sxs-lookup"><span data-stu-id="fdc1f-292">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="fdc1f-293">Uso di più ambienti</span><span class="sxs-lookup"><span data-stu-id="fdc1f-293">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="fdc1f-294">Archiviazione sicura di segreti dell'app durante lo sviluppo</span><span class="sxs-lookup"><span data-stu-id="fdc1f-294">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="fdc1f-295">Hosting in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdc1f-295">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="fdc1f-296">Inserimento dipendenze</span><span class="sxs-lookup"><span data-stu-id="fdc1f-296">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* <span data-ttu-id="fdc1f-297">[Azure Key Vault configuration provider](xref:security/key-vault-configuration) (Provider di configurazione di Azure Key Vault)</span><span class="sxs-lookup"><span data-stu-id="fdc1f-297">[Azure Key Vault configuration provider](xref:security/key-vault-configuration)</span></span>
