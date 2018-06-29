---
title: Implementar e monitorizar módulos para limite de IoT do Azure (CLI) | Microsoft Docs
description: Gerir os módulos que são executadas em dispositivos de limite
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 705f7bfa62154bff62b2357bd8f33c01e97404d1
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036538"
---
# <a name="deploy-and-monitor-iot-edge-modules-at-scale-using-the-azure-cli"></a>Implementar e monitorizar os módulos de limite de IoT com dimensionamento, com a CLI do Azure

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-edge-how-to-deploy-monitor-selector.md)]

Limite de IoT do Azure permite-lhe mover analytics para o limite e fornece uma interface de nuvem, para que possa gerir e monitorizar os dispositivos de IoT Edge sem ter de aceder fisicamente cada um deles. A capacidade para gerir remotamente os dispositivos é cada vez mais importante como soluções de Internet das coisas estão a crescer maiores e mais complexas. Limite de IoT do Azure foi concebido para suportar os objetivos de negócio, independentemente do quantos dispositivos a adicionar.

Pode gerir os dispositivos individuais e implementá-las módulos um de cada vez. No entanto, se pretender efetuar alterações nos dispositivos em grande escala, pode criar um **implementação automática de limite de IoT**, que faz parte automática da gestão de dispositivos do IoT Hub. As implementações são dinâmicos processos que permitem-lhe implementar vários módulos em vários dispositivos de uma só vez, controlar o estado e estado de funcionamento dos módulos e efetuar alterações quando for necessário. 

Neste artigo, configura a CLI 2.0 do Azure e a extensão de IoT. Em seguida, irá aprender a implementar módulos a um conjunto de dispositivos de limite de IoT e monitorizar o progresso utilizando os comandos da CLI disponíveis.

## <a name="cli-prerequisites"></a>Pré-requisitos do CLI

