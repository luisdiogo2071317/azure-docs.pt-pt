---
title: Registar um novo dispositivo de limite de IoT do Azure (VS Code) | Microsoft Docs
description: Utilize o Visual Studio Code para criar um novo dispositivo de limite de IoT no seu hub IoT do Azure
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/14/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 7902461f58df1b4fe0c3ed3b577f668fe8be4cc2
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036530"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Registar um novo dispositivo de limite de IoT do Azure a partir do código do Visual Studio

Antes de poder utilizar os seus dispositivos de IoT com limite de IoT do Azure, terá de registá-los no seu IoT hub. Depois de registar um dispositivo, receberá uma cadeia de ligação que pode ser utilizada para configurar o seu dispositivo para cargas de trabalho de limite. 

Este artigo mostra como registar um dispositivo de limite de IoT novo utilizando o Visual Studio Code (VS Code). Existem várias formas para efetuar a maioria das operações no VS Code. Este artigo utiliza o Explorador, mas também pode utilizar a paleta de comando para executar a maioria dos passos. 

## <a name="prerequisites"></a>Pré-requisitos

* Um [IoT hub](../iot-hub/iot-hub-create-through-portal.md) na sua subscrição do Azure
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensão de limite de IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code

## <a name="sign-in-to-access-your-iot-hub"></a>Iniciar sessão para aceder ao seu IoT hub

Pode utilizar as extensões de IoT do Azure para Visual Studio Code para realizar operações com o seu IoT hub. Para estas operações trabalhar, terá de iniciar sessão na sua conta do Azure e selecione o hub IoT está a trabalhar.

1. No Visual Studio Code, abra o **Explorer** vista.

2. Na parte inferior do Explorador, expanda o **dispositivos do Azure IoT Hub** secção. 

   ![Expanda os dispositivos do IoT Hub do Azure](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

3. Clique em de **...**  no **dispositivos do Azure IoT Hub** cabeçalho de secção. Se não vir o botão de reticências, coloque o cursor sobre o cabeçalho. 

4. Escolha **selecione IoT Hub**.

5. Se não tiver iniciado sua conta do Azure, siga as instruções para o fazer. 

6. Selecione a sua subscrição do Azure. 

7. Selecione o seu IoT hub. 

## <a name="create-a-device"></a>Criar um dispositivo

1. No Explorador de código de VS, expanda o **dispositivos do Azure IoT Hub** secção. 

2. Clique em de **...**  no **dispositivos do Azure IoT Hub** cabeçalho de secção. Se não vir o botão de reticências, coloque o cursor sobre o cabeçalho. 

3. Selecione **criar dispositivo de limite de IoT**. 

4. Na caixa de texto que se abre, dê o seu dispositivo um ID. 

No ecrã de saída, consulte o resultado do comando. As informações de dispositivo está impresso, que inclui o **deviceId** fornecida e o **connectionString** que pode utilizar para ligar o seu dispositivo físico ao seu IoT hub. 

## <a name="view-all-devices"></a>Ver todos os dispositivos

Todos os dispositivos que ligam ao seu IoT hub constam o **dispositivos do Azure IoT Hub** secção do Explorador de código do Visual Studio. Dispositivos de limite de IoT são distinguishable dos dispositivos de limite não com um ícone diferente e o facto de que pode ser expandidos para mostrar os módulos implementados em cada dispositivo. 

   ![Ver dispositivos no VS Code](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>Obter a cadeia de ligação

Quando estiver pronto para configurar o seu dispositivo, terá da cadeia de ligação que liga o dispositivo físico com a respetiva identidade do IoT hub.

1. Clique com o botão direito no ID do seu dispositivo no **dispositivos do Azure IoT Hub** secção. 
2. Selecione **copie a cadeia de ligação do dispositivo**.

   A cadeia de ligação é copiada para a sua área de transferência. 

Também pode selecionar **obter informações do dispositivo** do menu de contexto para ver todas as informações do dispositivo, incluindo a cadeia de ligação, na janela de saída. 


## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar módulos num dispositivo com o Visual Studio Code](how-to-deploy-modules-vscode.md).
