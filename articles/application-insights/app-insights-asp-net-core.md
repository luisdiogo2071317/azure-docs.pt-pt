---
title: Azure Application Insights para ASP.NET Core | Documentos da Microsoft
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
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: mbullwin
ms.openlocfilehash: 6635906b6aae418fa3754f1152fae3e0d8903ffc
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989771"
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights para Núcleo do ASP.NET

O Azure Application Insights fornece monitorização detalhada do seu aplicativo da web até o nível de código. Pode monitorizar facilmente a sua aplicação web de disponibilidade, desempenho e utilização. Também pode identificar e diagnosticar erros rapidamente na sua aplicação sem ter de esperar que um utilizador os comunique.

Este artigo explica como criar um exemplo de ASP.NET Core [páginas Razor](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) application no Visual Studio e como começar a monitorizar com o Azure Application Insights.

## <a name="prerequisites"></a>Pré-requisitos

- SDK NET Core 2.0.0 ou posterior.
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) versão 15.7.3 ou posterior com a carga de trabalho de desenvolvimento de ASP.NET e web. 

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Criar um projeto do ASP.NET Core no Visual Studio

1. Com o botão direito e inicie **Visual Studio 2017** como administrador.
2. Selecione **arquivo** > **nova** > **projeto** (Ctrl-Shift-N).

   ![Captura de ecrã do Menu de projeto do Visual Studio ficheiro novo](./media/app-insights-asp-net-core/001-new-project.png)

3. Expanda **em Visual C#** > selecione **.NET Core** > **aplicação Web ASP.NET Core**. Introduza um **Name** > **nome da solução** > verifique **criar novo repositório de Git**.

   ![Captura de ecrã do Assistente de projeto do Visual Studio ficheiro novo](./media/app-insights-asp-net-core/002-asp-net-core-web-application.png)

