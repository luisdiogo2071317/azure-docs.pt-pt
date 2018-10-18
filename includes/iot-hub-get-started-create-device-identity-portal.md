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
ms.openlocfilehash: ffd5da239f8e271a8c9b2aaf3f6d5fd9f885c79c
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400262"
---
## <a name="create-a-device-identity"></a>Criar uma identidade de dispositivo

Nesta secção, vai utilizar o [portal do Azure](https://portal.azure.com) para criar uma identidade de dispositivo no registo de identidade do IoT hub. Não é possível ligar um dispositivo ao hub IoT, exceto se tiver uma entrada no registo de identidade. Para obter mais informações, consulte a secção "Registo de identidade" a [Guia do programador do IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md) utilizar o **dispositivos IoT** painel no portal para gerar um ID de dispositivo exclusivo e a chave para o seu dispositivo identificar -se para o IoT Hub. Os IDs dos dispositivos são sensíveis às maiúsculas e minúsculas.

1. Inicie sessão no [portal do Azure](https://portal.azure.com)

2. Selecione **todos os recursos** e localize o recurso do hub IoT.

3. Quando o recurso do hub IoT é aberto, clique nas **dispositivos IoT** ferramenta e, em seguida, clique em **Add** na parte superior. 

    ![Criar a identidade de dispositivo no portal](./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png)

4. Forneça um nome para o seu dispositivo novo, como **myDeviceId**e clique em **guardar**. Esta ação cria uma nova identidade de dispositivo do hub IoT.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

   ![Adicionar um novo dispositivo](./media/iot-hub-get-started-create-device-identity-portal/create-a-device.png)

5. Na lista de dispositivos, clique no dispositivo recém-criado e copie os **cadeia de ligação---chave primária** para utilizar mais tarde.

    ![Cadeia de ligação do dispositivo](./media/iot-hub-get-started-create-device-identity-portal/device-details.png)

> [!NOTE]
> O registo de identidade do Hub IoT apenas armazena identidades de dispositivos para permitir um acesso seguro ao Hub IoT. Armazena os IDs do dispositivo e as chaves a utilizar como credenciais de segurança e um sinalizador ativado/desativado que pode utilizar para desativar o acesso de um dispositivo individual. Se a sua aplicação tiver de armazenar outros metadados específicos do dispositivo, deverá utilizar um armazenamento específico da aplicação.  Para obter mais informações, consulte [Guia do programador do IoT Hub](../articles/iot-hub/iot-hub-devguide-identity-registry.md).
