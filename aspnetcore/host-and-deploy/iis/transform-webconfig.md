---
title: Trasformare web.config
author: guardrex
description: Informazioni su come trasformare il file web.config quando si pubblica un'app ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2019
uid: host-and-deploy/iis/transform-webconfig
ms.openlocfilehash: bd8cf7d8515e874eefd2c326727f56d0a4b502a7
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248603"
---
# <a name="transform-webconfig"></a><span data-ttu-id="54267-103">Trasformare web.config</span><span class="sxs-lookup"><span data-stu-id="54267-103">Transform web.config</span></span>

<span data-ttu-id="54267-104">Di [Vijay Ramakrishnan](https://github.com/vijayrkn) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="54267-104">By [Vijay Ramakrishnan](https://github.com/vijayrkn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="54267-105">Le trasformazioni per il file *web.config* possono essere applicate automaticamente quando viene pubblicata un'app in base a:</span><span class="sxs-lookup"><span data-stu-id="54267-105">Transformations to the *web.config* file can be applied automatically when an app is published based on:</span></span>

* [<span data-ttu-id="54267-106">Configurazione della build</span><span class="sxs-lookup"><span data-stu-id="54267-106">Build configuration</span></span>](#build-configuration)
* [<span data-ttu-id="54267-107">Profilo</span><span class="sxs-lookup"><span data-stu-id="54267-107">Profile</span></span>](#profile)
* [<span data-ttu-id="54267-108">Ambiente</span><span class="sxs-lookup"><span data-stu-id="54267-108">Environment</span></span>](#environment)
* [<span data-ttu-id="54267-109">Personalizzato</span><span class="sxs-lookup"><span data-stu-id="54267-109">Custom</span></span>](#custom)

<span data-ttu-id="54267-110">Queste trasformazioni si verificano per uno degli scenari di generazione di *web.config* seguenti:</span><span class="sxs-lookup"><span data-stu-id="54267-110">These transformations occur for either of the following *web.config* generation scenarios:</span></span>

* <span data-ttu-id="54267-111">Generati automaticamente dall'SDK `Microsoft.NET.Sdk.Web`.</span><span class="sxs-lookup"><span data-stu-id="54267-111">Generated automatically by the `Microsoft.NET.Sdk.Web` SDK.</span></span>
* <span data-ttu-id="54267-112">Forniti dallo sviluppatore nella radice del contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="54267-112">Provided by the developer in the content root of the app.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="54267-113">Configurazione della compilazione</span><span class="sxs-lookup"><span data-stu-id="54267-113">Build configuration</span></span>

<span data-ttu-id="54267-114">Le trasformazioni di configurazione della build vengono eseguite per prime.</span><span class="sxs-lookup"><span data-stu-id="54267-114">Build configuration transforms are run first.</span></span>

<span data-ttu-id="54267-115">Includere un file *web.{CONFIGURAZIONE}.config* per ogni [configurazione della build (Debug|Versione)](/dotnet/core/tools/dotnet-publish#options) che richiede una trasformazione di *web.config*.</span><span class="sxs-lookup"><span data-stu-id="54267-115">Include a *web.{CONFIGURATION}.config* file for each [build configuration (Debug|Release)](/dotnet/core/tools/dotnet-publish#options) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="54267-116">Nell'esempio seguente, una variabile di ambiente specifica della configurazione viene impostata in *web.Release.config*:</span><span class="sxs-lookup"><span data-stu-id="54267-116">In the following example, a configuration-specific environment variable is set in *web.Release.config*:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Configuration_Specific" 
                               value="Configuration_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="54267-117">La trasformazione viene applicata quando la configurazione è impostata su *Versione*:</span><span class="sxs-lookup"><span data-stu-id="54267-117">The transform is applied when the configuration is set to *Release*:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="54267-118">La proprietà di MSBuild per la configurazione è `$(Configuration)`.</span><span class="sxs-lookup"><span data-stu-id="54267-118">The MSBuild property for the configuration is `$(Configuration)`.</span></span>

## <a name="profile"></a><span data-ttu-id="54267-119">Profilo</span><span class="sxs-lookup"><span data-stu-id="54267-119">Profile</span></span>

<span data-ttu-id="54267-120">Le trasformazioni di profilo vengono eseguite per seconde, dopo le trasformazioni di [configurazione della build](#build-configuration).</span><span class="sxs-lookup"><span data-stu-id="54267-120">Profile transformations are run second, after [Build configuration](#build-configuration) transforms.</span></span>

<span data-ttu-id="54267-121">Includere un file *web.{PROFILO}.config* per ogni configurazione del profilo che richiede una trasformazione di *web.config*.</span><span class="sxs-lookup"><span data-stu-id="54267-121">Include a *web.{PROFILE}.config* file for each profile configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="54267-122">Nell'esempio seguente, una variabile di ambiente per il profilo è impostata in *web.FolderProfile.config* per un profilo di pubblicazione di cartella:</span><span class="sxs-lookup"><span data-stu-id="54267-122">In the following example, a profile-specific environment variable is set in *web.FolderProfile.config* for a folder publish profile:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Profile_Specific" 
                               value="Profile_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="54267-123">La trasformazione viene applicata quando il profilo è *FolderProfile*:</span><span class="sxs-lookup"><span data-stu-id="54267-123">The transform is applied when the profile is *FolderProfile*:</span></span>

```console
dotnet publish --configuration Release /p:PublishProfile=FolderProfile
```

<span data-ttu-id="54267-124">La proprietà di MSBuild per il nome del profilo è `$(PublishProfile)`.</span><span class="sxs-lookup"><span data-stu-id="54267-124">The MSBuild property for the profile name is `$(PublishProfile)`.</span></span>

<span data-ttu-id="54267-125">Se non viene passato alcun profilo, il nome del profilo predefinito è **FileSystem** e viene applicato *web.FileSystem.config* se il file è presente nella radice del contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="54267-125">If no profile is passed, the default profile name is **FileSystem** and *web.FileSystem.config* is applied if the file is present in the app's content root.</span></span>

## <a name="environment"></a><span data-ttu-id="54267-126">Ambiente</span><span class="sxs-lookup"><span data-stu-id="54267-126">Environment</span></span>

<span data-ttu-id="54267-127">Le trasformazioni di ambiente vengono eseguite per terze, dopo le trasformazioni di [configurazione della build](#build-configuration) e di [profilo](#profile).</span><span class="sxs-lookup"><span data-stu-id="54267-127">Environment transformations are run third, after [Build configuration](#build-configuration) and [Profile](#profile) transforms.</span></span>

<span data-ttu-id="54267-128">Includere un file *web.{AMBIENTE}.config* per ogni [ambiente](xref:fundamentals/environments) che richiede una trasformazione di *web.config*.</span><span class="sxs-lookup"><span data-stu-id="54267-128">Include a *web.{ENVIRONMENT}.config* file for each [environment](xref:fundamentals/environments) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="54267-129">Nell'esempio seguente, una variabile di ambiente specifica dell'ambiente viene impostata in *web.Production.config* per l'ambiente Production:</span><span class="sxs-lookup"><span data-stu-id="54267-129">In the following example, a environment-specific environment variable is set in *web.Production.config* for the Production environment:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Environment_Specific" 
                               value="Environment_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="54267-130">La trasformazione viene applicata quando l'ambiente è *Production*:</span><span class="sxs-lookup"><span data-stu-id="54267-130">The transform is applied when the environment is *Production*:</span></span>

```console
dotnet publish --configuration Release /p:EnvironmentName=Production
```

<span data-ttu-id="54267-131">La proprietà di MSBuild per l'ambiente è `$(EnvironmentName)`.</span><span class="sxs-lookup"><span data-stu-id="54267-131">The MSBuild property for the environment is `$(EnvironmentName)`.</span></span>

<span data-ttu-id="54267-132">Quando di esegue la pubblicazione da Visual Studio usando un profilo di pubblicazione, vedere <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span><span class="sxs-lookup"><span data-stu-id="54267-132">When publishing from Visual Studio and using a publish profile, see <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span></span>

<span data-ttu-id="54267-133">La variabile di ambiente `ASPNETCORE_ENVIRONMENT` viene aggiunta automaticamente al file *Web.config* quando viene specificato il nome dell'ambiente.</span><span class="sxs-lookup"><span data-stu-id="54267-133">The `ASPNETCORE_ENVIRONMENT` environment variable is automatically added to the *web.config* file when the environment name is specified.</span></span>

## <a name="custom"></a><span data-ttu-id="54267-134">Custom (Personalizzati)</span><span class="sxs-lookup"><span data-stu-id="54267-134">Custom</span></span>

<span data-ttu-id="54267-135">Le trasformazioni personalizzate vengono eseguite per ultime, dopo le trasformazioni di [configurazione della build](#build-configuration), di [profilo](#profile) e di [ambiente](#environment).</span><span class="sxs-lookup"><span data-stu-id="54267-135">Custom transformations are run last, after [Build configuration](#build-configuration), [Profile](#profile), and [Environment](#environment) transforms.</span></span>

<span data-ttu-id="54267-136">Includere un file *{NOME_PERSONALIZZATO}.transform* per ogni configurazione personalizzata che richiede una trasformazione di *web.config*.</span><span class="sxs-lookup"><span data-stu-id="54267-136">Include a *{CUSTOM_NAME}.transform* file for each custom configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="54267-137">Nell'esempio seguente viene impostata una variabile di ambiente per una trasformazione personalizzata in *custom.transform*:</span><span class="sxs-lookup"><span data-stu-id="54267-137">In the following example, a custom transform environment variable is set in *custom.transform*:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Custom_Specific" 
                               value="Custom_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="54267-138">La trasformazione viene applicata al passaggio della proprietà `CustomTransformFileName` al comando [dotnet publish](/dotnet/core/tools/dotnet-publish):</span><span class="sxs-lookup"><span data-stu-id="54267-138">The transform is applied when the `CustomTransformFileName` property is passed to the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

```console
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

<span data-ttu-id="54267-139">La proprietà di MSBuild per il nome del profilo è `$(CustomTransformFileName)`.</span><span class="sxs-lookup"><span data-stu-id="54267-139">The MSBuild property for the profile name is `$(CustomTransformFileName)`.</span></span>

## <a name="prevent-webconfig-transformation"></a><span data-ttu-id="54267-140">Impedire trasformazioni di web.config</span><span class="sxs-lookup"><span data-stu-id="54267-140">Prevent web.config transformation</span></span>

<span data-ttu-id="54267-141">Per impedire le trasformazioni del file *web.config*, impostare la proprietà MSBuild `$(IsWebConfigTransformDisabled)`:</span><span class="sxs-lookup"><span data-stu-id="54267-141">To prevent transformations of the *web.config* file, set the MSBuild property `$(IsWebConfigTransformDisabled)`:</span></span>

```console
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a><span data-ttu-id="54267-142">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="54267-142">Additional resources</span></span>

* [<span data-ttu-id="54267-143">Sintassi di trasformazione di Web.config per la distribuzione di un progetto di applicazione Web</span><span class="sxs-lookup"><span data-stu-id="54267-143">Web.config Transformation Syntax for Web Application Project Deployment</span></span>](http://go.microsoft.com/fwlink/?LinkId=301874)
* <span data-ttu-id="54267-144">[Sintassi di trasformazione di Web.config per la distribuzione di un progetto Web tramite Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))</span><span class="sxs-lookup"><span data-stu-id="54267-144">[Web.config Transformation Syntax for Web Project Deployment Using Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))</span></span>