---
title: incluir ficheiro
description: incluir ficheiro
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 04/05/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: b08bfcd4cb9e85f9e682efe0f599b6dd88897962
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
## <a name="create-a-device-identity"></a>Criar uma identidade de dispositivo

Esta secção, irá utilizar o [portal do Azure] [ lnk-azure-portal] para criar uma identidade de dispositivo no registo de identidade no seu IoT hub. Não é possível ligar um dispositivo ao hub IoT, exceto se tiver uma entrada no registo de identidade. Para obter mais informações, veja a secção "Identity registry" (Registo de identidades) do [Hub IoT developer guide (Guia do programador do Hub IoT)][lnk-devguide-identity]. Utilize o **dispositivos IoT** painel no portal para gerar um ID de dispositivo exclusivo e a chave do seu dispositivo para utilizar para se identificar ao IoT Hub. Os IDs dos dispositivos são sensíveis às maiúsculas e minúsculas.

1. Certifique-se de tem sessão iniciada para o [portal do Azure][lnk-azure-portal].

1. No Jumpbar, clique em **todos os recursos** e localizar o recurso do hub IoT.

    ![Navegue até ao seu Iot hub][img-find-iothub]

1. Quando o recurso do hub IoT é aberto, clique em de **dispositivos IoT** ferramenta e, em seguida, clique em **adicionar** na parte superior. Forneça o nome para o novo dispositivo, como **myDeviceId**e clique em **guardar**.

    ![Criar a identidade de dispositivo no portal][img-create-device]

   Esta ação cria uma nova identidade de dispositivo do seu hub IoT.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. No **dispositivos IoT**da lista de dispositivos, clique no dispositivo recém-criado e anote o **cadeia de ligação---chave primária**.

    ![Cadeia de ligação do dispositivo][img-connection-string]

> [!NOTE]
> O registo de identidade do Hub IoT apenas armazena identidades de dispositivos para permitir um acesso seguro ao Hub IoT. Armazena os IDs do dispositivo e as chaves a utilizar como credenciais de segurança e um sinalizador ativado/desativado que pode utilizar para desativar o acesso de um dispositivo individual. Se a sua aplicação tiver de armazenar outros metadados específicos do dispositivo, deverá utilizar um armazenamento específico da aplicação.  Para obter mais informações, veja o [IoT Hub developer guide (Guia do programador do Hub IoT)][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

