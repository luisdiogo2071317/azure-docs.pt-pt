---
title: Configurar e monitorizar dispositivos de IoT em escala com o Azure IoT Hub (CLI) | Documentos da Microsoft
description: Utilizar configurações de automático de dispositivos do IoT Hub do Azure para atribuir uma configuração para vários dispositivos
author: ChrisGMsft
manager: bruz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: chrisgre
ms.openlocfilehash: f81ef3c231874f314d6fe023ba247a0bcff61e90
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/13/2018
ms.locfileid: "42054765"
---
# <a name="configure-and-monitor-iot-devices-at-scale-using-the-azure-cli"></a>Configurar e monitorizar dispositivos de IoT em escala com a CLI do Azure

[!INCLUDE [iot-edge-how-to-deploy-monitor-selector](../../includes/iot-hub-auto-device-config-selector.md)]

Gerenciamento automático de dispositivos no IoT Hub do Azure automatiza muitas das tarefas repetitivas e complexas de gerenciamento de frotas de dispositivo grandes sobre a totalidade dos respetivos ciclos de vida. Com a gestão de dispositivos automático, pode direcionar um conjunto de dispositivos com base nas respetivas propriedades, definir uma configuração desejada e permitir que o IoT Hub, atualize os dispositivos sempre que eles vão para o âmbito.  Este procedimento é efetuado com uma configuração de dispositivos automático, que também permitirá que resumir conclusão e a conformidade, a mesclagem de identificador e a conflitos e implementar configurações numa abordagem faseada.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Trabalho de configurações de automático de dispositivos ao atualizar um conjunto de dispositivos duplos com as propriedades pretendidas e um resumo com base no dispositivo duplo de relatórios de propriedades comunicadas.  Ele introduz uma nova classe e o documento JSON chamado um *configuração* que possui três partes:

* O **condição de destino** define o âmbito dos dispositivos duplos para ser atualizado. A condição de destino é especificada como uma consulta nas etiquetas do dispositivo duplo e/ou propriedades comunicadas.

* O **conteúdo de destino** define as propriedades pretendidas para ser adicionados ou atualizados nos gémeos de dispositivo de destino. O conteúdo inclui um caminho para a secção de propriedades pretendidas para serem alterados.

* O **métricas** definem as contagens de resumidas de vários Estados de configuração, tais como **êxito**, **em curso**, e **erro**. Métricas personalizadas são especificadas como consultas no dispositivo duplo de propriedades comunicadas.  As métricas do sistema são métricas predefinidas que medem o estado de atualização de duplo, como o número de dispositivos duplos, que são visados e o número de duplos foram atualizadas com êxito. 

## <a name="cli-prerequisites"></a>Pré-requisitos CLI

