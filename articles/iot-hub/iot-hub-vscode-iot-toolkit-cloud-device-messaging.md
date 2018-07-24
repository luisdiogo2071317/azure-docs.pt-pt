---
title: Gerir o dispositivo do IoT Hub do Azure na cloud messaging com a extensão IoT Toolkit do Azure para Visual Studio Code | Documentos da Microsoft
description: Saiba como utilizar a extensão IoT Toolkit do Azure para Visual Studio Code para monitorizar o dispositivo para mensagens na cloud e enviar na cloud para mensagens de dispositivos no IoT Hub do Azure.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 7/20/2018
ms.author: junhan
ms.openlocfilehash: 7bcb6eebdb6ceba6b07aeb19c1a74309fd4227d0
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39206113"
---
# <a name="use-azure-iot-toolkit-extension-for-visual-studio-code-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Utilize a extensão IoT Toolkit do Azure para Visual Studio Code para enviar e receber mensagens entre o seu dispositivo e o IoT Hub

![Diagrama de ponto a ponto](media/iot-hub-get-started-e2e-diagram/2.png)

[Kit de ferramentas de IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) é uma extensão do Visual Studio Code útil que facilita a gestão do IoT Hub. Este artigo se concentra em como usar a extensão IoT Toolkit do Azure para Visual Studio Code para enviar e receber mensagens entre o seu dispositivo e o seu hub IoT.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>O que aprenderá

Saiba como utilizar a extensão IoT Toolkit do Azure para Visual Studio Code para monitorizar mensagens do dispositivo para a cloud e para enviar mensagens da cloud para o dispositivo. Mensagens do dispositivo para a cloud podem ser dados de sensor que o dispositivo recolhe e, em seguida, envia ao seu hub IoT. Mensagens cloud para o dispositivo podem ser comandos que o seu hub IoT envia para o seu dispositivo para blink um LED que está ligada ao seu dispositivo.

## <a name="what-you-will-do"></a>O que fará

- Utilize a extensão IoT Toolkit do Azure para Visual Studio Code para monitorizar mensagens do dispositivo para a cloud.
- Utilize a extensão IoT Toolkit do Azure para Visual Studio Code para enviar mensagens de cloud-para-dispositivo.

## <a name="what-you-need"></a>Do que precisa

- Uma subscrição ativa do Azure.
- Um hub IoT do Azure com a sua subscrição.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Toolkit do IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="sign-in-to-access-your-iot-hub"></a>Entre para acessar o seu hub IoT

1. Na **Explorer** ver do VS Code, expanda **dispositivos do Azure IoT Hub** secção no canto inferior esquerdo.
1. Clique em **selecione o IoT Hub** no menu de contexto.
1. Mostra um pop-up no canto inferior direito para permitem-lhe iniciar sessão no Azure pela primeira vez.
1. Depois de iniciar sessão, sua lista de subscrições do Azure serão apresentados, em seguida, selecione a subscrição do Azure e o IoT Hub.
1. A lista de dispositivos será mostrada na **dispositivos do Azure IoT Hub** guia em poucos segundos.

   > [!Note]
   > Também pode preencher o conjunto de cópia de segurança ao escolher **definir cadeia de ligação do IoT Hub**. Introduza a cadeia de ligação do hub IoT que se liga o dispositivo de IoT na janela de pop-up.
   
## <a name="monitor-device-to-cloud-messages"></a>Monitorizar mensagens do dispositivo para a cloud

Para monitorizar mensagens que são enviadas a partir do seu dispositivo ao seu hub IoT, siga estes passos:

1. O dispositivo com o botão direito e selecione **iniciar mensagens D2C monitorização**.
1. As mensagens monitorizadas serão mostradas na **saída** > **Kit de ferramentas do Azure IoT** vista.
1. Para parar a monitorização, clique com botão direito a **saída** ver e selecionar **parar mensagens D2C monitorização**.

## <a name="send-cloud-to-device-messages"></a>Enviar mensagens da cloud para o dispositivo

Para enviar uma mensagem do seu hub IoT para o seu dispositivo, siga estes passos:

1. O dispositivo com o botão direito e selecione **enviar mensagem de C2D para dispositivos**. 
1. Introduza a mensagem na caixa de entrada.
1. Os resultados serão mostrados na **saída** > **Kit de ferramentas do Azure IoT** vista.

## <a name="next-steps"></a>Passos Seguintes

Já aprendeu a monitorizar mensagens do dispositivo para a cloud e enviar mensagens de cloud-para-dispositivo entre o seu dispositivo IoT e IoT Hub do Azure.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
