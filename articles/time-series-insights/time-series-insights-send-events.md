---
title: Como enviar eventos para um ambiente do Azure Time Series Insights | Documentos da Microsoft
description: Este tutorial explica como criar e configurar o hub de eventos e executar uma aplicação de exemplo para enviar eventos para ser mostrada no Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/26/2018
ms.openlocfilehash: 64fb9f72cfd7edef18d56f15cbcce726dd33b50d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847274"
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Enviar eventos para um ambiente do Time Series Insights com um hub de eventos

Este artigo explica como criar e configurar o hub de eventos e executa uma aplicação de exemplo para enviar eventos. Se tiver um hub de eventos existente com eventos no formato JSON, ignorar este tutorial e ver o seu ambiente no [do Azure Time Series Insights](./time-series-insights-update-create-environment.md).

## <a name="configure-an-event-hub"></a>Configurar um Hub de eventos

1. Para criar um Hub de eventos, siga as instruções do Hub de eventos [documentação](https://docs.microsoft.com/azure/event-hubs/).
1. Procure o Hub de eventos na barra de pesquisa. Clique em **os Hubs de eventos** na lista devolvida.
1. Selecione o seu Hub de eventos ao clicar no respetivo nome.
1. Sob **entidades** na janela de configuração média, clique em **dos Hubs de eventos** novamente.
1. Selecione o nome do Hub de eventos para configurá-lo.

    ![grupo de consumidores][1]

1. Sob **entidades**, selecione **grupos de consumidores**.
1. Certifique-se de que cria um grupo de consumidores que seja utilizado exclusivamente pela origem de evento TSI.

> [!IMPORTANT]
> Certifique-se de que este grupo de consumidores não é utilizado por nenhum outro serviço (por exemplo, a tarefa do Stream Analytics ou outro ambiente do TSI). Se o grupo de consumidores for utilizado por outros serviços, a operação de leitura é afetada negativamente neste ambiente e os outros serviços. Se estiver a utilizar `$Default` como o grupo de consumidores pode levar à potencial reutilização por outros leitores.

1. Sob o **configurações** cabeçalho, selecione **políticas de acesso de partilha**.
1. No hub de eventos, crie **MySendPolicy** que é utilizado para enviar eventos no C# exemplo.

    ![partilhado = um acesso][2]

    ![partilhado-access-dois][3]

## <a name="add-time-series-insights-instances"></a>Adicionar instâncias do Time Series Insights

A atualização do TSI usa instâncias para adicionar dados contextuais para dados telemétricos recebidos. Os dados que estão associados ao tempo de consulta utilizando um **ID de série de tempo**. O **ID de série de tempo** para os windmills de exemplo é projeto `Id`. Para saber mais sobre as instâncias de série de tempo e **IDs de série de tempo**, leia [modelos de série de tempo](./time-series-insights-update-tsm.md).

### <a name="create-time-series-insights-event-source"></a>Criar a origem de eventos do Time Series Insights

1. Se não tiver criado uma origem de eventos, siga estas instruções para criar uma origem de evento.
1. Especifique a `timeSeriesId` – veja [modelos de série de tempo](./time-series-insights-update-tsm.md) para saber mais sobre **IDs de série de tempo**.

### <a name="push-events-sample-windmills"></a>Enviar eventos (windmills de exemplo)

1. Procure o hub de eventos na barra de pesquisa. Clique em **os Hubs de eventos** na lista devolvida.
1. Selecione o seu hub de eventos ao clicar no respetivo nome.
1. Aceda a **partilhados políticas de acesso** e, em seguida **RootManageSharedAccessKey**. Copiar o **ligação chave primária sting**

   ![connection-string][4]

1. Aceda a https://tsiclientsample.azurewebsites.net/windFarmGen.html. Esta solução é executada windmill simulado dispositivos.
1. Cole a cadeia de ligação que copiou no passo três no **cadeia de ligação do Hub de eventos**
1. Clique em **clique para iniciar**
1. Regresse ao seu Hub de eventos. Deverá ver os novos eventos que está a ser recebidos pelo hub:

   ![telemetria][5]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ver o seu ambiente no Explorador do Time Series Insights](https://insights.timeseries.azure.com).

<!-- Images -->
[1]: media/send-events/consumer-group.png
[2]: media/send-events/shared-access-policy.png
[3]: media/send-events/shared-access-policy-2.png
[4]: media/send-events/sample-code-connection-string.png
[5]: media/send-events/telemetry.png