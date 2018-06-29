---
title: Configurar e monitorizar dispositivos de IoT de escala com o Azure IoT Hub (CLI) | Microsoft Docs
description: Utilizar configurações de automático de dispositivos do IoT Hub do Azure para atribuir uma configuração em vários dispositivos
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: c12a07aabdecb070cfa99f8851f907499599a1fc
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036724"
---
# <a name="configure-and-monitor-iot-devices-at-scale-using-the-azure-cli"></a>Configurar e monitorizar dispositivos de IoT com dimensionamento, com a CLI do Azure

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Gestão de dispositivos automática no IoT Hub do Azure automatiza muitas das tarefas repetitivas e complexas de gerir dispositivos grande fleets através da totalidade da respetiva ciclos de vida. Com a gestão de dispositivos automático, pode um conjunto de dispositivos com base nas respetivas propriedades de destino, definir uma configuração desejada e permitir que o IoT Hub, atualize-os sempre que entram em âmbito.  Este procedimento é efetuado através de uma configuração de dispositivos automático, que também permitirá resumir conclusão e conformidade, identificador de intercalação e entra em conflito e implementar configurações numa abordagem faseada.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Trabalho de configurações automático de dispositivos através da atualização de um conjunto de dispositivos duplos com propriedades pretendidas e reporta um resumo com base no dispositivo duplo comunicadas propriedades.  Introduz uma nova classe e o documento JSON chamado um _configuração_ que tem três partes:

* O **condição de destino** define o âmbito dos dispositivos duplos ser atualizados. A condição de destino é especificada como uma consulta em tags de duplo de dispositivo e/ou registadas propriedades.

* O **destino conteúdo** define as propriedades pretendidas para ser adicionados ou atualizados nos dispositivos visados duplos. O conteúdo inclui um caminho para a secção de propriedades pretendidas para ser alterada.

* O **métricas** definir o número de resumo de vários Estados de configuração, tais como **êxito**, **em curso**, e **erro**. Métricas personalizadas são especificadas como consultas no dispositivo duplo comunicadas propriedades.  Métricas de sistema são nas métricas predefinidas que medem o estado de atualização de duplo, tais como o número de dispositivos duplos destinadas e o número de duplos foram atualizadas com êxito. 

## <a name="cli-prerequisites"></a>Pré-requisitos do CLI

