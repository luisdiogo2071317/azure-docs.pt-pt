---
title: Limites do serviço de pré-visualização do Azure duplos Digital pública | Documentos da Microsoft
description: Limites do serviço de pré-visualização de compreensão Azure Digital gémeos de público
author: dwalthermsft
manager: deshner
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: dwalthermsft
ms.openlocfilehash: aa5f6053bf1c98d2b84c02617da30f5d856ed3fc
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324186"
---
# <a name="public-preview-service-limits"></a>Limites do serviço de pré-visualização pública

Durante a pré-visualização pública, duplos Digital do Azure terão subscrição temporária, a instância e limites de velocidade estão descritos abaixo.

Essas restrições existem para o ajudar a simplificar a aprendizagem sobre o novo serviço e seus inúmeros recursos.

> [!NOTE]
> Estes limites vão ser aumentados e/ou removidos por disponibilidade geral (GA).

## <a name="per-subscription-limits"></a>Limites de por subscrição

Durante a pré-visualização pública, cada subscrição do Azure pode criar ou ter executado exatamente uma instância de duplos Digital do Azure de cada vez.

> [!TIP]
> Eliminar a instância do permitirá que crie um novo.

## <a name="per-instance-limits"></a>Limites de por instância

Por sua vez, cada instância de duplos Digital do Azure pode ter:

- Um `IoTHub` recursos
- Um `EventHub` ponto final para o tipo de evento DeviceMessage
- Até três `EventHub`, `ServiceBus`, ou `EventGrid` pontos de extremidade de tipo de evento `SensorChange`, `SpaceChange`, `TopologyOperation`, ou `UdfCustom`

## <a name="management-api-limits"></a>Limites de API de gestão

Os limites de velocidade do pedido para a API de gestão são:

- 100 pedidos por segundo para gestão de API
- Uma única consulta de API de gestão pode retornar até 1000 objetos

> [!IMPORTANT]
> Se ultrapassar o limite de objeto de 1000, receberá um erro e terá de simplificar a sua consulta.

## <a name="udf-rate-limits"></a>Limites de velocidade UDF

Os seguintes limites definir o número total de todas as chamadas de função definida pelo utilizador feita à sua instância de duplos Digital do Azure:

- 400 chamadas de biblioteca de cliente por segundo
- 100 SendNotification chamadas por segundo

> [!NOTE]
> As seguintes ações podem fazer com que os limites de velocidade adicionais a serem aplicadas temporariamente:
> - Edições de metadados do objeto de topologia
> - Atualizações de definição de UDF
> - Dispositivos a enviar telemetria pela primeira vez

## <a name="device-telemetry-limits"></a>Limites de telemetria do dispositivo

Os limites abaixo do limite o número total de mensagens de todos os seus dispositivos podem enviar para a sua instância de duplos Digital do Azure:

- 100 mensagens por segundo

## <a name="next-steps"></a>Passos Seguintes

Para experimentar um exemplo de duplos Digital do Azure, aceda a [guia de introdução de salas disponíveis](./quickstart-view-occupancy-dotnet.md).