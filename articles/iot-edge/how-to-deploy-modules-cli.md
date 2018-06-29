---
title: Implementar módulos de limite de IoT do Azure (CLI) | Microsoft Docs
description: Utilizar a extensão de IoT para Azure CLI 2.0 para implementar módulos para um dispositivo de limite de IoT
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/08/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 02a7533cf1c06142f564ed9fd6008a25be9fc603
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036364"
---
# <a name="deploy-azure-iot-edge-modules-with-azure-cli-20"></a>Implementar módulos de limite de IoT do Azure com o Azure CLI 2.0

Depois de criar o limite de IoT módulos com a lógica de negócio, pretende implementá-las nos seus dispositivos para funcionar no limite. Se tiver vários módulos que trabalham em conjunto para recolher e processar dados, pode implementá-las, uma vez e declarar as regras de encaminhamento que ligue-os. 

A [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) é uma ferramenta de linha de comandos multiplataformas de código aberto para gerir recursos do Azure, como o IoT Edge. Permite-lhe gerir recursos, instâncias de serviço aprovisionamento de dispositivos e ligado hubs a Box IoT Hub do Azure. A nova extensão de IoT otimiza a CLI 2.0 do Azure com funcionalidades como a gestão de dispositivos e a capacidade completa do IoT Edge.

Este artigo mostra como criar um manifesto de implementação de JSON, em seguida, utilizar esse ficheiro para emitir a implementação para um dispositivo de limite de IoT. Para obter informações sobre como criar uma implementação que tenha como destino vários dispositivos com base nas respetivas etiquetas partilhadas, consulte [implementar e monitorizar os módulos de limite de IoT à escala](how-to-deploy-monitor-cli.md)

## <a name="prerequisites"></a>Pré-requisitos

* Um [IoT hub](../iot-hub/iot-hub-create-using-cli.md) na sua subscrição do Azure. 
* Um [dispositivo de limite de IoT](how-to-register-device-cli.md) com o tempo de execução de limite de IoT instalado.
* [CLI 2.0 do Azure ](https://docs.microsoft.com/cli/azure/install-azure-cli) no seu ambiente. A versão mínima da CLI 2.0 do Azure tem de ser 2.0.24 ou superior. Utilize `az –-version` para validar. Esta versão suporta comandos de extensão az e apresenta a arquitetura de comandos Knack. 
* O [extensão de IoT do Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Configurar um manifesto de implementação

O manifesto de implementação é um documento JSON que descreve quais os módulos que para implementar, como os dados fluem entre os módulos e propriedades pretendidas de duplos o módulo. Para obter mais informações sobre como implementação manifestos trabalho e como criá-los, consulte [compreender como os módulos de IoT limite podem ser utilizados, configurados e reutilizados](module-composition.md).

Para implementar módulos com o Azure CLI 2.0, guarde o manifesto de implementação localmente como um ficheiro. txt. Irá utilizar o caminho do ficheiro na secção seguinte, quando executar o comando para aplicar a configuração para o seu dispositivo. 

Eis um manifesto de implementação básica com um módulo como exemplo:

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
               "loggingOptions": "",
               "registryCredentials": {
                 "registryName": {
                   "username": "",
                   "password": "",
                   "address": ""
                 }
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
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
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

## <a name="deploy-to-your-device"></a>Implementar no seu dispositivo

Implementar módulos para o seu dispositivo ao aplicar o manifesto de implementação que configurou com as informações do módulo. 

Utilize o seguinte comando para aplicar a configuração para um dispositivo de limite de IoT:

   ```cli
   az iot hub apply-configuration --device-id [device id] --hub-name [hub name] --content [file path]
   ```

O parâmetro de id de dispositivo é maiúsculas e minúsculas. Os pontos de parâmetro de conteúdo para a implementação do manifestam do ficheiro que guardou. 

## <a name="view-modules-on-your-device"></a>Módulos de ver no seu dispositivo

Assim que tiver implementado módulos no seu dispositivo, pode ver todos eles com o seguinte comando: 

Veja os módulos no seu dispositivo do IoT Edge:
    
   ```cli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

O parâmetro de id de dispositivo é maiúsculas e minúsculas.

   ![Listar módulos](./media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar e monitorizar os módulos de limite de IoT à escala](how-to-deploy-monitor.md)