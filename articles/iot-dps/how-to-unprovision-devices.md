---
title: Como desaprovisionar dispositivos que foram aprovisionados com o serviço de aprovisionamento de dispositivos do Azure IoT Hub | Microsoft Docs
description: Como os dispositivos de desaprovisionamento que foram aprovisionados com o serviço de aprovisionamento de dispositivos do Azure IoT Hub
services: iot-dps
keywords: ''
author: bryanla
ms.author: v-jamebr;bryanla
ms.date: 04/06/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 439d4ffa8eec12481f52bd15f0060800411f316e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-deprovision-devices-that-were-previously-auto-provisioned"></a>Como desaprovisionar dispositivos que foram auto-aprovisionados anteriormente 

Pode considerar necessário para dispositivos de desaprovisionamento que foram anteriormente automaticamente aprovisionado através do serviço de aprovisionamento de dispositivos. Por exemplo, um dispositivo pode ser vendido ou movido para um hub IoT diferente, ou pode ser perda, roubo ou ficarem comprometida. 

Em geral, o desaprovisionamento um dispositivo envolve dois passos:

1. Disenroll o dispositivo a partir do seu serviço de aprovisionamento, para impedir futuro automática aprovisionamento. Dependendo se pretende revogar o acesso temporariamente ou permanentemente, poderá pretender desativar ou eliminar uma entrada de inscrição. Para dispositivos que utilizam o atestado de x. 509, pode pretender desativar/eliminar uma entrada na hierarquia de grupos de inscrição existentes.  
 
   - Para saber como disenroll um dispositivo, consulte o artigo [como disenroll um dispositivo a partir do serviço de aprovisionamento de dispositivos do Azure IoT Hub](how-to-revoke-device-access-portal.md).
   - Para saber como disenroll um dispositivo através de programação utilizando um dos SDKs de serviço aprovisionamento, consulte o artigo [gerir inscrições de dispositivos com o serviço SDKs](how-to-manage-enrollments-sdks.md).

2. Anular o registo do dispositivo a partir do seu IoT Hub, para impedir a transferência de dados e comunicações futura. Novamente, pode desativar temporariamente ou eliminar permanentemente a entrada do dispositivo no registo de identidade para o IoT Hub onde foi aprovisionado. Consulte [desativar dispositivos](/azure/iot-hub/iot-hub-devguide-identity-registry.md#disable-devices) para saber mais sobre disablement. Consulte "Dispositivo gestão / dispositivos IoT" para o recurso do IoT Hub, no [portal do Azure](https://portal.azure.com).

Os passos exatos que tomar para desaprovisionar um dispositivo dependem o mecanismo de atestado e a respetiva entrada de registo aplicável com o serviço de aprovisionamento. As secções seguintes fornecem uma descrição geral do processo, consoante o tipo de inscrição e de atestado.

## <a name="individual-enrollments"></a>Inscrições individuais
Dispositivos que utilizam o atestado de TPM ou atestado de x. 509 com um certificado de folha são aprovisionados através de uma entrada de registo individuais. 

Para desaprovisionar num dispositivo que tenha uma inscrição individuais: 

1. Disenroll o dispositivo a partir do seu serviço de aprovisionamento:

   - Para dispositivos que utilizam o atestado de TPM, elimine a entrada de registo individuais para revogar o acesso do dispositivo para o serviço de aprovisionamento, permanentemente, ou desativar a entrada temporariamente revogar o acesso. 
   - Para dispositivos que utilizam o atestado de x. 509, pode eliminar ou desativar a entrada. Tenha em atenção, no entanto, se eliminar um registo individual para um dispositivo que utiliza o x. 509 e existe um grupo de inscrição ativados para um certificado de assinatura em que a cadeia de certificados do dispositivo, o dispositivo pode inscrever novamente. Para esses dispositivos, pode ser mais segura desativar a entrada de inscrição. Ao fazê-lo, por isso, impede que o dispositivo de reinscrição, independentemente da se um grupo de inscrição ativado existe para um dos respetivos certificados de assinatura.

2. Desative ou elimine o dispositivo no registo de identidade do hub IoT que tenha sido aprovisionado para. 


## <a name="enrollment-groups"></a>Grupos de inscrição
Com o atestado de x. 509, os dispositivos também podem ser aprovisionados através de um grupo de inscrição. Grupos de inscrição estão configurados com um certificado de assinatura, ou um intermédio ou certificado da AC de raiz e controlam o acesso ao serviço de aprovisionamento para dispositivos com esse certificado na respetiva cadeia de certificados. Para obter mais informações sobre grupos de inscrição e certificados x. 509 com o serviço de aprovisionamento, consulte [certificados x. 509](concepts-security.md#x509-certificates). 

Para ver uma lista de dispositivos que foram aprovisionados através de um grupo de inscrição, pode ver detalhes do grupo de inscrição. Esta é uma forma fácil de compreender que o IoT hub a cada dispositivo ter sido aprovisionado para. Para ver a lista de dispositivos: 

1. Inicie sessão no portal do Azure e clique em **todos os recursos** no menu da esquerda.
2. Clique em seu serviço de aprovisionamento na lista de recursos.
3. No seu serviço de aprovisionamento, clique em **gerir inscrições**, em seguida, selecione **inscrição grupos** separador.
4. Clique no grupo de inscrição para abri-lo.

   ![Ver a entrada de grupo de inscrição no portal](./media/how-to-unprovision-devices/view-enrollment-group.png)

Com os grupos de inscrição, existem dois cenários a ter em consideração:

- Para desaprovisionar todos os dispositivos que foram aprovisionados através de um grupo de inscrição:
  1. Desative o grupo de inscrição na lista de proibições o certificado de assinatura. 
  2. Utilize a lista de dispositivos aprovisionados para esse grupo de inscrição para desativar ou eliminar cada dispositivo no registo de identidade do seu IoT hub correspondentes. 
  3. Depois de desativar ou eliminar todos os dispositivos da respetiva os respetivos hubs IoT, opcionalmente, pode eliminar o grupo de inscrição. Tenha em atenção, no entanto, que, se eliminar o grupo de inscrição e existir um grupo de inscrição ativados para um certificado de assinatura superior a cópia de segurança na cadeia de certificados de uma ou mais dos dispositivos, esses dispositivos podem inscrever novamente. 

- Para desaprovisionar um único dispositivo de um grupo de inscrição:
  1. Crie uma inscrição individuais desativada para o seu certificado de folha (dispositivos). Isto revoga acesso ao serviço de aprovisionamento para que o dispositivo, contudo permite acesso de outros dispositivos que tenham o certificado de assinatura do grupo de inscrição na respetiva cadeia. Não elimine a inscrição individuais desativada para o dispositivo. Se o fizer, irá permitir que o dispositivo inscrever novamente através do grupo de inscrição. 
  2. Utilize a lista de dispositivos aprovisionados para esse grupo de inscrição para localizar o IoT hub que o dispositivo foi aprovisionado para e desative ou elimine-o partir do registo de identidade que hub. 
  
  










