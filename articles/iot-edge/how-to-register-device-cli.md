---
title: Registar um novo dispositivo de limite de IoT do Azure (CLI) | Microsoft Docs
description: Utilizar a extensão de IoT para Azure CLI 2.0 para registar um dispositivo de limite de IoT novo
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 05/30/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a6935dfe5b32364e402017cd3005ab17969ce1ce
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036385"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli-20"></a>Registar um novo dispositivo de limite de IoT do Azure com o Azure CLI 2.0

Antes de poder utilizar os seus dispositivos de IoT com limite de IoT do Azure, terá de registá-los no seu IoT hub. Depois de registar um dispositivo, receberá uma cadeia de ligação que pode ser utilizada para configurar o seu dispositivo para cargas de trabalho de limite. 

A [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) é uma ferramenta de linha de comandos multiplataformas de código aberto para gerir recursos do Azure, como o IoT Edge. Permite-lhe gerir recursos, instâncias de serviço aprovisionamento de dispositivos e ligado hubs a Box IoT Hub do Azure. A nova extensão de IoT otimiza a CLI 2.0 do Azure com funcionalidades como a gestão de dispositivos e a capacidade completa do IoT Edge.

Este artigo mostra como registar um novo dispositivo de limite de IoT a utilizar o Azure CLI 2.0.

## <a name="prerequisites"></a>Pré-requisitos

* Um [IoT hub](../iot-hub/iot-hub-create-using-cli.md) na sua subscrição do Azure. 
* [CLI 2.0 do Azure ](https://docs.microsoft.com/cli/azure/install-azure-cli) no seu ambiente. A versão mínima da CLI 2.0 do Azure tem de ser 2.0.24 ou superior. Utilize `az –-version` para validar. Esta versão suporta comandos de extensão az e apresenta a arquitetura de comandos Knack. 
* O [extensão de IoT do Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Criar um dispositivo

Utilize o seguinte comando para criar uma nova identidade de dispositivo no seu IoT hub: 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Este comando inclui três parâmetros:
* **id de dispositivo**: forneça um nome descritivo que seja exclusivo ao seu IoT hub.
* **nome do hub**: forneça o nome do seu IoT hub.
* **ativado por limite**: este parâmetro declara que o dispositivo é para utilização com limite de IoT.

   ![Criar dispositivo do IoT Edge](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Ver todos os dispositivos

Utilize o seguinte comando para ver todos os dispositivos no seu IoT hub:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Qualquer dispositivo que está registado como um dispositivo de limite de IoT irão ter a propriedade **capabilities.iotEdge** definido como **verdadeiro**. 

## <a name="retrieve-the-connection-string"></a>Obter a cadeia de ligação

Quando estiver pronto para configurar o seu dispositivo, terá da cadeia de ligação que liga o dispositivo físico com a respetiva identidade do IoT hub. Utilize o seguinte comando para devolver a cadeia de ligação para um único dispositivo:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

O parâmetro de id de dispositivo é maiúsculas e minúsculas. 

## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar módulos num dispositivo com o Azure CLI 2.0](how-to-deploy-modules-cli.md)