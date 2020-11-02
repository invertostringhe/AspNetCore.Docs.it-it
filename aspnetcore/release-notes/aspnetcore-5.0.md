---
title: Novità di ASP.NET Core 5,0
author: rick-anderson
description: Informazioni sulle nuove funzionalità di ASP.NET Core 5,0.
ms.author: riande
ms.custom: mvc
ms.date: 10/29/2020
no-loc:
- ':::no-loc(ASP.NET Core Identity):::'
- ':::no-loc(cookie):::'
- ':::no-loc(Cookie):::'
- ':::no-loc(Blazor):::'
- ':::no-loc(Blazor Server):::'
- ':::no-loc(Blazor WebAssembly):::'
- ':::no-loc(Identity):::'
- ":::no-loc(Let's Encrypt):::"
- ':::no-loc(Razor):::'
- ':::no-loc(SignalR):::'
- ':::no-loc(Kestrel):::'
uid: aspnetcore-5.0
ms.openlocfilehash: e9c74f7b45ebcdffc19a0483b4e98ad2f44d5747
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2020
ms.locfileid: "93061777"
---
# <a name="whats-new-in-aspnet-core-50"></a><span data-ttu-id="9404c-103">Novità di ASP.NET Core 5,0</span><span class="sxs-lookup"><span data-stu-id="9404c-103">What's new in ASP.NET Core 5.0</span></span>

<span data-ttu-id="9404c-104">In questo articolo vengono evidenziate le modifiche più significative in ASP.NET Core 5,0 con i collegamenti alla documentazione pertinente.</span><span class="sxs-lookup"><span data-stu-id="9404c-104">This article highlights the most significant changes in ASP.NET Core 5.0 with links to relevant documentation.</span></span>

## <a name="aspnet-core-mvc-and-no-locrazor-improvements"></a><span data-ttu-id="9404c-105">ASP.NET Core MVC e :::no-loc(Razor)::: miglioramenti</span><span class="sxs-lookup"><span data-stu-id="9404c-105">ASP.NET Core MVC and :::no-loc(Razor)::: improvements</span></span>

### <a name="model-binding-datetime-as-utc"></a><span data-ttu-id="9404c-106">Associazione di modelli DateTime come ora UTC</span><span class="sxs-lookup"><span data-stu-id="9404c-106">Model binding DateTime as UTC</span></span>

<span data-ttu-id="9404c-107">L'associazione di modelli ora supporta l'associazione di stringhe di ora UTC a `DateTime` .</span><span class="sxs-lookup"><span data-stu-id="9404c-107">Model binding now supports binding UTC time strings to `DateTime`.</span></span> <span data-ttu-id="9404c-108">Se la richiesta contiene una stringa di ora UTC, l'associazione di modelli lo associa a un formato UTC `DateTime` .</span><span class="sxs-lookup"><span data-stu-id="9404c-108">If the request contains a UTC time string, model binding binds it to a UTC `DateTime`.</span></span> <span data-ttu-id="9404c-109">Ad esempio, la stringa temporale seguente viene associata all'ora UTC `DateTime` : `https://example.com/mycontroller/myaction?time=2019-06-14T02%3A30%3A04.0576719Z`</span><span class="sxs-lookup"><span data-stu-id="9404c-109">For example, the following time string is bound the UTC `DateTime`: `https://example.com/mycontroller/myaction?time=2019-06-14T02%3A30%3A04.0576719Z`</span></span>

### <a name="model-binding-and-validation-with-c-9-record-types"></a><span data-ttu-id="9404c-110">Associazione di modelli e convalida con i tipi di record C# 9</span><span class="sxs-lookup"><span data-stu-id="9404c-110">Model binding and validation with C# 9 record types</span></span>

