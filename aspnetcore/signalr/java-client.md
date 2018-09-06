---
title: Client ASP.NET Core SignalR Java
author: mikaelm12
description: Informazioni su come usare il client Java di ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: f110f5391ac34f5cb4a72f64c16d86c8a37369a2
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/06/2018
ms.locfileid: "43995415"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="fd3ca-103">ASP.NET Core SignalR Java Client</span><span class="sxs-lookup"><span data-stu-id="fd3ca-103">ASP.NET Core SignalR Java Client</span></span>

<span data-ttu-id="fd3ca-104">Da [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="fd3ca-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="fd3ca-105">Il client Java consente la connessione a un server di ASP.NET Core SignalR dal codice Java, incluse le app Android.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="fd3ca-106">Ad esempio la [client JavaScript](xref:signalr/javascript-client) e il [client .NET](xref:signalr/dotnet-client), il client Java consente di ricevere e inviare messaggi a un hub in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="fd3ca-107">Il client Java è disponibile in ASP.NET Core 2.2 e successive.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="fd3ca-108">L'app console Java di esempio citato nel presente articolo usa il client Java di SignalR.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="fd3ca-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fd3ca-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="fd3ca-110">Installare il pacchetto client Java di SignalR</span><span class="sxs-lookup"><span data-stu-id="fd3ca-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="fd3ca-111">Il *signalr-0.1.0-preview1-35029* file con estensione JAR consente ai client di connettersi a un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-111">The *signalr-0.1.0-preview1-35029* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="fd3ca-112">Per trovare il numero di versione di file con estensione JAR più recente, vedere la [risultati della ricerca di Maven](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span><span class="sxs-lookup"><span data-stu-id="fd3ca-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span></span>

<span data-ttu-id="fd3ca-113">Se si usa Gradle, aggiungere la riga seguente al `dependencies` sezione il *Build. gradle* file:</span><span class="sxs-lookup"><span data-stu-id="fd3ca-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview1-35029'
```

<span data-ttu-id="fd3ca-114">Se con Maven, aggiungere le seguenti righe all'interno di `<dependencies>` elemento delle *POM. XML* file:</span><span class="sxs-lookup"><span data-stu-id="fd3ca-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="fd3ca-115">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="fd3ca-115">Connect to a hub</span></span>

<span data-ttu-id="fd3ca-116">Per stabilire una `HubConnection`, il `HubConnectionBuilder` deve essere utilizzato.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="fd3ca-117">Il livello di log e URL hub può essere configurato durante la compilazione di una connessione.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="fd3ca-118">Configurare le opzioni necessarie, chiamare uno dei `HubConnectionBuilder` metodi prima `build`.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="fd3ca-119">Avviare la connessione con `start`.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="fd3ca-120">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="fd3ca-120">Call hub methods from client</span></span>

<span data-ttu-id="fd3ca-121">Una chiamata a `send` richiama un metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="fd3ca-122">Passare il nome del metodo dell'hub e gli argomenti definiti nel metodo dell'hub per `send`.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="fd3ca-123">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="fd3ca-123">Call client methods from hub</span></span>

<span data-ttu-id="fd3ca-124">Usare `hubConnection.on` per definire i metodi sul client che può chiamare l'hub.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="fd3ca-125">Definire i metodi dopo la compilazione, ma prima di avviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a><span data-ttu-id="fd3ca-126">Limitazioni note</span><span class="sxs-lookup"><span data-stu-id="fd3ca-126">Known limitations</span></span>

<span data-ttu-id="fd3ca-127">Si tratta di una versione di anteprima preliminare del client Java.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-127">This is an early preview release of the Java client.</span></span> <span data-ttu-id="fd3ca-128">Esistono molte funzionalità che non sono ancora supportate.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-128">There are many features that aren't supported yet.</span></span> <span data-ttu-id="fd3ca-129">I seguenti gap si sta lavorando per le versioni successive:</span><span class="sxs-lookup"><span data-stu-id="fd3ca-129">The following gaps are being worked on for future releases:</span></span>

* <span data-ttu-id="fd3ca-130">Solo i tipi primitivi possono essere accettati in quanto i parametri e i tipi restituiscono.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-130">Only primitive types can be accepted as parameters and return types.</span></span>
* <span data-ttu-id="fd3ca-131">Le API sono sincrone.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-131">The APIs are synchronous.</span></span>
* <span data-ttu-id="fd3ca-132">In questo momento è supportato solo il tipo di chiamata "Send".</span><span class="sxs-lookup"><span data-stu-id="fd3ca-132">Only the "Send" call type is supported at this time.</span></span> <span data-ttu-id="fd3ca-133">"Invoke" e lo streaming dei valori restituiti non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-133">"Invoke" and streaming of return values aren't supported.</span></span>
* <span data-ttu-id="fd3ca-134">Il client non supporta attualmente le [servizio Azure SignalR](/azure/azure-signalr/).</span><span class="sxs-lookup"><span data-stu-id="fd3ca-134">The client doesn't currently support the [Azure SignalR Service](/azure/azure-signalr/).</span></span>
* <span data-ttu-id="fd3ca-135">È supportato solo il protocollo JSON.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="fd3ca-136">È supportato solo il trasporto WebSocket.</span><span class="sxs-lookup"><span data-stu-id="fd3ca-136">Only the WebSockets transport is supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fd3ca-137">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fd3ca-137">Additional resources</span></span>

* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>