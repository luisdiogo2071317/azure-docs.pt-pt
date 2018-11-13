---
title: Registar um novo dispositivo Azure IoT Edge (VS Code) | Documentos da Microsoft
description: Utilizar o Visual Studio Code para criar um novo dispositivo IoT Edge no seu hub IoT do Azure
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/14/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: cf9603c65454f076a494789e784c9352fb7bef33
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578710"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Registar um novo dispositivo Azure IoT Edge do Visual Studio Code

Antes de poder utilizar os seus dispositivos IoT com o Azure IoT Edge, tem de registá-los com o seu hub IoT. Depois de se registar um dispositivo, receberá uma cadeia de ligação que pode ser utilizada para configurar o dispositivo para cargas de trabalho do Edge. 

Este artigo mostra como registar um novo dispositivo IoT Edge com o Visual Studio Code (código de VS). Existem várias maneiras de executar a maioria das operações no VS Code. Este artigo utiliza o Explorador, mas também pode utilizar a paleta de comandos para executar a maioria dos passos. 

## <a name="prerequisites"></a>Pré-requisitos

* Uma [IoT hub](../iot-hub/iot-hub-create-through-portal.md) na sua subscrição do Azure
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code

## <a name="sign-in-to-access-your-iot-hub"></a>Entre para acessar o seu hub IoT

Pode utilizar as extensões de IoT do Azure para Visual Studio Code para realizar operações com o seu hub IoT. Para essas operações trabalhar, terá de iniciar sessão na sua conta do Azure e selecione o hub de IoT que está a trabalhar.

1. No Visual Studio Code, abra a **Explorer** vista.

2. Na parte inferior do Explorer, expanda o **dispositivos do Azure IoT Hub** secção. 

   ![Expanda os dispositivos do Hub IoT do Azure](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

3. Clique no **...**  no **dispositivos do Azure IoT Hub** cabeçalho de secção. Se não vir o botão de reticências, clique em ou coloque o cursor sobre o cabeçalho. 

4. Escolher **selecione o IoT Hub**.

5. Se não tiver sessão iniciada sua conta do Azure, siga as instruções para fazer isso. 

6. Selecione a sua subscrição do Azure. 

7. Selecione o seu hub IoT. 

## <a name="create-a-device"></a>Criar um dispositivo

1. No VS Code Explorer, expanda o **dispositivos do Azure IoT Hub** secção. 

2. Clique no **...**  no **dispositivos do Azure IoT Hub** cabeçalho de secção. Se não vir o botão de reticências, clique em ou coloque o cursor sobre o cabeçalho. 

3. Selecione **criar dispositivo IoT Edge**. 

4. Na caixa de texto que aparece, dê o seu dispositivo um ID. 

No ecrã da saída, pode ver o resultado do comando. As informações de dispositivo é impresso, que inclui a **deviceId** fornecida e o **connectionString** que pode utilizar para ligar o dispositivo físico ao seu hub IoT. 

## <a name="view-all-devices"></a>Ver todos os dispositivos

Todos os dispositivos que se ligam ao seu hub IoT estão listados na **dispositivos do Azure IoT Hub** seção do Gerenciador de código do Visual Studio. Dispositivos IoT Edge são distintas a partir de dispositivos de não-Edge com um ícone diferente e o fato de que pode ser expandidos para mostrar os módulos implementados em cada dispositivo. 

   ![Ver os dispositivos no VS Code](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>Obter a cadeia de ligação

Quando estiver pronto para configurar o dispositivo, terá da cadeia de ligação que liga o dispositivo físico com a respetiva identidade no IoT hub.

1. Com o botão direito no ID do seu dispositivo na **dispositivos do Azure IoT Hub** secção. 
2. Selecione **copiar a cadeia de ligação do dispositivo**.

   A cadeia de ligação é copiada para a área de transferência. 

Também pode selecionar **obter informações de dispositivo** no menu de contexto para ver todas as informações do dispositivo, incluindo a cadeia de ligação, na janela de saída. 


## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar módulos num dispositivo com o Visual Studio Code](how-to-deploy-modules-vscode.md).
