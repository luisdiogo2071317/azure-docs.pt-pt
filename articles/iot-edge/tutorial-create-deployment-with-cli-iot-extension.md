---
title: "Implementar módulos em dispositivos de IoT limite a utilizar a extensão de IoT para Azure CLI 2.0 | Microsoft Docs"
description: "Implementar módulos para um dispositivo de limite de IoT utilizando a extensão de IoT para Azure CLI 2.0"
services: iot-edge
keywords: 
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 03/02/2018
ms.topic: article
ms.service: iot-edge
ms.custom: mvc
ms.reviewer: kgremban
ms.openlocfilehash: 1986c9881c09ffa480103e009dc42d18aad4e2aa
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Implementar módulos para um dispositivo de limite de IoT utilizando a extensão de IoT para Azure CLI 2.0

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) é um open source entre a ferramenta de linha de comandos de plataforma para gerir recursos do Azure, tais como o limite de IoT. Azure CLI 2.0 está disponível no Windows, Linux e MacOS.

2.0 do CLI do Azure permite-lhe gerir recursos, instâncias de serviço aprovisionamento de dispositivos e ligado hubs a Box do IoT Hub do Azure. A nova extensão de IoT otimiza Azure CLI 2.0 com funcionalidades como a gestão de dispositivos e a capacidade total do limite de IoT.

Neste tutorial, primeiro conclua os passos para configurar o Azure CLI 2.0 e a extensão de IoT. Em seguida, irá aprender a implementar módulos para um dispositivo de limite de IoT utilizando os comandos da CLI disponíveis.

## <a name="prerequisites"></a>Pré-requisitos

* Uma conta do Azure. Se ainda não tem uma, pode [criar uma conta gratuita](https://azure.microsoft.com/free/?v=17.39a) hoje. 

* [Python 2.7 x ou Python 3](https://www.python.org/downloads/).

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) no seu ambiente. No mínimo, a versão 2.0 do Azure CLI tem de ser 2.0.24 ou superior. Utilize `az –-version` para validar. Esta versão suporta comandos de extensão az e apresenta a arquitetura de comando Knack. Uma forma simples de instalar o Windows é para transferir e instalar o [MSI](https://aka.ms/InstallAzureCliWindows).

* [A extensão de IoT para Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension):
   1. Execute `az extension add --name azure-cli-iot-ext`. 
   2. Após a instalação, utilize `az extension list` para validar as extensões atualmente instaladas ou `az extension show --name azure-cli-iot-ext` para ver detalhes sobre a extensão de IoT.
   3. Para remover a extensão, utilize `az extension remove --name azure-cli-iot-ext`.


## <a name="create-an-iot-edge-device"></a>Criar um dispositivo de limite de IoT
Este artigo dá instruções para criar uma implementação de limite de IoT. O exemplo mostra como iniciar sessão na sua conta do Azure, criar um grupo de recursos do Azure (um contentor que retém recursos relacionados para uma solução do Azure), criar um IoT Hub, crie três identidade de dispositivos de IoT Edge, etiquetas e em seguida, criar uma implementação de limite de IoT que destina-se esses dispositivos. 

Inicie sessão sua conta do Azure. Depois de introduzir o seguinte comando de início de sessão, é-lhe pedido que utilizem um browser para iniciar sessão com um código Monouso: 

   ```cli
   az login
   ```

Criar um novo grupo de recursos chamado **IoTHubCLI** na região EUA Leste: 

   ```cli
   az group create -l eastus -n IoTHubCLI
   ```

   ![Criar grupo de recursos][2]

Criar um hub IoT chamado **CLIDemoHub** no grupo de recursos criado de novo:

   ```cli
   az iot hub create --name CLIDemoHub --resource-group IoTHubCLI --sku S1
   ```

   >[!TIP]
   >Cada subscrição é atribuída um IoT hub gratuito. Para criar um hub gratuito com o comando da CLI, substitua o valor SKU com `--sku F1`. Se já tiver um hub gratuito na sua subscrição, obterá uma mensagem de erro ao tentar criar um segundo. 

Crie um dispositivo de limite de IoT:

   ```cli
   az iot hub device-identity create --device-id edge001 -hub-name CLIDemoHub --edge-enabled
   ```

   ![Criar dispositivo de limite de IoT][4]

## <a name="configure-the-iot-edge-device"></a>Configurar o dispositivo de limite de IoT

Criar um modelo de implementação de JSON e guardá-lo localmente como um ficheiro txt. Terá do caminho para o ficheiro ao executar o comando de configuração de aplicar.

Modelos de implementação de JSON sempre devem incluir os módulos de sistema dois, edgeAgent e edgeHub. Para além das duas, pode utilizar este ficheiro para implementar módulos adicionais para o dispositivo de limite de IoT. Utilize o seguinte exemplo para configurar o dispositivo de limite de IoT com um módulo de tempSensor:

   ```json
   {
     "moduleContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": ""
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "microsoft/azureiotedge-agent:1.0-preview",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "microsoft/azureiotedge-hub:1.0-preview",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "tempSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {},
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "tempSensor": {
         "properties.desired": {}
       }
     }
   }
   ```

Aplicar a configuração para o seu dispositivo de limite de IoT:

   ```cli
   az iot hub apply-configuration --device-id edge001 --hub-name CLIDemoHub --content C:\<configuration.txt file path>
   ```

Ver os módulos no seu dispositivo de limite de IoT:
    
   ```cli
   az iot hub module-identity list --device-id edge001 --hub-name CLIDemoHub
   ```

   ![Lista de módulos][6]

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [utilizar um dispositivo de limite de IoT como um gateway](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.png
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.png

