---
title: Configurare il trimmer per ASP.NET Core Blazor
author: guardrex
description: Informazioni su come controllare il linker linguaggio intermedio (IL) (trimmer) quando si compila un' Blazor app.
monikerRange: '>= aspnetcore-5.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/14/2020
no-loc:
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
uid: blazor/host-and-deploy/configure-trimmer
ms.openlocfilehash: 57d8f069c79b558020253968d736f350bc8a6f03
ms.sourcegitcommit: 24106b7ffffc9fff410a679863e28aeb2bbe5b7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/17/2020
ms.locfileid: "90721736"
---
# <a name="configure-the-trimmer-for-aspnet-core-no-locblazor"></a>Configurare il trimmer per ASP.NET Core Blazor

Di [il](https://github.com/pranavkm) suo

Blazor WebAssembly esegue il taglio del [linguaggio intermedio (il)](/dotnet/standard/managed-code#intermediate-language--execution) per ridurre le dimensioni dell'output pubblicato.

Il taglio di un'app ottimizza le dimensioni, ma può avere effetti negativi. Le app che usano la reflection o le funzionalità dinamiche correlate potrebbero interrompersi quando vengono tagliate, perché il trimmer non è a conoscenza del comportamento dinamico e non può determinare in generale quali tipi sono necessari per la reflection in fase di esecuzione. Per tagliare tali app, è necessario che il trimmer sia informato sui tipi necessari per la reflection nel codice e nei pacchetti o Framework da cui dipende l'app.

Per assicurarsi che l'app tagliata funzioni correttamente una volta distribuita, è importante testare spesso l'output pubblicato durante lo sviluppo.

Il trimming per le app .NET può essere disabilitato impostando la `PublishTrimmed` Proprietà MSBuild su `false` nel file di progetto dell'app:

```xml
<PropertyGroup>
  <PublishTrimmed>false</PublishTrimmed>
</PropertyGroup>
```
Opzioni aggiuntive per la configurazione del trimmer sono disponibili in [Opzioni di taglio](/dotnet/core/deploying/trimming-options).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Trimming delle distribuzioni autonome e degli eseguibili](/dotnet/core/deploying/trim-self-contained)
* <xref:blazor/webassembly-performance-best-practices#intermediate-language-il-trimming>