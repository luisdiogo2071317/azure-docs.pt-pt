---
title: "Implementar módulos em dispositivos de IoT limite a utilizar a extensão de IoT para Azure CLI 2.0 | Microsoft Docs"
description: "Implementar módulos para um dispositivo de limite de IoT utilizando a extensão de IoT para Azure CLI 2.0"
services: iot-edge
keywords: 
author: chrissie926
manager: timlt
ms.author: menchi
ms.date: 01/11/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 26067187864f9a2a4c85c953ae8aca888458d245
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Implementar módulos para um dispositivo de limite de IoT utilizando a extensão de IoT para Azure CLI 2.0

[Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) é um open source entre a ferramenta de linha de comandos de plataforma para gerir recursos do Azure, tais como o limite de IoT. Azure CLI 2.0 está disponível no Windows, Linux e MacOS.

2.0 do CLI do Azure permite-lhe gerir recursos, instâncias de serviço aprovisionamento de dispositivos e ligado hubs a Box do IoT Hub do Azure. A nova extensão de IoT otimiza Azure CLI 2.0 com funcionalidades como a gestão de dispositivos e a capacidade total do limite de IoT.

Neste tutorial, primeiro conclua os passos para configurar o Azure CLI 2.0 e a extensão de IoT. Em seguida, irá aprender a implementar módulos para um dispositivo de limite de IoT utilizando os comandos da CLI disponíveis.

## <a name="installation"></a>Instalação 

### <a name="step-1---install-python"></a>Passo 1 – instalar Python

[Python 2.7 x ou Python 3](https://www.python.org/downloads/) é necessária.

### <a name="step-2---install-azure-cli-20"></a>Passo 2 - instalar a CLI do Azure 2.0

Siga o [instruções de instalação](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) ao configurar o Azure CLI 2.0 no seu ambiente. No mínimo, a versão 2.0 do Azure CLI tem de ser 2.0.24 ou superior. Utilize `az –version` para validar. Esta versão suporta comandos de extensão az e apresenta a arquitetura de comando Knack. Uma forma simples de instalar o Windows é para transferir e instalar o [MSI](https://aka.ms/InstallAzureCliWindows).

### <a name="step-3---install-iot-extension"></a>Passo 3 - extensão de instalação IoT

[A extensão de IoT ficheiro Leia-me](https://github.com/Azure/azure-iot-cli-extension) descreve várias formas de instalar a extensão. A forma mais simples consiste em executar `az extension add --name azure-cli-iot-ext`. Após a instalação, pode utilizar `az extension list` para validar as extensões atualmente instaladas ou `az extension show --name azure-cli-iot-ext` para ver detalhes sobre a extensão de IoT. Para remover a extensão, pode utilizar `az extension remove --name azure-cli-iot-ext`.


## <a name="deploy-modules-to-an-iot-edge-device"></a>Implementar módulos para um dispositivo de limite de IoT
Neste tutorial, irá aprender a criar uma implementação de limite de IoT. O exemplo mostra como iniciar sessão na sua conta do Azure, criar um grupo de recursos do Azure (um contentor que retém recursos relacionados para uma solução do Azure), criar um IoT Hub, crie três identidade de dispositivos de IoT Edge, etiquetas e em seguida, criar uma implementação de limite de IoT que destina-se esses dispositivos. Conclua os passos de instalação descritos anteriormente antes de começar. Se não tiver uma conta do Azure ainda, pode [criar uma conta gratuita](https://azure.microsoft.com/free/?v=17.39a) hoje. 


### <a name="1-login-to-the-azure-account"></a>1. Início de sessão para a conta do Azure
  
    az login

![início de sessão][1]

### <a name="2-create-a-resource-group-iothubblogdemo-in-eastus"></a>2. Criar um grupo de recursos IoTHubBlogDemo no eastus

    az group create -l eastus -n IoTHubBlogDemo

![Criar grupo de recursos][2]


### <a name="3-create-an-iot-hub-blogdemohub-under-the-newly-created-resource-group"></a>3. Criar um blogDemoHub do IoT Hub sob o grupo de recursos criado de novo

    az iot hub create --name blogDemoHub --resource-group IoTHubBlogDemo

![Criar o IoT Hub][3]


### <a name="4-create-an-iot-edge-device"></a>4. Criar um dispositivo de limite de IoT

    az iot hub device-identity create -d edge001 -n blogDemoHub --edge-enabled

![Criar dispositivo de limite de IoT][4]

### <a name="5-apply-configuration-to-the-iot-edge-device"></a>5. Aplicar a configuração para o dispositivo de limite de IoT

Guarde o modelo de implementação de JSON localmente como um ficheiro txt. Terá do caminho para o ficheiro ao executar o comando de configuração de aplicar.

Segue-se um modelo de JSON de implementação de exemplo que contém um módulo de tempSensor:

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
              "image": "edgepreview.azurecr.io/azureiotedge/edge-agent:1.0-preview",
              "createOptions": "{}"
            }
          },
          "edgeHub": {
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "edgepreview.azurecr.io/azureiotedge/edge-hub:1.0-preview",
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
              "image": "edgepreview.azurecr.io/azureiotedge/simulated-temperature-sensor:1.0-preview",
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

    az iot hub apply-configuration --device-id edge001 --hub-name blogDemoHub --content C:\<yourLocation>\edgeconfig.txt

![Aplicar a configuração][5]

### <a name="6-list-modules"></a>6. Lista de módulos
    
    az iot hub module-identity list --device-id edge001 --hub-name blogDemoHub

![Lista de módulos][6]

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, vai criar uma função do Azure que contém o código para filtrar dados não processados gerados pelo seu dispositivo de limite de IoT. Para manter explorar o Azure IoT Edge, saiba como utilizar um dispositivo de limite de IoT como um gateway. 

> [!div class="nextstepaction"]
> [Criar um dispositivo de gateway do limite de IoT](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-create-deployment-with-cli-iot-extension/login.jpg
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.jpg
[3]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-hub.jpg
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[5]: ./media/tutorial-create-deployment-with-cli-iot-extension/apply-configuration.PNG
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.PNG

