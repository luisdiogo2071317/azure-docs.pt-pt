---
title: Funcionalidades de pedido de dados de cliente
author: dominicbetts
ms.author: dobett
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-dps
ms.openlocfilehash: a9115ee9c49ea069999393a6430b010addf672de
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="summary-of-customer-data-request-features"></a>Resumo das funcionalidades de pedido de dados de cliente

IoT Hub do Azure é um serviço de nuvem baseada em REST API direcionado para os clientes empresariais que permite a comunicação segura, bidirecionais entre milhões de dispositivos e um serviço de Azure particionado.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Os dispositivos individuais são atribuídos um identificador de dispositivo (ID de dispositivo) por um administrador inquilino. Dados de dispositivo baseiam o ID do dispositivo atribuído. A Microsoft não mantém nenhuma informação e não tem acesso aos dados que lhe permita ID de dispositivo a correlação de utilizador.

Muitos dos dispositivos geridos no IoT Hub do Azure não são dispositivos pessoais, por exemplo um robot de thermostat ou definições de fábrica do office. Os clientes podem, no entanto, considere alguns dos dispositivos pessoais e ao seu critério podem manter os seus próprios asset ou inventário controlo métodos associar dispositivos para indivíduos. IoT Hub do Azure gere e armazena todos os dados associados a dispositivos como se fosse dados pessoais.

Os administradores inquilinos podem utilizar o portal do Azure ou APIs de REST do serviço para satisfazer os pedidos de informações ao exportar ou a eliminação de dados associados a um ID de dispositivo.

Se utilizar a funcionalidade de encaminhamento do serviço IoT Hub do Azure para reencaminhar mensagens do dispositivo para outros serviços, pedidos de dados devem ser executados pelo administrador de inquilino para cada ponto final de encaminhamento para concluir um pedido completo para um determinado dispositivo. Consulte a documentação de referência de cada ponto final para obter mais detalhes. Para obter mais informações sobre os pontos finais suportados, consulte [referência - pontos finais de IoT Hub](iot-hub-devguide-endpoints.md).

Se utilizar a funcionalidade de integração da grelha de eventos do Azure do serviço IoT Hub do Azure, pedidos de dados devem ser executados pelo administrador de inquilino para cada subscritor destes eventos. Para obter mais informações, consulte [reagir a eventos de IoT Hub, utilizando a grelha de evento](iot-hub-event-grid.md).

Se utilizar a funcionalidade de integração do Azure Monitor do serviço IoT Hub do Azure para criar registos de diagnóstico, pedidos de dados devem ser executados pelo administrador inquilino contra os registos armazenados. Para obter mais informações, consulte [monitorizar o estado de funcionamento do Azure IoT Hub](iot-hub-monitor-resource-health.md).

## <a name="deleting-customer-data"></a>Eliminar dados de cliente

Os administradores de inquilinos podem utilizar o painel de dispositivos de IoT da extensão do IoT Hub do Azure no portal do Azure para eliminar um dispositivo, o que elimina os dados associados esse dispositivo.

Também é possível efetuar operações de eliminação de dispositivos através de REST APIs. Para obter mais informações, consulte [Api do dispositivo - eliminar dispositivo](https://docs.microsoft.com/rest/api/iothub/deviceapi/deletedevice).

## <a name="exporting-customer-data"></a>Exportar dados de cliente

Os administradores inquilinos podem utilizar copiar e colar no painel de dispositivos de IoT de extensão do IoT Hub do Azure no portal do Azure para exportar os dados associados a um dispositivo.

Também é possível efetuar operações de exportação para dispositivos com as APIs REST. Para obter mais informações, consulte [Api do dispositivo - obter dispositivo](https://docs.microsoft.com/rest/api/iothub/deviceapi/getdevice).

> [!NOTE]
> Quando utilizar os serviços empresariais da Microsoft, a Microsoft gera algumas informações, conhecidas como registos gerados pelo sistema. Alguns registos do serviço de aprovisionamento de dispositivos do Azure IoT Hub gerado pelo sistema não são acessíveis ou exportável pelos administradores do inquilino. Estes registos constituem factual ações a realização de um serviço e dados de diagnóstico relacionadas com a dispositivos individuais.

## <a name="links-to-additional-documentation"></a>Ligações para documentação adicional

Documentação completa para as APIs do Azure IoT Hub dispositivo está localizada em [ https://docs.microsoft.com/rest/api/iothub/deviceapi ](https://docs.microsoft.com/rest/api/iothub/deviceapi).