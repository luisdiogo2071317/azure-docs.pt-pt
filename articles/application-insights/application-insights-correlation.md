---
title: Correlação de telemetria de informações de aplicação do Azure | Documentos da Microsoft
description: Correlação de telemetria do Application Insights
services: application-insights
documentationcenter: .net
author: lgayhardt
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/31/2018
ms.reviewer: sergkanz
ms.author: lagayhar
ms.openlocfilehash: 2f10db6eddcfc7d398072a3c0aa709798c72c72e
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721047"
---
# <a name="telemetry-correlation-in-application-insights"></a>Correlação de telemetria no Application Insights

No mundo dos microsserviços, cada operação lógica requer trabalho realizado em vários componentes do serviço. Cada um desses componentes pode ser monitorizada separadamente pelo [Application Insights](app-insights-overview.md). O componente da aplicação web se comunica com o componente de provedor de autenticação para validar as credenciais de utilizador e com o componente de API para obter dados para visualização. O componente de API no próprio giro pode consultar dados de outros serviços e usar componentes de fornecedor de cache e notificar o componente de faturação sobre esta chamada. Application Insights suporta telemetria distribuída correlação. Permite-lhe detetar que componente é responsável por falhas ou degradação do desempenho.

Este artigo explica o modelo de dados usado pelo Application Insights para correlacionar a telemetria enviada por vários componentes. Ele cobre as técnicas de propagação de contexto e protocolos. Também aborda de que a implementação dos conceitos de correlação em plataformas e idiomas diferentes.

## <a name="telemetry-correlation-data-model"></a>Modelo de dados de correlação de telemetria

O Application Insights define um [modelo de dados](application-insights-data-model.md) de correlação de telemetria distribuída. Para associar o funcionamento lógico telemetria, todos os itens de telemetria tem um campo de contexto chamado `operation_Id`. Este identificador é partilhado por todos os itens de telemetria no rastreio distribuído. Portanto, mesmo com a perda de telemetria a partir de uma única camada ainda pode associar telemetria comunicada pelos outros componentes.

Operação lógica distribuída normalmente consiste num conjunto de operações menores - pedidos processados por um dos componentes. Essas operações são definidas pelas [telemetria de pedido](application-insights-data-model-request-telemetry.md). Telemetria de cada pedido tem seu próprio `id` que o identifica exclusivamente globalmente. E toda a telemetria - rastreios, exceções, etc. associados a este pedido deve ser definido a `operation_parentId` para o valor da solicitação `id`.

Cada operação de saída, como a chamada http para outro componente representado pelo [telemetria de dependência](application-insights-data-model-dependency-telemetry.md). Telemetria de dependência também define sua própria `id` que seja globalmente exclusivo. Telemetria de pedido, iniciada por chamada de dependência, utiliza-o como `operation_parentId`.

Pode criar a vista do uso de operação lógica distribuída `operation_Id`, `operation_parentId`, e `request.id` com `dependency.id`. Esses campos também definem a ordem de causalidade das chamadas de telemetria.

Num ambiente de microsserviços, os rastreios a partir de componentes podem ir para os diferentes armazenamentos. Todos os componentes podem ter sua própria chave de instrumentação no Application Insights. Para obter telemetria para o funcionamento lógico, terá de consultar dados do armazenamento de cada. Quando o número de armazenamentos é enorme, tem de ter uma dica sobre onde procurar agora.

O Application Insights, modelo de dados define dois campos para resolver este problema: `request.source` e `dependency.target`. O primeiro campo identifica o componente a solicitação de dependência e o segundo identifica que componente devolveu a resposta da chamada de dependência.


## <a name="example"></a>Exemplo

Vejamos um exemplo de preços de STOCK uma aplicação que mostra o preço do mercado atual de uma ação com a API externa, chamada de API de AÇÕES. O aplicativo de preços de STOCK tem uma página `Stock page` aberto utilizando o cliente web browser `GET /Home/Stock`. A aplicação de consulta da API de STOCK através de uma chamada HTTP `GET /api/stock/value`.

Pode analisar a telemetria resultante, execução de uma consulta:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Na nota de exibição de resultado que todos os itens de telemetria partilham raiz `operation_Id`. Quando chamar o ajax feitas a partir da página – novo id exclusivo `qJSXU` é atribuído para a telemetria de dependência e o id da visualização de página é usada como `operation_ParentId`. Por sua vez o pedido de servidor utiliza a id do ajax como `operation_ParentId`, etc.

