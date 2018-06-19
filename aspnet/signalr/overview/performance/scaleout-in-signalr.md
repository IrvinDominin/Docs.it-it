---
uid: signalr/overview/performance/scaleout-in-signalr
title: Introduzione alla scalabilità orizzontale in SignalR | Documenti Microsoft
author: MikeWasson
description: Versioni del software utilizzato in questo argomento di Visual Studio 2013 .NET 4.5 SignalR le versioni precedenti di versione 2 di questo argomento per informazioni sulle versioni precedenti di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: f1d15250682305f6d0512b72bd2e40cb4a8a18e5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034594"
---
<a name="introduction-to-scaleout-in-signalr"></a>Introduzione alla scalabilità orizzontale in SignalR
====================
da [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


In generale, esistono due modi per scalare un'applicazione web: *scalabilità verticale* e *scalabilità*.

- La scalabilità verticale indica l'utilizzo di un server più grande (o una VM più grande) con più RAM, CPU e così via.
- Scalabilità orizzontale implica l'inserimento di più server per gestire il carico.

Il problema con la scalabilità verticale è che si è rapidamente raggiunto un limite per le dimensioni della macchina. Tuttavia, è necessario per scalare in orizzontale. Tuttavia, quando scala in orizzontale, i client possono ottenere indirizzati a server diversi. Un client connesso a un server non riceverà i messaggi inviati da un altro server.

![](scaleout-in-signalr/_static/image1.png)

Una soluzione consiste nell'inoltrare i messaggi tra server tramite un componente chiamato un *backplane*. Con un backplane è abilitato, ogni istanza dell'applicazione invia messaggi al backplane e li inoltra backplane alle altre istanze dell'applicazione. (In elettronica, un backplane è un gruppo di connettori paralleli. Per analogia, un backplane SignalR si connette più server.)

![](scaleout-in-signalr/_static/image2.png)

SignalR fornisce attualmente i ripiani posteriori tre delle:

- **Bus di servizio Azure**. Bus di servizio è un'infrastruttura di messaggistica che consente di inviare messaggi in modalità ad accoppiamento tra componenti.
- **Redis**. Redis è un archivio chiave-valore in memoria. Redis supporta un modello di pubblicazione/sottoscrizione ("pubblicazione/sottoscrizione") per l'invio di messaggi.
- **SQL Server**. SQL Server backplane scrive i messaggi alle tabelle SQL. Backplane utilizza Service Broker per la messaggistica efficiente. Tuttavia, funziona anche se Service Broker non è abilitato.

Se si distribuisce l'applicazione in Azure, è consigliabile utilizzare il backplane Redis utilizzando [Cache Redis di Azure](https://azure.microsoft.com/services/cache/). Se si distribuiscono alla farm di server, provare a SQL Server o i ripiani posteriori delle Redis.

Gli argomenti seguenti contengono esercitazioni dettagliate per ogni backplane:

- [Scalabilità orizzontale di SignalR con il bus di servizio di Azure](scaleout-with-windows-azure-service-bus.md)
- [Scalabilità orizzontale di SignalR con Redis](scaleout-with-redis.md)
- [Scalabilità orizzontale di SignalR con SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementazione

In SignalR, ogni messaggio viene inviato tramite un bus di messaggi. Implementa un bus di messaggi di [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfaccia, che fornisce un'astrazione di pubblicazione/sottoscrizione. Sostituendo il valore predefinito di lavoro dei ripiani posteriori delle **IMessageBus** con un bus progettato per tale backplane. Ad esempio, il bus di messaggi per Redis è [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx), e utilizza Redis [pub/sub](http://redis.io/topics/pubsub) meccanismo per inviare e ricevere messaggi.

Ogni istanza del server si connette al backplane tramite il bus. Quando viene inviato un messaggio, passa al backplane e backplane invia ogni server. Quando un server riceve un messaggio da backplane, inserisce il messaggio nella propria cache locale. Il server recapita i messaggi per i client dalla cache locale.

Per ogni connessione di client, lo stato di avanzamento del client nella lettura del flusso di messaggio verrà rilevata tramite un cursore. (Un cursore rappresenta una posizione nel flusso di messaggio). Se un client si disconnette e quindi si riconnette, viene chiesto il bus per tutti i messaggi ricevuti dopo il valore del cursore del client. Lo stesso avviene quando si utilizza una connessione [polling lungo](../getting-started/introduction-to-signalr.md#transports). Dopo il completamento di una richiesta di polling lungo, il client apre una nuova connessione e chiede i messaggi ricevuti dopo il cursore.

Il funzionamento del meccanismo cursore anche se un client viene indirizzato a un server diverso in riconnessione. Backplane è a conoscenza di tutti i server e un client si connette al server non è rilevante.

## <a name="limitations"></a>Limitazioni

Con un backplane, la velocità effettiva massima del messaggio è inferiore rispetto a quando i client di comunicare direttamente con un singolo nodo del server. Ciò avviene perché backplane inoltra ogni messaggio a ogni nodo, in modo backplane può diventare un collo di bottiglia. Se questa limitazione è un problema dipende dall'applicazione. Ad esempio, ecco alcuni scenari tipici di SignalR:

- [Trasmissione server](../getting-started/tutorial-server-broadcast-with-signalr.md) (ad esempio, le quotazioni di borsa): i ripiani posteriori delle funziona anche per questo scenario, perché il server controlla la frequenza con cui vengono inviati i messaggi.
- [Client-client](../getting-started/tutorial-getting-started-with-signalr.md) (ad esempio, chat): In questo scenario, backplane potrebbe essere un collo di bottiglia se il numero di messaggi viene ridimensionato con il numero di client; ovvero se si estende la quantità di messaggi client proporzionalmente all'aumentare del join.
- [In tempo reale ad alta frequenza](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (ad esempio, giochi in tempo reale): un backplane non è consigliato per questo scenario.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Abilitazione della traccia per la scalabilità orizzontale SignalR

Per abilitare la traccia per il ripiani posteriori delle, aggiungere le sezioni seguenti al file Web. config nella directory radice **configurazione** elemento:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