* Um [IoT hub](../iot-hub/iot-hub-create-using-cli.md) na sua subscrição do Azure. 
* [Dispositivos de limite de IoT](how-to-register-device-cli.md) com o tempo de execução de limite de IoT instalado.
* [CLI 2.0 do Azure ](https://docs.microsoft.com/cli/azure/install-azure-cli) no seu ambiente. A versão mínima da CLI 2.0 do Azure tem de ser 2.0.24 ou superior. Utilize `az –-version` para validar. Esta versão suporta comandos de extensão az e apresenta a arquitetura de comandos Knack. 
* O [extensão de IoT do Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="configure-a-deployment-manifest"></a>Configurar um manifesto de implementação

O manifesto de implementação é um documento JSON que descreve quais os módulos que para implementar, como os dados fluem entre os módulos e propriedades pretendidas de duplos o módulo. Para obter mais informações sobre como implementação manifestos trabalho e como criá-los, consulte [compreender como os módulos de IoT limite podem ser utilizados, configurados e reutilizados](module-composition.md).

Para implementar módulos com o Azure CLI 2.0, guarde o manifesto de implementação localmente como um ficheiro. txt. Irá utilizar o caminho do ficheiro na secção seguinte, quando executar o comando para aplicar a configuração para o seu dispositivo. 

Eis um manifesto de implementação básica com um módulo como exemplo:

   ```json
   {
        "content": {
         "modulesContent": {
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
   }
   ```


## <a name="identify-devices-using-tags"></a>Identificar dispositivos utilizando as etiquetas

Antes de poder criar uma implementação, tem de ser capazes de especificar quais os dispositivos que pretende afetar. Limite do Azure IoT identifica dispositivos utilizando **etiquetas** no dispositivo duplo. Cada dispositivo pode ter várias etiquetas e pode defini-las qualquer forma que faz sentido para a sua solução. Por exemplo, se gerir um campus dos edifícios inteligentes, poderá adicionar as seguintes etiquetas para um dispositivo:

```json
"tags":{
    "location":{
        "building": "20",
        "floor": "2"
    },
    "roomtype": "conference",
    "environment": "prod"
}
```

Para obter mais informações sobre dispositivos duplos e etiquetas, consulte [compreender e utilizar dispositivos duplos no IoT Hub][lnk-device-twin].

## <a name="create-a-deployment"></a>Criar uma implementação

Implementar módulos nos seus dispositivos de destino através da criação de uma implementação que consiste o manifesto de implementação, bem como outros parâmetros. 

Utilize o seguinte comando para criar uma implementação:

   ```cli
   az iot edge deployment create --deployment-id [deployment id] --labels [labels] --content [file path] --hub-name [hub name] --target-condition [target query] --priority [int]
   ```

* **– id de implementação** -o nome da implementação que será criado no IoT hub. Dê um nome exclusivo que é até 128 minúsculas da implementação. Evite espaços e os seguintes carateres inválidos: `& ^ [ ] { } \ | " < > /`.
* **-etiquetas** -adicionar etiquetas para ajudar a monitorizar as implementações. As etiquetas são nome, pares de valor que descrevem a sua implementação. Por exemplo, `HostPlatform, Linux` ou `Version, 3.0.1`
* **-conteúdo** -Filepath à implementação de manifesto JSON. 
* **-nome do hub** -nome do hub IoT em que a implementação será criada. O hub tem de ser na subscrição atual. Mudar para a subscrição com o comando pretendida `az account set -s [subscription name]`
* **-condição de destino** -introduza uma condição de destino para determinar quais os dispositivos que irão ser segmentados com esta implementação. A condição é baseada em etiquetas do dispositivo duplo ou dispositivo duplo pretendido propriedades e deve corresponder ao formato de expressão. Por exemplo, `tags.environment='test'` ou `properties.desired.devicemodel='4000x'`. 
* **-prioridade** -um número inteiro positivo. No caso de duas ou mais implementações destinam-se no mesmo dispositivo, será aplicada a implementação com o maior valor numérico de prioridade.

## <a name="monitor-a-deployment"></a>Monitorizar uma implementação

Utilize o seguinte comando para apresentar o conteúdo de uma implementação:

   ```cli
az iot edge deployment show --deployment-id [deployment id] --hub-name [hub name]
   ```
* **– id de implementação** -o nome da implementação que existe no IoT hub.
* **-nome do hub** -nome do hub IoT no qual existe a implementação. O hub tem de ser na subscrição atual. Mudar para a subscrição com o comando pretendida `az account set -s [subscription name]`

Inspecione a implementação na janela de comandos. O **métricas** listas de propriedades de uma contagem de cada métrica que é avaliada por cada hub:
* **targetedCount** -uma métrica de sistema que especifica o número de dispositivos duplos no IoT Hub que correspondem à condição filtragem.
* **appliedCount** -uma métrica de sistema Especifica o número de dispositivos que tenham a conteúdo de implementação aplicada aos respetivos duplos módulo no IoT Hub.
* **reportedSuccessfulCount** -uma métrica de dispositivo que especifica o número de dispositivos de limite na implementação de relatórios sucesso do tempo de execução do cliente de limite de IoT.
* **reportedFailedCount** -uma métrica de dispositivo que especifica o número de dispositivos de limite na implementação de relatórios de falha de tempo de execução do cliente do IoT Edge.

Pode apresentar uma lista de IDs de dispositivo ou objetos para cada uma das métricas usando o seguinte comando:

   ```cli
az iot edge deployment show-metric --deployment-id [deployment id] --metric-id [metric id] --hub-name [hub name] 
   ```

* **– id de implementação** -o nome da implementação que existe no IoT hub.
* **-métrica id** - o nome da métrica para o qual pretende ver a lista de dispositivos IDs, por exemplo `reportedFailedCount`
* **-nome do hub** -nome do hub IoT no qual existe a implementação. O hub tem de ser na subscrição atual. Mudar para a subscrição com o comando pretendida `az account set -s [subscription name]`

## <a name="modify-a-deployment"></a>Modificar uma implementação

Quando modifica uma implementação, as alterações imediatamente são replicadas para todos os dispositivos visados. 

Se atualizar a condição de destino, ocorrem as seguintes atualizações:
* Se um dispositivo não cumpre a condição de destino antigo, mas cumpre a condição de destino novo e esta implementação é a prioridade mais elevada para que o dispositivo, esta implementação é aplicada ao dispositivo. 
* Se um dispositivo atualmente em execução nesta implementação já não satisfaz a condição de destino, desinstala esta implementação e demora na implementação de prioridade mais alta seguinte. 
* Se um dispositivo atualmente em execução nesta implementação já não satisfaz a condição de destino e não satisfaz a condição de destino de todas as outras implementações, nenhuma alteração ocorre no dispositivo. O dispositivo continua em execução os respectivos módulos atuais no respetivo estado atual, mas não está a ser gerido como parte desta implementação já. Assim que cumpre a condição de destino de qualquer outra implementação, desinstala esta implementação e demora no novo. 

Utilize o seguinte comando para atualizar uma implementação:

   ```cli
az iot edge deployment update --deployment-id [deployment id] --hub-name [hub name] --set [property1.property2='value']
   ```
* **– id de implementação** -o nome da implementação que existe no IoT hub.
* **-nome do hub** -nome do hub IoT no qual existe a implementação. O hub tem de ser na subscrição atual. Mudar para a subscrição com o comando pretendida `az account set -s [subscription name]`
* **-definir** -atualizar uma propriedade na implementação. Pode atualizar as seguintes propriedades:
    * targetCondition - por exemplo `targetCondition=tags.location.state='Oregon'`
    * etiquetas 
    * prioridade


## <a name="delete-a-deployment"></a>Eliminar uma implementação

Quando elimina uma implementação, todos os dispositivos de colocar as suas implementações de maior prioridade seguinte. Se os seus dispositivos não cumprem a condição de destino de qualquer outra implementação, os módulos não são removidos quando a implementação é eliminada. 

Utilize o seguinte comando para eliminar uma implementação:

   ```cli
az iot edge deployment delete --deployment-id [deployment id] --hub-name [hub name] 
   ```
* **– id de implementação** -o nome da implementação que existe no IoT hub.
* **-nome do hub** -nome do hub IoT no qual existe a implementação. O hub tem de ser na subscrição atual. Mudar para a subscrição com o comando pretendida `az account set -s [subscription name]`

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [implementar módulos para dispositivos de limite][lnk-deployments].

<!-- Images -->
[1]: ./media/how-to-deploy-monitor/iot-edge-deployments.png

<!-- Links -->
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-portal]: https://portal.azure.com
[lnk-docker-create]: https://docs.docker.com/engine/reference/commandline/create/
[lnk-deployments]: module-deployment-monitoring.md

<!-- Anchor links -->
[anchor-monitor]: #monitor-a-deployment
