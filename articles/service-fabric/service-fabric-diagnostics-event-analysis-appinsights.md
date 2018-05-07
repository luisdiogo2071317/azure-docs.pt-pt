---
title: Análise de eventos de recursos de infraestrutura de serviço do Azure com o Application Insights | Microsoft Docs
description: Saiba mais sobre como visualizar e analisar eventos utilizando o Application Insights para monitorização e diagnóstico de clusters de Service Fabric do Azure.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/04/2018
ms.author: dekapur; srrengar
ms.openlocfilehash: ef7517e91965b0d7444d158f041b1d2bddea6bd2
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Análise de eventos e visualização com o Application Insights

Azure Application Insights é uma plataforma extensível para monitorização de aplicações e de diagnóstico. Inclui uma análise poderosa e consultar a ferramenta, dashboard personalizável e visualizações e ainda mais opções, incluindo automática de alertas. É a plataforma recomendada para a monitorização e diagnóstico de serviços e aplicações de Service Fabric. Este artigo ajuda a resolver as seguintes perguntas comuns

* Como posso saber que se passa no interior de telemetria de aplicações e serviços e recolha
* Como posso resolver problemas a minha aplicação, especialmente dos serviços de comunicar entre si
* Como posso obter métricas sobre a forma como os meus serviços estão a funcionar, por exemplo, tempo de carregamento de página, pedidos de http

