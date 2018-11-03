---
title: Limites do serviço de pré-visualização do Azure duplos Digital pública | Documentos da Microsoft
description: Compreender os que limites do serviço de pré-visualização do Azure duplos Digital pública
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: dwalthermsft
ms.openlocfilehash: 86ae75118dd1311ea2ae92fb718fe4c58b8e5673
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50961760"
---
# <a name="public-preview-service-limits"></a>Limites do serviço de pré-visualização pública

Durante a pré-visualização pública, duplos Digital do Azure tem a seguinte subscrição temporária, a instância e limites de velocidade.

Essas restrições existem para o ajudar a simplificar a aprendizagem sobre o novo serviço e seus inúmeros recursos.

> [!NOTE]
> Estes limites serão aumentados ou removidos pela disponibilidade geral (GA).

## <a name="per-subscription-limits"></a>Limites de por subscrição

Durante a pré-visualização pública, cada subscrição do Azure pode criar ou executar apenas uma instância de duplos Digital do Azure de cada vez.

> [!TIP]
> Se eliminar a instância, pode criar um novo.

## <a name="per-instance-limits"></a>Limites de por instância

Por sua vez, cada instância de duplos Digital do Azure pode ter:

- Um **IoTHub** recursos.
- Um **EventHub** ponto final para o tipo de evento **DeviceMessage**.
- Até três **EventHub**, **ServiceBus**, ou **EventGrid** pontos finais do tipo de evento **SensorChange**, **SpaceChange** , **TopologyOperation**, ou **UdfCustom**.

## <a name="management-api-limits"></a>Limites de API de gestão

Os limites de velocidade do pedido para a API de gestão são:

- 100 pedidos por segundo para a API de gestão.
- Até 1000 objetos devolvidos por uma única consulta de API de gestão. 

> [!IMPORTANT]
> Se ultrapassar o limite de 1.000-object, recebe um erro e deve simplificar a sua consulta.

## <a name="udf-rate-limits"></a>Limites de velocidade UDF

Os seguintes limites definir o número total de todas as chamadas de função definida pelo utilizador feita à sua instância de duplos Digital do Azure:

- 400 chamadas de biblioteca de cliente por segundo
- 100 **SendNotification** chamadas por segundo

> [!NOTE]
> As seguintes ações podem fazer com que os limites de velocidade adicionais a serem aplicadas temporariamente:
> - Edições feitas para os metadados do objeto de topologia
> - Atualizações feitas a definição de UDF
> - Dispositivos que enviam telemetria pela primeira vez

## <a name="device-telemetry-limits"></a>Limites de telemetria do dispositivo

Os seguintes limites limitar o número total de mensagens de todos os que seus dispositivos podem enviar para a sua instância de duplos Digital do Azure:

- 100 mensagens por segundo

## <a name="next-steps"></a>Passos Seguintes

Para experimentar um exemplo de duplos Digital do Azure, aceda a [guia de introdução de salas disponíveis](./quickstart-view-occupancy-dotnet.md).