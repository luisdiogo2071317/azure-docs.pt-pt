---
title: Criar um Hub de IoT do Azure com o Kit de ferramentas do Azure IoT Hub para o VS Code | Documentos da Microsoft
description: Como usar o Kit de ferramentas do Azure IoT Hub para o VS Code para criar um hub IoT.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: junhan
ms.openlocfilehash: cee71ddfbf1a20cc7417976d60b04bff6f0deac8
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53339030"
---
# <a name="create-an-iot-hub-using-the-azure-iot-hub-toolkit-for-visual-studio-code"></a>Criar um hub IoT com o Kit de ferramentas do Hub de IoT do Azure para Visual Studio Code

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Este artigo mostra-lhe como utilizar o [Kit de ferramentas do Azure IoT Hub para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (anteriormente conhecido como Azure IoT Toolkit) para criar um hub IoT do Azure. 

Para concluir este artigo, precisa do seguinte:

- Uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

- [Visual Studio Code](https://code.visualstudio.com/)

- [Kit de ferramentas do Hub IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

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

Agora que implementou um hub IoT com o Kit de ferramentas do Hub de IoT do Azure para Visual Studio Code. Para explorar ainda mais, veja os artigos seguintes:

* [Utilizar a extensão do Kit de ferramentas do Azure IoT Hub para o Visual Studio Code para enviar e receber mensagens entre o dispositivo e um IoT Hub](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

* [Utilizar a extensão do Kit de ferramentas do Azure IoT Hub para o Visual Studio Code para gestão de dispositivos do IoT Hub do Azure](iot-hub-device-management-iot-toolkit.md)

* [Consulte a página wiki do Kit de ferramentas do Azure IoT Hub](https://github.com/microsoft/vscode-azure-iot-toolkit/wiki).
