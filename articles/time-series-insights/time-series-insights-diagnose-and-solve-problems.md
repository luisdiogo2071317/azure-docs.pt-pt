---
title: Diagnosticar e resolver problemas no Azure Time Series Insights | Documentos da Microsoft
description: Este artigo descreve como diagnosticar, solucionar problemas e resolver problemas comuns que poderá encontrar no seu ambiente do Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 04/09/2018
ms.openlocfilehash: ef06e7b1abd66a2204ef982943fe24354bd7f122
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837448"
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Diagnosticar e resolver problemas no seu ambiente do Time Series Insights

Este artigo descreve alguns problemas que poderá ver no seu ambiente do Time Series Insights. Ele oferece possíveis causas e soluções para a resolução.

## <a name="video"></a>Vídeo: 

### <a name="in-this-video-we-cover-common-time-series-insights-customer-challenges-and-mitigationsbr"></a>Neste vídeo, vamos abordar os desafios de cliente do Time Series Insights e atenuações comuns.</br>

> [!VIDEO https://www.youtube.com/embed/7U0SwxAVSKw]

## <a name="problem-1-no-data-is-shown"></a>Problema 1: Não existem dados são mostrados
Há vários motivos comuns, por que não poderá ver os dados no [Explorador de documentos do Azure Time Series Insights](https://insights.timeseries.azure.com):

### <a name="possible-cause-a-event-source-data-is-not-in-json-format"></a>Causa possível dados de origem de evento de r: não estão no formato JSON
O Azure Time Series Insights suporta apenas os dados JSON. Para exemplos de JSON, veja [formas JSON suportadas](./how-to-shape-query-json.md).

### <a name="possible-cause-b-event-source-key-is-missing-a-required-permission"></a>Chave de origem do evento de b: causa possível está em falta uma permissão necessária
* Para um IoT Hub, tem de fornecer a chave que tenha **ligação de serviço** permissão.

   ![Permissão de ligação de serviço do IoT Hub](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Conforme mostrado na imagem anterior, qualquer uma das políticas **iothubowner** e **service** funcionaria, pois têm **serviço ligar** permissão.

* Para um hub de eventos, tem de fornecer a chave que tenha **escutar** permissão.

   ![Permissão de escuta do hub de eventos](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Conforme mostrado na imagem anterior, qualquer uma das políticas **ler** e **gerir** funcionaria, pois têm **escutar** permissão.

### <a name="possible-cause-c-the-consumer-group-provided-is-not-exclusive-to-time-series-insights"></a>Possível fazer com que c: O grupo de consumidores fornecido não é exclusiva para o Time Series Insights
Durante o registo de am IoT Hub ou hub de eventos, especifique o grupo de consumidores que deve ser utilizado para ler os dados. Tem este grupo de consumidores **não** ser partilhado. Se o grupo de consumidores for partilhado, o hub de eventos subjacente automaticamente desliga um dos leitores aleatoriamente. Forneça um grupo de consumidores exclusivo para ler a partir do Time Series Insights.

## <a name="problem-2-some-data-is-shown-but-some-is-missing"></a>Problema 2: Alguns dados são mostrados, mas alguns está em falta
Quando pode ver os dados parcialmente, mas os dados é atrasado, existem várias possibilidades a considerar:

### <a name="possible-cause-a-your-environment-is-getting-throttled"></a>R: o seu ambiente de causa possível é obter limitada
Este é um problema comum quando os ambientes são aprovisionados após a criação de uma origem de eventos com dados.  Os Hubs IoT e dos Hubs de eventos do Azure armazenam dados até sete dias.  TSI sempre será iniciada a partir do evento mais antigo (FIFO), da origem do evento.  Então, se tiver cinco milhões de eventos numa origem de eventos quando se liga a uma S1, ambiente de TSI única unidade, o TSI lerá aproximadamente um milhão de eventos por dia.  Isso talvez pareça parecer como se TSI está com cinco dias de latência à primeira vista.  O que acontece na realidade é que o ambiente está a ser limitado.  Se tiver eventos antigos na origem de evento, pode abordar uma das seguintes formas:

- Limites de retenção da sua origem de evento para o ajudar a eliminar eventos antigos que não pretende que apareçam no TSI de alteração
- Aprovisione um tamanho maior de ambiente (em termos de número de unidades de) para aumentar o débito de eventos antigos.  Com o exemplo acima, se aumentado nesse mesmo ambiente de S1 para cinco unidades durante um dia, o ambiente deve catch-up agora durante o dia.  Em seguida, se sua produção de eventos de estado estável é 1 milhão ou menos eventos/dia, pode reduzir a capacidade do evento de volta para uma unidade depois que detetou.  

O limite de limitação é imposto com base no tipo de SKU do ambiente e a capacidade. Todas as origens de eventos no ambiente de partilham esta capacidade. Se a origem do evento para o seu IoT Hub ou hub de eventos é enviar por push dados para além dos limites de imposto, verá a limitação e um atraso.

O diagrama seguinte mostra um ambiente do Time Series Insights que tenha uma SKU S1 e uma capacidade de 3. É possível 3 milhões de eventos de entrada por dia.

![Capacidade atual de SKU de ambiente](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Por exemplo, suponha que este ambiente está a ingerir mensagens a partir de um hub de eventos. Observe a taxa de entrada mostrada no diagrama seguinte:

![Taxa de entrada de exemplo para um hub de eventos](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Conforme mostrado no diagrama, a taxa de entrada diária é ~ 67,000 mensagens. Essa taxa traduz-se aproximadamente 46 mensagens a cada minuto. Se cada mensagem do hub de eventos foi simplificada para um único evento de Time Series Insights, este ambiente não vê nenhuma limitação. Se cada mensagem do hub de eventos foi simplificada para 100 eventos do Time Series Insights, em seguida, 4,600 eventos devem ser ingeridos a cada minuto. Um ambiente de S1 SKU que tem a capacidade de 3 pode 2.100 apenas eventos de entrada a cada minuto (1 milhão de eventos por dia = 700 eventos por minuto em 3 unidades = 2.100 eventos por minuto). Por conseguinte, ver um atraso devido à limitação. 

Para uma compreensão geral de como funciona a mesclar lógica, consulte [formas JSON suportadas](./how-to-shape-query-json.md).

### <a name="recommended-resolution-steps-for-excessive-throttling"></a>Passos de resolução recomendada para a limitação excessiva de
Para corrigir o atraso, aumente a capacidade SKU do seu ambiente. Para obter mais informações, consulte [como dimensionar o seu ambiente do Time Series Insights](time-series-insights-how-to-scale-your-environment.md).

### <a name="possible-cause-b-initial-ingestion-of-historical-data-is-causing-slow-ingress"></a>Causa possível ingestão b: inicial de dados históricos é fazendo com que a entrada lenta
Se estiver a ligar a uma origem de evento existente, é provável que o seu hub IoT Hub ou de um evento já tiver dados no mesmo. O ambiente começa a extrair dados desde o início do período de retenção de mensagem de origem do evento.

Este comportamento é o comportamento predefinido e não pode ser substituído. Pode interagir com a limitação e poderá demorar algum tempo para acompanhar a ingerir dados históricos.

#### <a name="recommended-resolution-steps-of-large-initial-ingestion"></a>Passos de resolução recomendada da ingestão inicial grande
Para corrigir o atraso, siga os passos seguintes:
1. Aumente a capacidade SKU para o valor máximo permitido (10 neste caso). Depois da capacidade for aumentada, o processo de entrada começa a capturar a cópia de segurança muito mais rapidamente. Pode ver rapidamente como estamos obtendo o gráfico de disponibilidade no [Explorador do Time Series Insights](https://insights.timeseries.azure.com). É-lhe cobrada a maior capacidade.
2. Depois do desfasamento é atualizado com, diminua a capacidade SKU para a taxa de entrada normal.

## <a name="problem-3-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Problema 3: Minha origem de evento *nome da propriedade timestamp* definição não funciona
Certifique-se de que o nome e o valor está em conformidade com as seguintes regras:
* É o nome da propriedade timestamp _diferencia maiúsculas de minúsculas_.
* O valor da propriedade timestamp que provêm de origem de evento, como uma cadeia de caracteres do JSON, deve ter o formato _aaaa-MM-ddTHH. FFFFFFFK_. Um exemplo de uma cadeia de caracteres é "2008-04-12T12:53Z".

A maneira mais fácil para garantir que seus *nome da propriedade timestamp* é capturado e a funcionar corretamente é utilizar o Explorador TSI.  No Explorador de TSI, com o gráfico, selecione um período de tempo depois que forneceu o *nome da propriedade timestamp*.  Com o botão direito a seleção e escolha o *explorar eventos* opção.  O cabeçalho de coluna primeiro deve ser sua *nome da propriedade timestamp* e deve ter um *($ts)* ao lado da palavra *Timestamp*, em vez:
- *(abc)* , que poderia indicar TSI está a ler os valores de dados como cadeias de caracteres
- *Ícone de calendário*, que poderia indicar TSI é ler o valor de dados como *datetime*
- *#*, que poderia indicar TSI é ler os valores de dados como um número inteiro


## <a name="next-steps"></a>Passos Seguintes
- Para obter mais ajuda, iniciar uma conversa sobre o [fórum MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) ou [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights). 
- Também pode utilizar [suporte do Azure](https://azure.microsoft.com/support/options/) para opções de suporte assistido.
