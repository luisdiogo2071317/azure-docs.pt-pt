---
title: Connnect um Raspberry Pi a sua aplicação do Azure IoT Central (Python) | Documentos da Microsoft
description: Como um desenvolvedor de dispositivo, como ligar um Raspberry Pi a sua aplicação do Azure IoT Central com o Python.
author: dominicbetts
ms.author: dobett
ms.date: 01/23/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: aa2d8f50d8fb4ba356af20a290976b8b32601ebf
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188796"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Ligar um Raspberry Pi a sua aplicação do Azure IoT Central (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Este artigo descreve como, como um desenvolvedor de dispositivo, para ligar um Raspberry Pi a sua aplicação do Microsoft Azure IoT Central com o Python de linguagem de programação.

## <a name="before-you-begin"></a>Antes de começar

Para executar os passos descritos neste artigo é necessário o seguinte:

* Uma aplicação do Azure IoT Central, criada a partir da **Devkits exemplo** o modelo de aplicativo. Para obter mais informações, consulte [criar seu aplicativo de Central de IoT do Azure](howto-create-application.md).
* Um dispositivo de Raspberry Pi com o sistema de operativo Raspbian. Terá de um monitor, teclado e mouse ligado ao seu Raspberry Pi para aceder ao ambiente de GUI. O Raspberry Pi deve conseguir [ligar à internet](https://www.raspberrypi.org/learning/software-guide/wifi/).
* Opcionalmente, um [sentido Hat](https://www.raspberrypi.org/products/sense-hat/) quadro de suplemento para o Raspberry Pi. Este quadro recolhe dados de telemetria de vários sensores para enviar a sua aplicação do Azure IoT Central. Se não tiver uma **sentido Hat** quadro, pode utilizar um emulador em vez disso.

## <a name="sample-devkits-application"></a>**Exemplo Devkits** aplicação

Um aplicativo criado a partir da **Devkits de exemplo** inclui o modelo de aplicativo um **Raspberry Pi** modelo de dispositivo com as seguintes características: 

- Telemetria que contém as medidas para o dispositivo **humidade**, **temperatura**, **pressão**, **Magnometer** (medida ao longo de X Y, o eixo Z), **Accelorometer** (medido ao longo de X, Y, eixo Z) e **Gyroscope** (medido ao longo de X, Y, eixo Z).
- As definições que mostra **tensão**, **atual**,**ventoinha velocidade** e uma **IR** Ativar/desativar.
- Propriedades que contém a propriedade do dispositivo **morrem número** e **localização** propriedade da cloud.


Para obter detalhes completos sobre a configuração do modelo de dispositivo, consulte [detalhes do modelo de dispositivo do Raspberry PI](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details)
    

## <a name="add-a-real-device"></a>Adicionar um dispositivo real

Na aplicação do Azure IoT Central, adicionar um dispositivo real a partir da **Raspberry Pi** modelo do dispositivo e tome nota da cadeia de ligação do dispositivo. Para obter mais informações, consulte [adicionar um dispositivo real à sua aplicação do Azure IoT Central](tutorial-add-device.md).

### <a name="configure-the-raspberry-pi"></a>Configurar o Raspberry Pi

Os passos seguintes descrevem como transferir e configurar a aplicação de Python de exemplo do GitHub. Este aplicativo de exemplo:

* Envia os valores de propriedade e telemetria para o Azure IoT Central.
* Responde a definir as alterações feitas no Azure IoT Central.

> [!NOTE]
> Para obter mais informações sobre o exemplo de Python do Raspberry Pi, consulte a [Leiame](https://github.com/Azure/iot-central-firmware/blob/master/RaspberryPi/README.md) ficheiros no GitHub.

1. Utilize o navegador da Web na área de trabalho Raspberry Pi para navegar para o [versões de firmware do Azure IoT Central](https://github.com/Azure/iot-central-firmware/releases) página.

1. Transfira o ficheiro zip que contém o firmware mais recente para a pasta raiz no Raspberry Pi. O nome de ficheiro é semelhante a `RaspberryPi-IoTCentral-X.X.X.zip`.

1. Para descomprimir o ficheiro de firmware, utilize o **Gestor de ficheiros** na área de trabalho Raspberry Pi. O ficheiro zip com o botão direito e escolher **extrair aqui**. Esta operação cria uma pasta chamada `RaspberryPi-IoTCentral-X.X.X` na sua pasta de raiz.

1. Se não tiver uma **sentido Hat** quadro ligado ao seu Raspberry Pi, tem de ativar o emulador:
    1. Na **Gestor de ficheiros**, na `RaspberryPi-IoTCentral-X.X.X` pasta, com o botão direito do **config.iot** do ficheiro e escolha **Editor de texto**.
    1. Altere a linha `"simulateSenseHat": false,` para `"simulateSenseHat": true,`.
    1. Guardar as alterações e fechar **Editor de texto**.

1. Iniciar uma **Terminal** sessão e utilizar o `cd` comando para navegar para a pasta que criou no passo anterior.

1. Para iniciar o aplicativo de exemplo em execução, escreva `./start.sh` no **Terminal** janela. Se estiver a utilizar o **sentido HAT emulador**, apresenta sua GUI. Pode utilizar a GUI para alterar os valores de telemetria enviados para a sua aplicação do Azure IoT Central.

1. O **Terminal** janela exibe uma mensagem semelhante à `Device information being served at http://192.168.0.60:8080`. O URL pode ser diferente no seu ambiente. Copie o URL e utilize o browser para navegar para a página de configuração:

    ![Configurar o dispositivo](media/howto-connect-raspberry-pi-python/configure.png)

1. Introduza a cadeia de ligação do dispositivo tomou nota do quando adicionou um dispositivo real à sua aplicação do Azure IoT Central. Em seguida, escolha **configurar dispositivo**. Verá uma mensagem **dispositivo configurado, o dispositivo deve começar a enviar dados para o Azure IoT Central momentaneamente**.

1. Na aplicação do Azure IoT Central, verá como o código que executa o Raspberry Pi interage com o aplicativo:

    * Sobre o **medidas** página para o seu dispositivo real, pode ver a telemetria enviada o Raspberry PI. Se estiver a utilizar o **sentido HAT emulador**, pode modificar os valores de telemetria na GUI no Raspberry Pi.
    * Sobre o **propriedades** página, pode ver o valor do comunicado **morrem número** propriedade.
    * Sobre o **definições** página, pode alterar várias definições no Raspberry Pi, por exemplo, a velocidade de tensão e fan. Quando o Raspberry Pi reconhece a alteração, a definição mostra como **sincronizado** no Azure IoT Central.


## <a name="raspberry-pi-device-template-details"></a>Detalhes do modelo do raspberry PI dispositivo

Um aplicativo criado a partir da **Devkits de exemplo** inclui o modelo de aplicativo um **Raspberry Pi** modelo de dispositivo com as seguintes características:

### <a name="telemetry-measurements"></a>Medições de telemetria

| Nome do campo     | Unidades  | Mínimo | Máximo | Casas decimais |
| -------------- | ------ | ------- | ------- | -------------- |
| humidade       | %      | 0       | 100     | 0              |
| Temp           | ° C     | -40,Year),temperature     | 120     | 0              |
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

### <a name="settings"></a>Definições

Definições de numérico

| Nome a apresentar | Nome do campo | Unidades | Casas decimais | Mínimo | Máximo | Inicial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Tensão      | setVoltage | Volts | 0              | 0       | 240     | 0       |
| Atual      | setCurrent | AMPS  | 0              | 0       | 100     | 0       |
| Ventoinha velocidade    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Ativar/desativar definições

| Nome a apresentar | Nome do campo | No texto | Desativar o texto | Inicial |
| ------------ | ---------- | ------- | -------- | ------- |
| RUNTIME DE INTEGRAÇÃO           | activateIR | ON      | OFF      | Desativado     |

### <a name="properties"></a>Propriedades

| Tipo            | Nome a apresentar | Nome do campo | Tipo de dados |
| --------------- | ------------ | ---------- | --------- |
| Propriedade do dispositivo | Morrem número   | dieNumber  | número    |
| Texto            | Localização     | localização   | N/A       |

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como ligar um Raspberry Pi a sua aplicação do Azure IoT Central, aqui estão os passos sugeridos seguintes:

* [Ligar uma aplicação de cliente genérica do node. js para o Azure IoT Central](howto-connect-nodejs.md)
