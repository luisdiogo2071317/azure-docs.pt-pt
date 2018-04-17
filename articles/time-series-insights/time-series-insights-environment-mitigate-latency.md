---
title: Como monitorizar e reduzir a limitação de informações de séries de tempo do Azure | Microsoft Docs
description: Este artigo descreve como monitorizar, diagnosticar e atenuar problemas de desempenho que causam a latência e limitação na informações de séries de tempo do Azure.
services: time-series-insights
ms.service: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 11/27/2017
ms.openlocfilehash: ac59359eb6af268f311534d90e1529fc5e41094f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights"></a>Monitorizar e a mitigar a limitação para reduzir a latência de informações de séries de tempo do Azure
Quando a quantidade de dados recebidos excede a configuração do seu ambiente, pode deparar-se latência ou limitação de informações de séries de tempo do Azure.

Pode evitar latência e limitação ao configurar corretamente o seu ambiente para a quantidade de dados que pretende analisar.

É mais provável experimentar a latência e limitação quando tiver:

- Adicione uma origem de evento que contém dados antigos que podem exceder a taxa de entrada atribuído (Insights de séries de tempo necessário para detetar).
- Adicione mais origens de eventos para um ambiente, resultando num pico de pedidos de eventos adicionais (o que poderá exceder a capacidade do seu ambiente).
- Emitir grandes quantidades de eventos históricos para uma origem de evento, resultando num atraso (Insights de séries de tempo necessário para detetar).
- Associe dados de referência com telemetria, resultando em maior dimensão do evento.  Numa perspetiva de limitação, um pacote de dados ingressed com um tamanho de pacotes de 32 KB é tratado como 32 eventos, cada um tamanho de 1 KB. O tamanho máximo do evento permitido é de 32 KB; pacotes de dados superiores a 32 KB serão truncados.


## <a name="monitor-latency-and-throttling-with-alerts"></a>Monitorizar a latência e limitação com alertas

Alertas podem ajudar a ajudar a diagnosticar e atenuar problemas de latência causados pelo seu ambiente. 

1. No portal do Azure, clique em **métricas**. 

   ![Métricas](media/environment-mitigate-latency/add-metrics.png)

2. Clique em **Adicionar alerta métrica**.  

    ![Adicionar alerta de métrica](media/environment-mitigate-latency/add-metric-alert.png)

A partir daí, pode configurar alertas utilizando as métricas seguintes:

|Métrica  |Descrição  |
|---------|---------|
|**Entrada recebidos Bytes**     | A contagem de bytes não processados ler a partir de origens de eventos. Contagem de não processada normalmente inclui o nome da propriedade e o valor.  |  
|**Entrada recebidas mensagens inválidas**     | Número de mensagens inválidos lida todas as origens de eventos de Event Hubs do Azure ou o IoT Hub do Azure.      |
|**Entrada recebidas mensagens**   | Contagem de mensagens de leitura de todos os Event Hubs ou os Hubs IoT origens de eventos.        |
|**Entrada armazenados Bytes**     | Total de tamanho de eventos armazenados e disponíveis para consulta. O tamanho é calculado apenas com o valor de propriedade.        |
|**Entrada armazenados eventos**     |   Contagem de eventos simplificadas armazenados e disponíveis para consulta.      |
|**Intervalo de tempo de Recieved mensagem de entrada**    |  Diferença entre a hora em que a mensagem é colocados em fila na origem de evento e o tempo que foi processado na entrada.      |
|**Atraso de contagem de mensagens de Recieved de entrada**    |  Diferença entre o número de sequência da última mensagem colocados em fila de eventos da origem de número de partição e a sequência de mensagem a ser processado na entrada.      |


![Latência](media/environment-mitigate-latency/latency.png)

Se de que está a ser limitada, verá um valor para o *entrada Recieved mensagem desfasamento*, informando-o número de minutos atrás TSI é desde o momento real, a mensagem chega a origem do evento (excluindo o tempo de indexação de appx. 30-60 segundos).  *Contagem de mensagens de Recieved entrada desfasamento* também deve ter um valor, permitindo-lhe determinar a quantidade de mensagens atrás é.  A forma mais fácil de obter processadas é aumentar a capacidade do seu ambiente para um tamanho que vai permitir-lhe ultrapassar a diferença.  

Por exemplo, se tiver um ambiente única unidade S1 e vê que existe um desfasamento de mensagem de milhões de cinco, foi possível aumentar o tamanho do seu ambiente para seis unidades para em torno de um dia para obter processadas.  Foi aumenta mesmo further to catch cópias de segurança mais rapidamente.  Este é um occurance comum quando aprovisionar inicialmente o ambiente, especialmente quando ligar a uma origem de evento que já tenha eventos na mesma ou quando efetuar em massa muitos de carregamento de dados históricos.

Outra técnica consiste em definir uma **eventos armazenados de entrada** alerta > = um limiar ligeiramente inferior a capacidade total do ambiente durante um período de duas horas.  Este alerta pode ajudá-lo a compreender se constantemente estão capacidade, o que indica uma elevada probabilidade de latência.  

Por exemplo, se tiver três unidades de S1 aprovisionadas (ou eventos de 2100 por capacidade de minutos de entrada), pode definir um **eventos armazenados de entrada** alerta > = 1900 eventos durante 2 horas. Se não são constantemente exceder este limiar e, por conseguinte, a acionar o alerta, está provavelmente em-aprovisionado.  

Além disso, se suspeitar que está a ser limitada, pode comparar o **entrada recebidas mensagens** com o evento de origem do egressed mensagens.  Se a entrada para o Hub de eventos é superior ao seu **entrada recebidas mensagens**, provavelmente estão a ser limitadas a suas informações de séries de tempo.

## <a name="improving-performance"></a>Melhorar o desempenho 
Para reduzir a limitação ou estão a ocorrer latência, a melhor forma para corrigi-lo é ao aumentar a capacidade do seu ambiente. 

Pode evitar latência e limitação ao configurar corretamente o seu ambiente para a quantidade de dados que pretende analisar. Para obter mais informações sobre como adicionar capacidade ao seu ambiente, consulte [dimensionar o seu ambiente](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Passos Seguintes
- Para obter os passos de resolução de problemas adicionais, [diagnosticar e resolver problemas no seu ambiente de tempo série Insights](time-series-insights-diagnose-and-solve-problems.md).
- Para obter ajuda adicional, iniciar uma conversação a [fórum MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) ou [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). Também pode contactar [suporte do Azure](https://azure.microsoft.com/support/options/) para as opções de suporte assistido.