O objetivo deste artigo é mostrar como obter informações e resolver problemas de dentro do App Insights. Se gostaria de saber como configurar AI com o Service Fabric, consulte este [tutorial](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="monitoring-in-app-insights"></a>Monitorização no App Insights

O Application Insights tem um avançada fora a experiência de caixa ao utilizar o Service Fabric. Na página Descrição geral, o AI fornece informações chave sobre o serviço como o tempo de resposta e o número de pedidos processados. Ao clicar no botão "Procurar" na parte superior, pode ver uma lista de pedidos recentes na sua aplicação. Além disso, será capaz de ver os pedidos falhados aqui e diagnosticar os erros podem ter ocorrido.

![Descrição geral de AI](media/service-fabric-diagnostics-event-analysis-appinsights/ai-overview.png)

No painel à direita na imagem anterior, existem dois tipos principais de entradas na lista: e eventos de pedidos. Pedidos são chamadas efetuadas neste caso, API a aplicação através de pedidos HTTP e eventos são personalizados, que agem como telemetria, que pode adicionar em qualquer lugar no seu código. Pode explorar ainda mais instrumentação das suas aplicações na [API do Application Insights para as métricas e eventos personalizados](../application-insights/app-insights-api-custom-events-metrics.md). Clicar num pedido seria apresentar mais detalhes conforme mostrado na imagem seguinte, incluindo dados específicos de Service Fabric, que são recolhidas no pacote nuget AI Service Fabric. Esta informação é útil para resolução de problemas e saber o que é o estado da sua aplicação, não sendo todas estas informações pesquisável no Application Insights

![Detalhes do pedido de AI](media/service-fabric-diagnostics-event-analysis-appinsights/ai-request-details.png)

O Application Insights tem uma vista designada para consulta contra todos os dados que é apresentada no. Clique em "Explorador de métricas" no topo da página de descrição geral para navegar para o portal de AI. Aqui pode executar consultas contra mencionados anteriormente, pedidos, exceções, contadores de desempenho, métricas e eventos personalizados outras utilizando a linguagem de consulta Kusto. O exemplo seguinte mostra todos os pedidos na última 1 hora.

![Detalhes do pedido de AI](media/service-fabric-diagnostics-event-analysis-appinsights/ai-metrics-explorer.png)

Para explorar ainda mais as capacidades do portal do App Insights, aceda ao [documentação portal do Application Insights](../application-insights/app-insights-dashboards.md).

### <a name="configuring-ai-with-wad"></a>Configurar AI com WAD

>[!NOTE]
>Isto só é aplicável a clusters do Windows neste momento.

Existem duas formas de principais para enviar dados de WAD para AI do Azure, que é conseguido ao adicionar um receptor de AI à configuração do WAD, conforme detalhado em [neste artigo](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Adicionar uma chave de instrumentação AI quando criar um cluster no portal do Azure

![Adicionar um AIKey](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Quando criar um cluster, se Diagnostics está ativada "", irá mostrar um campo opcional para introduzir uma chave de instrumentação do Application Insights. Se cole a chave de AI aqui, o sink de AI é automaticamente configurado por si no modelo do Resource Manager que é utilizado para implementar o cluster.

#### <a name="add-the-ai-sink-to-the-resource-manager-template"></a>Adicionar o Sink de AI para o modelo do Resource Manager

No "WadCfg" do modelo do Resource Manager, adicione "Sink", incluindo as seguintes duas alterações:

1. Adicionar a configuração de receptores imediatamente após a declaração do `DiagnosticMonitorConfiguration` concluída:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Incluem o Sink no `DiagnosticMonitorConfiguration` adicionando a seguinte linha no `DiagnosticMonitorConfiguration` do `WadCfg` (antes do `EtwProviders` declarados):

    ```json
    "sinks": "applicationInsights"
    ```

Em ambos os fragmentos de código anteriores, o nome "applicationInsights" foi utilizado para descrever o sink. Não é um requisito e desde que o nome do sink está incluído no "sinks", pode definir o nome para qualquer cadeia.

Atualmente, os registos do cluster apresentados como **rastreios** no Visualizador de registo do AI. Uma vez que a maioria os rastreios proveniente da plataforma de nível "Informativa", pode também considerar a alteração da configuração do sink para apenas enviar os registos do tipo "Críticas" ou "Error." Isto pode ser feito através da adição de "Canais de" para o sink, como é demonstrado na [neste artigo](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Se utilizar uma chave de AI incorreto no portal ou no seu modelo do Resource Manager, terá de alterar a chave manualmente e atualizar o cluster / reimplementá-lo.

### <a name="configuring-ai-with-eventflow"></a>Configurar AI com EventFlow

Se estiver a utilizar EventFlow para agregar eventos, certifique-se importar o `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`pacote NuGet. É necessário o seguinte código no *produz* secção o *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        "instrumentationKey": "***ADD INSTRUMENTATION KEY HERE***"
    }
]
```

Certifique-se efetuar as alterações necessárias nos filtros, bem como incluir quaisquer outras entradas (juntamente com os seus respetivos pacotes de NuGet).

## <a name="aisdk"></a>AI. SDK

É recomendado utilizar EventFlow e WAD como soluções de agregação, porque permitem uma abordagem mais modulares para diagnóstico e de monitorização, ou seja, se pretender alterar as saídas de EventFlow, é necessária nenhuma alteração ao seu instrumento real, apenas um modificação Simple ao ficheiro de configuração. Se, no entanto, optar por investir em utilizar o Application Insights e não é provável que alterar para uma plataforma diferentes, deve ter o aspeto na utilizando novo SDK do AI para agregar eventos e enviando-as para AI. Isto significa que já não será necessário configurar EventFlow para enviar os dados para AI, mas em vez disso, instalará o pacote NuGet do Service Fabric do ApplicationInsight. Podem ser encontrados detalhes no pacote [aqui](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Suportem de Application Insights para micro-serviços e contentores](https://azure.microsoft.com/blog/app-insights-microservices/) mostra algumas das novas funcionalidades que estão a ser trabalhadas (beta ainda atualmente no), que permitem-lhe ter mais rica opções de monitorização de out of box com AI. Estes incluem o controlo de dependência (utilizado na criação de um AppMap de todos os seus serviços e aplicações em cluster e a comunicação entre elas) e melhor correlação de rastreios provenientes da sua serviços (ajuda-o no melhor pinpointing um problema no fluxo de trabalho do uma aplicação ou serviço).

Se estiver a desenvolver no .NET e irá, provavelmente, utilizar algumas das Service Fabric programação modelos e estão pretendo utilizar o AI como a plataforma para visualizar e analisar dados de eventos e de registo, em seguida, recomendamos que leia através da rota de AI SDK como as de monitorização e diagnos fluxo de trabalho tics. Leitura [isto](../application-insights/app-insights-asp-net-more.md) e [isto](../application-insights/app-insights-asp-net-trace-logs.md) para começar a utilizar com a utilização de AI para recolher e apresentar os seus registos.

## <a name="navigating-the-ai-resource-in-azure-portal"></a>Navegar o recurso de AI no portal do Azure

Assim que tiver configurado o AI como uma saída para os eventos e registos, devem começar informações apareçam no seu recurso AI dentro de alguns minutos. Navegue para o recurso de AI, o que leva-o para o dashboard de recursos de AI. Clique em **pesquisa** na barra de tarefas AI para ver os rastreios mais recentes que se tiver recebido e conseguir filtrar através de-los.

*Explorador de métricas* é uma ferramenta útil para a criação de dashboards personalizados com base nas métricas que as aplicações, serviços e cluster podem ser Reporting Services. Consulte [explorar métricas no Application Insights](../application-insights/app-insights-metrics-explorer.md) configurar alguns gráficos para si com base nos dados que está a recolher.

Ao clicar em **análise** leva-o ao portal do Application Insights Analytics, onde pode consultar eventos e rastreios com o maior âmbito e optionality. Saiba mais sobre no [análise no Application Insights](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Passos Seguintes

* [Configurar alertas no AI](../application-insights/app-insights-alerts.md) para ser notificado sobre as alterações no desempenho ou utilização
* [Smart deteção no Application Insights](../application-insights/app-insights-proactive-diagnostics.md) efetua uma análise proativa de telemetria a ser enviada para AI para o avisar de potenciais problemas de desempenho
