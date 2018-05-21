---
title: Funcionalidades de pedido de dados de cliente
author: dominicbetts
ms.author: dobett
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-dps
ms.openlocfilehash: 28cadac33c4e73e6158f878b3c79ff09b4765fff
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="summary-of-customer-data-request-features"></a>Resumo das funcionalidades de pedido de dados de cliente

O serviço de aprovisionamento de dispositivos do Azure IoT Hub é um serviço de nuvem baseada em REST API direcionado para os clientes empresariais que lhe permite totalmente integrada, automatizada zero touch aprovisionamento de dispositivos IoT Hub do Azure com segurança, que começa no dispositivo e termina com a nuvem.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Os dispositivos individuais são atribuídos um registo ID e o ID de dispositivo por um administrador inquilino. Baseia-se nestes IDs de dados a partir e sobre estes dispositivos. A Microsoft não mantém nenhuma informação e não tem acesso aos dados que lhe permita correlação destes dispositivos, até um indivíduo.

Muitos dos dispositivos geridos no serviço de aprovisionamento de dispositivos não são dispositivos pessoais, por exemplo um robot de thermostat ou definições de fábrica do office. Os clientes podem, no entanto, considere alguns dos dispositivos pessoais e ao seu critério podem manter os seus próprios asset ou inventário controlo métodos associar dispositivos para indivíduos. Serviço de aprovisionamento de dispositivos gere e armazena todos os dados associados a dispositivos como se fosse dados pessoais.

Os administradores inquilinos podem utilizar o portal do Azure ou APIs de REST do serviço para satisfazer os pedidos de informações através da exportação ou eliminar dados associados a um ID de dispositivo ou o ID de registo.

> [!NOTE]
> Os dispositivos que foram aprovisionados através do serviço de aprovisionamento de dispositivos do IoT Hub do Azure têm dados adicionais armazenados no serviço do IoT Hub do Azure. Consulte o [documentação de referência do IoT Hub do Azure](../iot-hub/iot-hub-customer-data-requests.md) para concluir um pedido completo para um determinado dispositivo.

## <a name="deleting-customer-data"></a>Eliminar dados de cliente

Serviço de aprovisionamento de dispositivos armazena as inscrições e registos de registo. As inscrições contêm informações sobre os dispositivos que estão autorizados a ser aprovisionado e mostrar de registos de registo que dispositivos já realizou o processo de aprovisionamento.

Os administradores inquilinos poderão remover inscrições através do portal do Azure e esta ação remove quaisquer registos, bem como de registo associado.

Para obter mais informações, consulte [como gerir inscrições de dispositivos](how-to-manage-enrollments.md).

Também é possível efetuar operações de eliminação de inscrições e registos de registo utilizando REST APIs:

* Para eliminar informações de registo para um único dispositivo, pode utilizar [inscrição de dispositivos - eliminar](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollment/delete).
* Para eliminar informações de inscrição de um grupo de dispositivos, pode utilizar [grupo de inscrição de dispositivos - eliminar](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollmentgroup/delete).
* Para eliminar informações sobre os dispositivos que foram aprovisionados, pode utilizar [estado de registo - estado de registo de eliminação](https://docs.microsoft.com/rest/api/iot-dps/registrationstate/deleteregistrationstate).

## <a name="exporting-customer-data"></a>Exportar dados de cliente

Serviço de aprovisionamento de dispositivos armazena as inscrições e registos de registo. As inscrições contêm informações sobre os dispositivos que estão autorizados a ser aprovisionado e mostrar de registos de registo que dispositivos já realizou o processo de aprovisionamento.

Os administradores inquilinos podem ver as inscrições e registos de registo através do portal do Azure e exportá-las através de copiar e colar.

Para obter mais informações sobre como gerir as inscrições, consulte [como gerir inscrições de dispositivos](how-to-manage-enrollments.md).

Também é possível executar operações de exportação para as inscrições e registos de registo utilizando REST APIs:

* Para exportar as informações de registo para um único dispositivo, pode utilizar [inscrição de dispositivos - Get](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollment/get).
* Para exportar as informações de inscrição de um grupo de dispositivos, pode utilizar [grupo de inscrição de dispositivos - Get](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollmentgroup/get).
* Para exportar as informações sobre os dispositivos que já foram aprovisionados, pode utilizar [estado de registo - estado de registo Get](https://docs.microsoft.com/rest/api/iot-dps/registrationstate/getregistrationstate).

> [!NOTE]
> Quando utilizar os serviços empresariais da Microsoft, a Microsoft gera algumas informações, conhecidas como registos gerados pelo sistema. Alguns registos gerados pelo sistema do serviço de aprovisionamento de dispositivos não são acessíveis ou exportável pelos administradores do inquilino. Estes registos constituem factual ações a realização de um serviço e dados de diagnóstico relacionadas com a dispositivos individuais.

## <a name="links-to-additional-documentation"></a>Ligações para documentação adicional

Documentação completa para APIs de serviço de aprovisionamento de dispositivo está localizada em [ https://docs.microsoft.com/rest/api/iot-dps ](https://docs.microsoft.com/rest/api/iot-dps).

IoT Hub do Azure [funcionalidades de pedido de dados de cliente](../iot-hub/iot-hub-customer-data-requests.md).