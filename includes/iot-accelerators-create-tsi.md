---
title: incluir ficheiro
description: incluir ficheiro
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/20/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 8c114ed137089e70899e601ebdc1d4d39f562601
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/01/2018
ms.locfileid: "43383161"
---
## <a name="create-a-consumer-group"></a>Criar um grupo de consumidores

Terá de criar um grupo de consumidores dedicado no seu hub IoT para a telemetria de fluxo para o Time Series Insights. Uma origem de evento no Time Series Insights deve ter a utilização exclusiva de um grupo de consumidores do IoT Hub.

Os passos seguintes utilizam a CLI do Azure no Azure Cloud Shell para criar o grupo de consumidores:

1. O hub IoT é um dos vários recursos que criou quando implementou o acelerador de solução de simulação do dispositivo. Executar a encontrar o seguinte comando o nome do IoT hub - não se esqueça de utilizar o nome do seu acelerador de solução:

    ```azurecli-interactive
    az resource list --resource-group contoso-simulation -o table
    ```

    O hub IoT é o tipo de recurso **iothubs**.

1. Adicionar um grupo de consumidores chamado **devicesimulationtsi** para o hub. No comando seguinte utiliza o nome do seu acelerador de solução e hub:

    ```azurecli-interactive
    az iot hub consumer-group create --hub-name contoso-simulation7d894 --name devicesimulationtsi --resource-group contoso-simulation
    ```

    Agora pode fechar o Azure Cloud Shell.

## <a name="create-a-new-time-series-insights-environment"></a>Criar um novo ambiente do Time Series Insights

[O Azure Time Series Insights](../articles/time-series-insights/time-series-insights-overview.md) é um serviço de análise, armazenamento e visualização totalmente gerido para o gerenciamento de dados de séries de tempo de escala de IoT na cloud. Para criar um novo ambiente do Time Series Insights:

1. Inicie sessão no [portal do Azure](http://portal.azure.com/).

1. Selecione **criar um recurso** > **Internet das coisas** > **Time Series Insights**:

    ![O Time Series Insights novo](./media/iot-accelerators-create-tsi/new-time-series-insights.png)

1. Para criar o seu ambiente do Time Series Insights no mesmo grupo de recursos do solution accelerator, utilize os valores na tabela a seguir:

    | Definição | Valor |
    | ------- | ----- |
    | Nome do ambiente | Captura de ecrã seguinte utiliza o nome **Contoso-TSI**. Escolha o seu próprio nome exclusivo quando concluir este passo. |
    | Subscrição | Selecione a sua subscrição do Azure na lista pendente. |
    | Grupo de recursos | **simulação de contoso**. Utilize o nome do seu solution accelerator. |
    | Localização | Este exemplo utiliza **E.U.A. Leste**. Crie o seu ambiente na mesma região que o seu acelerador de simulação do dispositivo. |
    | Sku |**S1** |
    | Capacidade | **1** |

    ![Criar o Time Series Insights](./media/iot-accelerators-create-tsi/new-time-series-insights-create.png)

    > [!NOTE]
    > Adicionar o Time Series Insights o ambiente para o mesmo grupo de recursos como o solution accelerator significa que este é eliminado quando elimina o solution accelerator.

1. Clique em **Criar**. Pode demorar alguns minutos para o ambiente ser criado.

## <a name="create-event-source"></a>Crie a origem de eventos

Crie uma nova origem de evento para ligar ao seu hub IoT. Utilize o grupo de consumidores que criou nos passos anteriores. Uma fonte de eventos do Time Series Insights requer um grupo de consumidores dedicado não está em utilização por outro serviço.

1. No portal do Azure, navegue para o seu novo ambiente de Time Series.

1. No lado esquerdo, clique em **origens de eventos**:

    ![Visualizar origens de eventos](./media/iot-accelerators-create-tsi/time-series-insights-event-sources.png)

1. Clique em **adicionar**:

    ![Adicionar origem de eventos](./media/iot-accelerators-create-tsi/time-series-insights-event-sources-add.png)

1. Para configurar o seu hub IoT como uma nova origem de evento, utilize os valores na tabela a seguir:

    | Definição | Valor |
    | ------- | ----- |
    | Nome da origem de evento | Captura de ecrã seguinte utiliza o nome **contoso-iot-hub**. Utilize o seu próprio nome exclusivo quando concluir este passo. |
    | Origem | **Hub IoT** |
    | Opção de Importar | **Utilize o IoT Hub a partir de subscrições disponíveis** |
    | ID da subscrição | Selecione a sua subscrição do Azure na lista pendente. |
    | Nome do hub IOT | **Contoso-simulation7d894**. Utilize o nome do hub IoT partir do solution accelerator de simulação do dispositivo. |
    | Nome de política do hub IOT | **iothubowner** |
    | Chave de política do hub IOT | Este campo é preenchido automaticamente. |
    | Grupo de consumidores do hub IOT | **devicesimulationtsi** |
    | Formato de serialização de eventos | **JSON** |
    | Nome da propriedade Timestamp | Deixar em branco |

    ![Criar origem de evento](./media/iot-accelerators-create-tsi/time-series-insights-event-source-create.png)

1. Clique em **Criar**.

> [!NOTE]
> Pode [conceder acesso a utilizadores adicionais](../articles/time-series-insights/time-series-insights-data-access.md#grant-data-access) para o Explorador do Time Series Insights.