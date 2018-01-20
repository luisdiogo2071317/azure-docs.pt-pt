---
title: "Como anular o aprovisionamento de dispositivos inscritos com o serviço de aprovisionamento de dispositivos do Azure IoT Hub | Microsoft Docs"
description: "Como anular o aprovisionamento de dispositivos inscritos pelo seu serviço DPS no Portal do Azure"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 01/08/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 83ed72c0f2eb342c372ef97e5443d60eab1e2174
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="how-to-unprovision-devices-enrolled-by-your-provisioning-service"></a>Como anular o aprovisionamento de dispositivos inscritos pelo seu serviço de aprovisionamento

Pode considerar necessário para dispositivos de não aprovisionamento que foram aprovisionados através do serviço de aprovisionamento de dispositivos. Por exemplo, um dispositivo pode ser vendido ou movido para um hub IoT diferente, ou pode ser perda, roubo ou ficarem comprometida. 

Em geral, a anulação de aprovisionamento de um dispositivo envolve dois passos:

1. Revogar o acesso do dispositivo ao seu serviço de aprovisionamento. Dependendo se pretende revogar o acesso temporariamente ou permanentemente ou, no caso do mecanismo de atestado de x. 509, na hierarquia dos seus grupos existentes de inscrição, poderá querer desativar ou eliminar uma entrada de inscrição. 
 
   - Para saber como revogar o acesso do dispositivo através do portal, consulte o artigo [revogar o acesso de dispositivo](how-to-revoke-device-access-portal.md).
   - Para saber como revogar o acesso do dispositivo através de programação utilizando um dos SDKs de serviço aprovisionamento, consulte o artigo [gerir inscrições de dispositivos com o serviço SDKs](how-to-manage-enrollments-sdks.md).

2. Desativar ou eliminar a entrada do dispositivo no registo de identidade para o IoT Hub onde foi aprovisionado. Para obter mais informações, consulte [gerir identidades de dispositivo](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices) na documentação do IoT Hub do Azure. 

Os passos exatos que tomar para anular o aprovisionamento de um dispositivo irão depender o mecanismo de atestado utiliza.

Dispositivos que utilizam o atestado de TPM ou atestado de x. 509 com um certificado autoassinado folha (cadeia de certificados) são aprovisionados através de uma entrada de registo individuais. Para estes dispositivos, pode eliminar a entrada permanentemente revogar o acesso do dispositivo para o serviço de aprovisionamento ou desativar a entrada temporariamente revogar o acesso e, em seguida, seguimento por um eliminar ou desativar o dispositivo no registo de identidade do IOT Hub de que foi aprovisionado para.

Com o atestado de x. 509, os dispositivos também podem ser aprovisionados através de um grupo de inscrição. Grupos de inscrição estão configurados com um certificado de assinatura, ou um intermédio ou certificado da AC de raiz e controlam o acesso ao serviço de aprovisionamento para dispositivos com esse certificado na respetiva cadeia de certificados. Para obter mais informações sobre grupos de inscrição e certificados x. 509 com o serviço de aprovisionamento, consulte [certificados x. 509](concepts-security.md#x509-certificates). 

Para ver uma lista de dispositivos que foram aprovisionados através de um grupo de inscrição, pode ver detalhes do grupo de inscrição. Esta é uma forma fácil de compreender que o IoT hub a cada dispositivo ter sido aprovisionado para. Para ver a lista de dispositivos: 

1. Inicie sessão no portal do Azure e clique em **todos os recursos** no menu da esquerda.
2. Clique em seu serviço de aprovisionamento na lista de recursos.
3. No seu serviço de aprovisionamento, clique em **gerir inscrições**, em seguida, selecione **inscrição grupos** separador.
4. Clique no grupo de inscrição para abri-lo.

   ![Ver a entrada de grupo de inscrição no portal](./media/how-to-unprovision-devices/view-enrollment-group.png)

Com os grupos de inscrição, existem dois cenários a ter em consideração:

- Para anular o aprovisionamento de todos os dispositivos que foram aprovisionados através de um grupo de inscrição, primeiro tem de desativar o grupo de inscrição na lista de proibições o certificado de assinatura. Em seguida, pode utilizar a lista de dispositivos aprovisionados para esse grupo de inscrição para desativar ou eliminar cada dispositivo no registo de identidade do seu IoT hub correspondentes. Depois de desativar ou eliminar todos os dispositivos da respetiva os respetivos hubs IoT, opcionalmente, pode eliminar o grupo de inscrição. Tenha em atenção, no entanto, que, se eliminar o grupo de inscrição e existir um grupo de inscrição ativados para um certificado de assinatura superior a cópia de segurança na cadeia de certificados de uma ou mais dos dispositivos, esses dispositivos podem inscrever novamente. 
- Para anular o aprovisionamento de um único dispositivo de um grupo de inscrição, tem de criar primeiro uma inscrição individuais desativada para o seu certificado de folha (dispositivos). Isto revoga acesso ao serviço de aprovisionamento para que o dispositivo, contudo permite acesso de outros dispositivos que tenham o certificado de assinatura do grupo de inscrição na respetiva cadeia. Em seguida, pode utilizar a lista de dispositivos aprovisionados nos detalhes do grupo de inscrição para localizar o IoT hub que o dispositivo foi aprovisionado para e desativar ou eliminá-la a partir do registo de identidade que hub. Não elimine a inscrição individuais desativada para o dispositivo. Se o fizer, irá permitir que o dispositivo inscrever novamente através do grupo de inscrição. 










