---
title: Layout in ASP.NET Core
author: ardalis
description: Informazioni su come usare layout comuni, condividere direttive ed eseguire codice comune prima di eseguire il rendering delle visualizzazioni in un'applicazione ASP.NET Core.
ms.author: riande
ms.date: 07/30/2019
no-loc:
- appsettings.json
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/layout
ms.openlocfilehash: 502df268e7f5f33acfffccd5ec0bd65267fa12da
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2020
ms.locfileid: "93060976"
---
# <a name="layout-in-aspnet-core"></a>Layout in ASP.NET Core

Di [Steve Smith](https://ardalis.com/) e [Dave Brock](https://twitter.com/daveabrock)

Le pagine e le visualizzazioni condividono spesso elementi visivi e programmatici. Questo articolo illustra come:

* Usare layout comuni.
* Condividere direttive.
* Eseguire codice comune prima del rendering di pagine o visualizzazioni.

Questo documento illustra i layout per i due diversi approcci per ASP.NET Core MVC: Razor pagine e controller con le visualizzazioni. Per questo argomento le differenze siano minime:

* Razor Le pagine si trovano nella cartella *pages* .
* I controller con visualizzazioni usano una cartella *Views* per le visualizzazioni.

## <a name="what-is-a-layout"></a>Che cos'è il layout?

La maggior parte delle applicazioni web presenta un layout comune che fornisce all'utente un'esperienza omogenea, nel passare da una pagina a un'altra. In genere, il layout comprende elementi dell'interfaccia utente comune, ad esempio l'intestazione dell'app, la navigazione o elementi di menu e piè di pagina.

![Esempio di layout di pagina](layout/_static/page-layout.png)

Anche le strutture HTML comuni, ad esempio gli script e i fogli di stile, vengono spesso usate da molte pagine all'interno di un'app. Tutti questi elementi condivisi possono essere definiti in un file di *layout* , a cui è possibile fare riferimento da qualsiasi visualizzazione usata nell'app. I layout riducono il codice duplicato nelle visualizzazioni.

Per convenzione, il layout predefinito per un'app ASP.NET Core è denominato *_Layout.cshtml* . I file di layout per i nuovi progetti ASP.NET Core creati con i modelli sono:

* Razor Pagine: *pagine/condivise/_Layout. cshtml*

  ![Cartella Pages in Esplora soluzioni](layout/_static/rp-web-project-views.png)

* Controller con visualizzazioni: *Views/Shared/_Layout.cshtml*

  ![Cartella Views in Esplora soluzioni](layout/_static/mvc-web-project-views.png)

Il layout definisce un modello di primo livello per le visualizzazioni nell'app. Le app non richiedono un layout. Le app possono definire più di un layout, con visualizzazioni diverse che specificano layout diversi.

Il codice seguente mostra il file di layout per un progetto creato da modello con un controller e visualizzazioni:

[!code-cshtml[](~/common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=44,72)]

## <a name="specifying-a-layout"></a>Definizione di un layout

Razor le visualizzazioni hanno una `Layout` Proprietà. Le visualizzazioni singole specificano un layout impostando la seguente proprietà:

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

Il layout specificato può usare un percorso completo (ad esempio, */Pages/Shared/_Layout.cshtml* o */Views/Shared/_Layout.cshtml* ) oppure un nome parziale (ad esempio: `_Layout`). Quando viene specificato un nome parziale, il Razor motore di visualizzazione Cerca il file di layout utilizzando il processo di individuazione standard. La ricerca viene eseguita prima nella cartella in cui è presente il metodo gestore (o controller) e poi nella cartella *Shared* . Questo processo di individuazione è identico a quello usato per individuare le [visualizzazioni parziali](xref:mvc/views/partial#partial-view-discovery).

Per impostazione predefinita, ogni layout deve chiamare `RenderBody`. Quando viene eseguita la chiamata a `RenderBody`, viene eseguito il rendering del contenuto della visualizzazione.

<a name="layout-sections-label"></a>
<!-- https://stackoverflow.com/questions/23327578 -->
### <a name="sections"></a>Sezioni

Un layout può facoltativamente fare riferimento a una o più *sezioni* , chiamando `RenderSection`. Le sezioni forniscono un modo per organizzare la posizione in cui devono essere inseriti determinati elementi di pagina. Ogni chiamata a `RenderSection` può specificare se tale sezione è obbligatoria o facoltativa:

```html
<script type="text/javascript" src="~/scripts/global.js"></script>

@RenderSection("Scripts", required: false)
```

Se una sezione richiesta non viene trovata, viene generata un'eccezione. Le singole visualizzazioni specificano il contenuto di cui eseguire il rendering all'interno di una sezione usando la `@section` Razor sintassi. Se una pagina o una visualizzazione definisce una sezione, è necessario eseguirne il rendering (o si verifica un errore).

Una `@section` definizione di esempio nella Razor visualizzazione pagine:

```html
@section Scripts {
     <script type="text/javascript" src="~/scripts/main.js"></script>
}
```

Nel codice precedente, *scripts/main.js* viene aggiunto alla sezione `scripts` in una pagina o visualizzazione. Altre pagine o visualizzazioni nella stessa app potrebbero non richiedere questo script e non definire una sezione scripts.

Il markup seguente usa l' [helper tag parziale](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) per eseguire il rendering di *_ValidationScriptsPartial.cshtml* :

```html
@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

Il markup precedente è stato generato dall' [impalcatura Identity ](xref:security/authentication/scaffold-identity).

Le sezioni definite in una pagina o in una visualizzazione sono disponibili solo nella relativa pagina di layout immediato. Non è possibile farvi riferimento da righe parzialmente eseguite, componenti di visualizzazione o altre parti del sistema di visualizzazione.

### <a name="ignoring-sections"></a>Esclusione di sezioni

Per impostazione predefinita, la pagina di layout deve eseguire il rendering della parte principale e di tutte le sezioni di una pagina di contenuto. Il Razor motore di visualizzazione impone questo oggetto tenendo traccia dell'eventuale rendering del corpo e di ogni sezione.

Per indicare al motore di visualizzazione di escludere la parte principale o le sezioni, chiamare i metodi `IgnoreBody` e `IgnoreSection`.

Il corpo e ogni sezione di una Razor pagina devono essere sottoposti a rendering o ignorati.

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>Importazione delle direttive condivise

Le viste e le pagine possono utilizzare Razor le direttive per importare gli spazi dei nomi e utilizzare l' [inserimento delle dipendenze](dependency-injection.md). Le direttive condivise da numerose visualizzazioni possono essere specificate in un file *_ViewImports.cshtml* comune. Il file `_ViewImports` supporta le direttive seguenti:

* `@addTagHelper`
* `@removeTagHelper`
* `@tagHelperPrefix`
* `@using`
* `@model`
* `@inherits`
* `@inject`

Il file non supporta altre Razor funzionalità, ad esempio le funzioni e le definizioni delle sezioni.

Esempio di file `_ViewImports.cshtml`:

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

Il file *_ViewImports.cshtml* per un'app ASP.NET Core MVC viene generalmente posizionato nella cartella *Pages* (o *Views* ). È possibile posizionare un file *_ViewImports.cshtml* in qualsiasi cartella, nel qual caso verrà applicato unicamente alle pagine o alle visualizzazioni all'interno di tale cartella e nelle relative sottocartelle. I file `_ViewImports` vengono elaborati a partire dal livello radice e quindi per ogni cartella fino alla posizione della pagina o della visualizzazione stessa. Le impostazioni `_ViewImports` specificate al livello radice possono essere sottoposto a override a livello di cartella.

Si supponga ad esempio che:

* Il file *_ViewImports.cshtml* al livello radice includa `@model MyModel1` e `@addTagHelper *, MyTagHelper1`.
* Un file *_ViewImports.cshtml* in una sottocartella includa `@model MyModel2` e `@addTagHelper *, MyTagHelper2`.

Le pagine e le visualizzazioni nella sottocartella avranno accesso sia agli helper tag che al modello `MyModel2`.

Se vengono trovati più file *_ViewImports.cshtml* nella gerarchia di file, il comportamento combinato delle direttive è il seguente:

* `@addTagHelper`, `@removeTagHelper`: eseguiti nell'ordine
* `@tagHelperPrefix`: il file più vicino alla visualizzazione esegue l'override di tutti gli altri
* `@model`: il file più vicino alla visualizzazione esegue l'override di tutti gli altri
* `@inherits`: il file più vicino alla visualizzazione esegue l'override di tutti gli altri
* `@using`: vengono inclusi tutti; i duplicati vengono esclusi
* `@inject`: per ogni proprietà, quella più vicina alla visualizzazione esegue l'override di tutte le altre con lo stesso nome di proprietà

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>Esecuzione di codice prima di ogni visualizzazione

Il codice che deve essere eseguito prima di ogni visualizzazione o pagina deve essere posizionato nel file *_ViewStart.cshtml* . Per convenzione, il file *_ViewStart.cshtml* si trova nella cartella *Pages* (o *Views* ). Le istruzioni elencate in *_ViewStart.cshtml* vengono eseguite prima di ogni visualizzazione completa (non dei layout e delle visualizzazioni parziali). Come [ViewImports.cshtml](xref:mvc/views/layout#viewimports), *_ViewStart.cshtml* è gerarchico. Se un file *_ViewStart.cshtml* viene definito nella cartella delle visualizzazioni o delle pagine, verrà eseguito dopo quello definito nella radice della cartella *Pages* (o *Views* ) (se presente).

File *_ViewStart.cshtml* di esempio:

[!code-cshtml[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

Il file precedente specifica che tutte le visualizzazioni useranno il layout *_Layout.cshtml* .

*_ViewStart.cshtml* e *_ViewImports.cshtml***non** vengono in genere posizionati nella cartella */Pages/Shared* (o */Views/Shared* ). Le versioni a livello di app di questi file devono essere posizionate direttamente nella cartella */Pages* (o */Views* ).
