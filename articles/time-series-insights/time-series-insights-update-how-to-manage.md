---
title: Gestão de série de tempo pré-visualização do Azure - como aprovisionar e gerir a pré-visualização de série de tempo do Azure. | Microsoft Docs
description: Noções básicas sobre como aprovisionar e gerir a pré-visualização do Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 1834a59ff675f109267a406e3c2b03411e3bb50b
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53273521"
---
# <a name="how-to-provision-and-manage-azure-time-series-insights-preview"></a>Como aprovisionar e gerir a pré-visualização do Azure Time Series Insights

Este documento descreve como criar e gerir um ambiente de pré-visualização do Azure Time Series Insights com o [portal do Azure](https://portal.azure.com/).

## <a name="overview"></a>Descrição geral

Uma breve visão geral sobre como aprovisionar um ambiente de pré-visualização do Azure Time Series Insights é descrita abaixo:

* Ambientes do Time Series Insights pré-visualização do Azure são **PAYG** ambientes.
  * Como parte do processo de criação de terá de fornecer uma série de tempo de ID. IDs de série de tempo pode ser até três chaves. Saiba mais sobre como selecionar um ID de série de tempo lendo [como escolher um ID de série de tempo](./time-series-insights-update-how-to-id.md).
  * Quando aprovisiona um ambiente de pré-visualização do Azure Time Series Insights, cria dois recursos do Azure:

    * Um ambiente de pré-visualização do Azure Time Series Insights  
    * Uma conta do armazenamento do Azure para fins gerais V1
  
    Saiba mais [como planejar seu ambiente](./time-series-insights-update-plan.md).

    >[!IMPORTANT]
    > Para os clientes com contas V2, não ative a frio/arquivamento propriedades na conta de armazenamento que irá utilizar.

* Cada ambiente de pré-visualização do Azure Time Series Insights pode ser (opcionalmente) associado uma origem de evento. Leia [como adicionar uma origem de Hub de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md) e [como adicionar uma origem do IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md) para obter mais informações.
  * Deve fornecer uma propriedade de ID de Timestamp e um grupo de consumidores exclusivo durante este passo. Dessa forma, garante que o ambiente tem acesso para os eventos adequados.

* Assim que o aprovisionamento estiver concluído, pode modificar as políticas de acesso e outros atributos de ambiente para o conjunto de seus requisitos empresariais.

## <a name="new-environment-creation"></a>Criação do novo ambiente

Os passos seguintes descrevem como criar um ambiente de pré-visualização do Azure Time Series Insights:

1. Selecione o **PAYG** botão sob os **SKU** menu. Fornecer um nome de ambiente, designar que grupos de subscrição e o grupo de recursos para utilizar. Em seguida, selecione uma localização suportada para o ambiente para serem alojadas na.

1. Introduza um ID de série de tempo

    >[!NOTE]
    > * O ID de série de tempo é maiúsculas e minúsculas e imutável (não pode ser alterado após ser definido).
    > * IDs de série de tempo pode ser até 3 chaves.
    > * Saiba mais sobre a seleção de um ID de série de tempo lendo [como escolher um ID de série de tempo](./time-series-insights-update-how-to-id.md).

1. Crie uma conta de armazenamento do Azure ao selecionar um nome de conta de armazenamento do Azure e designar uma opção de replicação. Fazer automaticamente cria uma conta do armazenamento do Azure para fins gerais V1. Será criada na mesma região que o ambiente de pré-visualização do Azure Time Series Insights que selecionou anteriormente.

    ![Crie uma instância do Azure Time Series Insights.][1]

1. Opcionalmente, pode adicionar uma origem de evento.

   * Time Series Insights suporta [IoT Hub do Azure](./time-series-insights-how-to-add-an-event-source-iothub.md) e [dos Hubs de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md) como opções. Embora só é possível adicionar uma origem de evento único no momento de criação do ambiente, pode adicionar uma origem de eventos adicionais mais tarde. É melhor criar um grupo de consumidores exclusivo para garantir que todos os eventos estão visíveis para a sua instância de pré-visualização do Azure Time Series Insights. Pode selecionar um grupo de consumidor existente ou criar um novo grupo de consumidor, ao adicionar a origem do evento.

   * Também deve designar a propriedade Timestamp apropriada. Por predefinição, o Azure Time Series Insights utiliza a hora de colocados em fila de mensagens para cada origem de evento.

     > [!TIP]
     > O tempo de colocados em fila de mensagens pode não ser a melhor definição configurada para utilizar no evento de batch ou cenários de carregamento de dados históricos. Certifique-se verificar a sua decisão de usar ou não usar uma propriedade Timestamp nestes casos.

    ![Adicione uma origem de evento para a sua instância.][2]

1. Rever e criar

    ![Adicione uma origem de evento para a sua instância.][3]

Confirme que o seu ambiente tiver sido aprovisionado com as definições pretendidas.

## <a name="management"></a>Gestão

Tem a capacidade de gerir o seu ambiente de pré-visualização do Azure Time Series Insights com o portal do Azure. Aqui estão as principais diferenças na gestão de um **PAYG** ambiente de pré-visualização do Azure Time Series Insights em vez de um ambiente de S1 ou S2 através do Portal do Azure:

* O portal do Azure *descrição geral* painel permanece inalterado no Azure Time Series Insights, exceto das seguintes formas:
  * Capacidade é removida, tal como não é relevante para este conceito **PAYG** ambientes.
  * Foi adicionada a propriedade de ID de série de tempo. Determina como os seus dados estão particionados.
  * Conjuntos de dados de referência são removidos.
  * A URL exibida lhe dá instruções para o [Explorador de pré-visualização do Azure Time Series Insights](./time-series-insights-update-explorer.md).
  * O nome da sua conta de armazenamento do Azure está listado.

* O Portal do Azure *configurar* painel foi removido na pré-visualização do Azure Time Series Insights, uma vez que existe **PAYG** ambientes não são configuráveis.

* O Portal do Azure *referência* painel de dados foi removido na pré-visualização do Azure Time Series Insights, uma vez que os dados de referência não são um componente do **PAYG** ambientes.

![Certifique-se a sua instância.][4]

## <a name="next-steps"></a>Passos Seguintes

Leia [como planejar seu ambiente](./time-series-insights-update-plan.md).

Leia [como adicionar uma origem de Hub de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md).

Leia [como adicionar uma origem do IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md).

<!-- Images -->
[1]: media/v2-update-manage/manage_one.png
[2]: media/v2-update-manage/manage_two.png
[3]: media/v2-update-manage/manage_three.png
[4]: media/v2-update-manage/manage_four.png
