---
title: Azure Application Insights para ASP.NET Core | Microsoft Docs
description: Monitorizar aplicações web de disponibilidade, desempenho e utilização.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/03/2018
ms.author: mbullwin
ms.openlocfilehash: 459fbbe734577af474fae8de3acf9be732780d6c
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736539"
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights para Núcleo do ASP.NET

As informações de aplicação do Azure fornece monitorização aprofundada da aplicação web para o nível de código. Que possa monitorizar facilmente a sua aplicação web de disponibilidade, desempenho e utilização. Também pode identificar e diagnosticar erros rapidamente na sua aplicação sem ter de esperar que um utilizador os comunique.

Este artigo explica como criar uma amostra ASP.NET Core [Razor páginas](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) aplicação no Visual Studio e, como iniciar a monitorização com o Azure Application Insights.

## <a name="prerequisites"></a>Pré-requisitos

- NET Core 2.0.0 SDK ou posterior.
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) versão 15.7.3 ou posterior com a carga de trabalho de desenvolvimento de ASP.NET e web. 

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Criar um projeto em ASP.NET Core no Visual Studio

1. Clique com botão direito e iniciar **Visual Studio 2017** como administrador.
2. Selecione **ficheiro** > **novo** > **projeto** (Ctrl-Shift-N).

   ![Captura de ecrã do Menu do Visual Studio ficheiro novo projeto](./media/app-insights-asp-net-core/001-new-project.png)

3. Expanda **Visual c#** > selecione **.NET Core** > **aplicação Web de ASP.NET Core**. Introduza um **nome** > **nome da solução** > verificação **criar novo repositório de Git**.

   ![Captura de ecrã do Assistente de projeto do Visual Studio ficheiro novo](./media/app-insights-asp-net-core/002-asp-net-core-web-application.png)

