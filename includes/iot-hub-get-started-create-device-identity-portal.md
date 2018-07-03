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
ms.openlocfilehash: 1df3e188b71b8fa2d5223bad8bc5914513e26286
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "34371206"
---
## <a name="create-a-device-identity"></a>Criar uma identidade de dispositivo

Nesta secção, vai utilizar o [portal do Azure] [ lnk-azure-portal] para criar uma identidade de dispositivo no registo de identidade do IoT hub. Não é possível ligar um dispositivo ao hub IoT, exceto se tiver uma entrada no registo de identidade. Para obter mais informações, veja a secção "Identity registry" (Registo de identidades) do [Hub IoT developer guide (Guia do programador do Hub IoT)][lnk-devguide-identity]. Utilizar o **dispositivos IoT** painel no portal para gerar um ID de dispositivo exclusivo e a chave para o seu dispositivo utilizar para se identificar no IoT Hub. Os IDs dos dispositivos são sensíveis às maiúsculas e minúsculas.

1. Inicie sessão no [Portal do Azure][lnk-azure-portal].

1. Selecione **todos os recursos** e localize o recurso do hub IoT.

1. Quando o recurso do hub IoT é aberto, clique nas **dispositivos IoT** ferramenta e, em seguida, clique em **Add** na parte superior. 

    ![Criar a identidade de dispositivo no portal][img-add-device]

1. Forneça um nome para o seu dispositivo novo, como **myDeviceId**e clique em **guardar**. Esta ação cria uma nova identidade de dispositivo do hub IoT.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

   ![Adicionar um novo dispositivo][img-create-device]

1. Na lista de dispositivos, clique no dispositivo recém-criado e copie os **cadeia de ligação---chave primária** para utilizar mais tarde.

    ![Cadeia de ligação do dispositivo][img-connection-string]

> [!NOTE]
> O registo de identidade do Hub IoT apenas armazena identidades de dispositivos para permitir um acesso seguro ao Hub IoT. Armazena os IDs do dispositivo e as chaves a utilizar como credenciais de segurança e um sinalizador ativado/desativado que pode utilizar para desativar o acesso de um dispositivo individual. Se a sua aplicação tiver de armazenar outros metadados específicos do dispositivo, deverá utilizar um armazenamento específico da aplicação.  Para obter mais informações, veja o [IoT Hub developer guide (Guia do programador do Hub IoT)][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-add-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png
[img-create-device]:./media/iot-hub-get-started-create-device-identity-portal/add-device.png

<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

