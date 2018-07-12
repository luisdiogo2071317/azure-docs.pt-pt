---
title: incluir ficheiro
description: incluir ficheiro
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: f8cd78e63099f864c5fc54b6268f6e558d738626
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724921"
---
## <a name="create-an-iot-hub"></a>Criar um hub IoT

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

Agora que criou um hub IoT, localize as informações importantes que utiliza para ligar dispositivos e aplicações ao seu hub IoT. 

No menu de navegação de hub do IoT, abra **políticas de acesso partilhado**.
Selecione o **iothubowner** política e, em seguida, copie a **cadeia de ligação---chave primária** do IoT hub. Para obter mais informações, veja [Controlar o acesso ao Hub IoT](../articles/iot-hub/iot-hub-devguide-security.md).

   > [!NOTE] 
   > Não é necessário esta cadeia de ligação iothubowner para este tutorial de configuração. No entanto, poderá ser necessária para alguns dos tutoriais ou diferentes cenários de IoT depois de concluir esta configuração.

   ![Obter a cadeia de ligação do hub IoT](./media/iot-hub-get-started-create-hub-and-device/create-iot-hub5.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Registar um dispositivo no hub IoT para o seu dispositivo

1. No menu de navegação de hub do IoT, abra **dispositivos IoT**, em seguida, clique em **Add** para registar um dispositivo no IoT hub.

   ![Adicionar um dispositivo em dispositivos IoT do IoT hub](./media/iot-hub-get-started-create-hub-and-device/create-identity-portal.png)

2. Introduza um **ID de dispositivo** para o novo dispositivo. Os IDs dos dispositivos são sensíveis às maiúsculas e minúsculas.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. Clique em **Guardar**.
5. Depois do dispositivo é criado, abra-na lista os **dispositivos IoT** painel.
6. Copiar o **cadeia de ligação---chave primária** para utilizar mais tarde.

   ![Obter a cadeia de ligação do dispositivo](./media/iot-hub-get-started-create-hub-and-device/device-connection-string.png)
