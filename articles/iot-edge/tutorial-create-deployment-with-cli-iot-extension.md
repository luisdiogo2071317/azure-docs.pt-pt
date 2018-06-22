---
title: Implementar módulos em dispositivos do IoT Edge com a extensão de IoT para a CLI 2.0 do Azure | Microsoft Docs
description: Implementar módulos num dispositivo do IoT Edge com a extensão de IoT para a CLI 2.0 do Azure
author: chrissie926
manager: ''
ms.author: menchi
ms.date: 03/02/2018
ms.topic: tutorial
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
md.custom: mvc
ms.openlocfilehash: deee54fe5d11d6d1cf5485357f853b1cb078f96d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631586"
---
# <a name="deploy-modules-to-an-iot-edge-device-using-iot-extension-for-azure-cli-20"></a>Implementar módulos num dispositivo do IoT Edge com a extensão de IoT para a CLI 2.0 do Azure

A [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) é uma ferramenta de linha de comandos multiplataformas de código aberto para gerir recursos do Azure, como o IoT Edge. A CLI 2.0 do Azure está disponível para Windows, Linux e MacOS.

A CLI 2.0 do Azure permite-lhe gerir recursos, instâncias do serviço de aprovisionamento de dispositivos e hubs ligados do Hub IoT do Azure. A nova extensão de IoT otimiza a CLI 2.0 do Azure com funcionalidades como a gestão de dispositivos e a capacidade completa do IoT Edge.

Neste artigo, configura a CLI 2.0 do Azure e a extensão de IoT. Em seguida, irá aprender a implementar módulos num dispositivo do IoT Edge com os comandos da CLI disponíveis.

## <a name="prerequisites"></a>Pré-requisitos

* Uma conta do Azure. Se ainda não tiver uma, pode [criar uma conta gratuita](https://azure.microsoft.com/free/?v=17.39a) já hoje. 

* [Python 2.7x ou Python 3.x](https://www.python.org/downloads/).

* [CLI 2.0 do Azure ](https://docs.microsoft.com/cli/azure/install-azure-cli) no seu ambiente. A versão mínima da CLI 2.0 do Azure tem de ser 2.0.24 ou superior. Utilize `az –-version` para validar. Esta versão suporta comandos de extensão az e apresenta a arquitetura de comandos Knack. Uma forma simples de instalar no Windows é transferir e instalar o [MSI](https://aka.ms/InstallAzureCliWindows).

* [A extensão de IoT para a CLI 2.0 do Azure](https://github.com/Azure/azure-iot-cli-extension):
   1. Execute `az extension add --name azure-cli-iot-ext`. 
   2. Após a instalação, utilize `az extension list` para validar as extensões atualmente instaladas ou `az extension show --name azure-cli-iot-ext` para ver detalhes sobre a extensão de IoT.
   3. Para remover a extensão, utilize `az extension remove --name azure-cli-iot-ext`.


## <a name="create-an-iot-edge-device"></a>Criar um dispositivo do IoT Edge
Este artigo aborda instruções para criar uma implementação do IoT Edge. O exemplo mostra como iniciar sessão na sua conta do Azure, criar um Grupo de Recursos do Azure (um contentor com os recursos relacionados para uma solução do Azure), criar um Hub IoT, criar três identidades do dispositivo IoT Edge, definir etiquetas e, em seguida, criar uma implementação do IoT Edge que segmenta esses dispositivos. 

Inicie sessão na sua conta do Azure. Depois de introduzir o seguinte comando de início de sessão, é-lhe pedido para utilizar um browser para iniciar sessão com um código único: 

   ```cli
   az login
   ```

Crie um novo grupo de recursos chamado **IoTHubCLI** na região E.U.A Leste: 

   ```cli
   az group create -l eastus -n IoTHubCLI
   ```

   ![Criar grupo de recursos][2]

Crie um hub IoT chamado **CLIDemoHub** no grupo de recursos recém-criado:

   ```cli
   az iot hub create --name CLIDemoHub --resource-group IoTHubCLI --sku S1
   ```

   >[!TIP]
   >Cada subscrição recebe um hub IoT gratuito. Para criar um hub gratuito com o comando da CLI, substitua o valor da SKU por `--sku F1`. Se já tiver um hub gratuito na sua subscrição, irá receber uma mensagem de erro ao tentar criar um segundo. 

Criar um dispositivo do IoT Edge:

   ```cli
   az iot hub device-identity create --device-id edge001 --hub-name CLIDemoHub --edge-enabled
   ```

   ![Criar dispositivo do IoT Edge][4]

## <a name="configure-the-iot-edge-device"></a>Configurar o dispositivo do IoT Edge

Crie um modelo JSON de implementação e guarde-o localmente como um ficheiro txt. Irá precisar do caminho para o ficheiro ao executar o comando apply-configuration.

Os modelos JSON de implementação devem incluir sempre os dois módulos do sistema, edgeAgent e edgeHub. Para além desses dois, pode utilizar este ficheiro para implementar módulos adicionais no dispositivo do IoT Edge. Utilize o exemplo seguinte para configurar o dispositivo do IoT Edge com um módulo do tempSensor:

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

Aplique a configuração no seu dispositivo do IoT Edge:

   ```cli
   az iot hub apply-configuration --device-id edge001 --hub-name CLIDemoHub --content C:\<configuration.txt file path>
   ```

Veja os módulos no seu dispositivo do IoT Edge:
    
   ```cli
   az iot hub module-identity list --device-id edge001 --hub-name CLIDemoHub
   ```

   ![Listar módulos][6]

## <a name="next-steps"></a>Passos seguintes

* Saiba como [utilizar um dispositivo do IoT Edge como um gateway](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[2]: ./media/tutorial-create-deployment-with-cli-iot-extension/create-resource-group.png
[4]: ./media/tutorial-create-deployment-with-cli-iot-extension/Create-edge-device.png
[6]: ./media/tutorial-create-deployment-with-cli-iot-extension/list-modules.png

