---
title: Proteggere un' Blazor WebAssembly app ospitata ASP.NET Core con Azure Active Directory
author: guardrex
description: Informazioni su come proteggere un' Blazor WebAssembly app ospitata ASP.NET Core con Azure Active Directory.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: devx-track-csharp, mvc
ms.date: 10/27/2020
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
uid: blazor/security/webassembly/hosted-with-azure-active-directory
ms.openlocfilehash: 3d0cc8d9891e0f656651a83dcd0bfdebfc266920
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2020
ms.locfileid: "93055321"
---
# <a name="secure-an-aspnet-core-no-locblazor-webassembly-hosted-app-with-azure-active-directory"></a>Proteggere un' Blazor WebAssembly app ospitata ASP.NET Core con Azure Active Directory

Di [Javier Calvarro Nelson](https://github.com/javiercn) e [Luke Latham](https://github.com/guardrex)

Questo articolo descrive come creare un' [ Blazor WebAssembly app ospitata](xref:blazor/hosting-models#blazor-webassembly) che usa [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) per l'autenticazione.

::: moniker range=">= aspnetcore-5.0"

> [!NOTE]
> Per Blazor WebAssembly le app create in Visual Studio configurate per supportare gli account in una directory organizzativa di AAD, Visual Studio non configura correttamente l'app nella generazione del progetto. Questa operazione verrà risolta in una versione futura di Visual Studio. Questo articolo illustra come creare l'app con il `dotnet new` comando interfaccia della riga di comando di .NET Core. Se si preferisce creare l'app con Visual Studio prima che l'IDE venga aggiornato per i modelli più recenti Blazor in ASP.NET Core 5,0, fare riferimento a ogni sezione di questo articolo e confermare o aggiornare la configurazione dell'app dopo che Visual Studio ha creato l'app.

::: moniker-end

## <a name="register-apps-in-aad-and-create-solution"></a>Registrare le app in AAD e creare la soluzione

### <a name="create-a-tenant"></a>Creare un tenant

Seguire le istruzioni riportate nella [Guida introduttiva: configurare un tenant](/azure/active-directory/develop/quickstart-create-new-tenant) per creare un tenant in AAD.

### <a name="register-a-server-api-app"></a>Registrare un'app per le API server

Seguire le istruzioni disponibili in [Guida introduttiva: registrare un'applicazione con la piattaforma di identità Microsoft](/azure/active-directory/develop/quickstart-register-app) e gli argomenti di Azure AAD successivi per registrare un'app AAD per l'app per le *API del server* e quindi eseguire le operazioni seguenti:

1. In **Azure Active Directory**  >  **registrazioni app** selezionare **nuova registrazione** .
1. Specificare un **nome** per l'app (ad esempio, **Blazor Server AAD** ).
1. Scegliere i **tipi di conto supportati** . Per questa esperienza è possibile selezionare **solo gli account in questa directory aziendale** (tenant singolo).
1. L' *app* per le API del server non richiede un **URI di reindirizzamento** in questo scenario, quindi lasciare l'elenco a discesa impostato su **Web** e non immettere un URI di reindirizzamento.
1. Deselezionare la **Permissions**  >  casella di controllo autorizzazioni **Concedi amministratore per OpenID e autorizzazioni offline_access** .
1. Selezionare **Registra** .

Registrare le seguenti informazioni:

* *App per le API server* ID applicazione (client) (ad esempio, `41451fa7-82d9-4673-8fa5-69eff5a761fd` )
* ID directory (tenant) (ad esempio, `e86c78e2-8bb4-4c41-aefd-918e0565a45e` )
* Dominio primario/editore/tenant di AAD (ad esempio, `contoso.onmicrosoft.com` ): il dominio è disponibile come **dominio del server di pubblicazione** nel pannello **personalizzazione** del portale di Azure per l'app registrata.

In **autorizzazioni API** rimuovere l'autorizzazione **Microsoft Graph**  >  **utente. Read** perché l'app non richiede l'accesso o il profilo utente.

In **esporre un'API** :

1. Selezionare **Aggiungi un ambito** .
1. Fai clic su **Salva e continua** .
1. Specificare un **nome** per l'ambito (ad esempio, `API.Access` ).
1. Fornire un **nome visualizzato** per il consenso dell'amministratore, ad esempio `Access API` .
1. Fornire una **Descrizione del consenso dell'amministratore** , ad esempio `Allows the app to access server app API endpoints.` .
1. Verificare che lo **stato** sia impostato su **abilitato** .
1. Selezionare **Aggiungi ambito** .

Registrare le seguenti informazioni:

* URI ID app (ad esempio, `api://41451fa7-82d9-4673-8fa5-69eff5a761fd` , `https://contoso.onmicrosoft.com/41451fa7-82d9-4673-8fa5-69eff5a761fd` o il valore personalizzato fornito)
* Nome ambito (ad esempio, `API.Access` )

### <a name="register-a-client-app"></a>Registrare un'app client

Seguire le istruzioni disponibili in [Guida introduttiva: registrare un'applicazione con la piattaforma di identità Microsoft](/azure/active-directory/develop/quickstart-register-app) e gli argomenti di Azure AAD successivi per registrare un'app AAD per l' *`Client`* app e quindi eseguire le operazioni seguenti:

::: moniker range=">= aspnetcore-5.0"

1. In **Azure Active Directory** > **registrazioni app** selezionare **nuova registrazione** .
1. Specificare un **nome** per l'app (ad esempio, **Blazor client AAD** ).
1. Scegliere i **tipi di conto supportati** . Per questa esperienza è possibile selezionare **solo gli account in questa directory aziendale** (tenant singolo).
1. Impostare l'elenco a discesa **URI di reindirizzamento** sull' **applicazione a pagina singola (Spa)** e specificare l'URI di reindirizzamento seguente: `https://localhost:{PORT}/authentication/login-callback` . La porta predefinita per un'app in esecuzione in Kestrel è la 5001. Se l'app viene eseguita su una porta di Gheppio diversa, usare la porta dell'app. Per IIS Express, la porta generata in modo casuale per l'app si trova nelle *`Server`* proprietà dell'app nel pannello **debug** . Poiché l'app non esiste in questo momento e la porta IIS Express non è nota, tornare a questo passaggio dopo la creazione dell'app e aggiornare l'URI di reindirizzamento. Un contrassegno viene visualizzato nella sezione [creare l'app](#create-the-app) per ricordare IIS Express agli utenti di aggiornare l'URI di reindirizzamento.
1. Deselezionare la **Permissions** > casella di controllo autorizzazioni **Concedi amministratore per OpenID e autorizzazioni offline_access** .
1. Selezionare **Registra** .

Registrare l' *`Client`* ID dell'applicazione App (client) (ad esempio, `4369008b-21fa-427c-abaa-9b53bf58e538` ).

Nelle configurazioni della piattaforma di **autenticazione** > **Platform configurations** > **applicazione a pagina singola (Spa)** :

1. Verificare che l' **URI di reindirizzamento** di `https://localhost:{PORT}/authentication/login-callback` sia presente.
1. Per **concessione implicita** , assicurarsi che le caselle di controllo per i **token di accesso** e i **token ID** **non** siano selezionate.
1. Per questa esperienza sono accettabili le impostazioni predefinite rimanenti per l'app.
1. Fare clic sul pulsante **Salva** .

::: moniker-end

::: moniker range="< aspnetcore-5.0"

1. In **Azure Active Directory** > **registrazioni app** selezionare **nuova registrazione** .
1. Specificare un **nome** per l'app (ad esempio, **Blazor client AAD** ).
1. Scegliere i **tipi di conto supportati** . Per questa esperienza è possibile selezionare **solo gli account in questa directory aziendale** (tenant singolo).
1. Lasciare l'elenco a discesa **URI di reindirizzamento** impostato su **Web** e specificare l'URI di reindirizzamento seguente: `https://localhost:{PORT}/authentication/login-callback` . La porta predefinita per un'app in esecuzione in Kestrel è la 5001. Se l'app viene eseguita su una porta di Gheppio diversa, usare la porta dell'app. Per IIS Express, la porta generata in modo casuale per l'app si trova nelle *`Server`* proprietà dell'app nel pannello **debug** . Poiché l'app non esiste in questo momento e la porta IIS Express non è nota, tornare a questo passaggio dopo la creazione dell'app e aggiornare l'URI di reindirizzamento. Un contrassegno viene visualizzato nella sezione [creare l'app](#create-the-app) per ricordare IIS Express agli utenti di aggiornare l'URI di reindirizzamento.
1. Deselezionare la **Permissions** > casella di controllo autorizzazioni **Concedi amministratore per OpenID e autorizzazioni offline_access** .
1. Selezionare **Registra** .

Registrare l' *`Client`* ID dell'applicazione App (client) (ad esempio, `4369008b-21fa-427c-abaa-9b53bf58e538` ).

In **Authentication** > **configurazioni piattaforma** di autenticazione > **Web** :

1. Verificare che l' **URI di reindirizzamento** di `https://localhost:{PORT}/authentication/login-callback` sia presente.
1. Per **concessione implicita** , selezionare le caselle di controllo per i token di **accesso** e i **token ID** .
1. Per questa esperienza sono accettabili le impostazioni predefinite rimanenti per l'app.
1. Fare clic sul pulsante **Salva** .

::: moniker-end

In **autorizzazioni API** :

1. Verificare che l'app disponga **Microsoft Graph**  >  autorizzazione **User. Read** .
1. Selezionare **Aggiungi un'autorizzazione** seguita da **API personali** .
1. Selezionare l' *app per le API server* dalla colonna **nome** (ad esempio, **Blazor Server AAD** ).
1. Aprire l'elenco di **API** .
1. Abilitare l'accesso all'API (ad esempio, `API.Access` ).
1. Selezionare **Aggiungi autorizzazioni** .
1. Selezionare il pulsante **concedere il consenso dell'amministratore per {tenant Name}** . Selezionare **Sì** per confermare.

### <a name="create-the-app"></a>Creare l'app

In una cartella vuota, sostituire i segnaposto nel comando seguente con le informazioni registrate in precedenza ed eseguire il comando in una shell dei comandi:

```dotnetcli
dotnet new blazorwasm -au SingleOrg --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{TENANT DOMAIN}" -ho -o {APP NAME} --tenant-id "{TENANT ID}"
```

| Segnaposto                  | Nome portale di Azure                                     | Esempio                                      |
| ---------------------------- | ----------------------------------------------------- | -------------------------------------------- |
| `{APP NAME}`                 | &mdash;                                               | `BlazorSample`                               |
| `{CLIENT APP CLIENT ID}`     | ID applicazione (client) per l' *`Client`* app        | `4369008b-21fa-427c-abaa-9b53bf58e538`       |
| `{DEFAULT SCOPE}`            | Nome ambito                                            | `API.Access`                                 |
| `{SERVER API APP CLIENT ID}` | ID applicazione (client) per l' *app per le API server*      | `41451fa7-82d9-4673-8fa5-69eff5a761fd`       |
| `{SERVER API APP ID URI}`    | URI dell'ID applicazione                                    | `api://41451fa7-82d9-4673-8fa5-69eff5a761fd` |
| `{TENANT DOMAIN}`            | Dominio primario/server di pubblicazione/tenant                       | `contoso.onmicrosoft.com`                    |
| `{TENANT ID}`                | ID directory (tenant)                                 | `e86c78e2-8bb4-4c41-aefd-918e0565a45e`       |

Il percorso di output specificato con l'opzione `-o|--output` consente di creare una cartella di progetto se non esiste e viene incluso nel nome dell'app.

::: moniker range=">= aspnetcore-5.0"

> [!NOTE]
> Potrebbe essere necessaria una modifica alla configurazione quando si usa un tenant di Azure con un dominio Publisher non verificato, descritto nella sezione [impostazioni app](#app-settings) .

::: moniker-end

::: moniker range="< aspnetcore-5.0"

> [!NOTE]
> Potrebbe essere necessaria una modifica alla configurazione quando si usa un tenant di Azure con un dominio del server di pubblicazione non verificato, descritto nella sezione [ambiti dei token di accesso](#access-token-scopes) .

::: moniker-end

> [!NOTE]
> Nell'portale di Azure l' *`Client`* **URI di reindirizzamento** della configurazione della piattaforma dell'app è configurato per la porta 5001 per le app eseguite nel server gheppio con le impostazioni predefinite.
>
> Se l' *`Client`* app viene eseguita su una porta IIS Express casuale, la porta per l'app si trova nelle proprietà dell' *app* per le API del server nel pannello **debug** .
>
> Se la porta non è stata configurata in precedenza con la *`Client`* porta nota dell'app, tornare alla *`Client`* registrazione dell'app nel portale di Azure e aggiornare l'URI di reindirizzamento con la porta corretta.

## <a name="server-app-configuration"></a>*`Server`* configurazione dell'app

*Questa sezione è relativa all'app della soluzione **`Server`** .*

### <a name="authentication-package"></a>Pacchetto di autenticazione

::: moniker range=">= aspnetcore-5.0"

Il supporto per l'autenticazione e l'autorizzazione delle chiamate a ASP.NET Core API Web con la Identity piattaforma Microsoft è fornito dai pacchetti seguenti:

* [`Microsoft.Identity.Web`](https://www.nuget.org/packages/Microsoft.Identity.Web)
* [`Microsoft.Identity.Web.UI`](https://www.nuget.org/packages/Microsoft.Identity.Web.UI)

```xml
<PackageReference Include="Microsoft.Identity.Web" Version="{VERSION}" />
<PackageReference Include="Microsoft.Identity.Web.UI" Version="{VERSION}" />
```

Per il segnaposto `{VERSION}` , la versione stabile più recente del pacchetto che corrisponde alla versione del Framework condiviso dell'app è disponibile nella cronologia delle **versioni** del pacchetto in NuGet.org.

::: moniker-end

::: moniker range="< aspnetcore-5.0"

Il pacchetto fornisce il supporto per l'autenticazione e l'autorizzazione delle chiamate a ASP.NET Core API Web [`Microsoft.AspNetCore.Authentication.AzureAD.UI`](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureAD.UI) :

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureAD.UI" 
  Version="{VERSION}" />
```

Per il segnaposto `{VERSION}` , la versione stabile più recente del pacchetto che corrisponde alla versione del Framework condiviso dell'app è disponibile nella cronologia delle **versioni** del pacchetto in [NuGet.org](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureAD.UI).

::: moniker-end

### <a name="authentication-service-support"></a>Supporto del servizio di autenticazione

::: moniker range=">= aspnetcore-5.0"

Il `AddAuthentication` metodo imposta i servizi di autenticazione all'interno dell'app e configura il gestore di JWT Bearer come metodo di autenticazione predefinito. Il <xref:Microsoft.Identity.Web.MicrosoftIdentityWebApiAuthenticationBuilderExtensions.AddMicrosoftIdentityWebApi%2A> metodo configura i servizi per proteggere l'API Web con Microsoft Identity Platform v 2.0. Questo metodo prevede una `AzureAd` sezione nella configurazione dell'app con le impostazioni necessarie per inizializzare le opzioni di autenticazione.

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApi(Configuration.GetSection("AzureAd"));
```

::: moniker-end

::: moniker range="< aspnetcore-5.0"

Il `AddAuthentication` metodo imposta i servizi di autenticazione all'interno dell'app e configura il gestore di JWT Bearer come metodo di autenticazione predefinito. Il <xref:Microsoft.AspNetCore.Authentication.AzureADAuthenticationBuilderExtensions.AddAzureADBearer%2A> metodo imposta i parametri specifici nel gestore di JWT Bearer necessario per convalidare i token emessi dal Azure Active Directory:

```csharp
services.AddAuthentication(AzureADDefaults.BearerAuthenticationScheme)
    .AddAzureADBearer(options => Configuration.Bind("AzureAd", options));
```

::: moniker-end

<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication%2A> e <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A> verificare che:

* L'app tenta di analizzare e convalidare i token nelle richieste in ingresso.
* Eventuali richieste che tentano di accedere a una risorsa protetta senza credenziali appropriate hanno esito negativo.

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="userno-locidentityname"></a>Utente. Identity . Nome

Per impostazione predefinita, l' *`Server`* API dell'app viene popolata `User.Identity.Name` con il valore del `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` tipo di attestazione (ad esempio, `2d64b3da-d9d5-42c6-9352-53d8df33d770@contoso.onmicrosoft.com` ).

Per configurare l'app per la ricezione del valore dal `name` tipo di attestazione, configurare il <xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType?displayProperty=nameWithType> di <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> in `Startup.ConfigureServices` :

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;

...

services.Configure<JwtBearerOptions>(
    AzureADDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a>Impostazioni delle app

::: moniker range=">= aspnetcore-5.0"

Il `appsettings.json` file contiene le opzioni per configurare il gestore di connessione JWT usato per convalidare i token di accesso:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{SERVER API APP CLIENT ID}",
    "CallbackPath": "/signin-oidc"
  }
}
```

Esempio:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "contoso.onmicrosoft.com",
    "TenantId": "e86c78e2-8bb4-4c41-aefd-918e0565a45e",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
    "CallbackPath": "/signin-oidc"
  }
}
```

[!INCLUDE[](~/includes/blazor-security/azure-scope-5x.md)]

::: moniker-end

::: moniker range="< aspnetcore-5.0"

Il `appsettings.json` file contiene le opzioni per configurare il gestore di connessione JWT usato per convalidare i token di accesso:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "{DOMAIN}",
    "TenantId": "{TENANT ID}",
    "ClientId": "{SERVER API APP CLIENT ID}",
  }
}
```

Esempio:

```json
{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "contoso.onmicrosoft.com",
    "TenantId": "e86c78e2-8bb4-4c41-aefd-918e0565a45e",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
  }
}
```

::: moniker-end

### <a name="weatherforecast-controller"></a>Controller WeatherForecast

Il controller WeatherForecast ( *Controllers/WeatherForecastController. cs* ) espone un'API protetta con l' [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attributo applicato al controller. È **importante** comprendere che:

* L' [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attributo in questo controller API è l'unico elemento che protegge questa API da accessi non autorizzati.
* L' [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attributo usato nell' Blazor WebAssembly app funge solo da hint per l'app che l'utente deve essere autorizzato affinché l'app funzioni correttamente.

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a>*`Client`* configurazione dell'app

*Questa sezione è relativa all'app della soluzione **`Client`** .*

### <a name="authentication-package"></a>Pacchetto di autenticazione

Quando viene creata un'app per usare gli account aziendali o dell'Istituto di istruzione ( `SingleOrg` ), l'app riceve automaticamente un riferimento al pacchetto per [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) ( [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal) ). Il pacchetto fornisce un set di primitive che consentono all'app di autenticare gli utenti e ottenere i token per chiamare le API protette.

Se si aggiunge l'autenticazione a un'app, aggiungere manualmente il pacchetto al file di progetto dell'app:

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
  Version="{VERSION}" />
```

Per il segnaposto `{VERSION}` , la versione stabile più recente del pacchetto che corrisponde alla versione del Framework condiviso dell'app è disponibile nella cronologia delle **versioni** del pacchetto in [NuGet.org](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal).

Il [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal) pacchetto aggiunge il [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication) pacchetto all'app in modo transitivo.

### <a name="authentication-service-support"></a>Supporto del servizio di autenticazione

<xref:System.Net.Http.HttpClient>Viene aggiunto il supporto per le istanze di che includono token di accesso quando si effettuano richieste al progetto server.

`Program.cs`:

```csharp
builder.Services.AddHttpClient("{APP ASSEMBLY}.ServerAPI", client => 
        client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddScoped(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("{APP ASSEMBLY}.ServerAPI"));
```

Il segnaposto `{APP ASSEMBLY}` è il nome dell'assembly dell'app (ad esempio, `BlazorSample.ServerAPI` ).

Il supporto per l'autenticazione degli utenti viene registrato nel contenitore del servizio con il <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> metodo di estensione fornito dal [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal) pacchetto. Questo metodo configura i servizi necessari per l'interazione dell'app con il Identity provider (IP).

`Program.cs`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAd", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

Il <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> metodo accetta un callback per configurare i parametri necessari per autenticare un'app. Quando si registra l'app, è possibile ottenere i valori necessari per la configurazione dell'app dalla configurazione di AAD del portale di Azure.

La configurazione viene fornita dal `wwwroot/appsettings.json` file:

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/{TENANT ID}",
    "ClientId": "{CLIENT APP CLIENT ID}",
    "ValidateAuthority": true
  }
}
```

Esempio:

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/e86c78e2-...-918e0565a45e",
    "ClientId": "4369008b-21fa-427c-abaa-9b53bf58e538",
    "ValidateAuthority": true
  }
}
```

### <a name="access-token-scopes"></a>Ambiti del token di accesso

Gli ambiti dei token di accesso predefiniti rappresentano l'elenco degli ambiti dei token di accesso:

* Incluso per impostazione predefinita nella richiesta di accesso.
* Utilizzato per eseguire il provisioning di un token di accesso immediatamente dopo l'autenticazione.

Tutti gli ambiti devono appartenere alla stessa app per ogni regola di Azure Active Directory. Per altre app per le API è possibile aggiungere altri ambiti, se necessario:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

Specificare gli ambiti aggiuntivi con `AdditionalScopesToConsent` :

```csharp
options.ProviderOptions.AdditionalScopesToConsent.Add("{ADDITIONAL SCOPE URI}");
```

::: moniker range="< aspnetcore-5.0"

[!INCLUDE[](~/includes/blazor-security/azure-scope-3x.md)]

::: moniker-end

Per ulteriori informazioni, vedere le sezioni seguenti dell'articolo *scenari aggiuntivi* :

* [Richiedere token di accesso aggiuntivi](xref:blazor/security/webassembly/additional-scenarios#request-additional-access-tokens)
* [Connetti token alle richieste in uscita](xref:blazor/security/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

::: moniker range=">= aspnetcore-5.0"

### <a name="login-mode"></a>Modalità di accesso

[!INCLUDE[](~/includes/blazor-security/msal-login-mode.md)]

::: moniker-end

### <a name="imports-file"></a>Importa file

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a>Pagina di indice

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a>Componente app

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>Componente RedirectToLogin

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>Componente LoginDisplay

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a>Componente di autenticazione

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>Componente FetchData

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>Eseguire l'app

Eseguire l'app dal progetto server. Quando si usa Visual Studio, eseguire una delle operazioni seguenti:

* Impostare l'elenco a discesa **progetti di avvio** nella barra degli strumenti sull'app per le *API server* e selezionare il pulsante **Esegui** .
* Selezionare il progetto server in **Esplora soluzioni** , quindi fare clic sul pulsante **Esegui** nella barra degli strumenti o avviare l'app dal menu **debug** .

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:blazor/security/webassembly/additional-scenarios>
* [Richieste API Web non autenticate o non autorizzate in un'app con un client predefinito sicuro](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:blazor/security/webassembly/aad-groups-roles>
* <xref:security/authentication/azure-active-directory/index>
* [Documentazione di Microsoft Identity Platform](/azure/active-directory/develop/)
