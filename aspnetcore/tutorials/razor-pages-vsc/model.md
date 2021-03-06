---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core con Visual Studio Code
author: rick-anderson
description: Informazioni su come aggiungere un modello a un'app Razor Pages in ASP.NET Core con Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: b891b921baf1fe6d167c7bfb8b4c5278ce9fe9f5
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055864"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Aggiungere un modello a un'app Razor Pages in ASP.NET Core con Visual Studio Code

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Aggiungere un modello di dati

* Aggiungere una cartella denominata *Modelli*.
* Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.
* Aggiungere il codice seguente al file *Modelli/Movie.cs*:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a>Pacchetto NuGet di Entity Framework Core per SQLite

Dalla riga di comando, eseguire il seguente comando dell'interfaccia della riga di comando di .NET Core:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Compilare il progetto per verificare che non ci siano errori.

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacchetti NuGet di Entity Framework Core per le migrazioni

Gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Per installare questo pacchetto, aggiungerlo alla collezione `DotNetCliToolReference` nel file *.csproj*. **Nota:** è necessario installare questo pacchetto modificando il file *.csproj* ; non è possibile utilizzare il comando `install-package` o la GUI di gestione di pacchetti.

Modificare il file *RazorPagesMovie.csproj*:

* Selezionare **File** > **Apri file** e selezionare il file *RazorPagesMovie.csproj*.
* Aggiungere il riferimento allo strumento per `Microsoft.EntityFrameworkCore.Tools.DotNet` al secondo **\<ItemGroup >**:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Eseguire lo scaffolding del modello di filmato

* Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).
* Eseguire il comando seguente:

**Nota: eseguire il comando riportato di seguito su Windows. Per MacOS e Linux, vedere il comando successivo**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* In MacOS e Linux, eseguire il comando seguente:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Se viene visualizzato l'errore:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Chiudere Visual Studio ed eseguire nuovamente il comando.

[!INCLUDE [model 4](../../includes/RP/model4.md)]

L'esercitazione successiva illustra i file creati tramite scaffolding.

> [!div class="step-by-step"]
> [Precedente: Introduzione](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages-vsc/page)
