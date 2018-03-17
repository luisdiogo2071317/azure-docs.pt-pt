---
title: Azure Application Insights para ASP.NET Core | Microsoft Docs
description: "Monitorizar aplicações web de disponibilidade, desempenho e utilização."
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
ms.date: 02/21/2018
ms.author: mbullwin
ms.openlocfilehash: 77c48a22f974e027b4e8858d5e38018bbf5bb54f
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights para Núcleo do ASP.NET

As informações de aplicação do Azure fornece monitorização aprofundada da aplicação web para o nível de código. Que possa monitorizar facilmente a sua aplicação web de disponibilidade, desempenho e utilização. Também pode identificar e diagnosticar erros rapidamente na sua aplicação sem ter de esperar que um utilizador os comunique.

Este artigo explica como criar uma amostra ASP.NET Core [Razor páginas](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) aplicação no Visual Studio e, como iniciar a monitorização com o Azure Application Insights.

## <a name="prerequisites"></a>Pré-requisitos

- NET Core 2.0.0 SDK ou posterior.
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) versão 15.3 ou posterior com a carga de trabalho de desenvolvimento de ASP.NET e web.

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Criar um projeto em ASP.NET Core no Visual Studio

1. Clique com botão direito e iniciar **Visual Studio 2017** como administrador.
2. Selecione **ficheiro** > **novo** > **projeto** (Ctrl-Shift-N).

   ![Captura de ecrã do Menu do Visual Studio ficheiro novo projeto](./media/app-insights-asp-net-core/0001-file-new-project.png)

3. Expanda **Visual c#** > selecione **.NET Core** > **aplicação Web de ASP.NET Core**. Introduza um **nome** > **nome da solução** > verificação **criar novo repositório de Git**.

   ![Captura de ecrã do Assistente de projeto do Visual Studio ficheiro novo](./media/app-insights-asp-net-core/0002-new-project-web-application.png)

4. Selecione **.Net Core** > **núcleo de ASP.NET 2.0** **aplicação Web** > **OK**.

    ![Captura de ecrã do Menu de seleção de projeto novo do Visual Studio ficheiro](./media/app-insights-asp-net-core/0003-dot-net-core.png)

## <a name="add-application-insights-telemetry"></a>Adicionar Telemetria do Application Insights

1. Selecione **projeto** > **adicionar telemetria do Application Insights...** (Em alternativa, pode com o botão direito **serviços ligados** > adicionar o serviço ligado.)

    ![Captura de ecrã do Menu de seleção de projeto novo do Visual Studio ficheiro](./media/app-insights-asp-net-core/0004-add-application-insights-telemetry.png)

2. Selecione **Iniciar Gratuitamente**.

    ![Captura de ecrã do Menu de seleção de projeto novo do Visual Studio ficheiro](./media/app-insights-asp-net-core/0005-start-free.png)

3. Selecione um adequado **subscrição** > **recursos** > e se deve ou não permitir a coleção mais de 1 GB por mês > **registar**.

    ![Captura de ecrã do Menu de seleção de projeto novo do Visual Studio ficheiro](./media/app-insights-asp-net-core/0006-register.png)

## <a name="changes-made-to-your-project"></a>Alterações Made ao seu projeto

O Application Insights está overhead muito baixo. Para rever as alterações efetuadas ao seu projeto ao adicionar telemetria do Application Insights:

Selecione **vista** > **equipa Explorer** (Ctrl +\, Ctrl + M) > **projeto** > **alterações**

- Totais de quatro alterações:

  ![Captura de ecrã dos ficheiros alterada ao adicionar o Application Insights](./media/app-insights-asp-net-core/0007-changes.png)

- É criado um novo ficheiro:

   **ConnectedService.json**

  ![Captura de ecrã dos ficheiros alterada ao adicionar o Application Insights](./media/app-insights-asp-net-core/0008-connectedservice-json.png)