* Uma [IoT hub](../iot-hub/iot-hub-create-using-cli.md) na sua subscrição do Azure. 
* [CLI 2.0 do Azure ](https://docs.microsoft.com/cli/azure/install-azure-cli) no seu ambiente. A versão mínima da CLI 2.0 do Azure tem de ser 2.0.24 ou superior. Utilize `az –-version` para validar. Esta versão suporta comandos de extensão az e apresenta a arquitetura de comandos Knack. 
* O [extensão de IoT do Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

## <a name="implement-device-twins-to-configure-devices"></a>Implementar dispositivos duplos para configurar dispositivos

Configurações de dispositivos automático requerem a utilização de dispositivos duplos para sincronizar o estado entre a cloud e dispositivos.  Consulte a [compreender e utilizar dispositivos duplos no IoT Hub](iot-hub-devguide-device-twins.md) para obter orientações sobre como utilizar dispositivos duplos.

## <a name="identify-devices-using-tags"></a>Identificar os dispositivos utilizando etiquetas

Antes de poder criar uma configuração, tem de especificar quais os dispositivos que quer afetar. O IoT Hub do Azure identifica dispositivos utilizando as etiquetas no dispositivo duplo. Cada dispositivo pode ter várias etiquetas e pode defini-las qualquer forma que faça sentido para a sua solução. Por exemplo, se gerir dispositivos em diferentes locais, pode adicionar as seguintes etiquetas para um dispositivo duplo:

```json
"tags": {
    "location": {
        "state": "Washington",
        "city": "Tacoma"
    }
},
```

## <a name="define-the-target-content-and-metrics"></a>Definir o conteúdo de destino e métricas

O conteúdo de destino e métrica consultas especificadas como documentos JSON que descrevem o dispositivo duplo propriedades desejadas ao conjunto e propriedades comunicadas para ser medido.  Para criar uma configuração do dispositivo automática com a CLI 2.0 do Azure, salve o conteúdo de destino e as métricas localmente como arquivos. txt. Utilize os caminhos de ficheiro numa secção posterior seguinte ao executar o comando para aplicar a configuração para o seu dispositivo. 

Eis um exemplo de conteúdo do destino básico:

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
   az iot hub configuration create --config-id [configuration id] \
     --labels [labels] --content [file path] --hub-name [hub name] \
     --target-condition [target query] --priority [int] \
     --metrics [metric queries]
```

* --**id de configuração** -o nome da configuração que será criada no IoT hub. Dê um nome exclusivo que é até 128 minúsculas da configuração. Evite espaços e os seguintes carateres inválidos: `& ^ [ ] { } \ | " < > /`.

* --**etiquetas** -adicionar etiquetas para ajudar a controlar a configuração. As etiquetas são pares de valor que descrevem a implementação, nome de. Por exemplo, `HostPlatform, Linux` ou `Version, 3.0.1`

* --**conteúdo** -propriedades pretendidas do JSON de Inline ou caminho do ficheiro para o conteúdo de destino para ser definida como duplo. 

* --**nome do hub** -nome do hub IoT em que a configuração será criada. O hub tem de ser na subscrição atual. Mude para a subscrição pretendida com o comando `az account set -s [subscription name]`

* --**condição de destino** -introduza uma condição de destino para determinar quais os dispositivos que serão visados para esta configuração. A condição baseia-se nas etiquetas de twin do dispositivo ou o dispositivo duplo propriedades pretendidas e deve corresponder ao formato de expressão. Por exemplo, `tags.environment='test'` ou `properties.desired.devicemodel='4000x'`. 

* --**prioridade** -um número inteiro positivo. No caso de dois ou mais configurações destinam-se no mesmo dispositivo, será aplicada a configuração com o maior valor numérico para prioridade.

* --**métricas** -Filepath para as consultas de métricas. Métricas fornecem contagens de resumidas dos diversos Estados em que um dispositivo pode comunicar de volta como resultado de aplicar o conteúdo de configuração. Por exemplo, pode criar uma métrica para definições alterações pendentes, uma métrica para erros e uma métrica para alterações de definições concluída com êxito. 

## <a name="monitor-a-configuration"></a>Uma configuração de monitor

Utilize o seguinte comando para exibir o conteúdo de uma configuração de:

```cli
az iot hub configuration show --config-id [configuration id] \
  --hub-name [hub name]
```

* --**id de configuração** -o nome da configuração que existe no IoT hub.

* --**nome do hub** -nome do hub IoT na qual existe a configuração. O hub tem de ser na subscrição atual. Mude para a subscrição pretendida com o comando `az account set -s [subscription name]`

Verifique a configuração na janela de comando. O **métricas** listas de propriedades de uma contagem para cada métrica que é avaliada por cada hub:

* **targetedCount** -uma métrica de sistema que especifica o número de dispositivos duplos no IoT Hub que correspondem à condição de destino.

* **appliedCount** -uma métrica do sistema Especifica o número de dispositivos que tenham o conteúdo de destino aplicado.

* **A métrica personalizada** -qualquer métricas definidas serão consideradas métricas de utilizador.

Pode mostrar uma lista de identificações de dispositivo ou objetos para cada uma das métricas de utilizando o seguinte comando:

```cli
az iot hub configuration show-metric --config-id [configuration id] \
   --metric-id [metric id] --hub-name [hub name] --metric-type [type] 
```

* --**id de configuração** -o nome da implementação que existe no IoT hub.

* --**id de métrica** – o nome da métrica para o qual pretende ver a lista de dispositivos IDs, por exemplo `appliedCount`.

* --**nome do hub** -nome do hub IoT na qual existe a implementação. O hub tem de ser na subscrição atual. Mude para a subscrição pretendida com o comando `az account set -s [subscription name]`.

* --**tipo de métrica** -pode ser o tipo de métrica `system` ou `user`.  As métricas do sistema são `targetedCount` e `appliedCount`. Todas as outras métricas são métricas de utilizador.

## <a name="modify-a-configuration"></a>Modificar uma configuração

Quando modifica uma configuração, as alterações são replicadas imediatamente para todos os dispositivos direcionados. 

Se atualizar a condição de destino, ocorrem as seguintes atualizações:

* Se um dispositivo duplo não cumpre a condição de destino antigo, mas que atenda à nova condição de destino e esta configuração é a prioridade mais alta para esse dispositivo duplo, esta configuração é aplicada para o dispositivo duplo. 

* Se um dispositivo duplo já não cumpre a condição de destino, serão possível remover as definições da configuração e o dispositivo duplo será modificado pela configuração de prioridade mais alta seguinte. 

* Se um dispositivo duplo atualmente em execução esta configuração já não cumpre a condição de destino e não cumpre a condição de destino de quaisquer outras configurações, em seguida, as definições da configuração serão removidas e nenhuma outra alteração serão feitas no duplo. 

Utilize o seguinte comando para atualizar uma configuração:

```cli
az iot hub configuration update --config-id [configuration id] \
   --hub-name [hub name] --set [property1.property2='value']
```

* --**id de configuração** -o nome da configuração que existe no IoT hub.

* --**nome do hub** -nome do hub IoT na qual existe a configuração. O hub tem de ser na subscrição atual. Mude para a subscrição pretendida com o comando `az account set -s [subscription name]`.

* --**definir** -atualizar uma propriedade na configuração. Pode atualizar as seguintes propriedades:

    * targetCondition - por exemplo `targetCondition=tags.location.state='Oregon'`

    * etiquetas 

    * prioridade

## <a name="delete-a-configuration"></a>Eliminar uma configuração

Quando elimina uma configuração, quaisquer dispositivos duplos assumem a seguinte configuração de prioridade mais alta. Se os dispositivos duplos não cumprem a condição de destino de qualquer outra configuração, não existem outras definições são aplicadas. 

Utilize o seguinte comando para eliminar uma configuração:

```cli
az iot hub configuration delete --config-id [configuration id] \
   --hub-name [hub name] 
```
* --**id de configuração** -o nome da configuração que existe no IoT hub.

* --**nome do hub** -nome do hub IoT na qual existe a configuração. O hub tem de ser na subscrição atual. Mude para a subscrição pretendida com o comando `az account set -s [subscription name]`.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como configurar e monitorizar dispositivos de IoT em escala. Siga estas ligações para saber mais sobre como gerir o IoT Hub do Azure:

* [Gerir as identidades de dispositivo do IoT Hub em massa](iot-hub-bulk-identity-mgmt.md)
* [Métricas do IoT Hub](iot-hub-metrics.md)
* [Monitorização de operações](iot-hub-operations-monitoring.md)

Para explorar ainda mais os recursos do IoT Hub, veja:

* [guia para programadores do IoT Hub](iot-hub-devguide.md)
* [Implementar o AI em dispositivos de ponta com o Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)

Para explorar, utilizando o serviço de aprovisionamento de dispositivos do IoT Hub para ativar o aprovisionamento sem toque e just-in-time, consulte: 

* [Serviço Aprovisionamento de Dispositivos no Hub IoT do Azure](/azure/iot-dps)
