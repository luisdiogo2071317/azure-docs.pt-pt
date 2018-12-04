---
title: Como diagnosticar e resolver problemas relacionados com o Azure Time Series Insights (pré-visualização) | Documentos da Microsoft
description: Noções básicas sobre como diagnosticar e resolver problemas com o Azure Time Series Insights (pré-visualização)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: d13d373169287a0ec5931d5437b0a3bc70ecd79a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856354"
---
# <a name="how-to-diagnose-and-troubleshoot"></a>Como diagnosticar e resolver problemas

Este artigo resume vários problemas comuns que poderá encontrar o trabalho com o seu ambiente do Azure Time Series Insights (TSI). O artigo também descreve possíveis causas e soluções para cada um.

## <a name="problem-no-data-is-seen-in-the-time-series-insights-preview-explorer"></a>Problema: Não existem dados são vistos no Time Series Insights (pré-visualização) Explorer

Há vários motivos comuns, por que não poderá ver os dados no Explorador de TSI do Azure:

1. A origem do evento pode não estar a receber dados.

    Certifique-se de que a origem do evento (Hub de eventos ou o IoT Hub) está a receber dados das suas marcas / instâncias. Pode fazê-lo ao navegar para a página de descrição geral do seu recurso no portal do Azure.

    ![informações de dashboard][1]

1. Os dados de origem do evento não estão no formato JSON

    O Azure TSI suporta apenas os dados JSON. Para exemplos de JSON, veja [formas JSON suportadas](./how-to-shape-query-json.md).

1. A chave de origem do evento está em falta uma permissão necessária

    ![configuração][2]

    * Para um IoT Hub, terá de fornecer a chave que tenha permissão de ligação de serviço.
    * Conforme mostrado na imagem anterior, qualquer uma das políticas *iothubowner* e serviço funcionaria, uma vez que ambos têm permissão de ligação de serviço.
    * Para um hub de eventos, terá de fornecer a chave que tenha permissão de escuta.
    * Conforme mostrado na imagem anterior, qualquer uma das políticas de ler e gerir funcionaria, uma vez que têm permissão de escuta.

    ![permissões][3]

1. O grupo de consumidores fornecido não é exclusivo para TSI

    Durante o registo de am IoT Hub ou hub de eventos, especifique o grupo de consumidores que deve ser utilizado para ler os dados. Este grupo de consumidores não tem de ser partilhado. Se o grupo de consumidores for partilhado, o hub de eventos subjacente automaticamente desliga um dos leitores aleatoriamente. Forneça um grupo de consumidores exclusivo para o TSI ler a partir de.

## <a name="problem-some-data-is-shown-but-some-is-missing"></a>Problema: A alguns dados são apresentados, mas alguns está em falta

Isto pode ocorrer porque o seu ambiente está a ser limitado.

> [!NOTE]
> Neste momento, o Azure TSI suporta uma taxa máxima de ingestão de 6 MBps.

## <a name="problem-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Problema: A definição de nome de propriedade de timestamp minha origem de evento não funciona

Certifique-se de que o nome e o valor está em conformidade com as seguintes regras:

* O **Timestamp** nome da propriedade diferencia maiúsculas de minúsculas.
* O **Timestamp** valor da propriedade que provêm de origem de evento, como uma cadeia de caracteres do JSON, deve ter o formato `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`. Um exemplo de uma cadeia de caracteres é `“2008-04-12T12:53Z”`.

A maneira mais fácil para garantir que seu nome de propriedade timestamp é capturado e a funcionar corretamente é utilizar o Explorador TSI. No Explorador de TSI, utilize o gráfico para selecionar um período de tempo depois que forneceu o nome da propriedade timestamp. Com o botão direito a seleção e escolha o **explorar eventos** opção. O cabeçalho de coluna primeiro deve ser sua **Timestamp** nome da propriedade e devem ter um `($ts)` junto a palavra `Timestamp`, em vez:

* `(abc)`, que poderia indicar TSI está a ler os valores de dados como cadeias de caracteres
* Ícone de calendário, o que poderia indicar que Tsi é ler o valor de dados como datetime
* `#`, que poderia indicar TSI é ler os valores de dados como um número inteiro

## <a name="problem-my-time-series-id-property-is-incorrect-missing-or-null"></a>Problema: A minha propriedade de ID de série de tempo é incorreto, em falta ou é nulo

Isto pode ocorrer se o **ID de série de tempo** propriedade não está configurada corretamente ao tempo de aprovisionamento do ambiente. Consulte a [melhores práticas para a escolha de um ID de série de tempo](./time-series-insights-update-how-to-id.md) artigo ao escolher um **ID de série de tempo**. Neste momento, não é possível atualizar um ambiente existente do TSI (pré-visualização) para utilizar um diferente **ID de série de tempo**.

## <a name="problem-all-my-instances-in-time-series-insights-preview-explorer-dont-have-a-parent"></a>Problema: Todas as minhas instâncias no Time Series Insights (pré-visualização) Explorer não tem um elemento principal

Isto pode ocorrer se o seu ambiente não tem um **modelo de série de tempo** hierarquia definida. Veja este artigo para obter mais informações sobre [como trabalhar com modelos de série de tempo](./time-series-insights-update-how-to-tsm.md).

## <a name="next-steps"></a>Passos Seguintes

* Leia [como trabalhar com modelos de série de tempo](./time-series-insights-update-how-to-tsm.md).

* Leia [formas JSON suportadas](./how-to-shape-query-json.md).

<!-- Images -->
[1]: media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png
[2]: media/v2-update-diagnose-and-troubleshoot/configuration.png
[3]: media/v2-update-diagnose-and-troubleshoot/permissions.png