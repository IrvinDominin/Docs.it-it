[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Il codice precedente:

* Definisce una classe controller vuota. Nelle sezioni successive vengono aggiunti i metodi per implementare l'API.
* Il costruttore usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`TodoContext `) nel controller. Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.
* Se non esiste, il costruttore aggiunge un elemento al database in memoria.

## <a name="getting-to-do-items"></a>Recupero degli elementi attività

Per ottenere gli elementi attività, aggiungere i metodi seguenti alla classe `TodoController`.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Questi metodi implementano i due metodi GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Di seguito viene riportata una risposta HTTP di esempio per il metodo `GetAll`:

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

Più avanti nell'esercitazione verrà illustrato come può essere visualizzata la risposta HTTP con [Postman](https://www.getpostman.com/) o [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Routing e percorsi di URL

L'attributo `[HttpGet]` specifica un metodo HTTP GET. Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:

* Prendere la stringa di modello nell'attributo `Route` del controller:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* Sostituire `[controller]` con il nome del controller, ovvero il nome della classe controller meno il suffisso "Controller". In questo esempio, il nome della classe controller è **Todo**Controller e il nome della radice è "todo". Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.
* Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("/products")]`, aggiungerlo al percorso. In questo esempio non si usa un modello. Per altre informazioni, vedere [Routing degli attributi con attributi Http [verbo]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Nel metodo `GetById`:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

`"{id}"` è una variabile segnaposto per l'ID dell'elemento `todo`. Quando `GetById` viene richiamato, assegna il valore di "{id}" nell'URL al parametro `id` del metodo.

`Name = "GetTodo"` crea una route denominata. Le route denominate:

* Consentono all'app di creare un collegamento HTTP usando il nome della route.
* Sono spiegate più avanti nell'esercitazione.

### <a name="return-values"></a>Valori restituiti

Il metodo `GetAll` restituisce un tipo `IEnumerable`. MVC serializza automaticamente l'oggetto su [JSON](http://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta. Il codice di risposta per questo metodo è 200, presupponendo che non esistano eccezioni non gestite. Le eccezioni non gestite vengono convertite in errori 5xx.

Al contrario, il metodo `GetById` restituisce il tipo `IActionResult` più generale, che rappresenta un'ampia gamma di tipi restituiti. `GetById` presenta due tipi restituiti diversi:

* Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un errore 404. La restituzione di `NotFound` restituisce una risposta HTTP 404.

* In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON. La restituzione di `ObjectResult` restituisce una risposta HTTP 200.
