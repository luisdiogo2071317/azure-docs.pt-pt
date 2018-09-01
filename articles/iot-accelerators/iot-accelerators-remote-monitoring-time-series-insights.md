---
title: Visualizar a telemetria de monitorização remota com o Azure Time Series Insights | Documentos da Microsoft
description: Saiba como configurar o ambiente de Time Series Insights, explorar e analisar a telemetria gerada pelo acelerador de solução de monitorização remota.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 08/20/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 02b3afbc62be7a7a9c396de888378d762405248a
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/01/2018
ms.locfileid: "43383223"
---
# <a name="use-time-series-insights-to-visualize-telemetry-sent-from-the-remote-monitoring-solution-accelerator"></a>Utilizar o Time Series Insights para ver a telemetria enviada pelo acelerador de solução de monitorização remota

Como um operador, pode querer aumentar as capacidades de visualização de dados de out-of-the-box do solution accelerator monitorização remota. Este guia de procedimentos mostra-lhe como utilizar um ambiente do Time Series Insights para visualizar e analisar a telemetria enviada para o acelerador de solução de monitorização remota.

## <a name="prerequisites"></a>Pré-requisitos

Para seguir os passos neste guia de procedimentos, precisa de uma subscrição do Azure Active Directory. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Os passos neste guia de procedimentos partem do princípio de que implementar o acelerador de solução de monitorização remota para a sua subscrição do Azure. Se ainda não implementou o solution accelerator, siga os passos a [implementar e executar uma solução de monitorização remota com base na cloud](quickstart-remote-monitoring-deploy.md) início rápido.

Este artigo pressupõe que é o nome do seu solution accelerator **contoso-simulação**. Substitua **contoso-simulação** com o nome do seu solution accelerator à medida que conclua os seguintes passos.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [iot-accelerators-create-tsi.md](../../includes/iot-accelerators-create-tsi.md)]

## <a name="time-series-insights-explorer"></a>Explorador do Time Series Insights

O Explorador do Time Series Insights é uma aplicação web que pode utilizar para visualizar a sua telemetria.

1. No portal do Azure, selecione o Time Series Insights **descrição geral** separador.

1. Para abrir a aplicação de web de Explorador do Time Series Insights, clique em **vá para o ambiente**:

    ![Explorador do Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-environment.png)

1. No painel de seleção de tempo, selecione **últimos 30 minutos** partir rápido vezes menu e clique em **pesquisa**:

    ![Pesquisa de explorer do tempo Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-search-time.png)

1. No painel de termos no lado esquerdo, selecione **temperatura** como o **medida** e **iothub-ligação-dispositivo-id** como o **dividido por** valor:

    ![Consulta de Explorador do Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-query1.png)

1. Faça duplo clique no gráfico e selecione **explorar eventos**:

    ![Eventos de Explorador do Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-explore-events.png)

1. Mostram os dados do evento numa grade:

    ![Tabela de Explorador do Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-table.png)

1. Clique no botão de vista do ponto de vista:

    ![O ponto de vista do Time Series Insights explorer](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-explorer-perspective.png)

1. Clique em **+** para adicionar uma nova consulta para o ponto de vista:

    ![Explorador do Time Series Insights adicionar consulta](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-new-query.png)

1. Selecione **últimos 30 minutos** como o intervalo de tempo **humidade** como o **medida**, e **iothub-ligação-dispositivo-id** como o **Dividido por** valor:

    ![Consulta de Explorador do Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-query2.png)

1. Clique no botão de vista do ponto de vista para ver o painel de telemetria do dispositivo:

    ![Dashboard de Explorador do Time Series Insights](./media/iot-accelerators-remote-monitoring-time-series-insights/time-series-insights-dashboard.png)

[!INCLUDE [iot-accelerators-cleanup-tsi.md](../../includes/iot-accelerators-cleanup-tsi.md)]

## <a name="next-steps"></a>Próximos Passos

Para saber mais sobre como explorar e consultar dados no Explorador do Time Series Insights, veja [Explorador do Azure Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
