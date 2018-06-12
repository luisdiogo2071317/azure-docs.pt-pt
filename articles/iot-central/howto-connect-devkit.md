---
title: Ligar um dispositivo de DevKit à sua aplicação do Azure IoT Central | Microsoft Docs
description: Como um programador de dispositivo, saiba como ligar um dispositivo MXChip IoT DevKit à sua aplicação do Azure IoT Central.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: d7b92359e8875c281fd460f1f5307a7941c11c1f
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261581"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Ligue um dispositivo de MXChip IoT DevKit à sua aplicação do Azure IoT Central

Este artigo descreve como, como um programador de dispositivo, para ligar um dispositivo MXChip IoT DevKit (DevKit) a sua aplicação do Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de começar

Para executar os passos descritos neste artigo é necessário o seguinte:

1. Uma aplicação do Azure IoT Central criada a partir de **exemplo Devkits** modelo de aplicação. Para obter mais informações, consulte [criar a sua aplicação Central do Azure IoT](howto-create-application.md).
1. Um dispositivo DevKit. Para comprar um dispositivo DevKit, visite [MXChip IoT DevKit](http://mxchip.com/az3166).

Uma aplicação criada a partir de **Devkits de exemplo** modelo de aplicação inclui um **MXChip** modelo de dispositivo com as seguintes características:

### <a name="measurements"></a>Medições

#### <a name="telemetry"></a>Telemetria 

| Nome do campo     | Unidades  | Mínimo | Máximo | Casas decimais |
| -------------- | ------ | ------- | ------- | -------------- |
| humidade       | %      | 0       | 100     | 0              |
| Temp           | ° C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | grupo de gestão     | -2000   | 2000    | 0              |
| accelerometerY | grupo de gestão     | -2000   | 2000    | 0              |
| accelerometerZ | grupo de gestão     | -2000   | 2000    | 0              |
| gyroscopeX     | mdps   | -2000   | 2000    | 0              |
| gyroscopeY     | mdps   | -2000   | 2000    | 0              |
| gyroscopeZ     | mdps   | -2000   | 2000    | 0              |

#### <a name="states"></a>Estados 

| Nome          | Nome a apresentar   | NORMAL | ATENÇÃO | RISCO | 
| ------------- | -------------- | ------ | ------- | ------ | 
| DeviceState   | Estado do dispositivo   | Verde  | Orange  | Vermelho    | 

#### <a name="events"></a>Eventos 

| Nome             | Nome a apresentar      | 
| ---------------- | ----------------- | 
| ButtonBPressed   | Premido do botão B  | 



### <a name="settings"></a>Definições

Definições de um valor numérico

| Nome a apresentar | Nome do campo | Unidades | Casas decimais | Mínimo | Máximo | Inicial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Tensão      | setVoltage | Volts | 0              | 0       | 240     | 0       |
| Atual      | setCurrent | AMPS  | 0              | 0       | 100     | 0       |
| Ventoinha velocidade    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Definições de ativar/desativar

| Nome a apresentar | Nome do campo | No texto | Desativar texto | Inicial |
| ------------ | ---------- | ------- | -------- | ------- |
| RESPOSTA A INCIDENTES           | activateIR | ON      | OFF      | Desativado     |

### <a name="properties"></a>Propriedades

| Tipo            | Nome a apresentar | Nome do campo | Tipo de dados |
| --------------- | ------------ | ---------- | --------- |
| Propriedade do dispositivo | Die número   | dieNumber  | número    |
| Texto            | Localização     | localização   | N/A       |


### <a name="add-a-real-device"></a>Adicionar um dispositivo real

Na aplicação do Azure IoT Central, adicionar um dispositivo real do **MXChip** modelo do dispositivo e tome nota da cadeia de ligação de dispositivo. Para obter mais informações, consulte [adicionar um dispositivo real à sua aplicação do Azure IoT Central](tutorial-add-device.md).

## <a name="prepare-the-devkit-device"></a>Preparar o dispositivo DevKit

> [!TIP]
> Para o dispositivo de DevKit orientações de resolução de problemas, consulte [introdução ao IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/).

Para preparar o dispositivo DevKit:

1. Transferir o firmware do Azure IoT Central pré-criadas mais recente para MXChip do [versões](https://github.com/Azure/iot-central-firmware/releases) página no GitHub. O nome de ficheiro de transferência na página de versões aspeto `AZ3166-IoT-Central-X.X.X.bin`.

1. Ligue o dispositivo de DevKit ao seu computador de desenvolvimento com um cabo USB. No Windows, é aberta uma janela do Explorador de ficheiros numa unidade mapeada para o armazenamento no dispositivo DevKit. Por exemplo, a unidade pode ser chamada **AZ3166 (D:)**.

1. Arraste o **iotCentral.bin** ficheiro para a janela de unidade. Quando a cópia estiver concluída, o dispositivo é reiniciado com o firmware de novo.

1. Quando o dispositivo de DevKit for reiniciado, mostra o ecrã seguinte:

    ```
    Connect HotSpot:
    AZ3166_??????
    go-> 192.168.0.1 
    PIN CODE xxxxx
    ```

    > [!NOTE]
    > Se o ecrã apresenta há mais alguma coisa, prima a **repor** botão no dispositivo. 

1. O dispositivo está agora no modo de ponto (AP) de acesso. Pode ligar a este ponto de acesso Wi-Fi do seu computador ou dispositivo móvel.

1. No seu computador, telemóvel ou tablet estabelecer ligação com o nome da rede Wi-Fi aparecem no ecrã do dispositivo. Quando ligar a esta rede, não tem acesso à internet. Este estado é esperado, e apenas está ligado a esta rede para um curto período de tempo ao configurar o dispositivo.

1. Abra o browser e navegue para [ http://192.168.0.1/start ](http://192.168.0.1/start). Apresenta de página web seguinte:

    ![Página de configuração do dispositivo](media/howto-connect-devkit/configpage.png)

    Na página web: 
    - Adicione o nome da rede Wi-Fi 
    - a palavra-passe de rede de Wi-Fi 
    - CÓDIGO PIN apresentado no dispositivo LCD 
    - a cadeia de ligação do seu dispositivo. 
      Pode encontrar a cadeia de ligação @ `https://apps.iotcentral.com`  ->  `Device Explorer`  ->  `Device`  ->  `Select or Create a new Real Device`  ->  `Connect this device` (no canto superior direito) 
    - Selecione todos os valores de telemetria disponível! 

1. Após escolher **configurar dispositivo**, vê esta página:

    ![Dispositivos configurados](media/howto-connect-devkit/deviceconfigured.png)

1. Prima a **repor** botão no seu dispositivo.

> [!NOTE]
> Para reconfigurar o dispositivo para utilizar uma rede Wi-Fi diferente de cadeia de ligação ou medida de telemetria, prima ambos o **A** e **B** botões no Conselho em simultâneo. Se não funcionar, prima **repor** botão e tente novamente. 

## <a name="view-the-telemetry"></a>Ver a telemetria

Quando o dispositivo de DevKit for reiniciado, mostra o ecrã do dispositivo:

* O número de mensagens de telemetria enviado.
* O número de falhas.
* O número de propriedades pretendidos recebido e o número de propriedades comunicados enviada.

Abane o incremento de dispositivo o número de propriedades comunicados enviada. O dispositivo envia um número aleatório como o **Die número** propriedade do dispositivo.

Pode ver os valores de telemetria e valores de propriedade comunicado e configurar as definições no Azure IoT Central:

1. Utilize **Explorador de dispositivo** para navegar para o **medidas** página para o dispositivo MXChip real que adicionou:

    ![Navegue para o dispositivo real](media/howto-connect-devkit/realdevice.png)

1. No **medidas** página, pode ver a telemetria provém do dispositivo MXChip:

    ![Ver telemetria do dispositivo real](media/howto-connect-devkit/realtelemetry.png)

1. No **propriedades** página, pode ver o último número de die comunicado pelo dispositivo:

    ![Ver propriedades do dispositivo](media/howto-connect-devkit/deviceproperties.png)

1. No **definições** página, pode atualizar as definições no dispositivo MXChip:

    ![Ver definições de dispositivo](media/howto-connect-devkit/settings.png)

## <a name="download-the-source-code"></a>Transferir o código de origem

Se pretender que a explorar e modificar o código de dispositivo, poderá transferi-lo a partir do GitHub. Se pretender modificar o código, deve seguir estas instruções para [preparar o ambiente de desenvolvimento](https://microsoft.github.io/azure-iot-developer-kit/docs/get-started/#step-5-prepare-the-development-environment) para o seu sistema de operativo ambiente de trabalho.

Para transferir o código de origem, execute o seguinte comando no seu computador de ambiente de trabalho:

```cmd/sh
git clone https://github.com/Azure/iot-central-firmware
```

O comando anterior transfere o código de origem para uma pasta denominada `iot-central-firmware`. 

> [!NOTE]
> Se **git** não está instalado no seu ambiente de desenvolvimento, poderá transferi-lo de [ https://git-scm.com/download ](https://git-scm.com/download).

## <a name="review-the-code"></a>Rever o código

Utilizar o Visual Studio Code, que foi instalado quando preparar o ambiente de desenvolvimento, para abrir o `AZ3166` pasta o `iot-central-firmware` pasta: 

![Visual Studio Code](media/howto-connect-devkit/vscodeview.png)

Para ver como a telemetria é enviada para a aplicação do Azure IoT Central, abra o **main_telemetry.cpp** ficheiro na pasta de origem.

A função `buildTelemetryPayload` cria o payload de telemetria JSON utilizam dados dos sensores no dispositivo.

A função `sendTelemetryPayload` chamadas `sendTelemetry` no **iotHubClient.cpp** para enviar o payload JSON para o IoT Hub da Azure IoT Central utiliza de aplicação.

Para ver a forma como os valores de propriedade são comunicados para a aplicação do Azure IoT Central, abra o **main_telemetry.cpp** ficheiro na pasta de origem.

A função `telemetryLoop` envia o **doubleTap** comunicadas propriedade quando o accelerometer Deteta um duplo toque. Utiliza o `sendReportedProperty` funcionar o **iotHubClient.cpp** ficheiro de origem.

O código no **iotHubClient.cpp** ficheiro de origem utiliza as funções do [ SDKs do IoT do Microsoft Azure e bibliotecas para C](https://github.com/Azure/azure-iot-sdk-c) para interagir com o IoT Hub.

Para obter informações sobre como modificar, criar e carregar o código de exemplo para o seu dispositivo, consulte o **readme.md** ficheiros o `AZ3166` pasta.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como ligar um dispositivo DevKit a sua aplicação do Azure IoT Central, Eis os passos sugeridos:

* [Preparar e ligar um Raspberry Pi](howto-connect-raspberry-pi-python.md)