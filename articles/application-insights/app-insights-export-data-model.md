---
title: Modelo de dados do Azure Application Insights | Documentos da Microsoft
description: Descreve propriedades exportado da exportação contínua em JSON e usados como filtros.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: cabad41c-0518-4669-887f-3087aef865ea
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/21/2016
ms.author: mbullwin
ms.openlocfilehash: 6686b241b93a0ae46b6de134f6f01526bb1a3ad2
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52723453"
---
# <a name="application-insights-export-data-model"></a>Modelo de dados de exportação do Application Insights
Esta tabela lista as propriedades de telemetria enviada pela [Application Insights](app-insights-overview.md) SDKs para o portal.
Verá essas propriedades na saída de dados do [exportação contínua](app-insights-export-telemetry.md).
Eles também aparecem em filtros de propriedade no [Explorador de métricas](app-insights-metrics-explorer.md) e [pesquisa de diagnóstico](app-insights-diagnostic-search.md).

Pontos a serem observados:

* `[0]` nestas tabelas denota um ponto no caminho em que precisa inserir um índice; mas nem sempre é 0.
* Horas de duração estão em décimas das microssegundo, então, 10000000 = = 1 segundo.
* Datas e horas são UTC e são fornecidas no formato ISO `yyyy-MM-DDThh:mm:ss.sssZ`


