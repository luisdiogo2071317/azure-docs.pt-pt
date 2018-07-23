---
title: Visualizar dados com o Azure Time Series Insights de monitorização remota | Documentos da Microsoft
description: Saiba como configurar o ambiente do Time Series Insights para explorar e analisar os dados de séries de tempo da sua solução de monitorização remota.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 10617c129212d8196897af750c02647f0086c8e5
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185895"
---
# <a name="visualize-remote-monitoring-data-with-time-series-insights"></a>Visualizar dados de monitorização remotos com o Time Series Insights

Um operador pode desejar estender ainda mais o fora dos dados de caixa de visualização fornecida pela monitorização remota pré-configurar uma solução. Nosso solution accelerator oferece fora a total integração com o TSI. Nesta explicação de procedimento, aprenderá como configurar o Time Series Insights para analisar a telemetria do dispositivo e detetar anomalias.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este procedimentos, terá o seguinte:

* [Implementar a solução pré-configurada de monitorização remota](quickstart-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Criar um grupo de consumidores

Terá de criar um grupo de consumidores dedicado no seu hub IoT para ser utilizado para transmissão em fluxo de dados para o Time Series Insights.

> [!NOTE]
> Grupos de consumidores são utilizados por aplicações para obter dados do IoT Hub do Azure. Cada grupo de consumidores permite que até cinco consumidores de saída. Deve criar um novo grupo de consumidores para cada cinco sinks de saída e pode criar até 32 grupos de consumidores.

1. No portal do Azure, clique no botão do Cloud Shell.

1. Execute o seguinte comando para criar um novo grupo de consumidores:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="create-a-new-time-series-insights-environment"></a>Criar um novo ambiente do Time Series Insights

O Azure Time Series Insights é um serviço totalmente gerido de análise, armazenamento e visualização para a gestão de dados de séries temporais à escala de IoT na cloud. Este serviço fornece armazenamento de dados de séries temporais dimensionável e permite-lhe explorar e analisar milhares de milhões de eventos transmitidos em fluxo de todo o mundo numa questão de segundos. Utilize o Time Series Insights para armazenar e gerir terabytes de dados de séries temporais, explorar e visualizar milhares de milhões de eventos em simultâneo, realizar análises de causa raiz e comparar vários sites e recursos.

1. Inicie sessão no [portal do Azure](http://portal.azure.com/).

1. Selecione **criar um recurso** > **Internet das coisas** > **Time Series Insights**.

    ![O Time Series Insights novo](./media/iot-accelerators-time-series-insights/new-time-series-insights.png)

1. Para criar o seu ambiente do Time Series Insights, utilize os valores na tabela a seguir:

    | Definição | Valor |
    | ------- | ----- |
    | Nome do ambiente | Captura de ecrã seguinte utiliza o nome **contorosrmtsi**. Escolha o seu próprio nome exclusivo quando concluir este passo. |
    | Subscrição | Selecione a sua subscrição do Azure na lista pendente. |
    | Grupo de recursos | **Criar um novo**. Estamos a utilizar o nome **ContosoRM**. |
    | Localização | Estamos a utilizar **E.U.A. Leste**. Crie o seu ambiente na mesma região que a sua solução de monitorização remota. |
    | Sku |**S1** |
    | Capacidade | **1** |
    | Afixar ao dashboard | **Sim** |

    ![Criar o Time Series Insights](./media/iot-accelerators-time-series-insights/new-time-series-insights-create.png)

1. Clique em **Criar**. Pode demorar um momento para o ambiente ser criado.

## <a name="create-event-source"></a>Criar origem de evento

Crie uma nova origem de evento para ligar ao seu hub IoT. Certifique-se de que utilize o grupo de consumidores, criado nos passos anteriores. O Time Series Insights exige que cada serviço tem um grupo de consumidores dedicado não está em utilização por outro serviço.

1. Navegue para o seu novo ambiente de Time Series.

1. No lado esquerdo, selecione **origens de eventos**.

    ![Visualizar origens de eventos](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources.png)

1. Clique em **Adicionar**.

    ![Adicionar origem de eventos](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources-add.png)

1. Para configurar o seu hub IoT como uma nova origem de evento, utilize os valores na tabela a seguir:

    | Definição | Valor |
    | ------- | ----- |
    | Nome da origem de evento | Captura de ecrã seguinte utiliza o nome **contosorm-iot-hub**. Utilize o seu próprio nome exclusivo quando concluir este passo. |
    | Origem | **Hub IoT** |
    | Opção de Importar | **Utilize o IoT Hub a partir de subscrições disponíveis** |
    | ID da subscrição | Selecione a sua subscrição do Azure na lista pendente. |
    | Nome do hub IOT | **contosorma57a6**. Utilize o nome do hub IoT partir da sua solução de monitorização remota. |
    | Nome de política do hub IOT | **iothubowner** Certifique-se de que a diretiva usada é uma política de proprietário. |
    | Chave de política do hub IOT | Este campo é preenchido automaticamente. |
    | Grupo de consumidores do hub IOT | **timeseriesinsights** |
    | Formato de serialização de eventos | **JSON**     | Nome da propriedade Timestamp | Deixar em branco |

    ![Criar origem de evento](./media/iot-accelerators-time-series-insights/time-series-insights-event-source-create.png)

1. Clique em **Criar**.

> [!NOTE]
> Se precisar de conceder acesso de utilizadores adicionais para o Explorador do Time Series Insights, pode utilizar estes passos para [conceder acesso a dados](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access#grant-data-access).

## <a name="time-series-insights-explorer"></a>Explorador do Time Series Insights

O Explorador do Time Series Insights é uma aplicação web que ajuda a criar visualizações dos dados.

1. Selecione o **descrição geral** separador.

1. Clique em **vá para o ambiente**, que irá abrir a aplicação de web de Explorador do Time Series Insights.

    ![Explorador do Time Series Insights](./media/iot-accelerators-time-series-insights/time-series-insights-environment.png)

1. No painel de seleção de tempo, selecione **últimas 12 horas** partir rápido vezes menu e clique em **pesquisa**.

    ![Pesquisa de Explorador de informações de série de tempo](./media/iot-accelerators-time-series-insights/time-series-insights-search-time.png)

1. No painel de termos no lado esquerdo, selecione um valor de medida de **temperatura** e um valor de divisão por de **iothub-ligação-dispositivo-id**.

    ![Consulta de Explorador de informações de série de tempo](./media/iot-accelerators-time-series-insights/time-series-insights-query1.png)

1. Faça duplo clique no gráfico e selecione **explorar eventos**.

    ![Eventos de Explorer de informações de série de tempo](./media/iot-accelerators-time-series-insights/time-series-insights-explore-events.png)

1. Os eventos são compostos na grade em formato tabular.

    ![Tabela de Explorador de informações de série de tempo](./media/iot-accelerators-time-series-insights/time-series-insights-table.png)

1. Clique no botão de vista do ponto de vista.

    ![O ponto de vista do Time Series Insights Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-explorer-perspective.png)

1. Clique em **adicionar** para criar uma nova consulta na perspetiva.

    ![Explorador do Time Series Insights adicionar consulta](./media/iot-accelerators-time-series-insights/time-series-insights-new-query.png)

1. Selecione um tempo rápido de **últimas 12 horas**, uma medida de **humidade** e uma divisão por de **iothub-ligação-dispositivo-id**.

    ![Consulta de Explorador de informações de série de tempo](./media/iot-accelerators-time-series-insights/time-series-insights-query2.png)

1. Clique no botão de vista do ponto de vista para ver o dashboard das métricas de dispositivo.

    ![Dashboard de Explorador de informações de série de tempo](./media/iot-accelerators-time-series-insights/time-series-insights-dashboard.png)

## <a name="next-steps"></a>Próximos Passos

Para saber mais sobre como explorar e consultar dados no Explorador do Time Series Insights, veja [Explorador do Azure Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
