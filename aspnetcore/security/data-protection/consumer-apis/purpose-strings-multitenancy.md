---
title: Gerarchia scopo e multi-tenancy in ASP.NET Core
author: rick-anderson
description: Informazioni sulle funzionalità multi-tenancy e scopo stringa gerarchia, tra cui le API di protezione dati di ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: f0c39d54c164595c2135e0eb0d911796e215dd66
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273611"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Gerarchia scopo e multi-tenancy in ASP.NET Core

Poiché un `IDataProtector` è inoltre in modo implicito un `IDataProtectionProvider`, a scopo può essere concatenato. In questo senso, `provider.CreateProtector([ "purpose1", "purpose2" ])` equivale a `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

In questo modo per alcune relazioni gerarchiche interessanti attraverso il sistema di protezione dati. Nell'esempio precedente di [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), può chiamare il componente SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` dedicano una sola volta e memorizza nella cache il risultato in una privata `_myProvide` campo. Protezioni future possono quindi essere creati tramite chiamate a `_myProvider.CreateProtector("User: username")`, e queste protezioni possono essere utilizzate per la protezione dei singoli messaggi.

Questo può anche essere invertito. Prendere in considerazione una singola applicazione logica quali host più tenant (un CMS sembra ragionevole) e ogni tenant può essere configurato con un proprio sistema di gestione di autenticazione e lo stato. L'applicazione di tipo generico è un unico provider master, e chiama `provider.CreateProtector("Tenant 1")` e `provider.CreateProtector("Tenant 2")` per concedere a ogni tenant periodo isolato del sistema di protezione dati. I tenant Impossibile derivare le proprie singoli programmi di protezione in base alle proprie esigenze, ma indipendentemente dalla modalità tentano non possono creare protezioni con cui sono in conflitto con un altro tenant nel sistema. Graficamente, rappresentati come indicato di seguito.

![A scopo di tenancy multipla](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Si presuppone che il generico controlli dell'applicazione, le API sono disponibili ai singoli tenant e che i tenant non è possibile eseguire codice arbitrario nel server. Se un tenant può eseguire codice arbitrario, è stato possibile eseguire la reflection privata per interrompere le garanzie di isolamento oppure può solo leggere direttamente il materiale della chiave master e derivare il sottochiavi lo desiderano.

Il sistema di protezione dati utilizza effettivamente una sorta di multi-tenancy nella configurazione predefinita della casella. Per impostazione predefinita materiale della chiave master verrà archiviato nella cartella del profilo utente dell'account del processo di lavoro (o il Registro di sistema per le identità del pool di applicazioni IIS). Ma è piuttosto comune per utilizzare un singolo account per l'esecuzione di più applicazioni e pertanto tutte queste applicazioni interromperebbe la condivisione di materiale per le chiavi master. Per risolvere questo problema, il sistema di protezione dati consente di inserire automaticamente un identificatore univoco per ogni applicazione come primo elemento nella catena di scopo complessivo. Funzione implicita serve a [isolare le singole applicazioni](xref:security/data-protection/configuration/overview#per-application-isolation) uno da altro in modo efficace considerando ogni applicazione come un tenant univoco all'interno del sistema e il processo di creazione di protezione è identico alla figura precedente.