4. Selecione **.Net Core** > **núcleo de ASP.NET 2.0** **aplicação Web** > **OK**.

    ![Captura de ecrã do Menu de seleção de projeto novo do Visual Studio ficheiro](./media/app-insights-asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Pesquisa do Application Insights

Por predefinição no Visual Studio 2015 Update 2 ou posterior com um núcleo de ASP.NET 2 + com base em projeto pode de versão pode tirar partido de [pesquisa do Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-visual-studio) mesmo antes explicitamente adiciona o Application Insights ao seu projeto.

Para testar a esta funcionalidade:

1. Executar a aplicação ao clicar em Express de IIS ![Ícone de captura de ecrã do Visual Studio IIS Express](./media/app-insights-asp-net-core/004-iis-express.png)

2. Selecione **vista** > **outras janelas** > **pesquisa do Application Insights**.

   ![Captura de ecrã das ferramentas de diagnóstico do Visual Studio](./media/app-insights-asp-net-core/005-view-other-windows-search.png)

3. A telemetria de sessão de depuração está atualmente disponível para análise local apenas. Para ativar totalmente o Application Insights, selecione **telemetria preparação** no canto superior botão direito do rato ou, siga os passos na secção seguinte.

   ![Pesquisa de captura de ecrã do Visual Studio Application Insights](./media/app-insights-asp-net-core/006-search.png)

> [!NOTE]
> Para saber mais sobre como o lights do Visual Studio segurança funcionalidades [pesquisa do Application Insights](app-insights-visual-studio.md) e [CodeLens](app-insights-visual-studio-codelens.md) localmente antes de adicionar Application Insights ao seu projeto ASP.NET Core veja os explicação no [final deste artigo.](#Application-Insights-search-continued)

## <a name="add-application-insights-telemetry"></a>Adicionar Telemetria do Application Insights

1. Selecione **projeto** > **adicionar telemetria do Application Insights...** . (Ou pode faça duplo clique **serviços ligados** e selecione Adicionar o serviço ligado.)

    ![Captura de ecrã do Menu de seleção de projeto novo do Visual Studio ficheiro](./media/app-insights-asp-net-core/007-project-add-telemetry.png)

2. Selecione **começar**. (Dependendo da versão do Visual Studio o texto pode variar devido às ligeiramente. Algumas versões mais antigas em vez disso, tem um **iniciar livre** botão.)

    ![Captura de ecrã do Menu de seleção de projeto novo do Visual Studio ficheiro](./media/app-insights-asp-net-core/008-get-started.png)

3. Selecione um adequado **subscrição** > **recursos** > **registar**.

## <a name="changes-made-to-your-project"></a>Alterações Made ao seu projeto

O Application Insights está baixo overhead. Para rever as alterações efetuadas ao seu projeto ao adicionar telemetria do Application Insights:

Selecione **vista** > **equipa Explorer** (Ctrl +\, Ctrl + M) > **projeto** > **alterações**

- Totais de quatro alterações:

  ![Captura de ecrã dos ficheiros alterada ao adicionar o Application Insights](./media/app-insights-asp-net-core/009-changes.png)

- É criado um novo ficheiro:

   _ConnectedService.json_

```json
{
  "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
  "Version": "8.12.10405.1",
  "GettingStartedDocument": {
    "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
  }
}
```

- Três ficheiros são modificados: (comentários adicionais adicionados para realçar as alterações)

  _appsettings.json_

```json
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  },
// Changes to file post adding Application Insights Telemetry:
  "ApplicationInsights": {
    "InstrumentationKey": "10101010-1010-1010-1010-101010101010"
  }
}
//
```

  _ContosoDotNetCore.csproj_

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
 <!--Changes to file post adding Application Insights Telemetry:-->
    <ApplicationInsightsResourceId>/subscriptions/2546c5a9-fa20-4de1-9f4a-62818b14b8aa/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/DotNetCore</ApplicationInsightsResourceId>
    <ApplicationInsightsAnnotationResourceId>/subscriptions/2546c5a9-fa20-4de1-9f4a-62818b14b8aa/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/DotNetCore</ApplicationInsightsAnnotationResourceId>
<!---->
  </PropertyGroup>
  <ItemGroup>
 <!--Changes to file post adding Application Insights Telemetry:-->
    <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.1.1" />
<!---->
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.8" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.4" />
  </ItemGroup>
<!--Changes to file post adding Application Insights Telemetry:-->
  <ItemGroup>
    <WCFMetadata Include="Connected Services" />
  </ItemGroup>
<!---->
</Project>
```

   _Program. CS_

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

namespace DotNetCore
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
// Change to file post adding Application Insights Telemetry:
                .UseApplicationInsights()
//
                .UseStartup<Startup>()
                .Build();
    }
}
```

## <a name="synthetic-transactions-with-powershell"></a>Transações sintéticas com o PowerShell

Para automatizar pedidos contra a sua aplicação com transações sintéticas.

1. Executar a aplicação ao clicar em Express de IIS ![Ícone de captura de ecrã do Visual Studio IIS Express](./media/app-insights-asp-net-core/004-iis-express.png)

2. Copie o url da barra de endereço do browser. Está no formato http://localhost:{random número de porta}

   ![Captura de ecrã da barra de endereço de url do browser](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Execute o seguinte ciclo de PowerShell para criar 100 transações sintéticas contra a aplicação de teste. Modificar o número de porta após **localhost:** para corresponder ao url que copiou no passo anterior.

   ```PowerShell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Abrir o Application Insights Portal

Após a execução do PowerShell da secção anterior, inicie o Application Insights para ver as transações e confirme que estão a ser recolhidos dados. 

A partir do menu do Visual Studio, selecione **projeto** > **Application Insights** > **abra Portal de informações de aplicação**

   ![Captura de ecrã descrição geral do Application Insights](./media/app-insights-asp-net-core/010-portal.png)

> [!NOTE]
> A captura de ecrã de exemplo acima **em fluxo em direto**, **tempo de carregamento da vista de página**, e **pedidos falhados** atualmente não são recolhidos. A secção seguinte irá guiá-adicionar cada. Se já está a recolher **em fluxo em direto**, e **tempo de carregamento da vista de página**, em seguida, seguir os passos para **pedidos falhados**.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Recolher pedidos falhados, o fluxo em direto e o tempo de carregamento da vista de página

### <a name="failed-requests"></a>Pedidos com Falhas

Tecnicamente **pedidos falhados** estão a ser recolhidos, mas nenhum ocorreram ainda. Para acelerar o processo ao longo de uma exceção personalizado pode ser adicionada ao projeto existente para simular uma exceção do mundo real. Se a aplicação ainda está em execução no Visual Studio antes de continuar **pare a depuração** (Shift + F5)

1. No **Explorador de soluções** > expanda **páginas** > **About.cshtml** > Abra **About.cshtml.cs**.

   ![Captura de ecrã do Explorador de soluções do Visual Studio](./media/app-insights-asp-net-core/011-about.png)

2. Adicionar uma exceção em ``Message=`` e guardar a alteração para o ficheiro.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc.RazorPages;

    namespace DotNetCore.Pages
    {
        public class AboutModel : PageModel
        {
            public string Message { get; set; }

            public void OnGet()
            {
                Message = "Your application description page.";
                throw new Exception("Test Exception");
            }
        }
    }
    ```

### <a name="live-stream"></a>Live Stream

Para aceder a funcionalidade de fluxo em direto do Application Insights com o update ASP.NET Core para o **Microsoft.ApplicationInsights.AspNetCore 2.2.0** pacotes NuGet.

A partir do Visual Studio, selecione **projeto** > **gerir pacotes NuGet** > **Microsoft.ApplicationInsights.AspNetCore** > versão **2.2.0** > **atualização**.

  ![Captura de ecrã do Gestor de pacotes NuGet](./media/app-insights-asp-net-core/012-nuget-update.png)

Vários pedidos de confirmação irão ocorrer. Leia e aceite se concordar com as alterações.

### <a name="page-view-load-time"></a>Tempo de carregamento da vista de página

1. No Visual Studio, navegue para **Explorador de soluções** > **páginas** > dois ficheiros, terá de ser modificado: _layout. cshtml_, e  _ViewImports.cshtml_

2. No __ViewImports.cshtml_, adicionar:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. No **layout** adicione a linha abaixo antes do ``</head>`` etiquetas, mas também antes de quaisquer outros scripts.

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Falha de teste pedidos, o tempo de carregamento da vista de página, o fluxo em direto

Para testar e confirmar que tudo está a funcionar:

1. Executar a aplicação ao clicar em Express de IIS ![Ícone de captura de ecrã do Visual Studio IIS Express](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Navegue para o **sobre** página para acionar a exceção de teste. (Se estiver a executar no modo de depuração, terá de clicar em **continuar** no Visual Studio para a exceção a aparecer no Application Insights.)

3. Volte a executar o script de transação de PowerShell simulado da anteriormente (poderá ter de ajustar o número da porta no script.)

4. A descrição geral das informações de aplicações não esteja ainda aberta, no selecione de menu do Visual Studio **projeto** > **Application Insights** > **abrir a aplicação Insights Portal**. 

   > [!TIP]
   > Se não estiverem a ver o novo tráfego ainda, verifique o **intervalo de tempo** e clique em **atualizar**.

   ![Janela de captura de ecrã de descrição geral](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Selecione em fluxo em direto

   ![Captura de ecrã da transmissão de métricas em direto](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Se o PowerShell script ainda está em execução, deverá ver métricas em direto, se este tiver parado a executar o script de novamente em fluxo em direto de código aberto.)

## <a name="app-insights-sdk-comparison"></a>Comparação do App Insights SDK

O grupo de produtos do Application Insights tem trabalho rígido para alcançar paridade de funcionalidades entre o [completa do .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) e o .net Core SDK. O 2.2.0 o lançamento do [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore) para o Application Insights amplamente fechou o intervalo de funcionalidade.

Para saber mais sobre as diferenças e as responsabilidades entre [.NET e .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

   | Comparação SDK | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Métricas em direto**      | **+** |**-** | **+** |
   | **Canal de telemetria do servidor** | **+** |**-** | **+**|
   |**Amostragem adaptável**| **+** | **-** | **+**|
   | **Chamadas de dependência SQL**     | **+** |**-** | **+**|
   | **Contadores de desempenho*** | **+** | **-**| **-**|

_Contadores de desempenho_ neste contexto refere-se ao [contadores de desempenho do lado do servidor](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) como processadores, memória e utilização do disco.

## <a name="open-source-sdk"></a>SDK de open source
[Ler e contribuir para o código](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="application-insights-search-continued"></a>Pesquisa do Application Insights continuação

Para melhor compreenda como pesquisa do Application Insights funciona no Visual Studio para um projeto ASP.NET Core 2, mesmo quando uma instalação explícita dos pacotes de NuGet do Application Insights ainda não foi efetuada a ainda. Pode ser útil examinar a saída de depuração.

Se procurar o resultado para o word _conhecimentos aprofundados_ será realce resultados semelhantes aos seguintes:

```DebugOuput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

CoreCLR é duas assemblagens de carregamento: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

E o _anulada_ cada instância do Application Insights telemetria indica que esta aplicação não se encontra associada a um ikey pelo que os dados que são gerados enquanto a aplicação está em execução não estão a ser enviados para o Azure e é apenas está disponível para pesquisa local e de análise.

Parte de como isto é possível que é o pacote NuGet _Microsoft.AspNetCore.All_ aceita como uma dependência [ _Microsoft.ASPNetCoreApplicationInsights.HostingStartup_](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1)

![Captura de ecrã do NuGet dependência gráfico para Microsoft.AspNETCore.all](./media/app-insights-asp-net-core/013-dependency.png)

Fora do Visual Studio se foram a editar um projeto em ASP.NET Core VSCode ou alguns outro editor estas assemblagens não automaticamente carregar durante a depuração se explicitamente ainda não adicionou o Application Insights ao seu projeto.

No entanto, no Visual Studio este iluminação da cópia de segurança local funcionalidades do Application Insights de assemblagens externas é conseguida através da utilização do [IHostingStartup Interface](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) que adicione dinamicamente Application Insights durante a depuração.

Para saber mais sobre a otimização de uma aplicação de um [assemblagem externo em ASP.NET Core com IHostingStartup](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="how-to-disable-application-insights-in-visual-studio-net-core-projects"></a>Como desativar Application Insights nos projetos do Visual Studio .NET Core

Enquanto claro automático cópias de segurança da funcionalidade de pesquisa do Application Insights pode ser útil como alguns, ver a telemetria depuração gerada quando não foram à espera pode ser confuso.

Se desativar apenas a geração de telemetria é suficiente pode adicionar este bloco de código para o método de configuração do seu _Startup.cs_ ficheiro:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

O CoreCLR ainda carregará _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ e _Microsoft.ApplicationInsights.AspNetCore.dll_, mas que não fazem nada.

Se pretender desativar completamente o Application Insights no seu projeto de Visual Studio .NET Core, o método preferencial consiste em selecionar **ferramentas** > **opções**  >   **Projetos e soluções** > **projetos Web** > e selecione a caixa para desativar local Application Insights para projetos web de ASP.NET Core. Esta funcionalidade foi adicionada no Visual Studio 15,6.

![Ecrã de captura de ecrã do Visual Studio opções janela projetos Web](./media/app-insights-asp-net-core/014-disable.png)

Se estiver a executar uma versão anterior do Visual Studio e que pretende remover por completo todas as assemblagens carregadas através de IHostingStartup pode optar por adicionar:

`.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")`

para _Program.cs_:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

namespace DotNetCore
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
                .UseStartup<Startup>()
                .Build();
    }
}
```

Ou, em alternativa pode adicionar ``"ASPNETCORE_preventHostingStartup": "True"`` para _launchSettings.json_ variáveis de ambiente.

O problema com a utilização de um dos seguintes métodos é apenas não irá desativar o Application Insights que irá desativar nada no Visual Studio que foi tirar partido claro IHostingStartup segurança funcionalidade.

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Passos Seguintes
* [Explorar os utilizadores flui](app-insights-usage-flows.md) para compreender como os utilizadores navegam através da sua aplicação.
* [Configurar a recolha de instantâneo](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) para ver o estado do código de origem e as variáveis de momento, é emitida uma exceção.
* [Utilize a API](app-insights-api-custom-events-metrics.md) para enviar os seus próprios eventos e as métricas para uma vista mais detalhada do desempenho e a utilização da sua aplicação.
* [Testes de disponibilidade](app-insights-monitor-web-app-availability.md) verificar a sua aplicação constantemente a partir em todo o mundo.
