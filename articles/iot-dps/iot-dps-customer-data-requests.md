---
title: Recursos de pedido de dados do cliente
author: dominicbetts
ms.author: dobett
manager: timlt
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: d6355926c8fac62b01c36d28265842b1233ce213
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38666948"
---
# <a name="summary-of-customer-data-request-features"></a>Resumo das funcionalidades de pedido de dados do cliente

O serviço de aprovisionamento de dispositivos do Azure IoT Hub é um serviço de cloud baseadas em REST API destinado a clientes empresariais que permite o transparente e automática o zero touch aprovisionamento de dispositivos hub IoT do Azure com segurança que começa no dispositivo e termina com a cloud.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Dispositivos individuais são atribuídos um registo de ID e o ID de dispositivo por um administrador de inquilino. Dados de e sobre estes dispositivos baseia-se estes IDs. A Microsoft não mantém nenhuma informação e não tem acesso aos dados que permitam a correlação desses dispositivos para um indivíduo.

Muitos dos dispositivos geridos no serviço aprovisionamento de dispositivos não são dispositivos pessoais, por exemplo um robô de termóstato ou de fábrica do office. Os clientes podem, no entanto, a considerar alguns dos dispositivos de identificação pessoal e a seu critério podem manter seus próprios recursos ou métodos que associe os dispositivos aos indivíduos de controle de inventário. Serviço de aprovisionamento de dispositivo gerencia e armazena todos os dados associados a dispositivos como se fosse dados pessoais.

Os administradores de inquilinos, podem utilizar o portal do Azure ou APIs de REST do serviço para satisfazer os pedidos de informações através da exportação ou a eliminação de dados associados a um ID de dispositivo ou o ID de registo.

> [!NOTE]
> Dispositivos que tiverem sido aprovisionados no IoT Hub do Azure através do serviço aprovisionamento de dispositivos têm dados adicionais, armazenados no serviço do IoT Hub do Azure. Consulte a [documentação de referência do IoT Hub do Azure](../iot-hub/iot-hub-customer-data-requests.md) para concluir um pedido completo para um determinado dispositivo.

## <a name="deleting-customer-data"></a>A eliminação de dados do cliente

Serviço de aprovisionamento de dispositivo armazena inscrições e registos de registo. Inscrições contêm informações sobre os dispositivos que têm permissão para ser aprovisionado e registo registos mostram que dispositivos já passaram pelo processo de aprovisionamento.

Os administradores de inquilinos podem remover inscrições do portal do Azure, e esta ação remove quaisquer registos de registo associadas também.

Para obter mais informações, consulte [como gerir inscrições de dispositivos](how-to-manage-enrollments.md).

Também é possível realizar operações de exclusão para inscrições e registos de registo utilizando REST APIs:

* Para eliminar informações de inscrição de um dispositivo individual, pode utilizar [inscrição de dispositivos – eliminar](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollment/delete).
* Para eliminar informações de inscrição para um grupo de dispositivos, pode utilizar [grupo de inscrição de dispositivos - Delete](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollmentgroup/delete).
* Para eliminar informações sobre os dispositivos que foram aprovisionados, pode usar [estado do registo - estado de registo de eliminação](https://docs.microsoft.com/rest/api/iot-dps/registrationstate/deleteregistrationstate).

## <a name="exporting-customer-data"></a>Exportar dados do cliente

Serviço de aprovisionamento de dispositivo armazena inscrições e registos de registo. Inscrições contêm informações sobre os dispositivos que têm permissão para ser aprovisionado e registo registos mostram que dispositivos já passaram pelo processo de aprovisionamento.

Os administradores de inquilinos podem ver inscrições e registos de registo através do portal do Azure e exportá-los usando o copiar e colar.

Para obter mais informações sobre como gerir inscrições, consulte [como gerir inscrições de dispositivos](how-to-manage-enrollments.md).

Também é possível realizar operações de exportação para inscrições e registos de registo utilizando REST APIs:

* Para exportar as informações de inscrição de um dispositivo individual, pode utilizar [inscrição de dispositivos – Get](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollment/get).
* Para exportar as informações de inscrição para um grupo de dispositivos, pode utilizar [grupo de inscrição de dispositivos - Get](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollmentgroup/get).
* Para exportar as informações sobre os dispositivos que já tiverem sido aprovisionadas, pode usar [estado do registo - estado de registo de Get](https://docs.microsoft.com/rest/api/iot-dps/registrationstate/getregistrationstate).

> [!NOTE]
> Quando utiliza os serviços empresariais da Microsoft, a Microsoft gera algumas informações, conhecidas como registos gerados pelo sistema. Alguns registos gerados pelo sistema do serviço aprovisionamento de dispositivos não são acessíveis ou exportável pelos administradores do inquilino. Estes registos constituem ações factuais realizadas dentro do serviço e os dados de diagnóstico relacionadas com dispositivos individuais.

## <a name="links-to-additional-documentation"></a>Ligações para documentação adicional

Documentação completa para APIs de serviço de aprovisionamento de dispositivo está localizada em [ https://docs.microsoft.com/rest/api/iot-dps ](https://docs.microsoft.com/rest/api/iot-dps).

O IoT Hub do Azure [os dados do cliente solicitar recursos](../iot-hub/iot-hub-customer-data-requests.md).