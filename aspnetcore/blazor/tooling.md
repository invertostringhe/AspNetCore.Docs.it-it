---
title: Strumenti per ASP.NET CoreBlazor
author: guardrex
description: Informazioni sugli strumenti disponibili per la compilazione di Blazor app.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/07/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/tooling
zone_pivot_groups: operating-systems
ms.openlocfilehash: 33245e669b317ed577a8a1652b2eed8f9ea5b915
ms.sourcegitcommit: 6fb27ea41a92f6d0e91dfd0eba905d2ac1a707f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2020
ms.locfileid: "86407645"
---
# <a name="tooling-for-aspnet-core-blazor"></a><span data-ttu-id="a8d07-103">Strumenti per ASP.NET CoreBlazor</span><span class="sxs-lookup"><span data-stu-id="a8d07-103">Tooling for ASP.NET Core Blazor</span></span>

<span data-ttu-id="a8d07-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a8d07-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

::: zone pivot="os-windows"

1. <span data-ttu-id="a8d07-105">Installare la versione più recente di [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) con il carico di lavoro **sviluppo di ASP.NET e Web** .</span><span class="sxs-lookup"><span data-stu-id="a8d07-105">Install the latest version of [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

1. <span data-ttu-id="a8d07-106">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="a8d07-106">Create a new project.</span></span>

1. <span data-ttu-id="a8d07-107">Selezionare \*\* Blazor app\*\*.</span><span class="sxs-lookup"><span data-stu-id="a8d07-107">Select **Blazor App**.</span></span> <span data-ttu-id="a8d07-108">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a8d07-108">Select **Next**.</span></span>

1. <span data-ttu-id="a8d07-109">Specificare il nome di un progetto nel campo **Nome progetto** oppure accettare il nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="a8d07-109">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="a8d07-110">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="a8d07-110">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="a8d07-111">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a8d07-111">Select **Create**.</span></span>

1. <span data-ttu-id="a8d07-112">Per un' Blazor WebAssembly esperienza, scegliere il modello \*\* Blazor WebAssembly app\*\* .</span><span class="sxs-lookup"><span data-stu-id="a8d07-112">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="a8d07-113">Per un' Blazor Server esperienza, scegliere il modello \*\* Blazor Server app\*\* .</span><span class="sxs-lookup"><span data-stu-id="a8d07-113">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="a8d07-114">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a8d07-114">Select **Create**.</span></span>

   <span data-ttu-id="a8d07-115">Per informazioni sui due Blazor modelli di hosting *Blazor WebAssembly* e *Blazor Server* , vedere <xref:blazor/hosting-models> .</span><span class="sxs-lookup"><span data-stu-id="a8d07-115">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="a8d07-116">Premere <kbd>CTRL</kbd> + <kbd>F5</kbd> per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a8d07-116">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

<span data-ttu-id="a8d07-117">Per ulteriori informazioni su come considerare attendibile il certificato di sviluppo HTTPS ASP.NET Core, vedere <xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos> .</span><span class="sxs-lookup"><span data-stu-id="a8d07-117">For more information on trusting the ASP.NET Core HTTPS development certificate, see <xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos>.</span></span>

::: zone-end

::: zone pivot="os-linux"

1. <span data-ttu-id="a8d07-118">Installare la versione più recente di [.NET Core 3,1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span><span class="sxs-lookup"><span data-stu-id="a8d07-118">Install the latest version of the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).</span></span> <span data-ttu-id="a8d07-119">Se in precedenza è stato installato l'SDK, è possibile determinare la versione installata eseguendo il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="a8d07-119">If you previously installed the SDK, you can determine your installed version by executing the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet --version
   ```

1. <span data-ttu-id="a8d07-120">Installare la versione più recente di [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="a8d07-120">Install the latest version of [Visual Studio Code](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="a8d07-121">Installare la versione più recente [di C# per Visual Studio Code estensione](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span><span class="sxs-lookup"><span data-stu-id="a8d07-121">Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span></span>

1. <span data-ttu-id="a8d07-122">Per un' Blazor WebAssembly esperienza, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="a8d07-122">For a Blazor WebAssembly experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   <span data-ttu-id="a8d07-123">Per un' Blazor Server esperienza, eseguire il comando seguente in una shell dei comandi:</span><span class="sxs-lookup"><span data-stu-id="a8d07-123">For a Blazor Server experience, execute the following command in a command shell:</span></span>

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   <span data-ttu-id="a8d07-124">Per informazioni sui due Blazor modelli di hosting *Blazor WebAssembly* e *Blazor Server* , vedere <xref:blazor/hosting-models> .</span><span class="sxs-lookup"><span data-stu-id="a8d07-124">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="a8d07-125">Aprire la cartella `WebApplication1` in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a8d07-125">Open the `WebApplication1` folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="a8d07-126">Le richieste dell'IDE aggiungono risorse per compilare ed eseguire il debug del progetto.</span><span class="sxs-lookup"><span data-stu-id="a8d07-126">The IDE requests that you add assets to build and debug the project.</span></span> <span data-ttu-id="a8d07-127">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="a8d07-127">Select **Yes**.</span></span>

1. <span data-ttu-id="a8d07-128">Premere <kbd>CTRL</kbd> + <kbd>F5</kbd> per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a8d07-128">Press <kbd>Ctrl</kbd>+<kbd>F5</kbd> to run the app.</span></span>

## <a name="trust-a-development-certificate"></a><span data-ttu-id="a8d07-129">Attendibilità di un certificato di sviluppo</span><span class="sxs-lookup"><span data-stu-id="a8d07-129">Trust a development certificate</span></span>

<span data-ttu-id="a8d07-130">Non esiste un modo centralizzato per considerare attendibile un certificato in Linux.</span><span class="sxs-lookup"><span data-stu-id="a8d07-130">There's no centralized way to trust a certificate on Linux.</span></span> <span data-ttu-id="a8d07-131">Viene in genere adottato uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="a8d07-131">Typically, one of the following approaches is adopted:</span></span>

* <span data-ttu-id="a8d07-132">Escludere l'URL dell'app nell'elenco di esclusione del browser.</span><span class="sxs-lookup"><span data-stu-id="a8d07-132">Exclude the app's URL in browser's exclude list.</span></span>
* <span data-ttu-id="a8d07-133">Considerare attendibili tutti i certificati autofirmati per `localhost` .</span><span class="sxs-lookup"><span data-stu-id="a8d07-133">Trust all self-signed certificates for `localhost`.</span></span>
* <span data-ttu-id="a8d07-134">Aggiungere il certificato all'elenco dei certificati attendibili nel browser.</span><span class="sxs-lookup"><span data-stu-id="a8d07-134">Add the certificate to the list of trusted certificates in the browser.</span></span>

<span data-ttu-id="a8d07-135">Per altre informazioni, vedere le linee guida fornite dal browser e dalla distribuzione Linux.</span><span class="sxs-lookup"><span data-stu-id="a8d07-135">For more information, see the guidance provided by your browser and Linux distribution.</span></span>

::: zone-end

::: zone pivot="os-macos"

1. <span data-ttu-id="a8d07-136">Installare [Visual Studio per Mac](https://visualstudio.microsoft.com/vs/mac/).</span><span class="sxs-lookup"><span data-stu-id="a8d07-136">Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/).</span></span>

1. <span data-ttu-id="a8d07-137">Selezionare **file**  >  **nuova soluzione** o creare un **nuovo** progetto dalla **finestra Start**.</span><span class="sxs-lookup"><span data-stu-id="a8d07-137">Select **File** > **New Solution** or create a **New** project from the **Start Window**.</span></span>

1. <span data-ttu-id="a8d07-138">Nella barra laterale selezionare app **Web e console**  >  **App**.</span><span class="sxs-lookup"><span data-stu-id="a8d07-138">In the sidebar, select **Web and Console** > **App**.</span></span>

   <span data-ttu-id="a8d07-139">Per un' Blazor WebAssembly esperienza, scegliere il modello \*\* Blazor WebAssembly app\*\* .</span><span class="sxs-lookup"><span data-stu-id="a8d07-139">For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template.</span></span> <span data-ttu-id="a8d07-140">Per un' Blazor Server esperienza, scegliere il modello \*\* Blazor Server app\*\* .</span><span class="sxs-lookup"><span data-stu-id="a8d07-140">For a Blazor Server experience, choose the **Blazor Server App** template.</span></span> <span data-ttu-id="a8d07-141">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a8d07-141">Select **Next**.</span></span>

   <span data-ttu-id="a8d07-142">Per informazioni sui due Blazor modelli di hosting *Blazor WebAssembly* e *Blazor Server* , vedere <xref:blazor/hosting-models> .</span><span class="sxs-lookup"><span data-stu-id="a8d07-142">For information on the two Blazor hosting models, *Blazor WebAssembly* and *Blazor Server*, see <xref:blazor/hosting-models>.</span></span>

1. <span data-ttu-id="a8d07-143">Verificare che **l'autenticazione** sia impostata su **Nessuna autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="a8d07-143">Confirm that **Authentication** is set to **No Authentication**.</span></span> <span data-ttu-id="a8d07-144">Selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a8d07-144">Select **Next**.</span></span>

1. <span data-ttu-id="a8d07-145">Nel campo **nome progetto** assegnare un nome all'app `WebApplication1` .</span><span class="sxs-lookup"><span data-stu-id="a8d07-145">In the **Project Name** field, name the app `WebApplication1`.</span></span> <span data-ttu-id="a8d07-146">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a8d07-146">Select **Create**.</span></span>

1. <span data-ttu-id="a8d07-147">Selezionare **Esegui**  >  **Avvia senza eseguire debug** per eseguire l'app *senza il debugger*.</span><span class="sxs-lookup"><span data-stu-id="a8d07-147">Select **Run** > **Start Without Debugging** to run the app *without the debugger*.</span></span> <span data-ttu-id="a8d07-148">Eseguire l'app con **Esegui**  >  **debug Avvia debug** o il pulsante Esegui (&#9654;) per eseguire l'app *con il debugger*.</span><span class="sxs-lookup"><span data-stu-id="a8d07-148">Run the app with **Run** > **Start Debugging** or the Run (&#9654;) button to run the app *with the debugger*.</span></span>

<span data-ttu-id="a8d07-149">Se viene visualizzato un messaggio per considerare attendibile il certificato di sviluppo, considerare attendibile il certificato e continuare.</span><span class="sxs-lookup"><span data-stu-id="a8d07-149">If a prompt appears to trust the development certificate, trust the certificate and continue.</span></span> <span data-ttu-id="a8d07-150">Per considerare attendibile il certificato, è necessario specificare le password dell'utente e del keychain.</span><span class="sxs-lookup"><span data-stu-id="a8d07-150">The user and keychain passwords are required to trust the certificate.</span></span> <span data-ttu-id="a8d07-151">Per ulteriori informazioni su come considerare attendibile il certificato di sviluppo HTTPS ASP.NET Core, vedere <xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos> .</span><span class="sxs-lookup"><span data-stu-id="a8d07-151">For more information on trusting the ASP.NET Core HTTPS development certificate, see <xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos>.</span></span>

::: zone-end