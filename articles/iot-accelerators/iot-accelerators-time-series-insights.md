---
title: Visualizar dados com informações de séries de tempo do Azure de monitorização de remoto | Microsoft Docs
description: Saiba como configurar o ambiente de informações de séries de tempo para explorar e analisar os dados de séries de tempo da sua solução de monitorização remota.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 7a0a5d4f1fbba5d7bd2813e8b9c300a37853e06c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111481"
---
# <a name="visualize-remote-monitoring-data-with-time-series-insights"></a>Visualizar dados de monitorização remotos com informações de séries de tempo

Um operador poderá pretender expandir ainda mais a saída dos dados de caixa de visualização fornecida pela monitorização remota pré-configurar a solução. A nossa solution accelerator fornece sem a integração de caixa com TSI. Este procedimentos irá aprender a configurar as informações de séries de tempo para analisar a telemetria do dispositivo e detetar anomalias.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este procedimentos, irá precisar do seguinte:

* [Implementar a solução pré-configurada de monitorização remota](iot-accelerators-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Criar um grupo de consumidores

Terá de criar um grupo de consumidores dedicado no seu IoT hub a ser utilizada para transmissão em fluxo de dados às informações de séries de tempo.

> [!NOTE]
> Grupos de consumidores são utilizados por aplicações para obter dados do IoT Hub do Azure. Cada grupo de consumidores permite até cinco consumidores de saída. Deve criar um novo grupo de consumidores para todos os cinco saída sinks e pode criar até 32 grupos de consumidores.

1. No portal do Azure, clique no botão de Shell de nuvem.

1. Execute o seguinte comando para criar um novo grupo de consumidores:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="create-a-new-time-series-insights-environment"></a>Criar um novo ambiente de informações de séries de tempo

O Azure Time Series Insights é um serviço totalmente gerido de análise, armazenamento e visualização para a gestão de dados de séries temporais à escala de IoT na cloud. Este serviço fornece armazenamento de dados de séries temporais dimensionável e permite-lhe explorar e analisar milhares de milhões de eventos transmitidos em fluxo de todo o mundo numa questão de segundos. Utilize informações de séries de tempo para armazenar e gerir terabytes de dados de séries de tempo, explore e visualizar billions de eventos em simultâneo, realiza uma análise da causa raiz e para comparar a vários sites e recursos.

1. Inicie sessão no [portal do Azure](http://portal.azure.com/).

1. Selecione **crie um recurso** > **Internet das coisas** > **Insights de séries de tempo**.

    ![Novo Insights de séries de tempo](./media/iot-accelerators-time-series-insights/new-time-series-insights.png)

1. Para criar o seu ambiente de informações de séries de tempo, utilize os valores na tabela seguinte:

    | Definição | Valor |
    | ------- | ----- |
    | Nome do ambiente | Captura de ecrã seguinte utiliza o nome **contorosrmtsi**. Escolha o seu próprio nome exclusivo quando concluir este passo. |
    | Subscrição | Selecione a sua subscrição do Azure na lista pendente. |
    | Grupo de recursos | **Criar um novo**. Estamos a utilizar o nome **ContosoRM**. |
    | Localização | Estamos a utilizar **EUA Leste**. Crie o seu ambiente na mesma região que a sua solução de monitorização remota. |
    | Sku |**S1** |
    | Capacidade | **1** |
    | Afixar ao dashboard | **Sim** |

    ![Criar informações de séries de tempo](./media/iot-accelerators-time-series-insights/new-time-series-insights-create.png)

1. Clique em **Criar**. Pode demorar alguns minutos para o ambiente de criação.

## <a name="create-event-source"></a>Criar origem de evento

Crie uma nova origem de evento para ligar ao seu IoT hub. Certifique-se de que utiliza o grupo de consumidores criado nos passos anteriores. Informações de séries de tempo requer cada serviço tem um grupo de consumidores dedicado não ser utilizado por outro serviço.

1. Navegue para o novo ambiente de séries de tempo.

1. No lado esquerdo, selecione **origens de eventos**.

    ![Origens de eventos de vista](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources.png)

1. Clique em **Adicionar**.

    ![Adicionar a origem de evento](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources-add.png)

1. Para configurar o seu IoT hub como uma nova origem de evento, utilize os valores na tabela seguinte:

    | Definição | Valor |
    | ------- | ----- |
    | Nome da origem de evento | Captura de ecrã seguinte utiliza o nome **contosorm-iot-hub**. Utilize o seu próprio nome exclusivo quando concluir este passo. |
    | Origem | **Hub IoT** |
    | Opção de Importar | **Utilizar o IoT Hub a partir de subscrições disponíveis** |
    | ID da subscrição | Selecione a sua subscrição do Azure na lista pendente. |
    | Nome do hub IOT | **contosorma57a6**. Utilize o nome do seu IoT hub a partir da sua solução de monitorização remota. |
    | Nome da política do IOT hub | **iothubowner** Certifique-se de que a política utilizada é uma política de proprietário. |
    | Chave de política do IOT hub | Este campo é preenchido automaticamente. |
    | Grupo de consumidores do hub IOT | **timeseriesinsights** |
    | Formato de serialização de eventos | **JSON**     | Nome da propriedade Timestamp | Deixar em branco |

    ![Criar origem de evento](./media/iot-accelerators-time-series-insights/time-series-insights-event-source-create.png)

1. Clique em **Criar**.

> [!NOTE]
> Se precisar de conceder acesso de utilizadores adicionais para o Explorador de informações de séries de tempo, pode utilizar estes passos para [conceder acesso a dados](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-data-access#grant-data-access).

## <a name="time-series-insights-explorer"></a>Explorador do Time Series Insights

O Explorador de informações de séries de tempo é uma aplicação web que ajuda a criar visualizações de dados.

1. Selecione o **descrição geral** separador.

1. Clique em **vá para o ambiente**, que irá abrir a aplicação de web do Explorador de informações de séries de tempo.

    ![Explorador do Time Series Insights](./media/iot-accelerators-time-series-insights/time-series-insights-environment.png)

1. No painel de seleção de tempo, selecione **últimas 12 horas** de rápido vezes menu e **pesquisa**.

    ![Série de tempo Insights Explorer pesquisa](./media/iot-accelerators-time-series-insights/time-series-insights-search-time.png)

1. No painel de termos de licenciamento no lado esquerdo, selecione um valor de medida de **temperatura** e um divisão por valor **iothub-ligação-dispositivo-id**.

    ![Série de tempo Insights Explorer consulta](./media/iot-accelerators-time-series-insights/time-series-insights-query1.png)

1. Faça duplo clique no gráfico e selecione **explorar eventos**.

    ![Eventos de Explorador de informações de séries de tempo](./media/iot-accelerators-time-series-insights/time-series-insights-explore-events.png)

1. Os eventos são apresentados na grelha em formato tabular.

    ![Tabela de Explorador de informações de séries de tempo](./media/iot-accelerators-time-series-insights/time-series-insights-table.png)

1. Clique no botão de vista de perspetiva.

    ![Perspetiva de Explorador de informações de séries de tempo](./media/iot-accelerators-time-series-insights/time-series-insights-explorer-perspective.png)

1. Clique em **adicionar** para criar uma nova consulta na perspetiva.

    ![Explorador de Insights de séries de tempo adicionar a consulta](./media/iot-accelerators-time-series-insights/time-series-insights-new-query.png)

1. Selecione um tempo rápido de **últimas 12 horas**, uma medida de **humidade** e uma divisão de **iothub-ligação-dispositivo-id**.

    ![Série de tempo Insights Explorer consulta](./media/iot-accelerators-time-series-insights/time-series-insights-query2.png)

1. Clique no botão de vista de perspetiva para ver o dashboard de métricas de dispositivo.

    ![Dashboard de Explorador de informações de séries de tempo](./media/iot-accelerators-time-series-insights/time-series-insights-dashboard.png)

## <a name="next-steps"></a>Próximos Passos

Para saber mais sobre como a explorar e consultar dados no Explorador de informações de séries de tempo, consulte o artigo [Explorador de informações de séries de tempo de Azure](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
