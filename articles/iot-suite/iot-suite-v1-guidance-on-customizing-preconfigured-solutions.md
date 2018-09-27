---
title: Personalizar soluções pré-configuradas | Documentos da Microsoft
description: Fornece orientações sobre como personalizar as soluções pré-configuradas do Azure IoT Suite.
services: ''
suite: iot-suite
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 4653ae53-4110-4a10-bd6c-7dc034c293a8
ms.service: iot-suite
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: cb5955111cb3954f71f11602042b5153ccee3473
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106921"
---
# <a name="customize-a-preconfigured-solution"></a>Personalizar uma solução pré-configurada

As soluções pré-configuradas fornecidas com o Azure IoT Suite demonstram os serviços no suite trabalhar em conjunto para fornecer uma solução ponto-a-ponto. A partir deste ponto de partida, existem vários lugares em que pode estender e personalizar a solução para cenários específicos. As secções seguintes descrevem estes pontos de personalização comuns.

## <a name="find-the-source-code"></a>Encontrar o código de origem

O código-fonte para as soluções pré-configuradas está disponível no GitHub nos repositórios seguintes:

* Monitorização remota: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
* Manutenção preditiva: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)
* Fábrica ligada: [https://github.com/Azure/azure-iot-connected-factory](https://github.com/Azure/azure-iot-connected-factory)

O código-fonte para as soluções pré-configuradas é fornecido para demonstrar os padrões e práticas utilizadas para implementar a funcionalidade de ponta a ponta de uma solução de IoT com o Azure IoT Suite. Pode encontrar mais informações sobre como criar e implementar as soluções em repositórios do GitHub.

## <a name="change-the-preconfigured-rules"></a>Alterar as regras pré-configuradas

Solução de monitorização remota inclui três [do Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) trabalhos para lidar com as informações do dispositivo, telemetria e lógica de regras na solução.

As três tarefas do stream analytics e sua sintaxe são descritas em profundidade no [instruções da solução pré-configurada de monitorização remota](iot-suite-v1-remote-monitoring-sample-walkthrough.md). 

Pode editar estas tarefas diretamente para alterar a lógica ou adicionar lógica específica ao seu cenário. Pode encontrar tarefas do Stream Analytics da seguinte forma:

1. Aceda a [portal do Azure](https://portal.azure.com).
2. Navegue para o grupo de recursos com o mesmo nome que a sua solução de IoT. 
3. Selecione a tarefa do Azure Stream Analytics que pretende modificar. 
4. Parar a tarefa selecionando **parar** no conjunto de comandos. 
5. Edite as entradas, consulta e saídas.
   
    Uma modificação simple é alterar a consulta para o **regras** tarefa para utilizar um **"<"** em vez de um **">"**. O portal de solução continua a mostrar **">"** quando editar uma regra, mas observe como o comportamento é invertido devido à alteração na tarefa subjacente.
6. Iniciar a tarefa

> [!NOTE]
> O dashboard de monitorização remota depende dados específicos, para que alterar as tarefas pode fazer com que o dashboard efetuar a ativação.

## <a name="add-your-own-rules"></a>Adicionar suas próprias regras

Além de alterar as tarefas pré-configuradas do Azure Stream Analytics, pode utilizar o portal do Azure para adicionar novas tarefas ou adicionar novas consultas para as tarefas existentes.

## <a name="customize-devices"></a>Personalizar dispositivos

Uma das atividades de extensão mais comuns é a trabalhar com dispositivos específicos ao seu cenário. Existem vários métodos para trabalhar com dispositivos. Estes métodos incluem a alteração de um dispositivo simulado para corresponder ao seu cenário ou ao utilizar o [SDK de dispositivo IoT] [ IoT Device SDK] para ligar o dispositivo físico para a solução.

Para obter um guia passo a passo para adicionar dispositivos, consulte a [dispositivos de ligação do Iot Suite](iot-suite-v1-connecting-devices.md) artigo e o [exemplo de SDK de C de monitorização remota](https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer/samples/remote_monitoring). Este exemplo foi concebido para funcionar com a solução pré-configurada de monitorização remota.

### <a name="create-your-own-simulated-device"></a>Criar seu próprio dispositivo simulado

Incluído nos [código fonte da solução de monitorização remota](https://github.com/Azure/azure-iot-remote-monitoring), é um simulador de .NET. Este simulador é aquele aprovisionada como parte da solução e pode alterá-lo para enviar metadados diferentes, telemetria e responder a comandos diferentes e métodos.

O simulador pré-configurada na solução pré-configurada de monitorização remota simula um dispositivo mais legal que emite a telemetria de temperatura e humidade. Pode modificar o simulador no [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) quando tiver bifurcou o repositório do GitHub do projeto.

### <a name="available-locations-for-simulated-devices"></a>Localizações disponíveis para dispositivos simulados

O conjunto padrão de localizações está em Seattle/Redmond, Washington, Estados Unidos da América. Pode alterar estas localizações na [SampleDeviceFactory.cs][lnk-sample-device-factory].

### <a name="add-a-desired-property-update-handler-to-the-simulator"></a>Adicionar um processador de atualização de propriedade pretendida para o simulador

Pode definir um valor para uma propriedade pretendida para um dispositivo no portal da solução. É da responsabilidade do dispositivo para lidar com a propriedade de pedido de alteração, quando o dispositivo obtém o valor da propriedade pretendida. Para adicionar suporte para uma alteração do valor de propriedade por meio de uma propriedade pretendida, terá de adicionar um manipulador para o simulador.

O simulador contém manipuladores para o **SetPointTemp** e **TelemetryInterval** propriedades que pode atualizar, definindo assim o desejar valores no portal da solução.

O exemplo seguinte mostra o manipulador para o **SetPointTemp** propriedade no desejada a **CoolerDevice** classe:

```csharp
protected async Task OnSetPointTempUpdate(object value)
{
    var telemetry = _telemetryController as ITelemetryWithSetPointTemperature;
    telemetry.SetPointTemperature = Convert.ToDouble(value);

    await SetReportedPropertyAsync(SetPointTempPropertyName, telemetry.SetPointTemperature);
}
```

Este método atualiza a temperatura do ponto de telemetria e, em seguida, envia um relatório a alteração para o IoT Hub, definindo uma propriedade comunicada.

Pode adicionar seus próprios manipuladores para as suas propriedades, seguindo o padrão no exemplo anterior.

Tem também de vincular a propriedade pretendida para o manipulador conforme mostrado no exemplo seguinte do **CoolerDevice** construtor:

```csharp
_desiredPropertyUpdateHandlers.Add(SetPointTempPropertyName, OnSetPointTempUpdate);
```

Tenha em atenção que **SetPointTempPropertyName** é uma constante definida como "Config.SetPointTemp".

### <a name="add-support-for-a-new-method-to-the-simulator"></a>Adicionar suporte para um novo método para o simulador

Pode personalizar o simulador para adicionar suporte para uma nova [método (método direto)][lnk-direct-methods]. Existem dois passos principais necessários:

- O simulador tem de notificar o hub IoT na solução pré-configurada com detalhes do método.
- O simulador tem de incluir código para processar a chamada de método ao invocá-lo a partir da **detalhes do dispositivo** painel no Explorador de soluções ou por meio de uma tarefa.

A monitorização remota pré-configurada a solução utiliza *propriedades comunicadas* para enviar detalhes de métodos suportados para o hub IoT. O back-end de solução mantém uma lista de todos os métodos suportados por cada dispositivo juntamente com um histórico das invocações de método. Pode ver estas informações sobre os dispositivos e invocar métodos no portal da solução.

Para notificar o hub IoT que um dispositivo oferece suporte a um método, o dispositivo tem de adicionar detalhes do método para o **SupportedMethods** nó das propriedades comunicadas:

```json
"SupportedMethods": {
  "<method signature>": "<method description>",
  "<method signature>": "<method description>"
}
```

A assinatura do método tem o seguinte formato: `<method name>--<parameter #0 name>-<parameter #1 type>-...-<parameter #n name>-<parameter #n type>`. Por exemplo, para especificar a **InitiateFirmwareUpdate** método espera um parâmetro de cadeia de caracteres chamado **FwPackageURI**, utilize a seguinte assinatura do método:

```json
InitiateFirmwareUpate--FwPackageURI-string: "description of method"
```

Para obter uma lista de tipos de parâmetro suportados, consulte a **CommandTypes** classe no projeto de infra-estrutura.

Para eliminar um método, definir a assinatura do método como `null` nas propriedades comunicadas.

> [!NOTE]
> O back-end de solução apenas de atualizações de informações sobre métodos suportados quando recebe um *informações do dispositivo* mensagens do dispositivo.

O código de exemplo seguinte do **SampleDeviceFactory** classe no projeto comum mostra como adicionar um método à lista de **SupportedMethods** nas propriedades comunicadas enviadas pelo dispositivo:

```csharp
device.Commands.Add(new Command(
    "InitiateFirmwareUpdate",
    DeliveryType.Method,
    "Updates device Firmware. Use parameter 'FwPackageUri' to specifiy the URI of the firmware file, e.g. https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin",
    new[] { new Parameter("FwPackageUri", "string") }
));
```

Este fragmento de código adiciona detalhes sobre o **InitiateFirmwareUpdate** método incluindo o texto a apresentar no portal de solução e detalhes dos parâmetros de método necessário.

O simulador envia as propriedades comunicadas, incluindo a lista de métodos com suporte, para o IoT Hub quando o simulator é iniciado.

Adicione um manipulador para o código de simulador para cada método oferece suporte. Pode ver os manipuladores existentes na **CoolerDevice** classe no projeto Simulator.WebJob. O exemplo seguinte mostra o manipulador **InitiateFirmwareUpdate** método:

```csharp
public async Task<MethodResponse> OnInitiateFirmwareUpdate(MethodRequest methodRequest, object userContext)
{
    if (_deviceManagementTask != null && !_deviceManagementTask.IsCompleted)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "Device is busy"
        }, 409));
    }

    try
    {
        var operation = new FirmwareUpdate(methodRequest);
        _deviceManagementTask = operation.Run(Transport).ContinueWith(async task =>
        {
            // after firmware completed, we reset telemetry
            var telemetry = _telemetryController as ITelemetryWithTemperatureMeanValue;
            if (telemetry != null)
            {
                telemetry.TemperatureMeanValue = 34.5;
            }

            await UpdateReportedTemperatureMeanValue();
        });

        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = "FirmwareUpdate accepted",
            Uri = operation.Uri
        }));
    }
    catch (Exception ex)
    {
        return await Task.FromResult(BuildMethodRespose(new
        {
            Message = ex.Message
        }, 400));
    }
}
```

Tem de começar com os nomes de manipuladores de método `On` seguido do nome do método. O **methodRequest** parâmetro contém todos os parâmetros passados com a invocação de método da solução de back-end. O valor de retorno deve ser do tipo **tarefa&lt;MethodResponse&gt;**. O **BuildMethodResponse** método utilitário o ajuda a criar o valor de retorno.

Dentro do manipulador de método, pode:

- Inicie uma tarefa assíncrona.
- Obter as propriedades pretendidas a partir da *dispositivo duplo* no IoT Hub.
- Atualizar uma propriedade comunicada única com o **SetReportedPropertyAsync** método na **CoolerDevice** classe.
- Atualizar várias propriedades comunicadas através da criação de um **TwinCollection** instância e chamar os **Transport.UpdateReportedPropertiesAsync** método.

O exemplo de atualização de firmware anterior efetua os seguintes passos:

- Verifica que o dispositivo é capaz de aceitar o pedido de atualização de firmware.
- Modo assíncrono inicia a operação de atualização de firmware e repõe a telemetria quando a operação for concluída.
- Devolve imediatamente a mensagem "FirmwareUpdate aceites" para indicar que o pedido foi aceite pelo dispositivo.

### <a name="build-and-use-your-own-physical-device"></a>Criar e utilizar o seu próprio dispositivo (físico)

O [do Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) fornecer bibliotecas para ligar a vários tipos de dispositivo (idiomas e sistemas operacionais) em soluções de IoT.

## <a name="modify-dashboard-limits"></a>Modificar os limites de dashboard

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Número de dispositivos apresentados na lista pendente de dashboard

A predefinição é 200. Pode alterar este número num [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Número de pins a apresentar no controlo de mapas do Bing

A predefinição é 200. Pode alterar este número num [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Período de tempo do gráfico de telemetria

A predefinição é 10 minutos. Pode alterar este valor na [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="feedback"></a>Comentários

Tem uma personalização gostariam de ver abrangidas neste documento? Adicionar sugestões de funcionalidades para [User Voice](https://feedback.azure.com/forums/321918-azure-iot), ou comentário sobre este artigo. 

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre as opções para personalizar as soluções pré-configuradas, consulte:

* [Ligar aplicação lógica à sua solução do Azure IoT Suite monitorização remota pré-configurada][lnk-logicapp]
* [Utilizar a telemetria dinâmica com a solução pré-configurada de monitorização remota][lnk-dynamic]
* [Metadados de informações do dispositivo na solução pré-configurada de monitorização remota][lnk-devinfo]
* [Personalizar a forma como a solução de fábrica ligada apresenta dados de seus servidores OPC UA][lnk-cf-customize]

[lnk-logicapp]: iot-suite-v1-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-v1-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[IoT Device SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-cf-customize]:../iot-accelerators/iot-accelerators-connected-factory-customize.md