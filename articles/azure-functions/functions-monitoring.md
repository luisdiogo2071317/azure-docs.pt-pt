---
title: Monitorizar as Funções do Azure
description: Saiba como utilizar o Azure Application Insights com as funções do Azure para monitorizar a execução de função.
services: functions
author: ggailey777
manager: jeconnoc
keywords: funções do azure, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/15/2017
ms.author: glenga
ms.openlocfilehash: 9bb703f822a9b9cafe8318bfb7f7cc64c3854868
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/17/2018
ms.locfileid: "51853664"
---
# <a name="monitor-azure-functions"></a>Monitorizar as Funções do Azure

## <a name="overview"></a>Descrição geral 

[As funções do Azure](functions-overview.md) oferece integração incorporada com [Azure Application Insights](../application-insights/app-insights-overview.md) para funções de monitorização. Este artigo mostra como configurar as funções para enviar dados de telemetria para o Application Insights.

![Explorador de métricas do Application Insights](media/functions-monitoring/metrics-explorer.png)

As funções também têm [monitorização que incorporada não utiliza o Application Insights](#monitoring-without-application-insights). Recomendamos que o Application Insights porque oferece mais dados e melhores maneiras de analisar os dados.

## <a name="application-insights-pricing-and-limits"></a>Preços do Application Insights e limites

Pode experimentar a integração com aplicações de função do Application Insights gratuitamente. No entanto, existe um limite diário para a quantidade de dados pode ser processado gratuitamente e que pode usar esse limite durante o teste. O Azure disponibiliza o portal e notificações de e-mail quando o que está a atingir o seu limite diário.  Mas se perder os alertas e atingir o limite, de novos registos aparecer em consultas do Application Insights. Por isso, tenha em atenção o limite para evitar o tempo de resolução de problemas desnecessário. Para obter mais informações, consulte [gerir preços e volumes de dados no Application Insights](../application-insights/app-insights-pricing.md).

## <a name="enable-app-insights-integration"></a>Ativar a integração do App Insights

Para uma aplicação de função enviar dados para o Application Insights, ele precisa saber a chave de instrumentação de um recurso do Application Insights. A chave tem de ser fornecido numa definição de aplicação com o nome APPINSIGHTS_INSTRUMENTATIONKEY.

Pode configurar esta ligação no [portal do Azure](https://portal.azure.com):

* [Automaticamente para uma nova aplicação de função](#new-function-app)
* [Ligar-se manualmente um recurso do App Insights](#manually-connect-an-app-insights-resource)

### <a name="new-function-app"></a>Nova aplicação de funções

1. Vá para a aplicação de função **criar** página.

1. Definir o **Application Insights** mudar **no**.

2. Selecione um **localização do Application Insights**.

   Selecione a região mais próxima da região da sua aplicação de função, num [geografia do Azure](https://azure.microsoft.com/global-infrastructure/geographies/) onde pretende que os dados sejam armazenados.

   ![Ativar o Application Insights ao criar uma aplicação de funções](media/functions-monitoring/enable-ai-new-function-app.png)

3. Introduza as outras informações necessárias.

1. Selecione **Criar**.

A próxima etapa é [desativar o registo incorporado](#disable-built-in-logging).

### <a name="manually-connect-an-app-insights-resource"></a>Ligar-se manualmente um recurso do App Insights 

1. Crie o recurso do Application Insights. Definir o tipo de aplicação **gerais**.

   ![Criar um recurso do Application Insights, escreva geral](media/functions-monitoring/ai-general.png)

2. Copie a chave de instrumentação a partir da **Essentials** página de recurso do Application Insights. Paire o rato sobre o fim do valor da chave apresentado para obter um **clique para copiar** botão.

   ![Copie a chave de instrumentação do Application Insights](media/functions-monitoring/copy-ai-key.png)

1. Em que a aplicação de funções **as configurações do aplicativo** página, [adicionar uma definição da aplicação](functions-how-to-use-azure-function-app-settings.md#settings) clicando **Adicionar nova definição**. Nomeie a nova definição APPINSIGHTS_INSTRUMENTATIONKEY e cole a chave de instrumentação copiado.

   ![Adicionar chave de instrumentação para as definições da aplicação](media/functions-monitoring/add-ai-key.png)

1. Clique em **Guardar**.

## <a name="disable-built-in-logging"></a>Desativar o registo incorporado

Se ativar o Application Insights, recomendamos que desative o [logs interno que utiliza o armazenamento do Azure](#logging-to-storage). O logs interno é útil para testar com cargas de trabalho leves mas não se destina a utilização de produção de alta carga. Para a monitorização de produção, recomenda-se Application Insights. Se o registo incorporado é utilizado em produção, o registo de registo pode estar incompleto devido à limitação no armazenamento do Azure.

Para desativar o registo incorporado, elimine o `AzureWebJobsDashboard` definição de aplicação. Para obter informações sobre como eliminar as definições da aplicação no portal do Azure, consulte a **as configurações do aplicativo** secção [como gerir uma aplicação de funções](functions-how-to-use-azure-function-app-settings.md#settings). Antes de eliminar a definição de aplicação, certifique-se de que não existem funções existentes na mesma function app usá-la para acionadores do armazenamento do Azure ou ligações.

## <a name="view-telemetry-in-monitor-tab"></a>Ver a telemetria no separador Monitor

Depois que tiver definido a integração do Application Insights, conforme mostrado nas seções anteriores, pode ver os dados de telemetria no **Monitor** separador.

1. Na página da aplicação de função, selecione uma função que foi executada pelo menos uma vez depois do Application Insights foi configurado e, em seguida, selecione o **Monitor** separador.

   ![Selecione o separador Monitor](media/functions-monitoring/monitor-tab.png)

2. Selecione **atualizar** periodicamente até que é apresentada a lista de invocações de função.

   Pode demorar até 5 minutos para obter a lista sejam apresentadas, a forma como os dados de lotes de cliente de telemetria para a transmissão para o servidor. (Este atraso não se aplica para o [Stream de métricas em direto](../application-insights/app-insights-live-stream.md). Esse serviço liga-se para o anfitrião de funções quando carrega a página, para que os registos são transmitidas em fluxo diretamente para a página.)

   ![Lista de invocações](media/functions-monitoring/monitor-tab-ai-invocations.png)

2. Para ver os registos para uma invocação de função específica, selecione o **data** ligação de coluna para essa invocação.

   ![Ligação de detalhes de invocação](media/functions-monitoring/invocation-details-link-ai.png)

   A saída de registo para essa invocação será exibida numa nova página.

   ![Detalhes de invocação](media/functions-monitoring/invocation-details-ai.png)

Ligue ambas as páginas (lista de invocação e detalhes) para a consulta do Application Insights Analytics que obtém os dados:

![Abrir no Application Insights](media/functions-monitoring/run-in-ai.png)

![Lista de invocação do Application Insights Analytics](media/functions-monitoring/ai-analytics-invocation-list.png)

Estas consultas, pode ver que a lista de invocação é limitada para os últimos 30 dias, não mais do que 20 linhas (`where timestamp > ago(30d) | take 20`) e a lista de detalhes de invocação é para os últimos 30 dias sem limite.

Para obter mais informações, consulte [consultar dados de telemetria](#query-telemetry-data) mais adiante neste artigo.

## <a name="view-telemetry-in-app-insights"></a>Ver a telemetria no Application Insights

Para abrir o Application Insights a partir de uma aplicação de função no portal do Azure, selecione o **Application Insights** ligação na **configurado funcionalidades** secção da aplicação de função **descrição geral** página.

![Ligação de informações da aplicação na página Descrição geral](media/functions-monitoring/ai-link.png)


Para obter informações sobre como utilizar o Application Insights, consulte a [documentação do Application Insights](https://docs.microsoft.com/azure/application-insights/). Esta secção mostra alguns exemplos de como ver dados no Application Insights. Se já estiver familiarizado com o Application Insights, pode aceder diretamente ao [secções sobre como configurar e personalizar os dados de telemetria](#configure-categories-and-log-levels).

Na [Explorador de métricas](../application-insights/app-insights-metrics-explorer.md), pode criar gráficos e alertas com base em métricas, tais como número de chamadas de função, tempo de execução e taxa de êxito.

![Explorador de Métricas](media/functions-monitoring/metrics-explorer.png)

Sobre o [falhas](../application-insights/app-insights-asp-net-exceptions.md) separador, pode criar gráficos e alertas com base nas falhas de função e o servidor de exceções. O **nome da operação** é o nome de função. Falhas de dependências não são apresentadas a menos que implemente [telemetria personalizada](#custom-telemetry-in-c-functions) para dependências.

![Falhas](media/functions-monitoring/failures.png)

Sobre o [desempenho](../application-insights/app-insights-performance-counters.md) separador, pode analisar problemas de desempenho.

![Desempenho](media/functions-monitoring/performance.png)

O **servidores** separador mostra a utilização de recursos e o débito por servidor. Estes dados podem ser útil na depuração de cenários em que as funções são sobrecarregar os recursos subjacentes. Servidores são denominados **instâncias de função da Cloud**.

![Servidores](media/functions-monitoring/servers.png)

O [Stream de métricas em direto](../application-insights/app-insights-live-stream.md) separador mostra dados das métricas, já que é criado em tempo real.

![Transmissão em direto](media/functions-monitoring/live-stream.png)

## <a name="query-telemetry-data"></a>Consultar dados de telemetria

[O Application Insights Analytics](../application-insights/app-insights-analytics.md) permite-lhe aceder a todos os dados de telemetria na forma de tabelas numa base de dados. Analytics fornece uma linguagem de consulta para extrair, manipulação e visualizar os dados.

![Selecione a análise](media/functions-monitoring/select-analytics.png)

![Exemplo de análise](media/functions-monitoring/analytics-traces.png)

Eis um exemplo de consulta. Isso mostra a distribuição de pedidos por função de trabalho nos últimos 30 minutos.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

As tabelas que estão disponíveis são mostradas na **esquema** separador do painel esquerdo. Pode encontrar os dados gerados por invocações de função nas tabelas seguintes:

* **rastreios** -registos criados pelo tempo de execução e por código de função.
* **pedidos** -um para cada invocação de função.
* **exceções** - eventuais exceções geradas pelo tempo de execução.
* **customMetrics** -contagem de êxito e falha invocações, a taxa de êxito, a duração.
* **customEvents** -eventos controlados pelo tempo de execução, por exemplo: pedidos HTTP que acionam uma função.
* **performanceCounters** -informações sobre o desempenho dos servidores que executem as funções.

As outras tabelas destinam-se a testes de disponibilidade e a telemetria de browser do cliente. Pode implementar a telemetria personalizada para adicionar dados a eles.

Alguns dos dados de funções específicas dentro de cada tabela, está num `customDimensions` campo.  Por exemplo, a consulta seguinte obtém todos os rastreios que tenham o nível de registo `Error`.

```
traces 
| where customDimensions.LogLevel == "Error"
```

O runtime fornece `customDimensions.LogLevel` e `customDimensions.Category`. Pode fornecer campos adicionais nos registos de escrita no código da função. Ver [estruturado registo](#structured-logging) mais adiante neste artigo.

## <a name="configure-categories-and-log-levels"></a>Configurar as categorias e níveis de registo

Pode utilizar o Application Insights sem qualquer configuração personalizada, mas a configuração predefinida pode resultar em grandes volumes de dados. Se estiver a utilizar uma subscrição do Azure do Visual Studio, que pode usar o limite de dados do Application Insights. O restante deste artigo mostra como configurar e personalizar os dados que as suas funções enviam para o Application Insights.

### <a name="categories"></a>Categorias

O agente de log de funções do Azure inclui um *categoria* para cada registo. A categoria indica qual a parte do código de tempo de execução ou o código de função escreveu o registo. 

O runtime das funções cria registos que tenham um início de categoria com "Host". Por exemplo, a "função iniciada," "função" e os registos de "função concluída" tem categoria "Host.Executor". 

Se escrever registos no código da função, a categoria é "Function".

### <a name="log-levels"></a>Níveis de registo

O agente de log de funções do Azure também inclui um *nível de registo* com cada registo. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) é uma enumeração, e o código de número inteiro indica a importância relativa:

|LogLevel    |Código|
|------------|---|
|Rastreio       | 0 |
|Depurar       | 1 |
|Informações | 2 |
|Aviso     | 3 |
|Erro       | 4 |
|Crítica    | 5 |
|Nenhuma        | 6 |

Nível de registo `None` é explicado na próxima seção. 

### <a name="configure-logging-in-hostjson"></a>Configurar o registo no Host. JSON

O *Host. JSON* ficheiro configura o Registro em log uma aplicação de funções envia para o Application Insights. Para cada categoria, indica o nível de registo mínimo para enviar. Segue-se um exemplo:

#### <a name="functions-version-1"></a>As funções de versão 1 
```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Trace"
      }
    }
  }
}
```

#### <a name="functions-version-2"></a>As funções versão 2 
As funções v2 agora utiliza a [hierarquia de filtro de registo do .NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering). 
```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host.Results": "Error",
      "Function": "Error",
      "Host.Aggregator": "Trace"
    }
  }
}
```

Este exemplo configura as seguintes regras:

1. Para os registos com a categoria "Host.Results" ou "Function" de mensagens em fila, enviar apenas `Error` nível e superior para o Application Insights. Nos registos `Warning` nível e abaixo são ignorados.
2. Para os registos com categoria Host.Aggregator de mensagens em fila, envie todos os registos para o Application Insights. O `Trace` nível de registo é o mesmo que o que chamam de alguns agentes `Verbose`, mas usar `Trace` no *Host. JSON* ficheiro.
3. Para todos os outros registos, envie apenas `Information` nível e superior para o Application Insights.

O valor de categoria na *Host. JSON* controla o registo para todas as categorias que começam com o mesmo valor. Por exemplo, "alojar" na *Host. JSON* controla o registo para "Host.General", "Host.Executor", "Host.Results" e assim por diante.

Se *Host. JSON* inclui várias categorias que começam com a mesma cadeia de caracteres, mais aqueles são correspondidas pela primeira vez. Por exemplo, suponha que queira tudo, desde o tempo de execução, exceto "Host.Aggregator" para iniciar sessão no `Error` nível, mas deseja "Host.Aggregator" para iniciar sessão no `Information` nível:

#### <a name="functions-version-1"></a>As funções de versão 1 
```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

#### <a name="functions-version-2"></a>As funções versão 2 
```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host": "Error",
      "Function": "Error",
      "Host.Aggregator": "Information"
    }
  }
}
```

Para suprimir todos os registos para uma categoria, pode usar o nível de registo `None`. Não existem registos são escritos com essa categoria e não existe nenhum nível de registo acima dela.

As secções seguintes descrevem as principais categorias de registos que cria o tempo de execução. 

### <a name="category-hostresults"></a>Host.Results de categoria

Estes registos mostram como "pedidos" no Application Insights. Eles indicam o êxito ou falha de uma função.

![Gráfico de pedidos](media/functions-monitoring/requests-chart.png)

Todos estes registos são escritos em `Information` nível, por isso, se filtrar em `Warning` ou superior, não verá qualquer um desses dados.

### <a name="category-hostaggregator"></a>Host.Aggregator de categoria

Estes registos fornecem contagens e médias de invocações de função através de um [configuráveis](#configure-the-aggregator) período de tempo. O período predefinido é 30 segundos ou 1.000 resultados, o que ocorrer primeiro. 

Os registos estão disponíveis no **customMetrics** tabela no Application Insights. Os exemplos são o número de execuções, a taxa de êxito e a duração.

![consulta de customMetrics](media/functions-monitoring/custom-metrics-query.png)

Todos estes registos são escritos em `Information` nível, por isso, se filtrar em `Warning` ou superior, não verá qualquer um desses dados.

### <a name="other-categories"></a>Outras categorias

Todos os registos de categorias diferentes dos já encontram-se disponível na **rastreios** tabela no Application Insights.

![consulta de rastreios](media/functions-monitoring/analytics-traces.png)

Todos os registos com as categorias que começam com "Host" são escritos pelo tempo de execução de funções. Os registos de "Função concluído" e "Função started" têm a categoria "Host.Executor". Execuções com êxito, estes registos são `Information` nível; as exceções são registadas no `Error` nível. O tempo de execução também cria `Warning` nível registos, por exemplo: mensagens enviadas para a fila não processáveis da fila.

Registos de escrita por seu código de função têm a categoria "Function" e podem ser qualquer nível de registo.

## <a name="configure-the-aggregator"></a>Configurar o agregador

Conforme observado na seção anterior, o tempo de execução agrega dados sobre execuções de função durante um período de tempo. O período predefinido é 30 segundos ou 1.000 é executado, o que ocorrer primeiro. Pode configurar esta definição no *Host. JSON* ficheiro.  Segue-se um exemplo:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Configurar a amostragem

O Application Insights tem um [amostragem](../application-insights/app-insights-sampling.md) recurso que pode protegê-lo de produzir muitos dados de telemetria em momentos de pico de carga. Quando a taxa de telemetria de entrada exceder um limiar especificado, o Application Insights começa a aleatoriamente ignorar alguns dos itens recebidos. A configuração padrão para o número máximo de itens por segundo é 5. Pode configurar amostragem em *Host. JSON*.  Segue-se um exemplo:

```json
{
  "applicationInsights": {
    "sampling": {
      "isEnabled": true,
      "maxTelemetryItemsPerSecond" : 5
    }
  }
}
```

## <a name="write-logs-in-c-functions"></a>Registos de escrita no c# das funções

Pode escrever registos no código da função que aparecem como rastreios no Application Insights.

### <a name="ilogger"></a>ILogger

Utilize um [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) parâmetro nas suas funções em vez de um `TraceWriter` parâmetro. Logs criados utilizando `TraceWriter` vá para o Application Insights, mas `ILogger` permite-lhe fazer [estruturado registo](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Com uma `ILogger` objeto chamar `Log<level>` [os métodos de extensão ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) para criar registos. Por exemplo, o seguinte código escritas `Information` registos com a categoria "Function".

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Registo estruturado

A ordem dos marcadores de posição, não os respetivos nomes, determina quais parâmetros são usados na mensagem de registo. Por exemplo, suponha que tem o seguinte código:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Se manter a mesma cadeia de caracteres de mensagem e inverter a ordem dos parâmetros, o texto da mensagem resultante teria os valores nos lugares errados.

Marcadores de posição são tratadas dessa forma, para que pode fazer o registo estruturado. O Application Insights armazena os pares de nome-valor de parâmetro, além da cadeia de caracteres de mensagem. O resultado é que os argumentos de mensagem se tornar os campos que pode consultar no.

Por exemplo, se sua chamada de método logger parecer como no exemplo anterior, poderia consultar o campo `customDimensions.prop__rowKey`. O `prop__` prefixo é adicionado para se certificar de que não há nenhum colisões entre campos o runtime adiciona e campos de seu código de função adiciona.

Também pode consultar na cadeia de caracteres de mensagem original ao referenciar o campo `customDimensions.prop__{OriginalFormat}`.  

Aqui está uma representação JSON de exemplo de `customDimensions` dados:

```json
{
  customDimensions: {
    "prop__{OriginalFormat}":"C# Queue trigger function processed: {message}",
    "Category":"Function",
    "LogLevel":"Information",
    "prop__message":"c9519cbf-b1e6-4b9b-bf24-cb7d10b1bb89"
  }
}
```

### <a name="logging-custom-metrics"></a>Registo de métricas personalizadas  

No c# funções de script, pode utilizar o `LogMetric` método de extensão no `ILogger` para criar métricas personalizadas no Application Insights. Eis uma chamada de método de exemplo:

```csharp
logger.LogMetric("TestMetric", 1234); 
```

Esse código é uma alternativa ao chamar `TrackMetric` usando [a API do Application Insights para .NET](#custom-telemetry-in-c-functions).

## <a name="write-logs-in-javascript-functions"></a>Escrever registos nas funções de JavaScript

Nas funções do node. js, utilize `context.log` para escrever os registos. Registo estruturado não está ativado.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="logging-custom-metrics"></a>Registo de métricas personalizadas  

Nas funções do node. js, pode usar o `context.log.metric` método para criar métricas personalizadas no Application Insights. Eis uma chamada de método de exemplo:

```javascript
context.log.metric("TestMetric", 1234); 
```

Esse código é uma alternativa ao chamar `trackMetric` usando [o SDK de node. js do Application Insights](#custom-telemetry-in-javascript-functions).

## <a name="custom-telemetry-in-c-functions"></a>Telemetria personalizada em c# funções

Pode utilizar o [applicationinsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) pacote NuGet para enviar dados de telemetria personalizada para o Application Insights.

Eis um exemplo de código c# que utiliza a [telemetria personalizada API](../application-insights/app-insights-api-custom-events-metrics.md). O exemplo é para uma biblioteca de classes do .NET, mas o código do Application Insights é o mesmo para script c#.

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.Azure.WebJobs;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Linq;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        private static string key = TelemetryConfiguration.Active.InstrumentationKey = 
            System.Environment.GetEnvironmentVariable(
                "APPINSIGHTS_INSTRUMENTATIONKEY", EnvironmentVariableTarget.Process);

        private static TelemetryClient telemetryClient = 
            new TelemetryClient() { InstrumentationKey = key };

        [FunctionName("HttpTrigger2")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestMessage req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // parse query parameter
            string name = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Get request body
            dynamic data = await req.Content.ReadAsAsync<object>();

            // Set name to query string or body data
            name = name ?? data?.name;
         
            // Track an Event
            var evt = new EventTelemetry("Function called");
            UpdateTelemetryContext(evt.Context, context, name);
            telemetryClient.TrackEvent(evt);
            
            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            UpdateTelemetryContext(metric.Context, context, name);
            telemetryClient.TrackMetric(metric);
            
            // Track a Dependency
            var dependency = new DependencyTelemetry
                {
                    Name = "GET api/planets/1/",
                    Target = "swapi.co",
                    Data = "https://swapi.co/api/planets/1/",
                    Timestamp = start,
                    Duration = DateTime.UtcNow - start,
                    Success = true
                };
            UpdateTelemetryContext(dependency.Context, context, name);
            telemetryClient.TrackDependency(dependency);
        }
        
        // This correllates all telemetry with the current Function invocation
        private static void UpdateTelemetryContext(TelemetryContext context, ExecutionContext functionContext, string userName)
        {
            context.Operation.Id = functionContext.InvocationId.ToString();
            context.Operation.ParentId = functionContext.InvocationId.ToString();
            context.Operation.Name = functionContext.FunctionName;
            context.User.Id = userName;
        }
    }    
}
```

Não chame `TrackRequest` ou `StartOperation<RequestTelemetry>`, porque poderá ver pedidos de duplicação para uma invocação de função.  O runtime das funções controla automaticamente pedidos.

Não defina `telemetryClient.Context.Operation.Id`. Esta é uma configuração global e fará com que correllation incorreto quando muitas funções em execução simultânea. Em vez disso, crie uma nova instância de telemetria (`DependencyTelemetry`, `EventTelemetry`) e modificar seu `Context` propriedade. Em seguida, passamos a instância de telemetria para o correspondente `Track` método no `TelemetryClient` (`TrackDependency()`, `TrackEvent()`). Isto garante que a telemetria tenha os detalhes de correllation correto para a invocação de função atual.

## <a name="custom-telemetry-in-javascript-functions"></a>Telemetria personalizada nas funções de JavaScript

O [SDK de node. js do Application Insights](https://www.npmjs.com/package/applicationinsights) está atualmente na versão beta. Eis alguns exemplos de código que envia telemetria personalizada para o Application Insights:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup();
const client = appInsights.defaultClient;

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    client.trackEvent({name: "my custom event", tagOverrides:{"ai.operation.id": context.invocationId}, properties: {customProperty2: "custom property value"}});
    client.trackException({exception: new Error("handled exceptions can be logged with this method"), tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackMetric({name: "custom metric", value: 3, tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackTrace({message: "trace message", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true, tagOverrides:{"ai.operation.id": context.invocationId}});

    context.done();
};
```

O `tagOverrides` conjuntos de parâmetros `operation_Id` para ID de invocação. de função Esta definição permite-lhe correlacionar a toda a telemetria gerada automaticamente e personalizada para uma invocação de função especificada.

## <a name="known-issues"></a>Problemas conhecidos

### <a name="dependencies"></a>Dependências

Dependências de que a função tem a outros serviços não aparecem automaticamente, mas pode escrever código personalizado para mostrar as dependências. O código de exemplo da [secção de telemetria personalizada em C#](#custom-telemetry-in-c-functions) mostra como. O código de exemplo resulta numa *mapa da aplicação* no Application Insights, que é semelhante à seguinte:

![Mapeamento de aplicações](media/functions-monitoring/app-map.png)

### <a name="report-issues"></a>Comunicar problemas

Para comunicar um problema com a integração do Application Insights em funções ou para fazer uma sugestão ou a pedido, [criar um problema no GitHub](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="monitoring-without-application-insights"></a>Monitorização sem o Application Insights

Recomendamos que o Application Insights para funções de monitorização, porque oferece mais dados e melhores maneiras de analisar os dados. Mas se preferir o sistema de logs interno que utiliza o armazenamento do Azure, pode continuar a utilizá-la.

### <a name="logging-to-storage"></a>Registo para o armazenamento

Logs interno utiliza a conta de armazenamento especificada pela cadeia de ligação no `AzureWebJobsDashboard` definição de aplicação. Numa página da aplicação de função, selecione uma função e, em seguida, selecione o **Monitor** separador e optar por mantê-lo na vista clássica.

![Mudar para a vista clássica](media/functions-monitoring/switch-to-classic-view.png)

 Obter uma lista de execuções de função. Selecione uma execução de função para rever a duração, dados de entrada, erros e os ficheiros de registo associados.

Se ativou anteriormente o Application Insights, mas agora pretende ir para logs interno, desativar o Application Insights manualmente e, em seguida, selecione o **Monitor** separador. Para desativar a integração do Application Insights, elimine a definição de aplicação APPINSIGHTS_INSTRUMENTATIONKEY.

Mesmo que o **Monitor** separador mostra dados do Application Insights, pode ver dados de registo no sistema de ficheiros se ainda não [desativada logs interno](#disable-built-in-logging). No recurso de armazenamento, aceda a ficheiros, selecione o serviço de arquivo para a função e, em seguida, aceda a `LogFiles > Application > Functions > Function > your_function` para ver o ficheiro de registo.

### <a name="real-time-monitoring"></a>Monitorização em tempo real

Pode transmitir ficheiros de registo para uma sessão de linha de comandos numa estação de trabalho local, utilizando o [Interface de linha de comandos (CLI do Azure)](/cli/azure/install-azure-cli) ou [Azure PowerShell](/powershell/azure/overview).  

Para a CLI do Azure, utilize os seguintes comandos para iniciar sessão, escolha a sua subscrição e os ficheiros de registo do fluxo:

```
az login
az account list
az account set <subscriptionNameOrId>
az webapp log tail --resource-group <resource group name> --name <function app name>
```

Para o Azure PowerShell, utilize os seguintes comandos para adicionar a sua conta do Azure, escolha a sua subscrição e os ficheiros de registo do fluxo:

```
PS C:\> Add-AzureAccount
PS C:\> Get-AzureSubscription
PS C:\> Get-AzureSubscription -SubscriptionName "<subscription name>" | Select-AzureSubscription
PS C:\> Get-AzureWebSiteLog -Name <function app name> -Tail
```

Para obter mais informações, consulte [como transmitir registos](../app-service/web-sites-enable-diagnostic-log.md#streamlogs).

### <a name="viewing-log-files-locally"></a>Ficheiros de registo de visualização localmente

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, consulte os seguintes recursos:

* [Application Insights](/azure/application-insights/)
* [O registo do ASP.NET Core](/aspnet/core/fundamentals/logging/)
