---
title: Criar um Hub de IoT do Azure com o Kit de ferramentas do Azure IoT para o VS Code | Documentos da Microsoft
description: Como usar o Kit de ferramentas do Azure IoT para o VS Code para criar um hub IoT.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: 5097d7e1f6a0b9e94919ccc8731702206c0a1568
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047238"
---
# <a name="create-an-iot-hub-using-the-azure-iot-toolkit-for-visual-studio-code"></a>Criar um hub IoT com o Kit de ferramentas de IoT do Azure para Visual Studio Code

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Este artigo mostra-lhe como utilizar o [Kit de ferramentas do Azure IoT para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) para criar um hub IoT do Azure. 

Para concluir este artigo, precisa do seguinte:

- Uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

- [Visual Studio Code](https://code.visualstudio.com/)

- [Toolkit do IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="create-an-iot-hub"></a>Criar um hub IoT

1. No Visual Studio Code, abra a **Explorer** vista.

2. Na parte inferior do Explorer, expanda o **dispositivos do Azure IoT Hub** secção. 

   ![Expanda os dispositivos do Hub IoT do Azure](./media/iot-hub-create-use-iot-toolkit/azure-iot-hub-devices.png)

3. Clique no **...**  no **dispositivos do Azure IoT Hub** cabeçalho de secção. Se não vir o botão de reticências, Paire o rato sobre o cabeçalho. 

4. Escolher **criar o IoT Hub**.

5. Mostra um pop-up no canto inferior direito para permitem-lhe iniciar sessão no Azure pela primeira vez.

6. Selecione a subscrição do Azure. 

7. Selecionar grupo de recursos.

8. Selecione a localização.

9. Selecione o escalão de preço.

10. Introduza um nome globalmente exclusivo para o seu IoT Hub.

11. Aguarde alguns minutos até que o IoT Hub é criado.

## <a name="next-steps"></a>Passos Seguintes

Agora que implementou um hub IoT com o Kit de ferramentas de IoT do Azure para Visual Studio Code. Para explorar ainda mais, veja os artigos seguintes:

* [Utilizar a extensão do Kit de ferramentas do Azure IoT para o Visual Studio Code para enviar e receber mensagens entre o dispositivo e um IoT Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

* [Utilizar a extensão IoT Toolkit do Azure para Visual Studio Code para gestão de dispositivos do IoT Hub do Azure](iot-hub-device-management-iot-toolkit.md)

* [Consulte a página wiki do Kit de ferramentas do Azure IoT](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki).