| ItemType   | nome                      | ID           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| visualização de página   | Página de ações                |              | STYz               | STYz         |
| dependência | GET /Home/estoque           | qJSXU        | STYz               | STYz         |
| pedido    | GET Home/estoque            | KqKwlrSt9PA= | qJSXU              | STYz         |
| dependência | OBTER /api/stock/value      | bBrf2L7mm2g= | KqKwlrSt9PA=       | STYz         |

Agora quando a chamada `GET /api/stock/value` feita para um serviço externo que pretende saber a identidade desse servidor. Para que possa definir `dependency.target` campo adequadamente. Quando o serviço externo não suporta a monitorização - `target` está definido como o nome de anfitrião do serviço, como `stock-prices-api.com`. No entanto se esse serviço identifica-se ao devolver um predefinidos cabeçalho HTTP - `target` contém a identidade de serviço que permite-Application Insights criar o rastreio distribuído através da consulta de telemetria a partir desse serviço. 

## <a name="correlation-headers"></a>Cabeçalhos de correlação

Estamos a trabalhar na proposta de RFC para o [correlação protocolo HTTP](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Essa proposta seja define dois cabeçalhos de:

- `Request-Id` executar o id exclusivo global da chamada
- `Correlation-Context` -executar a coleção de pares de valor de nome das propriedades de rastreio distribuído

O padrão também define dois esquemas de `Request-Id` geração - simples e hierárquica. Com o esquema simples, há um conhecido `Id` chave definida para o `Correlation-Context` coleção.

O Application Insights define a [extensão](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) para a correlação de protocolo HTTP. Ele usa `Request-Context` nomear pares de valor para propagar a coleção de propriedades utilizadas pelo chamador imediato ou receptor. SDK do Application Insights utiliza este cabeçalho para definir `dependency.target` e `request.source` campos.

### <a name="w3c-distributed-tracing"></a>Rastreamento de distribuídas de W3C

Estão a transitar [formato de rastreio distribuído do W3C](https://w3c.github.io/trace-context/). Ela define:
- `traceparent` -tem o id de operação globalmente exclusivo e o identificador exclusivo da chamada
- `tracestate` -carrega o contexto específico do sistema de rastreamento.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-classic-apps"></a>Ativar o suporte de rastreamento do W3C distribuído para aplicações ASP.NET clássico

Esta funcionalidade está disponível em pacotes applicationinsights e Microsoft.ApplicationInsights.DependencyCollector começando com 2.8.0-beta1 de versão.
É **off** por predefinição, para habilitá-lo, altere `ApplicationInsights.config`:

* sob `RequestTrackingTelemetryModule` adicionar `EnableW3CHeadersExtraction` elemento com o valor definido como `true`
* sob `DependencyTrackingTelemetryModule` adicionar `EnableW3CHeadersInjection` elemento com o valor definido como `true`

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>Ativar o suporte de rastreamento do W3C distribuído para aplicações ASP.NET Core

Esta funcionalidade está em Microsoft.ApplicationInsights.AspNetCore com 2.5.0-beta1 de versão e Microsoft.ApplicationInsights.DependencyCollector 2.8.0-beta1 de versão.
É **off** por predefinição, para ativá-la definida `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` para `true`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

## <a name="open-tracing-and-application-insights"></a>Rastreamento de aberto e Application Insights

O [especificação do modelo de dados de rastreio aberto](http://opentracing.io/) e modelos de dados do Application Insights do mapa da seguinte forma:

| Application Insights                  | Abra o rastreio                                      |
|------------------------------------   |-------------------------------------------------  |
| `Request`, `PageView`                 | `Span` com o `span.kind = server`                  |
| `Dependency`                          | `Span` com o `span.kind = client`                  |
| `Id` de `Request` e `Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference` do tipo `ChildOf` (o intervalo de principal)   |

Para obter mais informações sobre o modelo de dados do Application Insights, veja [modelo de dados](application-insights-data-model.md). 

Consulte o rastreio Open [especificação](https://github.com/opentracing/specification/blob/master/specification.md) e [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) para obter definições de rastreio aberto conceitos.


## <a name="telemetry-correlation-in-net"></a>Correlação de telemetria no .NET

Ao longo do tempo .NET definido várias formas de correlacionar os registos de telemetria e diagnóstico. Há `System.Diagnostics.CorrelationManager` que permite controlar [LogicalOperationStack e ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource` e o Windows ETW definir o método [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger` utiliza [âmbitos de registo](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF e Http conectar "current" propagação de contexto.

No entanto esses métodos não tiver ativado o suporte de rastreio distribuído automática. `DiagnosticsSource` é uma forma para suportar automática cruzada correlação de máquina. Bibliotecas .NET suportam a origem de diagnóstico e permitem automáticas cruzada propagação de máquina de contexto de correlação por meio de transporte, como http.

O [guia para atividades](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) na origem de diagnóstico explica as noções básicas de atividades de controlo. 

ASP.NET Core 2.0 oferece suporte a extração de cabeçalhos de Http e iniciar a nova atividade. 

`System.Net.HttpClient` versão inicial `4.1.0` suporta injeção automática da correlação cabeçalhos Http e a chamada http como uma atividade de controlo.

Existe um novo módulo de Http [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) para a clássica do ASP.NET. Este módulo implementa a correlação de telemetria com DiagnosticsSource. Ele começa a atividade com base nos cabeçalhos de pedido de entrada. Ele também correlaciona telemetria a partir de diferentes fases do processamento do pedido. Mesmo para os casos quando cada estágio do processamento do IIS é executado num threads de gerir diferentes.

Versão inicial do Application Insights SDK `2.4.0-beta1` utiliza DiagnosticsSource e a atividade para recolher telemetria e associá-la com a atividade atual. 

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Correlação de telemetria no SDK do Java
O [SDK de Java do Application Insights](app-insights-java-get-started.md) oferece suporte a correlação automática de início de telemetria com a versão `2.0.0`. Povoa automaticamente `operation_id` para toda a telemetria (rastreios, exceções, eventos personalizados, etc.) emitida dentro do escopo de um pedido. Ele também se encarrega de propagar os cabeçalhos de correlação (descritos acima) para chamadas de serviço para serviço via HTTP se o [agente Java SDK](app-insights-java-agent.md) está configurado. Nota: apenas as chamadas efetuadas através do cliente HTTP Apache são suportadas para a funcionalidade de correlação. Se estiver a utilizar o modelo de Rest de Spring ou Feign, ambos podem ser utilizadas com o cliente de HTTP do Apache, nos bastidores.

Atualmente, a propagação automática de contexto em tecnologias de mensagens (por exemplo, Kafka, RabbitMQ, do Azure Service Bus) não é suportada. É possível, no entanto codificar manualmente esses cenários, utilizando o `trackDependency` e `trackRequest` APIS, no qual uma telemetria de dependência representa uma mensagem a ser colocados em fila por um produtor e o pedido representa uma mensagem a ser processada por um consumidor. Neste caso, ambos `operation_id` e `operation_parentId` deve ser propagadas nas propriedades da mensagem.

<a name="java-role-name"></a>
### <a name="role-name"></a>Nome da Função
Às vezes, pode querer personalizar a forma como os nomes de componentes são apresentados no [mapa da aplicação](app-insights-app-map.md). Para fazer isso, pode definir manualmente o `cloud_roleName` efetuando um dos seguintes procedimentos:

Se estiver a utilizar o `WebRequestTrackingFilter`, o `WebAppNameContextInitializer` definirá automaticamente o nome da aplicação. Adicione o seguinte ao ficheiro de configuração (applicationinsights. xml):
```XML
<ContextInitializers>
  <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebAppNameContextInitializer" />
</ContextInitializers>
```
Por meio da classe de contexto de cloud:
```Java
telemetryClient.getContext().getCloud().setRole("My Component Name");
```

## <a name="next-steps"></a>Passos Seguintes

- [Escrever telemetria personalizada](app-insights-api-custom-events-metrics.md)
- Carregar todos os componentes do seu serviço micro no Application Insights. Confira [plataformas suportadas](app-insights-platforms.md).
- Ver [modelo de dados](application-insights-data-model.md) para o modelo de tipos e dados do Application Insights.
- Saiba como [estender e filtrar telemetria](app-insights-api-filtering-sampling.md).
- [Referência de configuração do Application Insights](app-insights-configuration-with-applicationinsights-config.md)

