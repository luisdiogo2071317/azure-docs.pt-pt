---
title: Registar um novo dispositivo Azure IoT Edge (CLI) | Documentos da Microsoft
description: Utilize a extensão de IoT para a CLI 2.0 do Azure para registar um novo dispositivo IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/27/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 451f4df31cd1c520b14227829923f72fe80c38c3
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325501"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli-20"></a>Registar um novo dispositivo Azure IoT Edge com a CLI 2.0 do Azure

Antes de poder utilizar os seus dispositivos IoT com o Azure IoT Edge, tem de registá-los com o seu hub IoT. Depois de se registar um dispositivo, receberá uma cadeia de ligação que pode ser utilizada para configurar o dispositivo para cargas de trabalho do Edge. 

A [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) é uma ferramenta de linha de comandos multiplataformas de código aberto para gerir recursos do Azure, como o IoT Edge. Permite-lhe gerir recursos, instâncias de serviço aprovisionamento de dispositivos e hubs ligados a caixa do IoT Hub do Azure. A nova extensão de IoT otimiza a CLI 2.0 do Azure com funcionalidades como a gestão de dispositivos e a capacidade completa do IoT Edge.

Este artigo mostra como registar um novo dispositivo IoT Edge com a CLI 2.0 do Azure.

## <a name="prerequisites"></a>Pré-requisitos

* Uma [IoT hub](../iot-hub/iot-hub-create-using-cli.md) na sua subscrição do Azure. 
* [CLI 2.0 do Azure ](https://docs.microsoft.com/cli/azure/install-azure-cli) no seu ambiente. A versão mínima da CLI 2.0 do Azure tem de ser 2.0.24 ou superior. Utilize `az –-version` para validar. Esta versão suporta comandos de extensão az e apresenta a arquitetura de comandos Knack. 
* O [extensão de IoT do Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Criar um dispositivo

Utilize o seguinte comando para criar uma nova identidade de dispositivo do seu hub IoT: 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Este comando inclui três parâmetros:
* **id de dispositivo**: forneça um nome descritivo exclusivo ao seu hub IoT.
* **nome do hub**: forneça o nome do IoT hub.
* **habilitados no Edge**: este parâmetro declara que o dispositivo é para utilização com o IoT Edge.

   ![Criar dispositivo do IoT Edge](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Ver todos os dispositivos

Utilize o seguinte comando para ver todos os dispositivos no IoT hub:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Qualquer dispositivo que está registado como um dispositivo IoT Edge irá ter a propriedade **capabilities.iotEdge** definida como **verdadeiro**. 

## <a name="retrieve-the-connection-string"></a>Obter a cadeia de ligação

Quando estiver pronto para configurar o dispositivo, terá da cadeia de ligação que liga o dispositivo físico com a respetiva identidade no IoT hub. Utilize o seguinte comando para devolver a cadeia de ligação para um único dispositivo:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

O parâmetro de id de dispositivo diferencia maiúsculas de minúsculas. Não copia as aspas à volta a cadeia de ligação. 

## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar módulos num dispositivo com a CLI 2.0 do Azure](how-to-deploy-modules-cli.md)