<span data-ttu-id="9404c-111">I [tipi di record C# 9](/dotnet/csharp/whats-new/csharp-9#record-types) possono essere usati con l'associazione di modelli in un controller MVC o in una :::no-loc(Razor)::: pagina.</span><span class="sxs-lookup"><span data-stu-id="9404c-111">[C# 9 record types](/dotnet/csharp/whats-new/csharp-9#record-types) can be used with model binding in an MVC controller or a :::no-loc(Razor)::: Page.</span></span> <span data-ttu-id="9404c-112">I tipi di record rappresentano un modo efficace per modellare i dati trasmessi in rete.</span><span class="sxs-lookup"><span data-stu-id="9404c-112">Record types are a good way to model data being transmitted over the network.</span></span>

<span data-ttu-id="9404c-113">Il codice seguente, ad esempio, `PersonController` Usa il `Person` tipo di record con associazione di modelli e convalida dei moduli:</span><span class="sxs-lookup"><span data-stu-id="9404c-113">For example, the following `PersonController` uses the `Person` record type with model binding and form validation:</span></span>

```csharp
public record Person([Required] string Name, [Range(0, 150)] int Age);

public class PersonController
{
   public IActionResult Index() => View();

   [HttpPost]
   public IActionResult Index(Person person)
   {
          // ...
   }
}
```

<span data-ttu-id="9404c-114">Ecco il file `Person/Index.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="9404c-114">The `Person/Index.cshtml` file:</span></span>

```cshtml
@model Person

Name: <input asp-for="Model.Name" />
<span asp-validation-for="Model.Name" />

Age: <input asp-for="Model.Age" />
<span asp-validation-for="Model.Age" />
```

### <a name="improvements-to-dynamicroutevaluetransformer"></a><span data-ttu-id="9404c-115">Miglioramenti apportati a DynamicRouteValueTransformer</span><span class="sxs-lookup"><span data-stu-id="9404c-115">Improvements to DynamicRouteValueTransformer</span></span>

<span data-ttu-id="9404c-116">ASP.NET Core 3,1 è stato introdotto <xref:Microsoft.AspNetCore.Mvc.Routing.DynamicRouteValueTransformer> come un modo per usare l'endpoint personalizzato per selezionare dinamicamente un'azione del controller MVC o una :::no-loc(Razor)::: pagina.</span><span class="sxs-lookup"><span data-stu-id="9404c-116">ASP.NET Core 3.1 introduced <xref:Microsoft.AspNetCore.Mvc.Routing.DynamicRouteValueTransformer> as a way to use custom endpoint to dynamically select an MVC controller action or a :::no-loc(Razor)::: page.</span></span> <span data-ttu-id="9404c-117">ASP.NET Core 5,0 le app possono passare lo stato a un oggetto `DynamicRouteValueTransformer` e filtrare il set di endpoint scelti.</span><span class="sxs-lookup"><span data-stu-id="9404c-117">ASP.NET Core 5.0 apps can pass state to a `DynamicRouteValueTransformer` and filter the set of endpoints chosen.</span></span>

### <a name="miscellaneous"></a><span data-ttu-id="9404c-118">Varie</span><span class="sxs-lookup"><span data-stu-id="9404c-118">Miscellaneous</span></span>

* <span data-ttu-id="9404c-119">L'attributo [[compare]](xref:System.ComponentModel.DataAnnotations.CompareAttribute) può essere applicato alle proprietà di un :::no-loc(Razor)::: modello di pagina.</span><span class="sxs-lookup"><span data-stu-id="9404c-119">The [[Compare]](xref:System.ComponentModel.DataAnnotations.CompareAttribute) attribute can be applied to properties on a :::no-loc(Razor)::: Page model.</span></span>
* <span data-ttu-id="9404c-120">I parametri e le proprietà associati dal corpo sono considerati necessari per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9404c-120">Parameters and properties bound from the body are considered required by default.</span></span> <!-- Review: How is this different from 3.1
The validation system in .NET Core 3.0 and later treats non-nullable parameters or bound properties as if they had a [Required] attribute.
see https://docs.microsoft.com/aspnet/core/mvc/models/validation?view=aspnetcore-3.1   
-->

## <a name="web-api"></a><span data-ttu-id="9404c-121">API Web</span><span class="sxs-lookup"><span data-stu-id="9404c-121">Web API</span></span>

### <a name="openapi-specification-on-by-default"></a><span data-ttu-id="9404c-122">Specifica OpenAPI su per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="9404c-122">OpenAPI Specification on by default</span></span>

<span data-ttu-id="9404c-123">[Openapi Specification](http://spec.openapis.org/oas/v3.0.3) è uno standard di settore per la descrizione delle API HTTP e l'integrazione in processi aziendali complessi o con terze parti.</span><span class="sxs-lookup"><span data-stu-id="9404c-123">[OpenAPI Specification](http://spec.openapis.org/oas/v3.0.3) is an industry standard for describing HTTP APIs and integrating them into complex business processes or with third parties.</span></span> <span data-ttu-id="9404c-124">OpenAPI è ampiamente supportato da tutti i provider di servizi cloud e da molti registri API.</span><span class="sxs-lookup"><span data-stu-id="9404c-124">OpenAPI is widely supported by all cloud providers and many API registries.</span></span> <span data-ttu-id="9404c-125">Le app che emettono documenti OpenAPI da API Web hanno una varietà di nuove opportunità in cui è possibile usare tali API.</span><span class="sxs-lookup"><span data-stu-id="9404c-125">Apps that emit OpenAPI documents from web APIs have a variety of new opportunities in which those APIs can be used.</span></span> <span data-ttu-id="9404c-126">In collaborazione con i gestori del progetto open source [Swashbuckle. AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/), il modello API ASP.NET Core contiene una dipendenza NuGet da [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="9404c-126">In partnership with the maintainers of the open-source project [Swashbuckle.AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/), the ASP.NET Core API template contains a NuGet dependency on [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore).</span></span> <span data-ttu-id="9404c-127">Swashbuckle è un noto pacchetto NuGet open source che genera in modo dinamico i documenti di OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="9404c-127">Swashbuckle is a popular open-source NuGet package that emits OpenAPI documents dynamically.</span></span> <span data-ttu-id="9404c-128">Swashbuckle esegue questa operazione introspezione sui controller API e generando il documento OpenAPI in fase di esecuzione oppure in fase di compilazione usando l'interfaccia della riga di comando di Swashbuckle.</span><span class="sxs-lookup"><span data-stu-id="9404c-128">Swashbuckle does this by introspecting over the API controllers and generating the OpenAPI document at run-time, or at build time using the Swashbuckle CLI.</span></span>

<span data-ttu-id="9404c-129">In ASP.NET Core 5,0, i modelli di API Web abilitano il supporto OpenAPI per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9404c-129">In ASP.NET Core 5.0, the web API templates enable the OpenAPI support by default.</span></span> <span data-ttu-id="9404c-130">Per disabilitare OpenAPI:</span><span class="sxs-lookup"><span data-stu-id="9404c-130">To disable OpenAPI:</span></span>

* <span data-ttu-id="9404c-131">Dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="9404c-131">From the command line:</span></span>

    ```dotnetcli
    dotnet new webapi --no-openapi true
    ```
* <span data-ttu-id="9404c-132">Da Visual Studio: deselezionare **Abilita supporto openapi** .</span><span class="sxs-lookup"><span data-stu-id="9404c-132">From Visual Studio: Uncheck **Enable OpenAPI support** .</span></span>

<span data-ttu-id="9404c-133">Tutti i file con *estensione csproj* creati per i progetti di API Web contengono il riferimento al pacchetto NuGet [Swashbuckle. AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/) .</span><span class="sxs-lookup"><span data-stu-id="9404c-133">All *.csproj* files created for web API projects contain the [Swashbuckle.AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/) NuGet package reference.</span></span>

```xml
<ItemGroup>
    <PackageReference Include="Swashbuckle.AspNetCore" Version="5.5.1" />
</ItemGroup>
```

<span data-ttu-id="9404c-134">Il codice generato dal modello contiene codice in `Startup.ConfigureServices` che attiva la generazione di documenti openapi:</span><span class="sxs-lookup"><span data-stu-id="9404c-134">The template generated code contains code in `Startup.ConfigureServices` that activates OpenAPI document generation:</span></span>

[!code-csharp[](~/release-notes/sample/StartupSwagger.cs?name=snippet)]

<span data-ttu-id="9404c-135">Il `Startup.Configure` metodo aggiunge il middleware Swashbuckle, che Abilita:</span><span class="sxs-lookup"><span data-stu-id="9404c-135">The `Startup.Configure` method adds the Swashbuckle middleware, which enables the:</span></span>

* <span data-ttu-id="9404c-136">Processo di generazione del documento.</span><span class="sxs-lookup"><span data-stu-id="9404c-136">Document generation process.</span></span>
* <span data-ttu-id="9404c-137">Pagina dell'interfaccia utente di spavalderia per impostazione predefinita in modalità di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9404c-137">Swagger UI page by default in development mode.</span></span>

<span data-ttu-id="9404c-138">Il codice generato dal modello non esporrà accidentalmente la descrizione dell'API durante la pubblicazione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="9404c-138">The template generated code won't accidentally expose the API's description when publishing to production.</span></span>

[!code-csharp[](~/release-notes/sample/StartupSwagger.cs?name=snippet2)]

#### <a name="azure-api-management-import"></a><span data-ttu-id="9404c-139">Importazione di gestione API di Azure</span><span class="sxs-lookup"><span data-stu-id="9404c-139">Azure API Management Import</span></span>

<span data-ttu-id="9404c-140">Quando ASP.NET Core progetti API abilitano OpenAPI, Visual Studio 2019 versione 16,8 e successive pubblicano automaticamente un passaggio aggiuntivo nel flusso di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="9404c-140">When ASP.NET Core API projects enable OpenAPI, the Visual Studio 2019 version 16.8 and later publishing automatically offer an additional step in the publishing flow.</span></span> <span data-ttu-id="9404c-141">Gli sviluppatori che usano [gestione API di Azure](xref:tutorials/publish-to-azure-api-management-using-vs) hanno la possibilità di importare automaticamente le API in gestione API di Azure durante il flusso di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="9404c-141">Developers who use [Azure API Management](xref:tutorials/publish-to-azure-api-management-using-vs) have an opportunity to automatically import the APIs into Azure API Management during the publish flow:</span></span>

![Importazione e pubblicazione di gestione API di Azure](~/release-notes/static/publish-to-apim.png)

### <a name="better-launch-experience-for-web-api-projects"></a><span data-ttu-id="9404c-143">Migliore esperienza di avvio per i progetti API Web</span><span class="sxs-lookup"><span data-stu-id="9404c-143">Better launch experience for web API projects</span></span>

<span data-ttu-id="9404c-144">Con OpenAPI abilitato per impostazione predefinita, l'esperienza di avvio dell'app (F5) per gli sviluppatori di API Web migliora significativamente.</span><span class="sxs-lookup"><span data-stu-id="9404c-144">With OpenAPI enabled by default, the app launching experience (F5) for web API developers significantly improves.</span></span> <span data-ttu-id="9404c-145">Con ASP.NET Core 5,0, il modello API Web è preconfigurato per il caricamento della pagina dell'interfaccia utente di spavalderia.</span><span class="sxs-lookup"><span data-stu-id="9404c-145">With ASP.NET Core 5.0, the web API template comes pre-configured to load up the Swagger UI page.</span></span> <span data-ttu-id="9404c-146">La pagina dell'interfaccia utente di spavalderia fornisce sia la documentazione aggiunta per l'API pubblicata che consente di testare le API con un solo clic.</span><span class="sxs-lookup"><span data-stu-id="9404c-146">The Swagger UI page provides both the documentation added for the published API, and enables testing the APIs with a single click.</span></span>

![visualizzazione di spavalderia/index.html](~/release-notes/static/swagger-ui-page-rc1.png)

## :::no-loc(Blazor):::

### <a name="performance-improvements"></a><span data-ttu-id="9404c-148">Miglioramenti alle prestazioni</span><span class="sxs-lookup"><span data-stu-id="9404c-148">Performance improvements</span></span>

<span data-ttu-id="9404c-149">Per .NET 5 sono stati apportati miglioramenti significativi alle :::no-loc(Blazor WebAssembly)::: prestazioni in fase di esecuzione con un particolare interesse per il rendering dell'interfaccia utente complesso e la serializzazione JSON.</span><span class="sxs-lookup"><span data-stu-id="9404c-149">For .NET 5, we made significant improvements to :::no-loc(Blazor WebAssembly)::: runtime performance with a specific focus on complex UI rendering and JSON serialization.</span></span> <span data-ttu-id="9404c-150">Nei test delle prestazioni, :::no-loc(Blazor WebAssembly)::: in .NET 5 è più veloce da due a tre volte per la maggior parte degli scenari.</span><span class="sxs-lookup"><span data-stu-id="9404c-150">In our performance tests, :::no-loc(Blazor WebAssembly)::: in .NET 5 is two to three times faster for most scenarios.</span></span> <span data-ttu-id="9404c-151">Per altre informazioni, vedere [ASP.NET Blog: aggiornamenti ASP.NET Core in .NET 5 Release Candidate 1](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#blazor-webassembly-performance-improvements).</span><span class="sxs-lookup"><span data-stu-id="9404c-151">For more information, see [ASP.NET Blog: ASP.NET Core updates in .NET 5 Release Candidate 1](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#blazor-webassembly-performance-improvements).</span></span>

### <a name="css-isolation"></a><span data-ttu-id="9404c-152">Isolamento CSS</span><span class="sxs-lookup"><span data-stu-id="9404c-152">CSS isolation</span></span>

<span data-ttu-id="9404c-153">:::no-loc(Blazor)::: supporta ora la definizione di stili CSS che hanno come ambito un determinato componente.</span><span class="sxs-lookup"><span data-stu-id="9404c-153">:::no-loc(Blazor)::: now supports defining CSS styles that are scoped to a given component.</span></span> <span data-ttu-id="9404c-154">Gli stili CSS specifici del componente consentono di ragionare più facilmente sugli stili in un'applicazione ed evitare effetti collaterali indesiderati degli stili globali.</span><span class="sxs-lookup"><span data-stu-id="9404c-154">Component-specific CSS styles make it easier to reason about the styles in an app and to avoid unintentional side effects of global styles.</span></span> <span data-ttu-id="9404c-155">Per altre informazioni, vedere <xref:blazor/components/css-isolation>.</span><span class="sxs-lookup"><span data-stu-id="9404c-155">For more information, see <xref:blazor/components/css-isolation>.</span></span>

### <a name="new-inputfile-component"></a><span data-ttu-id="9404c-156">Nuovo `InputFile` componente</span><span class="sxs-lookup"><span data-stu-id="9404c-156">New `InputFile` component</span></span>

<span data-ttu-id="9404c-157">Il `InputFile` componente consente di leggere uno o più file selezionati da un utente per il caricamento.</span><span class="sxs-lookup"><span data-stu-id="9404c-157">The `InputFile` component allows reading one or more files selected by a user for upload.</span></span> <span data-ttu-id="9404c-158">Per altre informazioni, vedere <xref:blazor/file-uploads>.</span><span class="sxs-lookup"><span data-stu-id="9404c-158">For more information, see <xref:blazor/file-uploads>.</span></span>

### <a name="new-inputradio-and-inputradiogroup-components"></a><span data-ttu-id="9404c-159">Nuovi `InputRadio` `InputRadioGroup` componenti e</span><span class="sxs-lookup"><span data-stu-id="9404c-159">New `InputRadio` and `InputRadioGroup` components</span></span>

<span data-ttu-id="9404c-160">:::no-loc(Blazor)::: dispone di componenti predefiniti `InputRadio` e `InputRadioGroup` che semplificano data binding ai gruppi di pulsanti di opzione con convalida integrata.</span><span class="sxs-lookup"><span data-stu-id="9404c-160">:::no-loc(Blazor)::: has built-in `InputRadio` and `InputRadioGroup` components that simplify data binding to radio button groups with integrated validation.</span></span> <span data-ttu-id="9404c-161">Per altre informazioni, vedere <xref:blazor/forms-validation>.</span><span class="sxs-lookup"><span data-stu-id="9404c-161">For more information, see <xref:blazor/forms-validation>.</span></span>

### <a name="component-virtualization"></a><span data-ttu-id="9404c-162">Virtualizzazione di componenti</span><span class="sxs-lookup"><span data-stu-id="9404c-162">Component virtualization</span></span>

<span data-ttu-id="9404c-163">Migliorare le prestazioni percepite del rendering dei componenti utilizzando il :::no-loc(Blazor)::: supporto della virtualizzazione incorporato del Framework.</span><span class="sxs-lookup"><span data-stu-id="9404c-163">Improve the perceived performance of component rendering using the :::no-loc(Blazor)::: framework's built-in virtualization support.</span></span> <span data-ttu-id="9404c-164">Per altre informazioni, vedere <xref:blazor/forms-validation#radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="9404c-164">For more information, see <xref:blazor/forms-validation#radio-buttons>.</span></span>

### <a name="ontoggle-event-support"></a><span data-ttu-id="9404c-165">`ontoggle` supporto degli eventi</span><span class="sxs-lookup"><span data-stu-id="9404c-165">`ontoggle` event support</span></span>

<span data-ttu-id="9404c-166">:::no-loc(Blazor)::: gli eventi supportano ora l' `ontoggle` evento DOM.</span><span class="sxs-lookup"><span data-stu-id="9404c-166">:::no-loc(Blazor)::: events now support the `ontoggle` DOM event.</span></span> <span data-ttu-id="9404c-167">Per altre informazioni, vedere <xref:blazor/components/event-handling#event-argument-types>.</span><span class="sxs-lookup"><span data-stu-id="9404c-167">For more information, see <xref:blazor/components/event-handling#event-argument-types>.</span></span>

### <a name="set-ui-focus-in-no-locblazor-apps"></a><span data-ttu-id="9404c-168">Impostare lo stato attivo dell'interfaccia utente nelle :::no-loc(Blazor)::: app</span><span class="sxs-lookup"><span data-stu-id="9404c-168">Set UI focus in :::no-loc(Blazor)::: apps</span></span>

<span data-ttu-id="9404c-169">Usare il `FocusAsync` Metodo convenience sui riferimenti a elementi per impostare lo stato attivo dell'interfaccia utente su tale elemento.</span><span class="sxs-lookup"><span data-stu-id="9404c-169">Use the `FocusAsync` convenience method on element references to set the UI focus to that element.</span></span> <span data-ttu-id="9404c-170">Per altre informazioni, vedere <xref:blazor/components/event-handling#focus-an-element>.</span><span class="sxs-lookup"><span data-stu-id="9404c-170">For more information, see <xref:blazor/components/event-handling#focus-an-element>.</span></span>

### <a name="custom-validation-class-attributes"></a><span data-ttu-id="9404c-171">Attributi della classe di convalida personalizzata</span><span class="sxs-lookup"><span data-stu-id="9404c-171">Custom validation class attributes</span></span>

<span data-ttu-id="9404c-172">I nomi delle classi di convalida personalizzate sono utili per l'integrazione con Framework CSS, ad esempio bootstrap.</span><span class="sxs-lookup"><span data-stu-id="9404c-172">Custom validation class names are useful when integrating with CSS frameworks, such as Bootstrap.</span></span> <span data-ttu-id="9404c-173">Per altre informazioni, vedere <xref:blazor/forms-validation#custom-validation-class-attributes>.</span><span class="sxs-lookup"><span data-stu-id="9404c-173">For more information, see <xref:blazor/forms-validation#custom-validation-class-attributes>.</span></span>

### <a name="iasyncdisposable-support"></a><span data-ttu-id="9404c-174">Supporto di IAsyncDisposable</span><span class="sxs-lookup"><span data-stu-id="9404c-174">IAsyncDisposable support</span></span>

<span data-ttu-id="9404c-175">:::no-loc(Blazor)::: i componenti supportano ora l' <xref:System.IAsyncDisposable> interfaccia per la versione asincrona delle risorse allocate.</span><span class="sxs-lookup"><span data-stu-id="9404c-175">:::no-loc(Blazor)::: components now support the <xref:System.IAsyncDisposable> interface for the asynchronous release of allocated resources.</span></span>

### <a name="javascript-isolation-and-object-references"></a><span data-ttu-id="9404c-176">Isolamento JavaScript e riferimenti a oggetti</span><span class="sxs-lookup"><span data-stu-id="9404c-176">JavaScript isolation and object references</span></span>

<span data-ttu-id="9404c-177">:::no-loc(Blazor)::: Abilita l'isolamento JavaScript nei [moduli JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules)standard.</span><span class="sxs-lookup"><span data-stu-id="9404c-177">:::no-loc(Blazor)::: enables JavaScript isolation in standard [JavaScript modules](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules).</span></span> <span data-ttu-id="9404c-178">Per altre informazioni, vedere <xref:blazor/call-javascript-from-dotnet#blazor-javascript-isolation-and-object-references>.</span><span class="sxs-lookup"><span data-stu-id="9404c-178">For more information, see <xref:blazor/call-javascript-from-dotnet#blazor-javascript-isolation-and-object-references>.</span></span>

### <a name="form-components-support-display-name"></a><span data-ttu-id="9404c-179">I componenti del modulo supportano il nome visualizzato</span><span class="sxs-lookup"><span data-stu-id="9404c-179">Form components support display name</span></span>

<span data-ttu-id="9404c-180">I seguenti componenti predefiniti supportano i nomi visualizzati con il `DisplayName` parametro:</span><span class="sxs-lookup"><span data-stu-id="9404c-180">The following built-in components support display names with the `DisplayName` parameter:</span></span>

* `InputDate`
* `InputNumber`
* `InputSelect`

<span data-ttu-id="9404c-181">Per altre informazioni, vedere <xref:blazor/forms-validation#display-name-support>.</span><span class="sxs-lookup"><span data-stu-id="9404c-181">For more information, see <xref:blazor/forms-validation#display-name-support>.</span></span>

### <a name="catch-all-route-parameters"></a><span data-ttu-id="9404c-182">Parametri di route catch-all</span><span class="sxs-lookup"><span data-stu-id="9404c-182">Catch-all route parameters</span></span>

<span data-ttu-id="9404c-183">I parametri di route catch-all, che acquisiscono percorsi tra più limiti di cartella, sono supportati nei componenti di.</span><span class="sxs-lookup"><span data-stu-id="9404c-183">Catch-all route parameters, which capture paths across multiple folder boundaries, are supported in components.</span></span> <span data-ttu-id="9404c-184">Per altre informazioni, vedere <xref:blazor/fundamentals/routing#catch-all-route-parameters>.</span><span class="sxs-lookup"><span data-stu-id="9404c-184">For more information, see <xref:blazor/fundamentals/routing#catch-all-route-parameters>.</span></span>

### <a name="debugging-improvements"></a><span data-ttu-id="9404c-185">Miglioramenti apportati al debug</span><span class="sxs-lookup"><span data-stu-id="9404c-185">Debugging improvements</span></span>

<span data-ttu-id="9404c-186">Il debug :::no-loc(Blazor WebAssembly)::: delle app è stato migliorato in ASP.NET Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="9404c-186">Debugging :::no-loc(Blazor WebAssembly)::: apps is improved in ASP.NET Core 5.0.</span></span> <span data-ttu-id="9404c-187">Inoltre, il debug è ora supportato in Visual Studio per Mac.</span><span class="sxs-lookup"><span data-stu-id="9404c-187">Additionally, debugging is now supported on Visual Studio for Mac.</span></span> <span data-ttu-id="9404c-188">Per altre informazioni, vedere <xref:blazor/debug>.</span><span class="sxs-lookup"><span data-stu-id="9404c-188">For more information, see <xref:blazor/debug>.</span></span>

### <a name="microsoft-no-locidentity-v20-and-msal-v20"></a><span data-ttu-id="9404c-189">Microsoft :::no-loc(Identity)::: v 2.0 e MSAL v 2.0</span><span class="sxs-lookup"><span data-stu-id="9404c-189">Microsoft :::no-loc(Identity)::: v2.0 and MSAL v2.0</span></span>

<span data-ttu-id="9404c-190">:::no-loc(Blazor)::: la protezione ora usa Microsoft :::no-loc(Identity)::: v 2.0 ( [`Microsoft.:::no-loc(Identity):::.Web`](https://www.nuget.org/packages/Microsoft.:::no-loc(Identity):::.Web) e [`Microsoft.:::no-loc(Identity):::.Web.UI`](https://www.nuget.org/packages/Microsoft.:::no-loc(Identity):::.Web.UI) ) e MSAL v 2.0.</span><span class="sxs-lookup"><span data-stu-id="9404c-190">:::no-loc(Blazor)::: security now uses Microsoft :::no-loc(Identity)::: v2.0 ([`Microsoft.:::no-loc(Identity):::.Web`](https://www.nuget.org/packages/Microsoft.:::no-loc(Identity):::.Web) and [`Microsoft.:::no-loc(Identity):::.Web.UI`](https://www.nuget.org/packages/Microsoft.:::no-loc(Identity):::.Web.UI)) and MSAL v2.0.</span></span> <span data-ttu-id="9404c-191">Per ulteriori informazioni, vedere gli argomenti relativi alla [ :::no-loc(Blazor)::: sicurezza e al :::no-loc(Identity)::: nodo](xref:blazor/security/index).</span><span class="sxs-lookup"><span data-stu-id="9404c-191">For more information, see the topics in the [:::no-loc(Blazor)::: Security and :::no-loc(Identity)::: node](xref:blazor/security/index).</span></span>

### <a name="protected-browser-storage-for-no-locblazor-server"></a><span data-ttu-id="9404c-192">Archivio browser protetto per :::no-loc(Blazor Server):::</span><span class="sxs-lookup"><span data-stu-id="9404c-192">Protected Browser Storage for :::no-loc(Blazor Server):::</span></span>

<span data-ttu-id="9404c-193">:::no-loc(Blazor Server)::: le app possono ora usare il supporto incorporato per l'archiviazione dello stato dell'app nel browser protetto da manomissioni usando la protezione dei dati ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9404c-193">:::no-loc(Blazor Server)::: apps can now use built-in support for storing app state in the browser that has been protected from tampering using ASP.NET Core data protection.</span></span> <span data-ttu-id="9404c-194">I dati possono essere archiviati in un archivio del browser locale o in una sessione di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="9404c-194">Data can be stored in either local browser storage or session storage.</span></span> <span data-ttu-id="9404c-195">Per altre informazioni, vedere <xref:blazor/state-management>.</span><span class="sxs-lookup"><span data-stu-id="9404c-195">For more information, see <xref:blazor/state-management>.</span></span>

### <a name="no-locblazor-webassembly-prerendering"></a><span data-ttu-id="9404c-196">:::no-loc(Blazor WebAssembly)::: tempistiche</span><span class="sxs-lookup"><span data-stu-id="9404c-196">:::no-loc(Blazor WebAssembly)::: prerendering</span></span>

<span data-ttu-id="9404c-197">L'integrazione dei componenti è stata migliorata tra i modelli di hosting e :::no-loc(Blazor WebAssembly)::: le app possono ora eseguire il prerendering dell'output nel server.</span><span class="sxs-lookup"><span data-stu-id="9404c-197">Component integration is improved across hosting models, and :::no-loc(Blazor WebAssembly)::: apps can now prerender output on the server.</span></span> <!-- UNCOMMENT AFTER https://github.com/dotnet/AspNetCore.Docs/pull/19887 MERGES: For more information, see <xref:blazor/components/integrate-components-into-razor-pages-and-mvc-apps> and <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>. -->

### <a name="trimminglinking-improvements"></a><span data-ttu-id="9404c-198">Miglioramenti del taglio/collegamento</span><span class="sxs-lookup"><span data-stu-id="9404c-198">Trimming/linking improvements</span></span>

<span data-ttu-id="9404c-199">:::no-loc(Blazor WebAssembly)::: esegue il taglio e IL collegamento del linguaggio intermedio (IL) durante una compilazione per eliminare IL IL di il non necessario dagli assembly di output dell'app.</span><span class="sxs-lookup"><span data-stu-id="9404c-199">:::no-loc(Blazor WebAssembly)::: performs Intermediate Language (IL) trimming/linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="9404c-200">Con il rilascio di ASP.NET Core 5,0, :::no-loc(Blazor WebAssembly)::: esegue un taglio migliorato con altre opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="9404c-200">With the release of ASP.NET Core 5.0, :::no-loc(Blazor WebAssembly)::: performs improved trimming with additional configuration options.</span></span> <span data-ttu-id="9404c-201">Per ulteriori informazioni, vedere <xref:blazor/host-and-deploy/configure-trimmer> e [Opzioni di trimming](/dotnet/core/deploying/trimming-options).</span><span class="sxs-lookup"><span data-stu-id="9404c-201">For more information, see <xref:blazor/host-and-deploy/configure-trimmer> and [Trimming options](/dotnet/core/deploying/trimming-options).</span></span>

### <a name="browser-compatibility-analyzer"></a><span data-ttu-id="9404c-202">Analizzatore compatibilità browser</span><span class="sxs-lookup"><span data-stu-id="9404c-202">Browser compatibility analyzer</span></span>

<span data-ttu-id="9404c-203">:::no-loc(Blazor WebAssembly)::: le app sono destinate alla superficie di attacco dell'API .NET completa, ma non tutte le API .NET sono supportate nel webassembly a causa dei vincoli del sandbox del browser.</span><span class="sxs-lookup"><span data-stu-id="9404c-203">:::no-loc(Blazor WebAssembly)::: apps target the full .NET API surface area, but not all .NET APIs are supported on WebAssembly due to browser sandbox constraints.</span></span> <span data-ttu-id="9404c-204">Le API non supportate vengono generate <xref:System.PlatformNotSupportedException> durante l'esecuzione su webassembly.</span><span class="sxs-lookup"><span data-stu-id="9404c-204">Unsupported APIs throw <xref:System.PlatformNotSupportedException> when running on WebAssembly.</span></span> <span data-ttu-id="9404c-205">Un analizzatore della compatibilità della piattaforma avvisa lo sviluppatore quando l'app usa API non supportate dalle piattaforme di destinazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="9404c-205">A platform compatibility analyzer warns the developer when the app uses APIs that aren't supported by the app's target platforms.</span></span> <span data-ttu-id="9404c-206">Per altre informazioni, vedere <xref:blazor/components/class-libraries#browser-compatibility-analyzer-for-blazor-webassembly>.</span><span class="sxs-lookup"><span data-stu-id="9404c-206">For more information, see <xref:blazor/components/class-libraries#browser-compatibility-analyzer-for-blazor-webassembly>.</span></span>

### <a name="lazy-load-assemblies"></a><span data-ttu-id="9404c-207">Assembly di caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="9404c-207">Lazy load assemblies</span></span>

<span data-ttu-id="9404c-208">:::no-loc(Blazor WebAssembly)::: le prestazioni di avvio delle app possono essere migliorate posticipando il caricamento di alcuni assembly di applicazioni fino a quando non sono necessarie.</span><span class="sxs-lookup"><span data-stu-id="9404c-208">:::no-loc(Blazor WebAssembly)::: app startup performance can be improved by deferring the loading of some application assemblies until they're required.</span></span> <span data-ttu-id="9404c-209">Per altre informazioni, vedere <xref:blazor/webassembly-lazy-load-assemblies>.</span><span class="sxs-lookup"><span data-stu-id="9404c-209">For more information, see <xref:blazor/webassembly-lazy-load-assemblies>.</span></span>

### <a name="updated-globalization-support"></a><span data-ttu-id="9404c-210">Supporto per la globalizzazione aggiornato</span><span class="sxs-lookup"><span data-stu-id="9404c-210">Updated globalization support</span></span>

<span data-ttu-id="9404c-211">Il supporto per la globalizzazione è disponibile per la :::no-loc(Blazor WebAssembly)::: basata sui componenti internazionali per Unicode (ICU).</span><span class="sxs-lookup"><span data-stu-id="9404c-211">Globalization support is available for :::no-loc(Blazor WebAssembly)::: based on International Components for Unicode (ICU).</span></span> <span data-ttu-id="9404c-212">Per altre informazioni, vedere <xref:blazor/globalization-localization>.</span><span class="sxs-lookup"><span data-stu-id="9404c-212">For more information, see <xref:blazor/globalization-localization>.</span></span>

## <a name="grpc"></a><span data-ttu-id="9404c-213">gRPC</span><span class="sxs-lookup"><span data-stu-id="9404c-213">gRPC</span></span>

<span data-ttu-id="9404c-214">In [gRPC](https://grpc.io/)sono stati apportati numerosi miglioramenti alla conformità.</span><span class="sxs-lookup"><span data-stu-id="9404c-214">Many preformance improvements have been made in [gRPC](https://grpc.io/).</span></span> <span data-ttu-id="9404c-215">Per ulteriori informazioni, vedere la pagina relativa ai [miglioramenti delle prestazioni di gRPC in .NET 5](https://devblogs.microsoft.com/aspnet/grpc-performance-improvements-in-net-5/).</span><span class="sxs-lookup"><span data-stu-id="9404c-215">For more information, see [gRPC performance improvements in .NET 5](https://devblogs.microsoft.com/aspnet/grpc-performance-improvements-in-net-5/).</span></span>

<span data-ttu-id="9404c-216">Per ulteriori informazioni su gRPC, vedere <xref:grpc/index> .</span><span class="sxs-lookup"><span data-stu-id="9404c-216">For more gRPC information, see <xref:grpc/index>.</span></span>

## :::no-loc(SignalR):::

<span data-ttu-id="9404c-217">:::no-loc(SignalR)::: I filtri Hub, detti pipeline dell'hub in ASP.NET :::no-loc(SignalR)::: , sono una funzionalità che consente l'esecuzione del codice prima e dopo la chiamata dei metodi dell'hub.</span><span class="sxs-lookup"><span data-stu-id="9404c-217">:::no-loc(SignalR)::: Hub filters, called Hub pipelines in ASP.NET :::no-loc(SignalR):::, is a feature that allows code to run before and after Hub methods are called.</span></span> <span data-ttu-id="9404c-218">L'esecuzione di codice prima e dopo la chiamata dei metodi dell'hub è simile al modo in cui middleware è in grado di eseguire il codice prima e dopo una richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9404c-218">Running code before and after Hub methods are called is similar to how middleware has the ability to run code before and after an HTTP request.</span></span> <span data-ttu-id="9404c-219">Gli utilizzi comuni includono la registrazione, la gestione degli errori e la convalida degli argomenti.</span><span class="sxs-lookup"><span data-stu-id="9404c-219">Common uses include logging, error handling, and argument validation.</span></span>

<span data-ttu-id="9404c-220">Per altre informazioni, vedere [use Hub filters in :::no-loc(SignalR)::: ASP.NET Core ](xref:signalr/hub-filters).</span><span class="sxs-lookup"><span data-stu-id="9404c-220">For more information, see [Use hub filters in ASP.NET Core :::no-loc(SignalR):::](xref:signalr/hub-filters).</span></span>

### <a name="no-locsignalr-parallel-hub-invocations"></a><span data-ttu-id="9404c-221">:::no-loc(SignalR)::: chiamate dell'hub parallelo</span><span class="sxs-lookup"><span data-stu-id="9404c-221">:::no-loc(SignalR)::: parallel hub invocations</span></span>

<span data-ttu-id="9404c-222">ASP.NET Core :::no-loc(SignalR)::: è ora in grado di gestire le chiamate dell'hub parallelo.</span><span class="sxs-lookup"><span data-stu-id="9404c-222">ASP.NET Core :::no-loc(SignalR)::: is now capable of handling parallel hub invocations.</span></span> <span data-ttu-id="9404c-223">Il comportamento predefinito può essere modificato per consentire ai client di richiamare più di un metodo Hub alla volta:</span><span class="sxs-lookup"><span data-stu-id="9404c-223">The default behavior can be changed to allow clients to invoke more than one hub method at a time:</span></span>

[!code-csharp[](~/release-notes/sample/Startup:::no-loc(SignalR):::hubs.cs?name=snippet)]

### <a name="added-messagepack-support-in-no-locsignalr-java-client"></a><span data-ttu-id="9404c-224">Aggiunto il supporto di MessagePack in :::no-loc(SignalR)::: Java client</span><span class="sxs-lookup"><span data-stu-id="9404c-224">Added Messagepack support in :::no-loc(SignalR)::: Java client</span></span>

<span data-ttu-id="9404c-225">Un nuovo pacchetto, [com. Microsoft. SignalR. MessagePack](https://mvnrepository.com/artifact/com.microsoft.signalr.messagepack), aggiunge il supporto MessagePack al :::no-loc(SignalR)::: client Java.</span><span class="sxs-lookup"><span data-stu-id="9404c-225">A new package, [com.microsoft.signalr.messagepack](https://mvnrepository.com/artifact/com.microsoft.signalr.messagepack), adds MessagePack support to the :::no-loc(SignalR)::: Java client.</span></span> <span data-ttu-id="9404c-226">Per usare il protocollo dell'hub MessagePack, aggiungere `.withHubProtocol(new MessagePackHubProtocol())` al generatore di connessioni:</span><span class="sxs-lookup"><span data-stu-id="9404c-226">To use the MessagePack hub protocol, add `.withHubProtocol(new MessagePackHubProtocol())` to the connection builder:</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create(
                           "http://localhost:53353/MyHub")
               .withHubProtocol(new MessagePackHubProtocol())
               .build();
```

<!--
See [Update :::no-loc(SignalR)::: code](xref:migration/31-to-50#signalr) for migration instructions.
-->

## :::no-loc(Kestrel):::

* <span data-ttu-id="9404c-227">Endpoint ricaricabili tramite la configurazione: è :::no-loc(Kestrel)::: in grado di rilevare le modifiche apportate alla configurazione passata a [ :::no-loc(Kestrel):::ServerOptions.ConfigUre](xref:Microsoft.AspNetCore.Server.:::no-loc(Kestrel):::.Core.:::no-loc(Kestrel):::ServerOptions.Configure%2A) e di annullare l'associazione da endpoint esistenti ed eseguire il binding ai nuovi endpoint senza richiedere il riavvio dell'applicazione quando il `reloadOnChange` parametro è `true` .</span><span class="sxs-lookup"><span data-stu-id="9404c-227">Reloadable endpoints via configuration: :::no-loc(Kestrel)::: can detect changes to configuration passed to [:::no-loc(Kestrel):::ServerOptions.Configure](xref:Microsoft.AspNetCore.Server.:::no-loc(Kestrel):::.Core.:::no-loc(Kestrel):::ServerOptions.Configure%2A) and unbind from existing endpoints and bind to new endpoints without requiring an application restart when the `reloadOnChange` parameter is `true`.</span></span> <span data-ttu-id="9404c-228">Per impostazione predefinita <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults%2A> , quando si usa o, viene <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> :::no-loc(Kestrel)::: associato alla sottosezione di configurazione [" :::no-loc(Kestrel)::: "](https://github.com/dotnet/aspnetcore/blob/7e9e03b70124784b1de5564c573bd65cdaccbfcc/src/DefaultBuilder/src/WebHost.cs#L226) con `reloadOnChange` abilitato.</span><span class="sxs-lookup"><span data-stu-id="9404c-228">By default when using <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults%2A> or <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, :::no-loc(Kestrel)::: binds to the [":::no-loc(Kestrel):::"](https://github.com/dotnet/aspnetcore/blob/7e9e03b70124784b1de5564c573bd65cdaccbfcc/src/DefaultBuilder/src/WebHost.cs#L226) configuration subsection with `reloadOnChange` enabled.</span></span> <span data-ttu-id="9404c-229">Le app devono `reloadOnChange: true` essere passate quando `:::no-loc(Kestrel):::ServerOptions.Configure` si chiama manualmente per ottenere endpoint ricaricabili.</span><span class="sxs-lookup"><span data-stu-id="9404c-229">Apps must pass `reloadOnChange: true` when calling `:::no-loc(Kestrel):::ServerOptions.Configure` manually to get reloadable endpoints.</span></span>
* <span data-ttu-id="9404c-230">Miglioramenti delle intestazioni di risposta HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="9404c-230">HTTP/2 response headers improvements.</span></span> <span data-ttu-id="9404c-231">Per ulteriori informazioni, vedere [miglioramenti delle prestazioni](#performance-improvements) nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="9404c-231">For more information, see [Performance improvements](#performance-improvements) in the next section.</span></span>
* <span data-ttu-id="9404c-232">Supporto per altri tipi di endpoint nel trasporto di socket: l'aggiunta alla nuova API introdotta in <xref:System.Net.Sockets?displayProperty=nameWithType> , il trasporto predefinito dei socket in [:::no-loc(Kestrel):::](xref:fundamentals/servers/kestrel) consente il binding sia agli handle di file esistenti che ai socket di dominio UNIX.</span><span class="sxs-lookup"><span data-stu-id="9404c-232">Support for additional endpoints types in the sockets transport: Adding to the new API introduced in <xref:System.Net.Sockets?displayProperty=nameWithType>, the sockets default transport in [:::no-loc(Kestrel):::](xref:fundamentals/servers/kestrel) allows binding to both existing file handles and Unix domain sockets.</span></span> <span data-ttu-id="9404c-233">Il supporto per l'associazione agli handle di file esistenti consente di utilizzare l' `Systemd` integrazione esistente senza richiedere il `libuv` trasporto.</span><span class="sxs-lookup"><span data-stu-id="9404c-233">Support for binding to existing file handles enables using the existing `Systemd` integration without requiring the `libuv` transport.</span></span>
* <span data-ttu-id="9404c-234">Decodifica dell'intestazione personalizzata in :::no-loc(Kestrel)::: : le app possono specificare quali <xref:System.Text.Encoding> usare per interpretare le intestazioni in ingresso in base al nome dell'intestazione anziché al valore predefinito UTF-8.</span><span class="sxs-lookup"><span data-stu-id="9404c-234">Custom header decoding in :::no-loc(Kestrel):::: Apps can specify which <xref:System.Text.Encoding> to use to interpret incoming headers based on the header name instead of defaulting to UTF-8.</span></span> <span data-ttu-id="9404c-235">Impostare il commutatore </span><span class="sxs-lookup"><span data-stu-id="9404c-235">Set the</span></span> <!--<xref:Microsoft.AspNetCore.Server.:::no-loc(Kestrel):::.:::no-loc(Kestrel):::ServerOptions.RequestHeaderEncodingSelector> --> <span data-ttu-id="9404c-236">`Microsoft.AspNetCore.Server.:::no-loc(Kestrel):::.:::no-loc(Kestrel):::ServerOptions.RequestHeaderEncodingSelector` Proprietà per specificare la codifica da usare:</span><span class="sxs-lookup"><span data-stu-id="9404c-236">`Microsoft.AspNetCore.Server.:::no-loc(Kestrel):::.:::no-loc(Kestrel):::ServerOptions.RequestHeaderEncodingSelector` property to specify which encoding to use:</span></span>
 
  ```csharp
  public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.Configure:::no-loc(Kestrel):::(options =>
            {
                options.RequestHeaderEncodingSelector = encoding =>
                {
                      return encoding switch
                      {
                          "Host" => System.Text.Encoding.Latin1,
                          _ => System.Text.Encoding.UTF8,
                      };
                };
            });
            webBuilder.UseStartup<Startup>();
        });
  ```

### <a name="no-lockestrel-endpoint-specific-options-via-configuration"></a><span data-ttu-id="9404c-237">:::no-loc(Kestrel)::: opzioni specifiche dell'endpoint tramite configurazione</span><span class="sxs-lookup"><span data-stu-id="9404c-237">:::no-loc(Kestrel)::: endpoint-specific options via configuration</span></span>

<span data-ttu-id="9404c-238">È stato aggiunto il supporto per la configurazione delle :::no-loc(Kestrel)::: Opzioni specifiche dell'endpoint tramite la [configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9404c-238">Support has been added for configuring :::no-loc(Kestrel):::’s endpoint-specific options via [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="9404c-239">Le configurazioni specifiche dell'endpoint includono:</span><span class="sxs-lookup"><span data-stu-id="9404c-239">The endpoint-specific configurations includes the:</span></span>

* <span data-ttu-id="9404c-240">Protocolli HTTP utilizzati</span><span class="sxs-lookup"><span data-stu-id="9404c-240">HTTP protocols used</span></span>
* <span data-ttu-id="9404c-241">Protocolli TLS usati</span><span class="sxs-lookup"><span data-stu-id="9404c-241">TLS protocols used</span></span>
* <span data-ttu-id="9404c-242">Certificato selezionato</span><span class="sxs-lookup"><span data-stu-id="9404c-242">Certificate selected</span></span>
* <span data-ttu-id="9404c-243">Modalità certificato ciente</span><span class="sxs-lookup"><span data-stu-id="9404c-243">Cient certificate mode</span></span>

<span data-ttu-id="9404c-244">La configurazione consente di specificare il certificato selezionato in base al nome del server specificato.</span><span class="sxs-lookup"><span data-stu-id="9404c-244">Configuration allows specifying which certificate is selected based on the specified server name.</span></span> <span data-ttu-id="9404c-245">Il nome del server fa parte dell'estensione Indicazione nome server (SNI) al protocollo TLS come indicato dal client.</span><span class="sxs-lookup"><span data-stu-id="9404c-245">The server name is part of the Server Name Indication (SNI) extension to the TLS protocol as indicated by the client.</span></span> <span data-ttu-id="9404c-246">:::no-loc(Kestrel):::la configurazione di supporta anche un prefisso con caratteri jolly nel nome host.</span><span class="sxs-lookup"><span data-stu-id="9404c-246">:::no-loc(Kestrel):::'s configuration also supports a wildcard prefix in the host name.</span></span>

<span data-ttu-id="9404c-247">Nell'esempio seguente viene illustrato come specificare specifici dell'endpoint utilizzando un file di configurazione:</span><span class="sxs-lookup"><span data-stu-id="9404c-247">The following example shows how to specify endpoint-specific using a configuration file:</span></span>

```json
{
  ":::no-loc(Kestrel):::": {
    "Endpoints": {
      "EndpointName": {
        "Url": "https://*",
        "Sni": {
          "a.example.org": {
            "Protocols": "Http1AndHttp2",
            "SslProtocols": [ "Tls11", "Tls12"],
            "Certificate": {
              "Path": "testCert.pfx",
              "Password": "testPassword"
            },
            "ClientCertificateMode" : "NoCertificate"
          },
          "*.example.org": {
            "Certificate": {
              "Path": "testCert2.pfx",
              "Password": "testPassword"
            }
          },
          "*": {
            // At least one sub-property needs to exist per
            // SNI section or it cannot be discovered via
            // IConfiguration
            "Protocols": "Http1",
          }
        }
      }
    }
  }
}
```

<span data-ttu-id="9404c-248">Indicazione nome server (SNI) è un'estensione TLS per includere un dominio virtuale come parte della negoziazione SSL.</span><span class="sxs-lookup"><span data-stu-id="9404c-248">Server Name Indication (SNI) is a TLS extension to include a virtual domain as a part of SSL negotiation.</span></span> <span data-ttu-id="9404c-249">Ciò significa che il nome di dominio virtuale, o un nome host, può essere usato per identificare l'endpoint di rete.</span><span class="sxs-lookup"><span data-stu-id="9404c-249">What this effectively means is that the virtual domain name, or a hostname, can be used to identify the network end point.</span></span>

## <a name="performance-improvements"></a><span data-ttu-id="9404c-250">Miglioramenti alle prestazioni</span><span class="sxs-lookup"><span data-stu-id="9404c-250">Performance improvements</span></span>

### <a name="http2"></a><span data-ttu-id="9404c-251">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="9404c-251">HTTP/2</span></span>

* <span data-ttu-id="9404c-252">Riduzioni significative nelle allocazioni nel percorso del codice HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="9404c-252">Significant reductions in allocations in the HTTP/2 code path.</span></span>
* <span data-ttu-id="9404c-253">Supporto per la [compressione dinamica HPack](https://tools.ietf.org/html/rfc7541) delle intestazioni di risposta http/2 in [:::no-loc(Kestrel):::](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="9404c-253">Support for [HPack dynamic compression](https://tools.ietf.org/html/rfc7541) of HTTP/2 response headers in [:::no-loc(Kestrel):::](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="9404c-254">Per altre informazioni, vedere [dimensioni della tabella di intestazione](xref:fundamentals/servers/kestrel#header-table-size) e [HPACK: l'assassino invisibile (Feature) di http/2](https://blog.cloudflare.com/hpack-the-silent-killer-feature-of-http-2/).</span><span class="sxs-lookup"><span data-stu-id="9404c-254">For more information, see [Header table size](xref:fundamentals/servers/kestrel#header-table-size) and [HPACK: the silent killer (feature) of HTTP/2](https://blog.cloudflare.com/hpack-the-silent-killer-feature-of-http-2/).</span></span>
* <span data-ttu-id="9404c-255">Invio di frame PING HTTP/2: HTTP/2 dispone di un meccanismo per l'invio di frame PING per garantire che una connessione inattiva sia ancora funzionante.</span><span class="sxs-lookup"><span data-stu-id="9404c-255">Sending HTTP/2 PING frames: HTTP/2 has a mechanism for sending PING frames to ensure an idle connection is still functional.</span></span> <span data-ttu-id="9404c-256">Garantire una connessione valida è particolarmente utile quando si lavora con flussi di lunga durata che spesso sono inattivi, ma solo a intermittenza, ad esempio i flussi gRPC.</span><span class="sxs-lookup"><span data-stu-id="9404c-256">Ensuring a viable connection is especially useful when working with long-lived streams that are often idle but only intermittently see activity, for example, gRPC streams.</span></span> <span data-ttu-id="9404c-257">Le app possono inviare frame PING periodici in impostando i [:::no-loc(Kestrel):::](xref:fundamentals/servers/kestrel) limiti per <xref:Microsoft.AspNetCore.Server.:::no-loc(Kestrel):::.:::no-loc(Kestrel):::ServerOptions> :</span><span class="sxs-lookup"><span data-stu-id="9404c-257">Apps can send periodic PING frames in [:::no-loc(Kestrel):::](xref:fundamentals/servers/kestrel) by setting limits on <xref:Microsoft.AspNetCore.Server.:::no-loc(Kestrel):::.:::no-loc(Kestrel):::ServerOptions>:</span></span>

   ```csharp
   public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.Configure:::no-loc(Kestrel):::(options =>
            {
                options.Limits.Http2.KeepAlivePingInterval =
                                               TimeSpan.FromSeconds(10);
                options.Limits.Http2.KeepAlivePingTimeout =
                                               TimeSpan.FromSeconds(1);
            });
            webBuilder.UseStartup<Startup>();
        });
   ```
   <!-- review: KeepAlivePingInterval not found in RC1. Try testing with RC1. See https://github.com/dotnet/aspnetcore/pull/22565/files see C:/Users/riande/source/repos/WebApplication128/WebApplication128 -->

### <a name="containers"></a><span data-ttu-id="9404c-258">Contenitori</span><span class="sxs-lookup"><span data-stu-id="9404c-258">Containers</span></span>

<span data-ttu-id="9404c-259">Prima di .NET 5,0, la compilazione e la pubblicazione di un *Dockerfile* per un'app ASP.NET Core richiedevano l'estrazione dell'intera .NET Core SDK e dell'immagine ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9404c-259">Prior to .NET 5.0, building and publishing a *Dockerfile* for an ASP.NET Core app required pulling the entire .NET Core SDK and the ASP.NET Core image.</span></span> <span data-ttu-id="9404c-260">Con questa versione, il pull dei byte delle immagini dell'SDK viene ridotto e i byte estratti per l'immagine di ASP.NET Core vengono eliminati in gran parte.</span><span class="sxs-lookup"><span data-stu-id="9404c-260">With this release, pulling the SDK images bytes is reduced and the bytes pulled for the ASP.NET Core image is largely eliminated.</span></span> <span data-ttu-id="9404c-261">Per altre informazioni, vedere [questo commento sul problema GitHub](https://github.com/dotnet/dotnet-docker/issues/1814#issuecomment-625294750).</span><span class="sxs-lookup"><span data-stu-id="9404c-261">For more information, see [this GitHub issue comment](https://github.com/dotnet/dotnet-docker/issues/1814#issuecomment-625294750).</span></span>

## <a name="authentication-and-authorization"></a><span data-ttu-id="9404c-262">Autenticazione e autorizzazione</span><span class="sxs-lookup"><span data-stu-id="9404c-262">Authentication and authorization</span></span>

### <a name="azure-active-directory-authentication-with-microsoftno-locidentityweb"></a><span data-ttu-id="9404c-263">Azure Active Directory l'autenticazione con Microsoft. :::no-loc(Identity)::: . Web</span><span class="sxs-lookup"><span data-stu-id="9404c-263">Azure Active Directory authentication with Microsoft.:::no-loc(Identity):::.Web</span></span>

<span data-ttu-id="9404c-264">I modelli di progetto ASP.NET Core ora si integrano con <xref:Microsoft.:::no-loc(Identity):::.Web?displayProperty=fullName> per gestire l'autenticazione con la [directory delle attività di Azure](/azure/active-directory/fundamentals/active-directory-whatis) (Azure ad).</span><span class="sxs-lookup"><span data-stu-id="9404c-264">The ASP.NET Core project templates now integrate with <xref:Microsoft.:::no-loc(Identity):::.Web?displayProperty=fullName> to handle authentication with [Azure Activity Directory](/azure/active-directory/fundamentals/active-directory-whatis) (Azure AD).</span></span> <span data-ttu-id="9404c-265">[Microsoft. :::no-loc(Identity)::: . Il pacchetto Web](https://www.nuget.org/packages/Microsoft.:::no-loc(Identity):::.Web/) fornisce:</span><span class="sxs-lookup"><span data-stu-id="9404c-265">The [Microsoft.:::no-loc(Identity):::.Web package](https://www.nuget.org/packages/Microsoft.:::no-loc(Identity):::.Web/) provides:</span></span>

* <span data-ttu-id="9404c-266">Un'esperienza migliore per l'autenticazione tramite Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9404c-266">A better experience for authentication through Azure AD.</span></span>
* <span data-ttu-id="9404c-267">Un modo più semplice per accedere alle risorse di Azure per conto degli utenti, tra cui [Microsoft Graph](/graph/overview).</span><span class="sxs-lookup"><span data-stu-id="9404c-267">An easier way to access Azure resources on behalf of your users, including [Microsoft Graph](/graph/overview).</span></span> <span data-ttu-id="9404c-268">Vedere [Microsoft. :::no-loc(Identity)::: . Web Sample](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2), che inizia con un account di accesso di base e passa attraverso il multi-tenant, usando le API di Azure, usando Microsoft Graph e proteggendo le proprie API.</span><span class="sxs-lookup"><span data-stu-id="9404c-268">See the [Microsoft.:::no-loc(Identity):::.Web sample](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2), which starts with a basic login and advances through multi-tenancy, using Azure APIs, using Microsoft Graph, and protecting your own APIs.</span></span> <span data-ttu-id="9404c-269">`Microsoft.:::no-loc(Identity):::.Web` è disponibile insieme a .NET 5.</span><span class="sxs-lookup"><span data-stu-id="9404c-269">`Microsoft.:::no-loc(Identity):::.Web` is available alongside .NET 5.</span></span>

### <a name="allow-anonymous-access-to-an-endpoint"></a><span data-ttu-id="9404c-270">Consenti accesso anonimo a un endpoint</span><span class="sxs-lookup"><span data-stu-id="9404c-270">Allow anonymous access to an endpoint</span></span>

<span data-ttu-id="9404c-271">Il `AllowAnonymous` metodo di estensione consente l'accesso anonimo a un endpoint:</span><span class="sxs-lookup"><span data-stu-id="9404c-271">The `AllowAnonymous` extension method allows anonymous access to an endpoint:</span></span>

[!code-csharp[](~/release-notes/sample/StartupAnonEndpoint.cs?name=snippet)]

### <a name="custom-handling-of-authorization-failures"></a><span data-ttu-id="9404c-272">Gestione personalizzata degli errori di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="9404c-272">Custom handling of authorization failures</span></span>

<span data-ttu-id="9404c-273">La gestione personalizzata degli errori di autorizzazione è ora più semplice con la nuova interfaccia [IAuthorizationMiddlewareResultHandler](https://github.com/dotnet/aspnetcore/blob/v5.0.0-rc.1.20451.17/src/Security/Authorization/Policy/src/IAuthorizationMiddlewareResultHandler.cs) richiamata dal [middleware](xref:fundamentals/middleware/index)di [autorizzazione](xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A) .</span><span class="sxs-lookup"><span data-stu-id="9404c-273">Custom handling of authorization failures is now easier with the new [IAuthorizationMiddlewareResultHandler](https://github.com/dotnet/aspnetcore/blob/v5.0.0-rc.1.20451.17/src/Security/Authorization/Policy/src/IAuthorizationMiddlewareResultHandler.cs) interface that is invoked by the [authorization](xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A) [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="9404c-274">L'implementazione predefinita rimane invariata, ma un gestore personalizzato può essere registrato in [Dependency Injection, che consente risposte HTTP personalizzate a seconda del motivo per cui l'autorizzazione non è riuscita.</span><span class="sxs-lookup"><span data-stu-id="9404c-274">The default implementation remains the same, but a custom handler can be registered in [Dependency injection, which allows custom HTTP responses based on why authorization failed.</span></span> <span data-ttu-id="9404c-275">Vedere [questo esempio](https://github.com/dotnet/aspnetcore/blob/master/src/Security/samples/CustomAuthorizationFailureResponse/Authorization/SampleAuthorizationMiddlewareResultHandler.cs) che illustra l'uso di `IAuthorizationMiddlewareResultHandler` .</span><span class="sxs-lookup"><span data-stu-id="9404c-275">See [this sample](https://github.com/dotnet/aspnetcore/blob/master/src/Security/samples/CustomAuthorizationFailureResponse/Authorization/SampleAuthorizationMiddlewareResultHandler.cs) that demonstrates usage of the `IAuthorizationMiddlewareResultHandler`.</span></span>

### <a name="authorization-when-using-endpoint-routing"></a><span data-ttu-id="9404c-276">Autorizzazione quando si usa il routing degli endpoint</span><span class="sxs-lookup"><span data-stu-id="9404c-276">Authorization when using endpoint routing</span></span>

<span data-ttu-id="9404c-277">L'autorizzazione quando si usa il routing degli endpoint ora riceve `HttpContext` invece l'istanza dell'endpoint.</span><span class="sxs-lookup"><span data-stu-id="9404c-277">Authorization when using endpoint routing now receives the `HttpContext` rather than the endpoint instance.</span></span> <span data-ttu-id="9404c-278">Ciò consente al middleware di autorizzazione di accedere a `RouteData` e ad altre proprietà di `HttpContext` che non erano accessibili tramite la <xref:Microsoft.AspNetCore.Http.Endpoint> classe.</span><span class="sxs-lookup"><span data-stu-id="9404c-278">This allows the authorization middleware to access the `RouteData` and other properties of the `HttpContext` that were not accessible though the <xref:Microsoft.AspNetCore.Http.Endpoint> class.</span></span> <span data-ttu-id="9404c-279">L'endpoint può essere recuperato dal contesto utilizzando il [contesto. GetEndPoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint%2A).</span><span class="sxs-lookup"><span data-stu-id="9404c-279">The endpoint can be fetched from the context using [context.GetEndpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint%2A).</span></span>

### <a name="role-based-access-control-with-kerberos-authentication-and-ldap-on-linux"></a><span data-ttu-id="9404c-280">Controllo degli accessi in base al ruolo con autenticazione Kerberos e LDAP in Linux</span><span class="sxs-lookup"><span data-stu-id="9404c-280">Role-based access control with Kerberos authentication and LDAP on Linux</span></span>

<span data-ttu-id="9404c-281">Vedere [autenticazione Kerberos e controllo degli accessi in base al ruolo (RBAC)](xref:security/authentication/windowsauth#rbac)</span><span class="sxs-lookup"><span data-stu-id="9404c-281">See [Kerberos authentication and role-based access control (RBAC)](xref:security/authentication/windowsauth#rbac)</span></span>

## <a name="api-improvements"></a><span data-ttu-id="9404c-282">Miglioramenti delle API</span><span class="sxs-lookup"><span data-stu-id="9404c-282">API improvements</span></span>

### <a name="json-extension-methods-for-httprequest-and-httpresponse"></a><span data-ttu-id="9404c-283">Metodi di estensione JSON per HttpRequest e HttpResponse</span><span class="sxs-lookup"><span data-stu-id="9404c-283">JSON extension methods for HttpRequest and HttpResponse</span></span>

<span data-ttu-id="9404c-284">I dati JSON possono essere letti e scritti da un oggetto `HttpRequest` e `HttpResponse` usando i <xref:System.Net.Http.Json.HttpContentJsonExtensions.ReadFromJsonAsync%2A> nuovi `WriteAsJsonAsync` metodi di estensione e.</span><span class="sxs-lookup"><span data-stu-id="9404c-284">JSON data can be read and written to from an `HttpRequest` and `HttpResponse` using the new <xref:System.Net.Http.Json.HttpContentJsonExtensions.ReadFromJsonAsync%2A> and `WriteAsJsonAsync` extension methods.</span></span> <span data-ttu-id="9404c-285">Questi metodi di estensione usano il [System.Text.Jssul](xref:System.Text.Json) serializzatore per gestire i dati JSON.</span><span class="sxs-lookup"><span data-stu-id="9404c-285">These extension methods use the [System.Text.Json](xref:System.Text.Json) serializer to handle the JSON data.</span></span> <span data-ttu-id="9404c-286">Il nuovo `HasJsonContentType` metodo di estensione può anche controllare se una richiesta ha un tipo di contenuto JSON.</span><span class="sxs-lookup"><span data-stu-id="9404c-286">The new `HasJsonContentType` extension method can also check if a request has a JSON content type.</span></span>

<span data-ttu-id="9404c-287">I metodi di estensione JSON possono essere combinati con il [routing dell'endpoint](xref:fundamentals/routing) per creare API JSON in uno stile di programmazione chiamato \* **route to Code** _.</span><span class="sxs-lookup"><span data-stu-id="9404c-287">The JSON extension methods can be combined with [endpoint routing](xref:fundamentals/routing) to create JSON APIs in a style of programming we call \* **route to code** _.</span></span> <span data-ttu-id="9404c-288">Si tratta di una nuova opzione per gli sviluppatori che desiderano creare API JSON di base in modo semplice.</span><span class="sxs-lookup"><span data-stu-id="9404c-288">It is a new option for developers who want to create basic JSON APIs in a lightweight way.</span></span> <span data-ttu-id="9404c-289">Ad esempio, un'app Web che dispone solo di un numero limitato di endpoint può scegliere di usare il routing al codice anziché le funzionalità complete di ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="9404c-289">For example, a web app that has only a handful of endpoints might choose to use route to code rather than the full functionality of ASP.NET Core MVC:</span></span>

```csharp
endpoints.MapGet("/weather/{city:alpha}", async context =>
{
    var city = (string)context.Request.RouteValues["city"];
    var weather = GetFromDatabase(city);

    await context.Response.WriteAsJsonAsync(weather);
});
```

<span data-ttu-id="9404c-290">Per altre informazioni sui nuovi metodi di estensione JSON e sul routing al codice, vedere [questa mostra .NET](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Route-to-Code).</span><span class="sxs-lookup"><span data-stu-id="9404c-290">For more information on the new JSON extension methods and route to code, see [this .NET show](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Route-to-Code).</span></span>

### <a name="systemdiagnosticsactivity"></a><span data-ttu-id="9404c-291">System. Diagnostics. Activity</span><span class="sxs-lookup"><span data-stu-id="9404c-291">System.Diagnostics.Activity</span></span>

<span data-ttu-id="9404c-292">Il formato predefinito per <xref:System.Diagnostics.Activity?displayProperty=fullName> ora è il formato W3C.</span><span class="sxs-lookup"><span data-stu-id="9404c-292">The default format for <xref:System.Diagnostics.Activity?displayProperty=fullName> now defaults to the W3C format.</span></span> <span data-ttu-id="9404c-293">In questo modo, il supporto per la traccia distribuita viene reso ASP.NET Core interoperabile con più Framework per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9404c-293">This makes distributed tracing support in ASP.NET Core interoperable with more frameworks by default.</span></span>

### <a name="frombodyattribute"></a><span data-ttu-id="9404c-294">FromBodyAttribute</span><span class="sxs-lookup"><span data-stu-id="9404c-294">FromBodyAttribute</span></span>

<span data-ttu-id="9404c-295"><xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute> ome supporta la configurazione di un'opzione che consente di considerare questi parametri o proprietà come facoltativi:</span><span class="sxs-lookup"><span data-stu-id="9404c-295"><xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute> ow supports configuring an option that allows these parameters or properties to be considered optional:</span></span>

```csharp
public IActionResult Post([FromBody(EmptyBodyBehavior = EmptyBodyBehavior.Allow)]
                           MyModel model) {
     ...
     }
```

## <a name="miscellaneous-improvements"></a><span data-ttu-id="9404c-296">Miglioramenti vari</span><span class="sxs-lookup"><span data-stu-id="9404c-296">Miscellaneous improvements</span></span>

<span data-ttu-id="9404c-297">È stata avviata l'applicazione di [annotazioni Nullable](/dotnet/csharp/nullable-references#attributes-describe-apis) a ASP.NET Core assembly.</span><span class="sxs-lookup"><span data-stu-id="9404c-297">We’ve started applying [nullable annotations](/dotnet/csharp/nullable-references#attributes-describe-apis) to ASP.NET Core assemblies.</span></span> <span data-ttu-id="9404c-298">Si prevede di aggiungere annotazioni alla maggior parte della superficie API pubblica comune del Framework .NET 5.</span><span class="sxs-lookup"><span data-stu-id="9404c-298">We plan to annotate most of the common public API surface of the .NET 5 framework.</span></span> <!-- Review: what's the impact of this? How does it work? Need more info.  Check the link I added -->

### <a name="control-startup-class-activation"></a><span data-ttu-id="9404c-299">Controllare l'attivazione della classe di avvio</span><span class="sxs-lookup"><span data-stu-id="9404c-299">Control Startup class activation</span></span>

<span data-ttu-id="9404c-300"><xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseStartup%2A>È stato aggiunto un overload aggiuntivo che consente a un'app di fornire un metodo factory per controllare l' `Startup` attivazione della classe.</span><span class="sxs-lookup"><span data-stu-id="9404c-300">An additional <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseStartup%2A> overload has been added that lets an app provide a factory method for controlling `Startup` class activation.</span></span> <span data-ttu-id="9404c-301">Il controllo dell' `Startup` attivazione della classe è utile per passare parametri aggiuntivi a `Startup` che vengono inizializzati insieme all'host:</span><span class="sxs-lookup"><span data-stu-id="9404c-301">Controlling `Startup` class activation is useful to pass additional parameters to `Startup` that are initialized along with the host:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var logger = CreateLogger();
        var host = Host.CreateDefaultBuilder()
            .ConfigureWebHost(builder =>
            {
                builder.UseStartup(context => new Startup(logger));
            })
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="auto-refresh-with-dotnet-watch"></a><span data-ttu-id="9404c-302">Aggiornamento automatico con DotNet Watch</span><span class="sxs-lookup"><span data-stu-id="9404c-302">Auto refresh with dotnet watch</span></span>

<span data-ttu-id="9404c-303">In .NET 5 l'esecuzione di [DotNet Watch](xref:tutorials/dotnet-watch) in un progetto di ASP.NET Core avvia il browser predefinito e aggiorna automaticamente il browser in base alle modifiche apportate al codice.</span><span class="sxs-lookup"><span data-stu-id="9404c-303">In .NET 5, running [dotnet watch](xref:tutorials/dotnet-watch) on an ASP.NET Core project both launches the default browser and auto refreshes the browser as changes are made to the code.</span></span> <span data-ttu-id="9404c-304">Ciò significa che è possibile:</span><span class="sxs-lookup"><span data-stu-id="9404c-304">This means you can:</span></span>

<span data-ttu-id="9404c-305">_ Aprire un progetto ASP.NET Core in un editor di testo.</span><span class="sxs-lookup"><span data-stu-id="9404c-305">_ Open an ASP.NET Core project in a text editor.</span></span>
* <span data-ttu-id="9404c-306">Eseguire `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="9404c-306">Run `dotnet watch`.</span></span>
* <span data-ttu-id="9404c-307">Concentrarsi sulle modifiche al codice mentre gli strumenti gestiscono la ricompilazione, il riavvio e il ricaricamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="9404c-307">Focus on the code changes while the tooling handles rebuilding, restarting, and reloading the app.</span></span>

<span data-ttu-id="9404c-308">Ci auguriamo che in futuro la funzionalità di aggiornamento automatico venga riportata in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9404c-308">We hope to bring the auto refresh functionality to Visual Studio in the future.</span></span>

### <a name="console-logger-formatter"></a><span data-ttu-id="9404c-309">Formattatore del logger della console</span><span class="sxs-lookup"><span data-stu-id="9404c-309">Console Logger Formatter</span></span>

<span data-ttu-id="9404c-310">Sono stati apportati miglioramenti al provider di log della console nella `Microsoft.Extensions.Logging` libreria.</span><span class="sxs-lookup"><span data-stu-id="9404c-310">Improvements have been made to the console log provider in the `Microsoft.Extensions.Logging` library.</span></span> <span data-ttu-id="9404c-311">Gli sviluppatori possono ora implementare un oggetto personalizzato `ConsoleFormatter` per esercitare il controllo completo sulla formattazione e la colorazione dell'output della console.</span><span class="sxs-lookup"><span data-stu-id="9404c-311">Developers can now implement a custom `ConsoleFormatter` to exercise complete control over formatting and colorization of the console output.</span></span> <span data-ttu-id="9404c-312">Le API del formattatore consentono la formattazione avanzata implementando un subset delle sequenze di escape VT-100.</span><span class="sxs-lookup"><span data-stu-id="9404c-312">The formatter APIs allow for rich formatting by implementing a subset of the VT-100 escape sequences.</span></span> <span data-ttu-id="9404c-313">VT-100 è supportato dalla maggior parte dei terminali moderni.</span><span class="sxs-lookup"><span data-stu-id="9404c-313">VT-100 is supported by most modern terminals.</span></span> <span data-ttu-id="9404c-314">Il logger della console può analizzare sequenze di escape su terminali non supportati che consentono agli sviluppatori di creare un singolo formattatore per tutti i terminali.</span><span class="sxs-lookup"><span data-stu-id="9404c-314">The console logger can parse out escape sequences on unsupported terminals allowing developers to author a single formatter for all terminals.</span></span>

### <a name="json-console-logger"></a><span data-ttu-id="9404c-315">Logger console JSON</span><span class="sxs-lookup"><span data-stu-id="9404c-315">JSON Console Logger</span></span>

<span data-ttu-id="9404c-316">Oltre al supporto per formattatori personalizzati, è stato aggiunto anche un formattatore JSON incorporato che emette log JSON strutturati nella console.</span><span class="sxs-lookup"><span data-stu-id="9404c-316">In addition to support for custom formatters, we’ve also added a built-in JSON formatter that emits structured JSON logs to the console.</span></span> <span data-ttu-id="9404c-317">Il codice seguente illustra come passare dal logger predefinito a JSON:</span><span class="sxs-lookup"><span data-stu-id="9404c-317">The following code shows how to switch from the default logger to JSON:</span></span>

[!code-csharp[](~/release-notes/sample/ProgramJsonLog.cs?name=snippet)]

<span data-ttu-id="9404c-318">I messaggi di log emessi alla console sono in formato JSON:</span><span class="sxs-lookup"><span data-stu-id="9404c-318">Log messages emitted to the console are JSON formatted:</span></span>

```json
{
  "EventId": 0,
  "LogLevel": "Information",
  "Category": "Microsoft.Hosting.Lifetime",
  "Message": "Now listening on: https://localhost:5001",
  "State": {
    "Message": "Now listening on: https://localhost:5001",
    "address": "https://localhost:5001",
    "{OriginalFormat}": "Now listening on: {address}"
  }
}
```