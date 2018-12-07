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
ms.date: 12/06/2018
ms.openlocfilehash: a9078d2f8a738700a30d265d9cfa3cd77ad72f08
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015454"
---
# <a name="how-to-diagnose-and-troubleshoot"></a>Como diagnosticar e resolver problemas

Este artigo resume vários problemas comuns que poderá encontrar o trabalho com o seu ambiente do Azure Time Series Insights (TSI). O artigo também descreve possíveis causas e soluções para cada um.

## <a name="problem-i-cant-find-my-environment-in-the-time-series-insights-preview-explorer"></a>Problema: não consigo encontrar meu ambiente no Explorador do Time Series Insights (pré-visualização)

Isto pode ocorrer se não tiver permissões para aceder ao ambiente do TSI. Os utilizadores terão a função de acesso de nível de "leitor de" para ver o seu ambiente do TSI. Pode verificar os atuais níveis de acesso e conceder acesso adicional ao visitar a secção de políticas de acesso de dados no recurso de TSI [Portal do Azure](https://portal.azure.com/).

  ![environment][1]

## <a name="problem-no-data-is-seen-in-the-time-series-insights-preview-explorer"></a>Problema: Não existem dados são vistos no Explorador do Time Series Insights (pré-visualização)

Há vários motivos comuns, por que não poderá ver os dados no [explorer (pré-visualização) do Azure TSI](https://insights.timeseries.azure.com/preview):

1. A origem do evento pode não estar a receber dados.

    Certifique-se de que a origem do evento (Hub de eventos ou o IoT Hub) está a receber dados das suas marcas / instâncias. Pode fazê-lo ao navegar para a página de descrição geral do seu recurso no portal do Azure.

    ![informações de dashboard][2]

1. Os dados de origem do evento não estão no formato JSON

    O Azure TSI suporta apenas os dados JSON. Para exemplos de JSON, veja [formas JSON suportadas](./how-to-shape-query-json.md).

1. A chave de origem do evento está em falta uma permissão necessária

    * Para um IoT Hub, terá de fornecer a chave que tenha permissão de ligação de serviço.

    ![configuração][3]

    * Conforme mostrado na imagem anterior, qualquer uma das políticas *iothubowner* e serviço funcionaria, uma vez que ambos têm permissão de ligação de serviço.
    * Para um hub de eventos, terá de fornecer a chave que tenha permissão de escuta.
  
    ![permissões][4]

    * Conforme mostrado na imagem anterior, qualquer uma das políticas **ler** e **gerir** funcionaria, pois têm **escutar** permissão.

1. O grupo de consumidores fornecido não é exclusivo para TSI

    Durante o registo de um IoT Hub ou Hub de eventos, especifique o grupo de consumidores que deve ser utilizado para ler os dados. Esse grupo de consumidores não tem de ser partilhado. Se o grupo de consumidores for partilhado, o Hub de eventos subjacente automaticamente desliga um dos leitores aleatoriamente. Forneça um grupo de consumidores exclusivo para o TSI ler a partir de.

1. Sua propriedade de ID de série de tempo especificada no momento do provisionamento está incorreto, em falta ou é nulo

    Isto pode ocorrer se o **ID de série de tempo** propriedade não está configurada corretamente ao tempo de aprovisionamento do ambiente. Consulte a [melhores práticas para a escolha de um ID de série de tempo](./time-series-insights-update-how-to-id.md). Neste momento, não é possível atualizar um ambiente de atualização do Time Series Insights existente para utilizar um diferente **ID de série de tempo**.

## <a name="problem-some-data-is-shown-but-some-is-missing"></a>Problema: A alguns dados são apresentados, mas alguns está em falta

1. Pode ser o envio de dados sem o ID de série de tempo

    Este erro pode ocorrer quando está a enviar eventos sem o campo de ID de série de tempo no payload. Ver [formas JSON suportadas](./how-to-shape-query-json.md) para obter mais informações.

1. Isto pode ocorrer porque o seu ambiente está a ser limitado.

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

Se o **Timestamp** propriedade não for especificada explicitamente, iremos tirar partido IoT Hub ou Hub de eventos de um evento **colocados em fila tempo** como o período de tempo predefinido.

## <a name="problem-i-cant-edit-or-view-my-time-series-model"></a>Problema: não consigo editar ou ver o meu modelo de série de tempo

1. Pode acessar um Time Series Insights S1 ou S2 ambiente

   Modelos de série de tempo são suportados apenas num **PAYG** ambientes. Veja este artigo para obter mais informações sobre como acessar o seu ambiente de S1/S2 partir do Explorador de atualização de informações de série de tempo.

   ![access][5]

1. Pode não ter permissões para ver e editar o modelo

   Os utilizadores precisam de acesso de nível de "contribuinte" a editar e ver o seu modelo de série de tempo. Pode verificar os atuais níveis de acesso e conceder acesso adicional ao visitar a secção de políticas de acesso de dados no seu recurso do Time Series Insights no Portal do Azure.

## <a name="problem-all-my-instances-in-time-series-insights-preview-explorer-dont-have-a-parent"></a>Problema: Todas as minhas instâncias no Explorador do Time Series Insights (pré-visualização) não tem um elemento principal

Isto pode ocorrer se o seu ambiente não tem um **modelo de série de tempo** hierarquia definida. Veja este artigo para obter mais informações sobre [como trabalhar com modelos de série de tempo](./time-series-insights-update-how-to-tsm.md).

  ![TSM][6]

## <a name="next-steps"></a>Passos Seguintes

Leia [como trabalhar com modelos de série de tempo](./time-series-insights-update-how-to-tsm.md).

Leia [formas JSON suportadas](./how-to-shape-query-json.md).

<!-- Images -->
[1]: media/v2-update-diagnose-and-troubleshoot/environment.png
[2]: media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png
[3]: media/v2-update-diagnose-and-troubleshoot/configuration.png
[4]: media/v2-update-diagnose-and-troubleshoot/permissions.png
[5]: media/v2-update-diagnose-and-troubleshoot/access.png
[6]: media/v2-update-diagnose-and-troubleshoot/tsm.png