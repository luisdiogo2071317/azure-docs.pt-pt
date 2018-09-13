---
title: Modelo de dados de telemetria de informações de aplicação do Azure | Documentos da Microsoft
description: Visão geral do modelo de dados de informações do aplicativo
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: aaff60e847e0e9908a4cd9c07cb6cd47630c5e3a
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647707"
---
# <a name="application-insights-telemetry-data-model"></a>Modelo de dados de telemetria do Application Insights

[O Azure Application Insights](app-insights-overview.md) envia a telemetria da sua aplicação web para o portal do Azure, para que pode analisar o desempenho e a utilização da sua aplicação. O modelo de telemetria é padronizado para que seja possível criar a plataforma e independente de idioma de monitorização. 

Os modelos de dados recolhidos pelo Application Insights este padrão de execução do aplicativo típico:

![Modelo de aplicativo do Application Insights](./media/application-insights-data-model/application-insights-data-model.png)

Os seguintes tipos de telemetria são utilizados para monitorizar a execução da sua aplicação. Os seguintes três tipos são normalmente recolhidos automaticamente pelo SDK do Application Insights da estrutura de aplicativo da web:

* [**Pedido** ](application-insights-data-model-request-telemetry.md) - gerado para iniciar um pedido recebido pela sua aplicação. Por exemplo, a web do Application Insights SDK gera automaticamente um item de telemetria de pedido para cada solicitação HTTP que recebe a sua aplicação web. 

    Uma **operação** é os threads de execução que processa um pedido. Também pode [escrever código](app-insights-api-custom-events-metrics.md#trackrequest) monitorizar os outros tipos de operação, como um "despertar" numa web da tarefa ou periodicamente a função que processa os dados.  Cada operação tem um ID. Este ID que pode ser utilizado para [grupo](application-insights-correlation.md) toda a telemetria gerada enquanto a aplicação está a processar o pedido. Cada operação seja for concluída com êxito ou falha e tem uma duração de tempo.
* [**Exceção** ](application-insights-data-model-exception-telemetry.md) -normalmente, representa uma exceção que faz com que uma operação falhe.
* [**Dependência** ](application-insights-data-model-dependency-telemetry.md) -representa uma chamada a partir da sua aplicação para um serviço externo ou o armazenamento, como uma REST API ou SQL. No ASP.NET, chamadas de dependência para o SQL são definidas pelo `System.Data`. Chamadas para pontos de extremidade HTTP são definidas pelo `System.Net`. 

O Application Insights fornece três tipos de dados adicionais para telemetria personalizada:

* [Rastreio](application-insights-data-model-trace-telemetry.md) - usado diretamente ou através de um adaptador para implementar o registo de diagnósticos usando uma estrutura de instrumentação que é familiar para, como `Log4Net` ou `System.Diagnostics`.
* [Evento](application-insights-data-model-event-telemetry.md) - normalmente utilizado para capturar a interação do utilizador com o seu serviço, para analisar padrões de utilização.
* [Métrica](application-insights-data-model-metric-telemetry.md) - utilizado para reportar medições de escalar periódica.

Todos os itens de telemetria podem definir o [informações de contexto](application-insights-data-model-context.md) como id de sessão de utilizador ou de versão do aplicativo. O contexto é um conjunto de campos com rigidez de tipos que desbloqueia determinados cenários. Quando a versão da aplicação estará adequadamente inicializado, o Application Insights pode detetar novos padrões no comportamento do aplicativo correlacionado a reimplementação. Id de sessão pode ser utilizado para calcular a falha ou um impacto do problema sobre os usuários. Calcular Contagem distinta de valores de id de sessão para determinados falha de dependência, rastreio de erro ou exceção crítica dá uma boa compreensão do impacto.

Modelo de telemetria do Application Insights define uma maneira [correlacionar](application-insights-correlation.md) telemetria para a operação do qual é uma parte. Por exemplo, um pedido pode fazer um chamadas de base de dados SQL e registradas informações de diagnóstico. Pode definir o contexto de correlação para os itens de telemetria que vinculá-lo novamente para a telemetria de pedido.

## <a name="schema-improvements"></a>Melhorias do esquema

Modelo de dados do Application Insights é uma forma simples e básica e poderosa para modelar a telemetria da sua aplicação. Esforçamo-nos manter o modelo simples e reduzido para dar suporte a cenários essenciais e permitir a expandir o esquema para utilização avançada.

Para reportar problemas de modelo ou o esquema de dados e sugestões utilizá-lo [página inicial do Application Insights](https://github.com/Microsoft/ApplicationInsights-Home/labels/schema) repositório.

## <a name="next-steps"></a>Passos Seguintes

- [Escrever telemetria personalizada](app-insights-api-custom-events-metrics.md)
- Saiba como [estender e filtrar telemetria](app-insights-api-filtering-sampling.md).
- Uso [amostragem](app-insights-sampling.md) para minimizar a quantidade de telemetria com base no modelo de dados.
- Confira [plataformas](app-insights-platforms.md) suportada pelo Application Insights.