* Um [IoT hub](../iot-hub/iot-hub-create-using-cli.md) na sua subscrição do Azure. 
* [CLI 2.0 do Azure ](https://docs.microsoft.com/cli/azure/install-azure-cli) no seu ambiente. A versão mínima da CLI 2.0 do Azure tem de ser 2.0.24 ou superior. Utilize `az –-version` para validar. Esta versão suporta comandos de extensão az e apresenta a arquitetura de comandos Knack. 
* O [extensão de IoT do Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="implement-device-twins-to-configure-devices"></a>Implementar dispositivos duplos para configurar dispositivos

Configurações de dispositivos automático requerem a utilização de dispositivos duplos sincronizar estado entre a nuvem e de dispositivos.  Consulte [compreender e utilizar dispositivos duplos no IoT Hub] [ lnk-device-twin] para obter orientações sobre como utilizar dispositivos duplos.

## <a name="identify-devices-using-tags"></a>Identificar dispositivos utilizando as etiquetas

Antes de poder criar uma configuração, tem de especificar quais os dispositivos que pretende afetar. IoT Hub do Azure identifica dispositivos utilizando as etiquetas no dispositivo duplo. Cada dispositivo pode ter várias etiquetas e pode defini-las qualquer forma que faz sentido para a sua solução. Por exemplo, se gerir dispositivos em diferentes localizações, pode adicionar as seguintes etiquetas para um dispositivo duplo:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```
## <a name="define-the-target-content-and-metrics"></a>Definir o conteúdo de destino e métricas

O conteúdo de destino e métrica consultas especificadas como documentos JSON que descrevem o dispositivo duplo propriedades pretendidas para ser conjunto e propriedades comunicadas para ser medido.  Para criar uma configuração de dispositivos automático a utilizar o Azure CLI 2.0, guarde o conteúdo de destino e métrica localmente como ficheiros. txt. Irá utilizar os caminhos de ficheiro numa secção posterior seguinte ao executar o comando para aplicar a configuração para o seu dispositivo. 

Eis um exemplo de conteúdo de destino básico:

```json
{
  "content": {
    "deviceContent": {
      "properties.desired.chillerWaterSettings": {
        "temperature": 38,
        "pressure": 78
      }
    }
}
```

Seguem-se exemplos de consultas de métricas:

```json
{
  "queries": {
    "Compliant": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='current'",
    "Error": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='error'",
    "Pending": "select deviceId from devices where configurations.[[chillersettingswashington]].status = 'Applied' AND properties.reported.chillerWaterSettings.status='pending'"
  }
}
```

## <a name="create-a-configuration"></a>Criar uma configuração

Configurar dispositivos de destino através da criação de uma configuração que consiste o conteúdo de destino e as métricas. 

Utilize o seguinte comando para criar uma configuração:

   ```cli
   az iot hub configuration create --config-id [configuration id] --labels [labels] --content [file path] --hub-name [hub name] --target-condition [target query] --priority [int] --metrics [metric queries]
   ```

* **id de configuração -** -o nome da configuração que será criada no IoT hub. Dê um nome exclusivo que é até 128 letras minúsculas da configuração. Evite espaços e os seguintes carateres inválidos: `& ^ [ ] { } \ | " < > /`.
* **-etiquetas** -adicionar etiquetas para ajudar a controlar a configuração. As etiquetas são nome, pares de valor que descrevem a sua implementação. Por exemplo, `HostPlatform, Linux` ou `Version, 3.0.1`
* **-conteúdo** -Inline JSON ou caminho do ficheiro para o conteúdo de destino a ser definido como duplo pretendido propriedades. 
* **-nome do hub** -nome do hub IoT em que a configuração será criada. O hub tem de ser na subscrição atual. Mudar para a subscrição com o comando pretendida `az account set -s [subscription name]`
* **-condição de destino** -introduza uma condição de destino para determinar quais os dispositivos visados com esta configuração. A condição é baseada em etiquetas do dispositivo duplo ou dispositivo duplo pretendido propriedades e deve corresponder ao formato de expressão. Por exemplo, `tags.environment='test'` ou `properties.desired.devicemodel='4000x'`. 
* **-prioridade** -um número inteiro positivo. No caso de duas ou mais configurações destinam-se no mesmo dispositivo, será aplicada a configuração com o maior valor numérico de prioridade.
* **-as métricas** -Filepath para as consultas de métricas. Métricas fornecem contagens de resumidas dos Estados de vários que um dispositivo pode informar como resultado de aplicar o conteúdo de configuração. Por exemplo, pode criar uma métrica para pendentes alterações de definições, uma métrica de erros e uma métrica para as alterações de definições com êxito. 

## <a name="monitor-a-configuration"></a>Monitorizar uma configuração

Utilize o seguinte comando para apresentar o conteúdo de uma configuração:

   ```cli
az iot hub configuration show --config-id [configuration id] --hub-name [hub name]
   ```
* **id de configuração -** -o nome da configuração que existe no IoT hub.
* **-nome do hub** -nome do hub IoT no qual existe a configuração. O hub tem de ser na subscrição atual. Mudar para a subscrição com o comando pretendida `az account set -s [subscription name]`

Verifique a configuração na janela de comandos. O **métricas** listas de propriedades de uma contagem de cada métrica que é avaliada por cada hub:
* **targetedCount** -uma métrica de sistema que especifica o número de dispositivos duplos no IoT Hub que correspondem à condição filtragem.
* **appliedCount** -uma métrica de sistema Especifica o número de dispositivos que tenham o conteúdo de destino aplicado.
* **A métrica personalizada** -as métricas que definiu serão consideradas métricas de utilizador.

Pode apresentar uma lista de IDs de dispositivo ou objetos para cada uma das métricas usando o seguinte comando:

   ```cli
az iot hub configuration show-metric --config-id [configuration id] --metric-id [metric id] --hub-name [hub name] --metric-type [type] 
   ```

* **id de configuração -** -o nome da implementação que existe no IoT hub.
* **-métrica id** - o nome da métrica para o qual pretende ver a lista de dispositivos IDs, por exemplo `appliedCount`
* **-nome do hub** -nome do hub IoT no qual existe a implementação. O hub tem de ser na subscrição atual. Mudar para a subscrição com o comando pretendida `az account set -s [subscription name]`
* **-métrica tipo** -métrico tipo pode ser `system` ou `user`.  Métricas de sistema são `targetedCount` e `appliedCount`. Todas as outras métricas são métricas de utilizador.

## <a name="modify-a-configuration"></a>Modificar uma configuração

Quando modifica uma configuração, as alterações imediatamente são replicadas para todos os dispositivos visados. 

Se atualizar a condição de destino, ocorrem as seguintes atualizações:
* Se um dispositivo duplo não satisfaz a condição de destino antigo, mas se cumpre a condição de destino novo e esta configuração é a prioridade mais elevada para esse dispositivo duplo, em seguida, esta configuração é aplicada ao dispositivo duplo. 
* Se um dispositivo duplo já não satisfaz a condição de destino, as definições da configuração serão removidas e o dispositivo duplo irá ser modificado pela seguinte configuração de prioridade mais elevada. 
* Se um dispositivo duplo atualmente em execução esta configuração já não satisfaz a condição de destino e não satisfaz a condição de destino de quaisquer outras configurações, em seguida, as definições da configuração serão removidas e outras alterações não serão efetuadas no duplo. 

Utilize o seguinte comando para atualizar uma configuração:

   ```cli
az iot hub configuration update --config-id [configuration id] --hub-name [hub name] --set [property1.property2='value']
   ```
* **id de configuração -** -o nome da configuração que existe no IoT hub.
* **-nome do hub** -nome do hub IoT no qual existe a configuração. O hub tem de ser na subscrição atual. Mudar para a subscrição com o comando pretendida `az account set -s [subscription name]`
* **-definir** -atualizar uma propriedade na configuração. Pode atualizar as seguintes propriedades:
    * targetCondition - por exemplo `targetCondition=tags.location.state='Oregon'`
    * etiquetas 
    * prioridade

## <a name="delete-a-configuration"></a>Eliminar uma configuração

Quando elimina uma configuração, quaisquer dispositivos duplos execute a seguinte configuração de prioridade mais elevada. Se os dispositivos duplos não satisfazem a condição de destino de qualquer outra configuração, não existem outras definições são aplicadas. 

Utilize o seguinte comando para eliminar uma configuração:

   ```cli
az iot hub configuration delete --config-id [configuration id] --hub-name [hub name] 
   ```
* **id de configuração -** -o nome da configuração que existe no IoT hub.
* **-nome do hub** -nome do hub IoT no qual existe a configuração. O hub tem de ser na subscrição atual. Mudar para a subscrição com o comando pretendida `az account set -s [subscription name]`

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu como configurar e monitorizar dispositivos de IoT à escala. Siga estas ligações para saber mais sobre a gestão do Azure IoT Hub:

* [Gerir as identidades de dispositivo do IoT Hub em massa][lnk-bulkIDs]
* [Métricas de IoT Hub][lnk-metrics]
* [Operações de monitorização][lnk-monitor]

Para explorar ainda mais as capacidades do IoT Hub, consulte:

* [Guia para programadores do IoT Hub][lnk-devguide]
* [Implementar o AI em dispositivos de ponta com o Azure IoT Edge][lnk-iotedge]

Para explorar, utilizando o serviço de aprovisionamento de dispositivos do IoT Hub para ativar o aprovisionamento de zero touch, just-in-time, consulte: 

* [Serviço de aprovisionamento de dispositivos de IoT Hub do Azure][lnk-dps]

[lnk-device-twin]: iot-hub-devguide-device-twins.md
[lnk-bulkIDs]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dps]: https://azure.microsoft.com/documentation/services/iot-dps
[lnk-portal]: https://portal.azure.com
