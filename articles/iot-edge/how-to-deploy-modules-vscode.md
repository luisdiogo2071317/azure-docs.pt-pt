---
title: Implementar módulos de limite de IoT do Azure (VS Code) | Microsoft Docs
description: Utilizar o Visual Studio Code para implementar módulos para um dispositivo de limite de IoT
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.reviewer: ''
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 7e75f2ff5e2df3189683d084a315ad6c8730be84
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036358"
---
# <a name="deploy-azure-iot-edge-modules-from-visual-studio-code"></a>Implementar os módulos do Azure IoT Edge do Visual Studio Code

Depois de criar o limite de IoT módulos com a lógica de negócio, pretende implementá-las nos seus dispositivos para funcionar no limite. Se tiver vários módulos que trabalham em conjunto para recolher e processar dados, pode implementá-las, uma vez e declarar as regras de encaminhamento que ligue-os. 

Este artigo mostra como criar um manifesto de implementação de JSON, em seguida, utilizar esse ficheiro para emitir a implementação para um dispositivo de limite de IoT. Para obter informações sobre como criar uma implementação que tenha como destino vários dispositivos com base nas respetivas etiquetas partilhadas, consulte [implementar e monitorizar os módulos de limite de IoT à escala](how-to-deploy-monitor.md)

## <a name="prerequisites"></a>Pré-requisitos

* Um [IoT hub](../iot-hub/iot-hub-create-through-portal.md) na sua subscrição do Azure. 
* Um [dispositivo de limite de IoT](how-to-register-device-portal.md) com o tempo de execução de limite de IoT instalado. 
* [Visual Studio Code](https://code.visualstudio.com/).
* [Extensão de limite de IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code. 

## <a name="configure-a-deployment-manifest"></a>Configurar um manifesto de implementação

O manifesto de implementação é um documento JSON que descreve quais os módulos que para implementar, como os dados fluem entre os módulos e propriedades pretendidas de duplos o módulo. Para obter mais informações sobre como implementação manifestos trabalho e como criá-los, consulte [compreender como os módulos de IoT limite podem ser utilizados, configurados e reutilizados](module-composition.md).

Para implementar módulos com o Visual Studio Code, guardar o manifesto de implementação localmente como um. Ficheiro JSON. Irá utilizar o caminho do ficheiro na secção seguinte, quando executar o comando para aplicar a configuração para o seu dispositivo.

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
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
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
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
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

## <a name="sign-in-to-access-your-iot-hub"></a>Iniciar sessão para aceder ao seu IoT hub

Pode utilizar as extensões de IoT do Azure para Visual Studio Code para realizar operações com o seu IoT hub. Para estas operações trabalhar, terá de iniciar sessão na sua conta do Azure e selecione o hub IoT está a trabalhar.

1. No Visual Studio Code, abra o **Explorer** vista.

2. Na parte inferior do Explorador, expanda o **dispositivos do Azure IoT Hub** secção. 

   ![Expanda os dispositivos do IoT Hub do Azure](./media/how-to-deploy-modules-vscode/azure-iot-hub-devices.png)

3. Clique em de **...**  no **dispositivos do Azure IoT Hub** cabeçalho de secção. Se não vir o botão de reticências, coloque o cursor sobre o cabeçalho. 

4. Escolha **selecione IoT Hub**.

5. Se não tiver iniciado sua conta do Azure, siga as instruções para o fazer. 

6. Selecione a sua subscrição do Azure. 

7. Selecione o seu IoT hub. 


## <a name="deploy-to-your-device"></a>Implementar no seu dispositivo

Implementar módulos para o seu dispositivo ao aplicar o manifesto de implementação que configurou com as informações do módulo. 

1. Na vista do Explorador do Visual Studio Code, expanda o **dispositivos do Azure IoT Hub** secção. 

2. Faça duplo clique no dispositivo que pretende configurar com o manifesto de implementação. 

3. Selecione **criar a implementação para o dispositivo de limite de IoT**. 

4. Navegue para o implementação ficheiro de manifesto JSON que pretende utilizar e clique em **selecionar o manifesto de implementação do contorno**. 

   ![Selecione o manifesto de implementação do contorno](./media/how-to-deploy-modules-vscode/select-deployment-manifest.png)


Os resultados da implementação são indicados na saída VS Code. Implementações com êxito são aplicadas dentro de alguns minutos, se o dispositivo de destino está em execução e ligadas à internet. 

## <a name="view-modules-on-your-device"></a>Módulos de ver no seu dispositivo

Assim que tiver implementado módulos no seu dispositivo, pode ver todos eles no **dispositivos do Azure IoT Hub** secção. Selecione a seta junto ao seu dispositivo de limite de IoT para expandi-la. São apresentados todos os módulos em execução. 

Se implementou a novos módulos recentemente num dispositivo, coloque o cursor sobre o **dispositivos do Azure IoT Hub** secção cabeçalho e selecione o ícone de atualização para atualizar a vista. 

Clique no nome de um módulo para ver e editar o duplo de módulo. 

## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar e monitorizar os módulos de limite de IoT à escala](how-to-deploy-monitor.md)