## <a name="example"></a>Exemplo
    // A server report about an HTTP request
    {
    "request": [
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": ""
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0",
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0,
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }

## <a name="context"></a>Contexto
Todos os tipos de telemetria são acompanhados por uma seção de contexto. Nem todos estes campos são transmitidos com todos os pontos de dados.

| Caminho | Tipo | Notas |
| --- | --- | --- |
| Context.Custom.dimensions [0] |objeto [] |Pares de cadeia de caracteres de chave-valor definido pelo parâmetro de propriedades personalizadas. Os valores de comprimento de chave máximo 100, comprimento máximo de 1024. Mais de 100 valores exclusivos, a propriedade pode ser pesquisada, mas não pode ser utilizada para segmentação. Chaves de 200 máx. por ikey. |
| Context.Custom.Metrics [0] |objeto [] |Pares chave-valor definido pelo parâmetro de medições personalizadas e por TrackMetrics. Comprimento máximo 100 da chave, valores podem ser um valor numéricos. |
| context.data.eventTime |cadeia |UTC |
| context.data.isSynthetic |boolean |Pedido parece ser proveniente de um teste web ou de bot. |
| context.data.samplingRate |número |Percentagem de telemetria gerada pelo SDK do que é enviado para o portal. Intervalo de 0,0 100,0. |
| Context.Device |objeto |Dispositivo de cliente |
| Context.Device.browser |cadeia |O IE, Chrome,... |
| context.device.browserVersion |cadeia |Chrome 48.0,... |
| context.device.deviceModel |cadeia | |
| context.device.deviceName |cadeia | |
| Context.Device.ID |cadeia | |
| Context.Device.locale |cadeia |en-GB, de-DE... |
| Context.Device.Network |cadeia | |
| context.device.oemName |cadeia | |
| context.device.osVersion |cadeia |SO do Host |
| context.device.roleInstance |cadeia |ID de anfitrião do servidor |
| context.device.roleName |cadeia | |
| Context.Device.Type |cadeia |PC, o navegador,... |
| Context.location |objeto |Derivado dos clientip. |
| Context.location.City |cadeia |Derivado dos clientip, se conhecidos |
| Context.location.ClientIP |cadeia |Um último octógono é anónimos como 0. |
| Context.location.continent |cadeia | |
| Context.location.Country |cadeia | |
| Context.location.Province |cadeia |Distrito |
| Context.Operation.ID |cadeia |Itens que tenham o mesmo id de operação são apresentados como itens relacionados no portal. Normalmente, o id do pedido. |
| Context.Operation.Name |cadeia |nome de URL ou a pedido |
| context.operation.parentId |cadeia |Permite que os itens relacionados aninhados. |
| Context.Session.ID |cadeia |ID de um grupo de operações da mesma origem. Um período de 30 minutos sem uma operação sinaliza o término de uma sessão. |
| context.session.isFirst |boolean | |
| context.user.accountAcquisitionDate |cadeia | |
| context.user.anonAcquisitionDate |cadeia | |
| context.user.anonId |cadeia | |
| context.user.authAcquisitionDate |cadeia |[Utilizador autenticado](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated |boolean | |
| internal.data.documentVersion |cadeia | |
| INTERNAL.data.ID |cadeia | Id exclusivo atribuído quando um item é ingerido para o Application Insights |

## <a name="events"></a>Eventos
Eventos personalizados gerados pelo [trackevent ()](app-insights-api-custom-events-metrics.md#trackevent).

| Caminho | Tipo | Notas |
| --- | --- | --- |
| Contagem de eventos [0] |inteiro |100 / ([amostragem](app-insights-sampling.md) taxa). Por exemplo, 4 =&gt; 25%. |
| nome do evento [0] |cadeia |Nome do evento.  Comprimento máximo de 250. |
| url de eventos [0] |cadeia | |
| o evento [0] urlData.base |cadeia | |
| o evento [0] urlData.host |cadeia | |

## <a name="exceptions"></a>Exceções
Relatórios [exceções](app-insights-asp-net-exceptions.md) no servidor e no browser.

| Caminho | Tipo | Notas |
| --- | --- | --- |
| assembly de basicException [0] |cadeia | |
| Contagem de basicException [0] |inteiro |100 / ([amostragem](app-insights-sampling.md) taxa). Por exemplo, 4 =&gt; 25%. |
| exceptionGroup basicException [0] |cadeia | |
| exceptionType basicException [0] |cadeia | |
| failedUserCodeMethod basicException [0] |cadeia | |
| failedUserCodeAssembly basicException [0] |cadeia | |
| handledAt basicException [0] |cadeia | |
| hasFullStack basicException [0] |boolean | |
| id de basicException [0] |cadeia | |
| método basicException [0] |cadeia | |
| mensagem de basicException [0] |cadeia |Mensagem de exceção. Comprimento máximo de 10 mil. |
| outerExceptionMessage basicException [0] |cadeia | |
| outerExceptionThrownAtAssembly basicException [0] |cadeia | |
| outerExceptionThrownAtMethod basicException [0] |cadeia | |
| outerExceptionType basicException [0] |cadeia | |
| outerId basicException [0] |cadeia | |
| assembly de parsedStack [0] basicException [0] |cadeia | |
| o nome de ficheiro do basicException [0] parsedStack [0] |cadeia | |
| nível de parsedStack [0] basicException [0] |inteiro | |
| linha de parsedStack [0] basicException [0] |inteiro | |
| método de parsedStack [0] basicException [0] |cadeia | |
| pilha de basicException [0] |cadeia |Comprimento máximo de 10 mil |
| typeName basicException [0] |cadeia | |

## <a name="trace-messages"></a>Mensagens de Rastreio
Enviado pela [TrackTrace](app-insights-api-custom-events-metrics.md#tracktrace)e pela [adaptadores de Registro em log](app-insights-asp-net-trace-logs.md).

| Caminho | Tipo | Notas |
| --- | --- | --- |
| a mensagem [0] loggerName |cadeia | |
| parâmetros de mensagem [0] |cadeia | |
| mensagem [0] não processada |cadeia |A mensagem de registo, comprimento máximo de 10 mil. |
| a mensagem [0] severityLevel |cadeia | |

## <a name="remote-dependency"></a>Dependência remota
Enviado pela TrackDependency. Utilizado para o desempenho do relatório e a utilização da [chamadas para dependências](app-insights-asp-net-dependencies.md) no servidor e chamadas AJAX no navegador.

| Caminho | Tipo | Notas |
| --- | --- | --- |
| async remoteDependency [0] |boolean | |
| Nome_de_base remoteDependency [0] |cadeia | |
| commandName remoteDependency [0] |cadeia |Por exemplo "home/index" |
| Contagem de remoteDependency [0] |inteiro |100 / ([amostragem](app-insights-sampling.md) taxa). Por exemplo, 4 =&gt; 25%. |
| dependencyTypeName remoteDependency [0] |cadeia |HTTP, SQL,... |
| durationMetric.value remoteDependency [0] |número |Tempo da chamada para a conclusão da resposta pela dependência |
| id de remoteDependency [0] |cadeia | |
| nome de remoteDependency [0] |cadeia |URL. Comprimento máximo de 250. |
| resultCode remoteDependency [0] |cadeia |de dependência HTTP |
| sucesso de remoteDependency [0] |boolean | |
| tipo de remoteDependency [0] |cadeia |Http, Sql,... |
| url de remoteDependency [0] |cadeia |Comprimento máximo de 2000 |
| urlData.base remoteDependency [0] |cadeia |Comprimento máximo de 2000 |
| urlData.hashTag remoteDependency [0] |cadeia | |
| urlData.host remoteDependency [0] |cadeia |Comprimento máximo de 200 |

## <a name="requests"></a>Pedidos
Enviado pela [TrackRequest](app-insights-api-custom-events-metrics.md#trackrequest). Os módulos padrão usado para tempo de resposta do servidor de relatórios, medido no servidor.

| Caminho | Tipo | Notas |
| --- | --- | --- |
| Contagem de pedidos [0] |inteiro |100 / ([amostragem](app-insights-sampling.md) taxa). Por exemplo: 4 =&gt; 25%. |
| o pedido [0] durationMetric.value |número |Tempo do pedido que chegam à resposta. 1e7 = = 1s |
| id do pedido [0] |cadeia |ID da operação |
| nome do pedido [0] |cadeia |GET/pós + url base.  Comprimento máximo de 250 |
| o pedido [0] responseCode |inteiro |Resposta HTTP enviada ao cliente |
| pedido com êxito [0] |boolean |Predefinição = = (responseCode &lt; 400) |
| url do pedido [0] |cadeia |Não incluindo o anfitrião |
| o pedido [0] urlData.base |cadeia | |
| o pedido [0] urlData.hashTag |cadeia | |
| o pedido [0] urlData.host |cadeia | |

## <a name="page-view-performance"></a>Desempenho de vista de página
Enviado pelo navegador. Mede o tempo a processar uma página, do utilizador a iniciar o pedido para apresentar completa (excluindo as chamadas AJAX assíncronas).

Valores de contexto mostram o SO de cliente e a versão do browser.

| Caminho | Tipo | Notas |
| --- | --- | --- |
| clientProcess.value clientPerformance [0] |inteiro |Hora de fim de receber o HTML para apresentar a página. |
| nome de clientPerformance [0] |cadeia | |
| networkConnection.value clientPerformance [0] |inteiro |Tempo decorrido para estabelecer uma ligação de rede. |
| receiveRequest.value clientPerformance [0] |inteiro |Hora de fim de enviar o pedido para receber o HTML na resposta. |
| sendRequest.value clientPerformance [0] |inteiro |Tempo de direcionado para enviar o pedido HTTP. |
| total.value clientPerformance [0] |inteiro |Hora de começar a enviar o pedido para apresentar a página. |
| url de clientPerformance [0] |cadeia |URL deste pedido |
| urlData.base clientPerformance [0] |cadeia | |
| urlData.hashTag clientPerformance [0] |cadeia | |
| urlData.host clientPerformance [0] |cadeia | |
| urlData.protocol clientPerformance [0] |cadeia | |

## <a name="page-views"></a>Visualizações da Página
Enviado pelo trackPageView() ou [stopTrackPage](app-insights-api-custom-events-metrics.md#page-views)

| Caminho | Tipo | Notas |
| --- | --- | --- |
| Contagem de visualizações [0] |inteiro |100 / ([amostragem](app-insights-sampling.md) taxa). Por exemplo, 4 =&gt; 25%. |
| Ver durationMetric.value [0] |inteiro |Valor se desejar, defina no trackPageView() ou ao startTrackPage() - stopTrackPage(). Não são as mesmas como clientPerformance valores. |
| nome da vista [0] |cadeia |Título da página.  Comprimento máximo de 250 |
| Ver url [0] |cadeia | |
| Ver urlData.base [0] |cadeia | |
| Ver urlData.hashTag [0] |cadeia | |
| Ver urlData.host [0] |cadeia | |

## <a name="availability"></a>Disponibilidade
Relatórios [testes web de disponibilidade](app-insights-monitor-web-app-availability.md).

| Caminho | Tipo | Notas |
| --- | --- | --- |
| availabilityMetric.name disponibilidade [0] |cadeia |disponibilidade |
| availabilityMetric.value disponibilidade [0] |número |1.0 ou 0,0 |
| Contagem de disponibilidade [0] |inteiro |100 / ([amostragem](app-insights-sampling.md) taxa). Por exemplo, 4 =&gt; 25%. |
| dataSizeMetric.name disponibilidade [0] |cadeia | |
| dataSizeMetric.value disponibilidade [0] |inteiro | |
| durationMetric.name disponibilidade [0] |cadeia | |
| durationMetric.value disponibilidade [0] |número |Duração do teste. 1e7 = = 1s |
| mensagem de disponibilidade [0] |cadeia |Diagnóstico de falha |
| resultado de disponibilidade [0] |cadeia |Aprovação/reprovação |
| runLocation disponibilidade [0] |cadeia |Origem da geo de pedidos de http |
| testName disponibilidade [0] |cadeia | |
| testRunId disponibilidade [0] |cadeia | |
| testTimestamp disponibilidade [0] |cadeia | |

## <a name="metrics"></a>Métricas
Gerado por trackmetric ().

O valor da métrica é encontrado em context.custom.metrics[0]

Por exemplo:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Sobre os valores de métrica
Valores de métrica, tanto em relatórios de métrica e noutro local, são apresentadas com uma estrutura de objeto padrão. Por exemplo:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Atualmente - embora isso podem ser alteradas no futuro, em todos os valores reportada a partir de módulos SDK padrão, `count==1` apenas e o `name` e `value` campos são úteis. O único caso em que deve ser diferentes seria se escrever suas próprias chamadas TrackMetric no qual é definir os outros parâmetros.

A finalidade dos outros campos é permitir que as métricas para ser agregados no SDK, para reduzir o tráfego para o portal. Por exemplo, poderia média várias leituras sucessivas antes de enviar cada relatório de métrica. Em seguida, seria calcular o min, max, desvio padrão e valor de agregação (sum ou average) e defina a contagem de para o número de leituras representado pelo relatório.

Nas tabelas acima, omitimos a contagem de campos raramente utilizada, min, max, stdDev e sampledValue.

Em vez de pré-agregar métricas, pode usar [amostragem](app-insights-sampling.md) se precisar de reduzir o volume de telemetria.

### <a name="durations"></a>Durações
Exceto onde indicado caso contrário, durações são representadas no décimas de um microssegundo, para que 10000000.0 significa 1 segundo.

## <a name="see-also"></a>Consulte também
* [Application Insights](app-insights-overview.md)
* [Exportação contínua](app-insights-export-telemetry.md)
* [Exemplos de código](app-insights-export-telemetry.md#code-samples)
