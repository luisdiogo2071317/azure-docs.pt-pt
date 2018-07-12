---
title: Utilizar a telemetria dinâmica | Documentos da Microsoft
description: Siga este tutorial para aprender a utilizar a telemetria dinâmica com a solução pré-configurada de monitorização remota do Azure IoT Suite.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 562799dc-06ea-4cdd-b822-80d1f70d2f09
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 60e9ee00fabf15a62e782c70bca251b1a8e617c3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38724002"
---
# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Utilizar a telemetria dinâmica com a solução pré-configurada de monitorização remota

A telemetria dinâmica permite-lhe visualizar qualquer telemetria enviada para a solução pré-configurada de monitorização remota. Os dispositivos simulados que implementar a solução pré-configurada enviam telemetria de temperatura e umidade, que pode ver no dashboard. Se personalizar dispositivos simulados existentes, criar novos dispositivos simulados ou ligar dispositivos físicos à solução pré-configurada pode enviar outros valores de telemetria como a temperatura externa, RPM ou windspeed. Em seguida, é possível visualizar esta telemetria adicional no dashboard.

Este tutorial utiliza um dispositivo simulado de node. js simple que pode ser facilmente modificado para fazer experiências com a telemetria dinâmica.

Para concluir este tutorial, terá de:

* Uma subscrição ativa do Azure. Se não tiver uma conta, pode criar uma de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Azure Free Trial (Avaliação Gratuita do Azure)][lnk_free_trial].
* [NODE. js] [ lnk-node] versão 0.12.x ou posterior.

Pode concluir este tutorial em qualquer sistema operativo, como Windows ou Linux, onde pode instalar node. js.

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

[!INCLUDE [iot-suite-v1-send-external-temperature](../../includes/iot-suite-v1-send-external-temperature.md)]

## <a name="add-a-telemetry-type"></a>Adicionar um tipo de telemetria

A próxima etapa é substituir a telemetria gerada pelos dispositivos simulados node. js com um novo conjunto de valores:

1. Parar o dispositivo simulado do node. js, escrevendo **Ctrl + C** na sua linha de comandos ou shell.
2. No ficheiro remote_monitoring.js, pode ver os valores de base de dados para o existente de temperatura, humidade e telemetria de temperatura externa. Adicione um valor de base de dados para **rpm** da seguinte forma:

    ```nodejs
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. O node. js simulated dispositivo utiliza o **generateRandomIncrement** função no arquivo remote_monitoring.js para adicionar um incremento aleatório para os valores de base de dados. Tornar aleatórios a **rpm** valor ao adicionar uma linha de código após os randomizations existentes da seguinte forma:

    ```nodejs
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. Adicione o novo valor de rpm para o payload JSON, que o dispositivo envia para o IoT Hub:

    ```nodejs
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Execute o dispositivo simulado do node. js com o seguinte comando:

    `node remote_monitoring.js`

6. Observe o novo tipo de telemetria RPM apresenta no gráfico no dashboard:

![Adicionar o RPM ao dashboard][image3]

> [!NOTE]
> Poderá ter de desativar e, em seguida, ativar o dispositivo de node. js no **dispositivos** página no dashboard para ver a alteração imediatamente.

## <a name="customize-the-dashboard-display"></a>Personalizar a apresentação de dashboard

O **Device-Info** mensagem pode incluir metadados sobre a telemetria do dispositivo pode enviar para o IoT Hub. Estes metadados, podem especificar os tipos de telemetria que o dispositivo envia. Modificar a **deviceMetaData** valor no ficheiro remote_monitoring.js para incluir um **telemetria** seguinte da definição da **comandos** definição. O código seguinte trecho mostra a **comandos** definição (não se esqueça de adicionar um `,` depois do **comandos** definição):

```nodejs
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [!NOTE]
> Solução de monitorização remota utiliza uma correspondência de maiúsculas e minúsculas para comparar a definição de metadados com dados no fluxo de telemetria.


Adicionar uma **telemetria** definição, conforme mostrado no trecho de código anterior não altera o comportamento do dashboard. No entanto, os metadados também podem incluir uma **DisplayName** atributo para personalizar a apresentação no dashboard. Atualização do **telemetria** definição de metadados, como mostrado no seguinte fragmento:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Captura de ecrã seguinte mostra como essa alteração modifica a legenda do gráfico no dashboard:

![Personalizar a legenda do gráfico][image4]

> [!NOTE]
> Poderá ter de desativar e, em seguida, ativar o dispositivo de node. js no **dispositivos** página no dashboard para ver a alteração imediatamente.

## <a name="filter-the-telemetry-types"></a>Filtrar os tipos de telemetria

Por predefinição, o gráfico no dashboard mostra cada série de dados no fluxo de telemetria. Pode utilizar o **Device-Info** metadados para suprimir a apresentação de tipos de telemetria específica no gráfico. 

Para fazer com que o gráfico a mostrar apenas telemetria de temperatura e Umidade, omita **ExternalTemperature** partir do **Device-Info** **telemetria** metadados da seguinte forma:

```nodejs
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

O **temperatura equipamentos esportivos** já não é apresentado no gráfico:

![Filtrar a telemetria no dashboard][image5]

Esta alteração afeta apenas a exibição de gráfico. O **ExternalTemperature** valores de dados ainda são armazenados e disponibilizados para qualquer processamento de back-end.

> [!NOTE]
> Poderá ter de desativar e, em seguida, ativar o dispositivo de node. js no **dispositivos** página no dashboard para ver a alteração imediatamente.

## <a name="handle-errors"></a>Lidar com erros

Para um fluxo de dados apresentar no gráfico, seus **tipo** no **Device-Info** metadados tem de corresponder ao tipo de dados dos valores de telemetria. Por exemplo, se os metadados Especifica que o **tipo** de humidade os dados são **int** e um **duplo** se encontra no fluxo de telemetria de, em seguida, não apresenta a telemetria de humidade no gráfico. No entanto, o **humidade** valores ainda são armazenados e disponibilizados para qualquer processamento de back-end.

## <a name="next-steps"></a>Passos Seguintes

Agora que viu como usar a telemetria dinâmica, pode saber mais sobre como as soluções pré-configuradas utilizam as informações do dispositivo: [solução pré-configurada de metadados de informações do dispositivo na monitorização remota] [ lnk-devinfo].

[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[image3]: media/iot-suite-v1-dynamic-telemetry/image3.png
[image4]: media/iot-suite-v1-dynamic-telemetry/image4.png
[image5]: media/iot-suite-v1-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
