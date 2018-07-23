<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="ee115-101">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="ee115-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="ee115-102">Eseguire il codice seguente dalla riga di comando nella directory del progetto che contiene i file *Program.cs*, *Startup.cs* e *csproj*:</span><span class="sxs-lookup"><span data-stu-id="ee115-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="ee115-103">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="ee115-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="ee115-104">L'errore precedente si verifica quando ci si trova in una directory errata.</span><span class="sxs-lookup"><span data-stu-id="ee115-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="ee115-105">Aprire una shell dei comandi nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*) e quindi eseguire il comando precedente.</span><span class="sxs-lookup"><span data-stu-id="ee115-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="ee115-106">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="ee115-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="ee115-107">Chiudere Visual Studio ed eseguire nuovamente il comando.</span><span class="sxs-lookup"><span data-stu-id="ee115-107">Exit Visual Studio and run the command again.</span></span>
