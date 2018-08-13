---
title: Criar um Hub de IoT do Azure com o Kit de ferramentas do Azure IoT para o VS Code | Documentos da Microsoft
description: Como usar o Kit de ferramentas do Azure IoT para o VS Code para criar um hub IoT.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: af31f9375d6a41e13a9122e9730ba9532d3d52c5
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003074"
---
# <a name="create-an-iot-hub-using-the-azure-iot-toolkit-for-visual-studio-code"></a>Criar um hub IoT com o Kit de ferramentas de IoT do Azure para Visual Studio Code

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Pode usar [Kit de ferramentas do Azure IoT para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) para criar hubs IoT do Azure. Este artigo mostra-lhe como criar um hub IoT com o Kit de ferramentas do Azure IoT.

Para concluir este artigo, precisa do seguinte:

* Uma conta ativa do Azure.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Toolkit do IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="create-an-iot-hub"></a>Criar um hub IoT

1. No Visual Studio Code, abra a **Explorer** vista.

2. Na parte inferior do Explorer, expanda o **dispositivos do Azure IoT Hub** secção. 

   ![Expanda os dispositivos do Hub IoT do Azure](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Clique no **... ** no **dispositivos do Azure IoT Hub** cabeçalho de secção. Se não vir o botão de reticências, Paire o rato sobre o cabeçalho. 

4. Escolher **criar o IoT Hub**.

5. Mostra um pop-up no canto inferior direito para permitem-lhe iniciar sessão no Azure pela primeira vez.

6. Selecione a subscrição do Azure. 

7. Selecionar grupo de recursos.

8. Selecione a localização.

9. Selecione o escalão de preço.

10. Introduza um nome globalmente exclusivo para o seu IoT Hub.

11. Aguarde alguns minutos até que o IoT Hub é criado.

## <a name="next-steps"></a>Passos Seguintes

Agora que implementou um hub IoT com o Kit de ferramentas do Azure IoT para o Visual Studio Code, pode querer explorar ainda mais:

* [Utilize a extensão IoT Toolkit do Azure para Visual Studio Code para enviar e receber mensagens entre o seu dispositivo e o IoT Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).
* [Utilizar a extensão IoT Toolkit do Azure para Visual Studio Code para gestão de dispositivos do IoT Hub do Azure](iot-hub-device-management-iot-toolkit.md)
* [Página Wiki](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki) para o Kit de ferramentas do Azure IoT.