4. Selecione **.Net Core** > **ASP.NET Core 2.0** **aplicação Web** > **OK**.

    ![Captura de ecrã do Menu de seleção de projeto novos do Visual Studio ficheiro](./media/app-insights-asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Pesquisa do Application Insights

Por predefinição no Visual Studio 2015 Update 2 ou superior com um ASP.NET Core 2 + com base em projeto de versão pode aproveitar [pesquisa do Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio) até mesmo antes explicitamente de adicionar o Application Insights ao seu projeto.

Para testar esta funcionalidade:

1. Executar a sua aplicação ao clicar em IIS Express ![Ícone de captura de ecrã do Visual Studio IIS Express](./media/app-insights-asp-net-core/004-iis-express.png)

2. Selecione **View** > **Other Windows** > **pesquisa do Application Insights**.

   ![Captura de ecrã do diagnóstico do Visual Studio Tools](./media/app-insights-asp-net-core/005-view-other-windows-search.png)

3. A telemetria da sessão de depuração está atualmente disponível para apenas uma análise local. Para ativar totalmente o Application Insights, selecione **preparação de telemetria** no canto superior direito ou siga os passos na secção seguinte.

   ![Pesquisa de captura de ecrã do Visual Studio Application Insights](./media/app-insights-asp-net-core/006-search.png)

> [!NOTE]
> Para saber mais sobre como as luzes do Visual Studio desses recursos, como [pesquisa do Application Insights](app-insights-visual-studio.md) e [CodeLens](app-insights-visual-studio-codelens.md) localmente antes de adicionar o Application Insights ao seu projeto ASP.NET Core Verifique a explicação no [final deste artigo.](#Application-Insights-search-continued)

## <a name="add-application-insights-telemetry"></a>Adicionar Telemetria do Application Insights

1. Selecione **Project** > **adicionar telemetria do Application Insights...** . (Ou pode com o botão direito **serviços ligados** e selecione Adicionar serviço ligado.)

    ![Captura de ecrã do Menu de seleção de projeto novos do Visual Studio ficheiro](./media/app-insights-asp-net-core/007-project-add-telemetry.png)

2. Selecione **começar**. (Dependendo da versão do Visual Studio o texto pode variar um pouco. Algumas versões mais antigas em vez disso, tem um **começar gratuitamente** botão.)

    ![Captura de ecrã do Menu de seleção de projeto novos do Visual Studio ficheiro](./media/app-insights-asp-net-core/008-get-started.png)

3. Selecione um apropriado **subscrição** > **recursos** > **registar**.

## <a name="changes-made-to-your-project"></a>Altera Made ao seu projeto

O Application Insights é uma sobrecarga baixa. Para rever as modificações feitas em seu projeto ao adicionar telemetria do Application Insights:

Selecione **View** > **Team Explorer** (Ctrl +\, Ctrl + M) > **projeto** > **alterações**

- Total de quatro alterações:

  ![Captura de ecrã de ficheiros foi alterada ao adicionar o Application Insights](./media/app-insights-asp-net-core/009-changes.png)

- Um novo ficheiro é criado:

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

- Três arquivos são modificados: (comentários adicionais adicionados para realçar as alterações)

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

   _Program.CS_

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

Para automatizar a pedidos relativos a sua aplicação com transações sintéticas.

1. Executar a sua aplicação ao clicar em IIS Express ![Ícone de captura de ecrã do Visual Studio IIS Express](./media/app-insights-asp-net-core/004-iis-express.png)

2. Copie o url da barra de endereço do browser. Ele está no formato `http://localhost:{random port number}`

   ![Captura de ecrã da barra de endereço de url do browser](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Execute o seguinte loop de PowerShell para criar 100 transações sintéticas em relação a sua aplicação de teste. Modificar o número de porta depois **localhost:** para corresponder ao url que copiou no passo anterior.

   ```PowerShell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Abrir o Application Insights Portal

Depois de executar o PowerShell da seção anterior, inicie o Application Insights para exibir as transações e confirme que os dados estão a ser recolhidos. 

No menu do Visual Studio, selecione **Project** > **Application Insights** > **abrir Portal do Application Insights**

   ![Captura de ecrã de descrição geral do Application Insights](./media/app-insights-asp-net-core/010-portal.png)

> [!NOTE]
> A captura de ecrã de exemplo acima **Live Stream**, **tempo de carregamento da vista de página**, e **pedidos falhados** atualmente não são recolhidos. A secção seguinte irá guiá-se de adicionar cada uma. Se já está a recolher **Live Stream**, e **tempo de carregamento da vista de página**, em seguida, apenas siga os passos para **pedidos falhados**.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Coletar pedidos falhados, o Stream em direto e o tempo de carregamento da vista de página

### <a name="failed-requests"></a>Pedidos com Falhas

Tecnicamente **pedidos falhados** estão a ser recolhidos, mas nenhum ocorreram ainda. Para acelerar o processo ao longo de uma exceção personalizada pode ser adicionada ao projeto existente para simular uma exceção do mundo real. Se a aplicação ainda está em execução no Visual Studio antes de prosseguir **parar depuração** (Shift + F5)

1. Na **Explorador de soluções** > expanda **páginas** > **carregarei** > abrir **About.cshtml.cs**.

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

Para acessar a funcionalidade de Stream em direto do Application Insights com o update do ASP.NET Core para o **Microsoft.ApplicationInsights.AspNetCore 2.2.0** pacotes NuGet.

A partir do Visual Studio, selecione **Project** > **gerir pacotes NuGet** > **Microsoft.ApplicationInsights.AspNetCore** > versão **2.2.0** > **update**.

  ![Captura de ecrã do Gestor de pacotes do NuGet](./media/app-insights-asp-net-core/012-nuget-update.png)

Irão ocorrer em vários pedidos de confirmação. Leia e aceite se concordar com as alterações.

### <a name="page-view-load-time"></a>Tempo de carregamento da vista de página

1. No Visual Studio, navegue até **Explorador de soluções** > **páginas** > dois arquivos tem de ser modificados: _layout. cshtml_, e  _ViewImports.cshtml_

2. No __ViewImports.cshtml_, adicionar:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. Na **layout. cshtml** adicione a linha abaixo antes do ``</head>`` etiqueta, mas também antes de quaisquer outros scripts.

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Falha de teste, pedidos, o tempo de carregamento da vista de página, o Stream em direto

Para testar e confirmar que tudo o que está a funcionar:

1. Executar a sua aplicação ao clicar em IIS Express ![Ícone de captura de ecrã do Visual Studio IIS Express](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Navegue para o **sobre** página para acionar a exceção de teste. (Se estiver a executar no modo de depuração, terá de clicar **continuar** no Visual Studio para a exceção a aparecer no Application Insights.)

3. Volte a executar o script de transação de PowerShell simulado anteriores (poderá ter de ajustar o número de porta no script.)

4. Caso não esteja ainda aberta, no selecione de menu do Visual Studio a visão geral de informações de aplicativos **Project** > **Application Insights** > **abrir aplicação Portal das informações**. 

   > [!TIP]
   > Se ainda não vê o tráfego de novo, verifique os **intervalo de tempo** e clique em **atualizar**.

   ![Janela de captura de ecrã de descrição geral](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Selecione o Stream em direto

   ![Captura de ecrã do Stream de métricas em direto](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Se o seu PowerShell script ainda está em execução, deverá ver métricas em direto, se este tiver parado a executar o script novamente com o Live Stream o abrir.)

## <a name="app-insights-sdk-comparison"></a>Comparação SDK do App Insights

O grupo de produtos do Application Insights tem trabalhado arduamente para alcançar a paridade de funcionalidades entre o [completa do SDK do .NET Framework](https://github.com/Microsoft/ApplicationInsights-dotnet) e o .net Core SDK. O 2.2.0 da versão dos [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore) para o Application Insights amplamente fechou a diferença de funcionalidades.

Para saber mais sobre as diferenças e compensações entre [.NET e .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

   | Comparação SDK | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Métricas em direto**      | **+** |**-** | **+** |
   | **Canal de telemetria do servidor** | **+** |**-** | **+**|
   |**Amostragem adaptável**| **+** | **-** | **+**|
   | **Chamadas de dependência SQL**     | **+** |**-** | **+**|
   | **Contadores de desempenho*** | **+** | **-**| **-**|

_Contadores de desempenho_ neste contexto refere-se ao [contadores de desempenho do lado do servidor](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) como processador, memória e utilização do disco.

## <a name="open-source-sdk"></a>SDK de código-fonte aberto
[Ler e contribuir para o código](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="application-insights-search-continued"></a>Continuado de pesquisa do Application Insights

Para compreender melhor como pesquisa do Application Insights funciona no Visual Studio para um projeto do ASP.NET Core 2, mesmo quando uma instalação explícita os pacotes de NuGet do Application Insights ainda não ocorreu. Pode ser útil examinar a saída de depuração.

Se pesquisar a saída para a palavra _insight_ ele realçará resultados semelhantes ao seguinte:

```DebugOuput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

CoreCLR é a carregar dois assemblies: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

E o _não configurado_ em cada instância do Application Insights telemetria indica que este aplicativo não associado a uma ikey para que os dados que são gerados durante a execução de seu aplicativo não estão a ser enviados para o Azure e é apenas disponível para análise e pesquisa local.

Parte de como isso é possível é que o pacote NuGet _Microsoft.AspNetCore.All_ aceita como uma dependência [ _Microsoft.ASPNetCoreApplicationInsights.HostingStartup_](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1)

![Captura de ecrã do NuGet dependência gráfico para Microsoft.AspNETCore.all](./media/app-insights-asp-net-core/013-dependency.png)

Fora do Visual Studio se estava editando um projeto do ASP.NET Core no VSCode ou outro editor de alguns desses assemblies não automaticamente carregar durante a depuração se explicitamente ainda não adicionou o Application Insights ao seu projeto.

No entanto, no Visual Studio este iluminação cópia de segurança de funcionalidades do Application Insights locais de assemblies externos é conseguida através da utilização do [IHostingStartup Interface](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) dinamicamente que adiciona o Application Insights durante a depuração.

Para saber mais sobre como melhorar a uma aplicação a partir de um [assembly externo no ASP.NET Core com IHostingStartup](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="how-to-disable-application-insights-in-visual-studio-net-core-projects"></a>Como desativar o Application Insights em projetos do Visual Studio .NET Core

Embora automática agilizar da funcionalidade de pesquisa do Application Insights pode ser útil para alguns, ver de depuração de telemetria gerada quando não estavam esperando que ele pode ser confusa.

Se desativar apenas a geração de telemetria é suficiente pode adicionar este bloco de código para o método de configuração da sua _Startup.cs_ ficheiro:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

O CoreCLR ainda será carregado _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ e _Microsoft.ApplicationInsights.AspNetCore.dll_, mas eles não fará nada.

Se pretender desativar completamente o Application Insights no seu projeto do Visual Studio .NET Core, o método preferencial é selecionar **ferramentas** > **opções**  >   **Soluções e projetos** > **projetos Web** > e marque a caixa para desativar as informações da aplicação local para projetos da web ASP.NET Core. Esta funcionalidade foi adicionada no Visual Studio 15,6.

![Ecrã de captura de ecrã das opções janela Web projetos do Visual Studio](./media/app-insights-asp-net-core/014-disable.png)

Se estiver a executar uma versão anterior do Visual Studio e que pretende remover completamente todos os assemblies carregados por meio de IHostingStartup, pode adicionar:

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

Ou como alternativa poderia adicionar ``"ASPNETCORE_preventHostingStartup": "True"`` para _launchSettings.json_ variáveis de ambiente.

O problema com o uso de um desses métodos é que ele não simplesmente desabilitar desativa nada no Visual Studio que utilizasse a luz IHostingStartup a funcionalidade do Application Insights.

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Passos Seguintes
* [Explorar os utilizadores fluxos](app-insights-usage-flows.md) para compreender a forma como os utilizadores navegam pela aplicação.
* [Configurar a recolha de instantâneo](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) para ver o estado do código-fonte e variáveis no momento é emitida uma exceção.
* [Utilizar a API](app-insights-api-custom-events-metrics.md) para enviar os seus próprios eventos e métricas para uma visão mais detalhada do desempenho e a utilização da sua aplicação.
* [Testes de disponibilidade](app-insights-monitor-web-app-availability.md) verificar a sua aplicação a partir do constantemente em todo o mundo.
