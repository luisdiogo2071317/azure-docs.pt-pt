---
title: Recursos de pedido de dados do cliente
description: Resumo das funcionalidades de pedido de dados do cliente
author: dominicbetts
ms.author: dobett
manager: timlt
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: c0197b8f0cceab575aa5e830d094ddd2fb6b6b01
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436300"
---
# <a name="summary-of-customer-data-request-features"></a>Resumo das funcionalidades de pedido de dados do cliente

IoT Hub do Azure é um serviço de cloud baseadas em REST API destinado a clientes empresariais que permite a comunicação segura e bidirecionais entre milhões de dispositivos e um serviço do Azure particionado.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Dispositivos individuais são atribuídos um identificador de dispositivo (ID de dispositivo) por um administrador de inquilino. Dados do dispositivo são baseados no ID do dispositivo atribuído. A Microsoft não mantém nenhuma informação e não tem acesso aos dados que permitiria que o ID de dispositivo a correlação de utilizador.

Muitos dos dispositivos geridos no IoT Hub do Azure não são dispositivos pessoais, por exemplo um robô de termóstato ou de fábrica do office. Os clientes podem, no entanto, a considerar alguns dos dispositivos de identificação pessoal e a seu critério podem manter seus próprios recursos ou métodos que associe os dispositivos aos indivíduos de controle de inventário. O IoT Hub do Azure gere e armazena todos os dados associados a dispositivos como se fosse dados pessoais.

Os administradores de inquilinos, podem utilizar o portal do Azure ou APIs de REST do serviço para satisfazer os pedidos de informações por exportar ou eliminar os dados associados a um ID de dispositivo.

Se utilizar a funcionalidade de encaminhamento do serviço IoT Hub do Azure para encaminhar mensagens de dispositivo a outros serviços, pedidos de dados devem ser executados pelo administrador inquilino para cada ponto de extremidade de encaminhamento para concluir um pedido completo para um determinado dispositivo. Consulte a documentação de referência de cada ponto de extremidade para obter mais detalhes. Para obter mais informações sobre pontos finais suportados, consulte [referência - pontos finais do IoT Hub](iot-hub-devguide-endpoints.md).

Se utilizar a funcionalidade de integração do Azure Event Grid do serviço IoT Hub do Azure, os pedidos de dados devem ser executados pelo administrador inquilino para cada assinante desses eventos. Para obter mais informações, consulte [reagir a eventos do IoT Hub com o Event Grid](iot-hub-event-grid.md).

Se utilizar a funcionalidade de integração do Azure Monitor do serviço IoT Hub do Azure para criar registos de diagnóstico, pedidos de dados devem ser executados pelo administrador inquilino com os registos armazenados. Para obter mais informações, consulte [monitorizar o estado de funcionamento do IoT Hub do Azure](iot-hub-monitor-resource-health.md).

## <a name="deleting-customer-data"></a>A eliminação de dados do cliente

Os administradores de inquilinos podem utilizar o painel de dispositivos de IoT da extensão do IoT Hub do Azure no portal do Azure para eliminar um dispositivo, o que elimina os dados associados esse dispositivo.

Também é possível realizar operações de exclusão para dispositivos com as APIs REST. Para obter mais informações, consulte [serviço - eliminar dispositivo](https://docs.microsoft.com/rest/api/iothub/service/deletedevice).

## <a name="exporting-customer-data"></a>Exportar dados do cliente

Os administradores de inquilinos podem utilizar a cópia e cole no painel de dispositivos de IoT da extensão do IoT Hub do Azure no portal do Azure para exportar os dados associados a um dispositivo.

Também é possível realizar operações de exportação para dispositivos com as APIs REST. Para obter mais informações, consulte [Service - obter dispositivo](https://docs.microsoft.com/rest/api/iothub/service/getdevice).

> [!NOTE]
> Quando utiliza os serviços empresariais da Microsoft, a Microsoft gera algumas informações, conhecidas como registos gerados pelo sistema. Alguns registos gerados pelo sistema do IoT Hub do Azure não são acessíveis ou exportável pelos administradores do inquilino. Estes registos constituem ações factuais realizadas dentro do serviço e os dados de diagnóstico relacionadas com dispositivos individuais.

## <a name="links-to-additional-documentation"></a>Ligações para documentação adicional

Documentação completa para APIs de serviço do Azure IoT Hub está localizada em [ https://docs.microsoft.com/rest/api/iothub/service ](https://docs.microsoft.com/rest/api/iothub/service).