---
title: Criar um ambiente do Azure Time Series Insights (pré-visualização) | Documentos da Microsoft
description: Saiba como criar um ambiente de atualização do Azure Time Series
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 11/26/2018
ms.openlocfilehash: 7e256f032c67649a4a8b01311e70d6bc9307ae92
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52852974"
---
# <a name="provisioning-and-managing-an-azure-time-series-insights-preview-environment"></a>Aprovisionamento e gestão de um ambiente do Azure Time Series Insights (pré-visualização)

Este documento descreve como aprovisionar e gerir um novo ambiente do Azure Time Series Insights (pré-visualização) no portal do Azure.

## <a name="overview"></a>Descrição geral

Uma breve descrição sobre o aprovisionamento após a atualização do Time Series Insights:

* Aprovisione um ambiente de atualização do Azure Time Series Insights (TSI).
* Como parte do processo de criação de terá de fornecer um **ID de série de tempo**. Pode ser até **três** (3) chaves. Saiba mais sobre [IDs de série de tempo escolher](./time-series-insights-update-how-to-id.md).
* Quando aprovisiona um ambiente de atualização de Azure TSI cria dois recursos do Azure, um ambiente de atualização do TSI e uma conta do armazenamento do Azure para fins gerais V1.  
* No futuro, novos clientes do Azure por predefinição apenas poderão aprovisionar uma conta do armazenamento do Azure para fins gerais V2, por conseguinte, iremos suportá-lo quando ocorre a essa alteração.  
* Não ative a frio/arquivamento propriedades na conta de armazenamento que irá utilizar.
* Em seguida, pode ligar, opcionalmente, o ambiente de atualização para uma fonte de eventos suportados do Time Series Insights (por exemplo, o IoT Hub).
* Aqui deseja fornecer a **Timestamp ID** propriedade e forneça um grupo de consumidores exclusivo para garantir que o ambiente tem acesso a todos os seus eventos.
* Após o aprovisionamento, pode, em seguida, opcionalmente, gerir políticas de acesso e outros atributos de ambiente que suportam os requisitos de negócio.

## <a name="new-environment-creation"></a>Criação do novo ambiente

1. Selecione `PAYG` partir de **SKU** lista pendente. Também vai introduzir um nome de ambiente, designar que subscrição e grupo de recursos que pretende criar o ambiente no e selecione uma localização suportada para o ambiente estar situada em.  

1. Crie uma nova conta de armazenamento do Azure ao selecionar um nome de conta de armazenamento e designar uma opção de replicação. Se o fizer, irá criar automaticamente uma conta nova para o armazenamento do Azure para fins gerais V1 na mesma região que o ambiente do Azure TSI (pré-visualização) que selecionou anteriormente.  

1. Entrada **ID de série de tempo** propriedade:

    > [!IMPORTANT]
    > O **ID de série de tempo** diferencia maiúsculas de minúsculas imutável e não pode ser alterada uma vez definida.

    Saiba mais detalhes sobre como selecionar a adequada **ID de série de tempo** ler o artigo sobre [melhores práticas para a seleção de um ID de série de tempo](./time-series-insights-update-how-to-id.md).

    ![environment_details][1]

1. Opcionalmente, pode adicionar uma origem de evento:

    * Azure TSI suporta o IoT Hub do Azure e Hubs de eventos como opções. Embora só é possível adicionar uma origem de evento único no momento de criação do ambiente, pode adicionar uma origem de eventos adicionais mais tarde. É melhor criar um grupo de consumidores exclusivo para garantir que todos os eventos estão visíveis para o TSI. Pode selecionar um grupo de consumidor existente ou criar um novo grupo de consumidor, ao adicionar a origem do evento.

    * Também designar apropriado **Timestamp** propriedade. Por predefinição, o TSI utiliza a hora de colocados em fila de mensagens para cada origem de evento, que pode não ser adequada se estiver a criação de batches de eventos ou carregar dados históricos. Por conseguinte, é fundamental para a propriedade timestamp maiúsculas e minúsculas de entrada, ao adicionar a origem do evento.  

     ![environment_event_sources][2]

1. Reveja e criar:

    ![environment_review][3]

    Confirme que tudo esteja correto!

## <a name="management"></a>Gestão

Tem a capacidade de gerir o seu ambiente atualizado do TSI através do portal do Azure. Os utilizadores familiarizados com TSI será imediatamente confortáveis com a atualização TSI, uma vez que grande parte é transportado entre versões.

Principais diferenças na gestão de um ambiente de L1 TSI versus um S1 ou o ambiente de S2 no portal do Azure são apresentadas abaixo:

* Portal do TSI Azure *descrição geral* painel:

  * Usar o painel de descrição geral de permanece o mesmo, exceto para:

    * Capacidade é removida como esse conceito não é relevante para ambientes de L1.
    * O **ID de série de tempo** propriedade foi adicionada. Essa é uma propriedade imutável adicionado em tempo de aprovisionamento e define como os seus dados estão particionados.
    * Conjuntos de dados de referência são removidos.

* Portal do TSI Azure *configurar* painel:
  
  * Retenção é removida como retenção será definida como ilimitada.

    * Esperamos que adicionar mais controlos para isso, no futuro, mas por enquanto, não pode definir um limite sobre isso.
    * Capacidade, a Calculadora e o limite de armazenamento excederam o comportamento de todos os removido.

* Portal do TSI Azure *referência* painel de dados:

  * Este painel todo foi removido como dados de referência não são um componente de ambientes de L1.

[!INCLUDE [tsi-update-docs](../../includes/time-series-insights-update-documents.md)]

## <a name="next-steps"></a>Passos Seguintes

Leia sobre a configuração de armazenamento do TSI:

> [!div class="nextstepaction"]
> [O TSI do Azure (pré-visualização), armazenamento e de entrada](./time-series-insights-update-storage-ingress.md)

Saiba mais sobre os modelos de série de tempo:

> [!div class="nextstepaction"]
> [TSI do Azure (pré-visualização) TSM](./time-series-insights-update-tsm.md)

<!-- Images -->
[1]: media/v2-update-provision/environment_details.png
[2]: media/v2-update-provision/environment_event_sources.png
[3]: media/v2-update-provision/environment_review.png