- Três ficheiros são modificados:

  **appsettings.json**

   ![Captura de ecrã dos ficheiros alterada ao adicionar o Application Insights](./media/app-insights-asp-net-core/0009-appsettings-json.png)

  **ContosoDotNetCore.csproj**

   ![Captura de ecrã dos ficheiros alterada ao adicionar o Application Insights](./media/app-insights-asp-net-core/0010-contoso-netcore-csproj.png)

   **Program.cs**

   ![Captura de ecrã dos ficheiros alterada ao adicionar o Application Insights](./media/app-insights-asp-net-core/0011-program-cs.png)

## <a name="synthetic-transactions-with-powershell"></a>Transações sintéticas com o PowerShell

Iniciar a aplicação e, em seguida, clicar em torno em ligações manualmente podem ser utilizados para gerar tráfego de teste. No entanto, muitas vezes, é útil criar uma transação sintética simple no PowerShell.

1. Executar a aplicação ao clicar em Express de IIS ![Ícone de captura de ecrã do Visual Studio IIS Express](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Copie o url da barra de endereço do browser. Está a ser http://localhost o formato: {número da porta aleatória}

   ![Captura de ecrã da barra de endereço de url do browser](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Execute o seguinte ciclo de PowerShell para criar 100 transações sintéticas contra a aplicação de teste. Modificar o número de porta após **localhost:** para corresponder ao url que copiou no passo anterior.

   ```PS
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Abrir o Application Insights Portal

Após a execução do PowerShell da secção anterior, inicie o Application Insights para ver as transações e confirme que estão a ser recolhidos dados. 

A partir do menu do Visual Studio, selecione **projeto** > **Application Insights** > **abra Portal de informações de aplicação**

   ![Captura de ecrã descrição geral do Application Insights](./media/app-insights-asp-net-core/0014-portal-01.png)

> [!NOTE]
> A captura de ecrã de exemplo acima **em fluxo em direto**, **tempo de carregamento da vista de página**, e **pedidos falhados** atualmente não são recolhidos. A secção seguinte irá guiá-adicionar cada. Se já está a recolher **em fluxo em direto**, e **tempo de carregamento da vista de página**, em seguida, seguir os passos para **pedidos falhados**.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Recolher pedidos falhados, o fluxo em direto e o tempo de carregamento da vista de página

### <a name="failed-requests"></a>Pedidos Falhados

Tecnicamente **pedidos falhados** estão a ser recolhidos, mas nenhum ocorreram ainda. Para acelerar o processo ao longo de uma exceção personalizado pode ser adicionada ao projeto existente para simular uma exceção do mundo real. Se a aplicação ainda está em execução no Visual Studio antes de continuar **pare a depuração** (Shift + F5)

1. No **Explorador de soluções** > expanda **páginas** > **About.cshtml** > Abra **About.cshtml.cs**.

   ![Captura de ecrã do Explorador de soluções do Visual Studio](./media/app-insights-asp-net-core/0015-solution-explorer-about.png)

2. Adicionar uma exceção em ``Message=`` > guardar a alteração para o ficheiro.

   ```C#
   throw new Exception("Test Exception");
   ```

   ![Captura de ecrã do código de exceção](./media/app-insights-asp-net-core/000016-exception.png)

### <a name="live-stream"></a>Live Stream

Para aceder a funcionalidade de fluxo em direto do Application Insights com o update ASP.NET Core para o **Microsoft.ApplicationInsights.AspNetCore 2.2.0** pacotes NuGet.

A partir do Visual Studio, selecione **projeto** > **gerir pacotes NuGet** > **Microsoft.ApplicationInsights.AspNetCore** > versão **2.2.0** > **atualização**.

  ![Captura de ecrã do Gestor de pacotes NuGet](./media/app-insights-asp-net-core/0017-update-nuget.png)

Vários pedidos de confirmação irão ocorrer, leia e aceite se concordar com as alterações.

### <a name="page-view-load-time"></a>Tempo de carregamento da vista de página

1. No Visual Studio, navegue para **Explorador de soluções** > **páginas** > dois ficheiros, terá de ser modificado: **layout**, e **_ ViewImports.cshtml**

2. No **_ViewImports.cshtml**, adicionar:

   ```C#
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```
     ![Captura de ecrã da alteração do código para _ViewImports.cshtml](./media/app-insights-asp-net-core/00018-view-imports.png)

3. No **layout. cshtml** adicione a linha abaixo antes do ``</head>`` etiquetas, mas antes de quaisquer outros scripts.

    ```C#
    @Html.Raw(snippet.FullScript)
    ```
    ![Captura de ecrã da alteração do código para layout. cshtml](./media/app-insights-asp-net-core/0018-layout-cshtml.png)

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Falha de teste pedidos, o tempo de carregamento da vista de página, o fluxo em direto

Agora que concluiu os passos anteriores pode testar e confirmar que tudo está a funcionar.

1. Executar a aplicação ao clicar em Express de IIS ![Ícone de captura de ecrã do Visual Studio IIS Express](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Navegue para o **sobre** página para acionar a exceção de teste. (Se estiver a executar no modo de depuração, terá de clicar em **continuar** no Visual Studio antes da exceção será captada pelo Application Insights.)

3. Volte a executar o script de transação de PowerShell simulado da anteriormente (poderá ter de ajustar o número da porta no script.)

4. A descrição geral das informações de aplicações não esteja ainda aberta, no selecione de menu do Visual Studio **projeto** > **Application Insights** > **abrir a aplicação Insights Portal**. 

   > [!TIP]
   > Se não estiverem a ver o novo tráfego ainda, verifique o **intervalo de tempo** e clique em **atualizar**.

   ![Janela de captura de ecrã de descrição geral](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Selecione em fluxo em direto

   ![Captura de ecrã da transmissão de métricas em direto](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Se o script do PowerShell é parou de executar ainda, deverá ver métricas em direto, se executar o script novamente com o fluxo em direto abrir.)

## <a name="app-insights-sdk-comparison"></a>Comparação do App Insights SDK

O grupo de produtos do Application Insights tem trabalho rígido para alcançar tão próximos paridade de funcionalidades quanto possível entre o [completa do .NET Framework SDK](https://github.com/Microsoft/ApplicationInsights-dotnet) e o .net Core SDK. O 2.2.0 o lançamento do [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore) para o Application Insights amplamente fechou o intervalo de funcionalidade.

Para saber mais sobre as diferenças e as responsabilidades entre [.NET e .NET Core](https://docs.microsoft.com/en-us/dotnet/standard/choosing-core-framework-server).

   | Comparação SDK | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Métricas em direto**      | **+** |**-** | **+** |
   | **Canal de telemetria do servidor** | **+** |**-** | **+**|
   |**Amostragem adaptável**| **+** | **-** | **+**|
   | **Chamadas de dependência SQL**     | **+** |**-** | **+**|
   | **Contadores de desempenho*** | **+** | **-**| **-**|

_Contadores de desempenho_ neste contexto refere-se ao [contadores de desempenho do lado do servidor](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-performance-counters) como processadores, memória e utilização do disco.

## <a name="open-source-sdk"></a>SDK de open source
[Ler e contribuir para o código](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Passos Seguintes
* [Explorar os utilizadores flui](app-insights-usage-flows.md) para compreender como os utilizadores navegam através da sua aplicação.
* [Configurar a recolha de instantâneo](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) para ver o estado do código de origem e as variáveis de momento, é emitida uma exceção.
* [Utilize a API](app-insights-api-custom-events-metrics.md) para enviar os seus próprios eventos e as métricas para uma vista mais detalhada do desempenho e a utilização da sua aplicação.
* [Testes de disponibilidade](app-insights-monitor-web-app-availability.md) verificar a sua aplicação constantemente a partir em todo o mundo.