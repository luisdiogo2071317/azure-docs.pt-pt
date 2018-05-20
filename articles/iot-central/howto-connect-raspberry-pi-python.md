---
title: Connnect um Raspberry Pi à sua aplicação do Azure IoT Central (Python) | Microsoft Docs
description: Como um programador de dispositivo, como ligar um Raspberry Pi a sua aplicação do Azure IoT Central com o Python.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 01/23/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 23ab31d6d2357bfcb184e5b3022155bef5ace658
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Ligar um Raspberry Pi à sua aplicação do Azure IoT Central (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Este artigo descreve como, como um programador de dispositivo, para ligar um Raspberry Pi a sua aplicação do Microsoft Azure IoT Central utilizando a linguagem de programação de Python.

## <a name="before-you-begin"></a>Antes de começar

Para executar os passos descritos neste artigo é necessário o seguinte:

* Uma aplicação do Azure IoT Central criada a partir de **exemplo Devkits** modelo de aplicação. Para obter mais informações, consulte [criar a sua aplicação Central do Azure IoT](howto-create-application.md).
* Um dispositivo de Raspberry Pi com o sistema de operativo Raspbian. Precisa de um monitor, teclado e rato ligada ao seu Raspberry Pi para aceder ao ambiente de GUI. Raspberry Pi deve conseguir [estabelecer ligação à internet](https://www.raspberrypi.org/learning/software-guide/wifi/).
* Opcionalmente, um [sentido Hat](https://www.raspberrypi.org/products/sense-hat/) quadro de suplemento para Raspberry Pi. Esta placa recolhe dados telemétricos das várias sensores para enviar para a aplicação do Azure IoT Central. Se não tiver um **sentido Hat** painel, pode utilizar um emulador em vez disso.

Uma aplicação criada a partir de **Devkits de exemplo** modelo de aplicação inclui um **Raspberry Pi** modelo de dispositivo com as seguintes características:

### <a name="telemetry-measurements"></a>Medidas de telemetria

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

Na aplicação do Azure IoT Central, adicionar um dispositivo real do **Raspberry Pi** modelo do dispositivo e tome nota da cadeia de ligação de dispositivo. Para obter mais informações, consulte [adicionar um dispositivo real à sua aplicação do Azure IoT Central](tutorial-add-device.md).

## <a name="configure-the-raspberry-pi"></a>Configurar o Raspberry Pi

Os passos seguintes descrevem como transferir e configurar a aplicação de Python de exemplo a partir do GitHub. Esta aplicação de exemplo:

* Envia os valores de telemetria e propriedade para o Azure IoT Central.
* Responde a definição de alterações no Azure IoT Central.

> [!NOTE]
> Para obter mais informações sobre o exemplo Raspberry Pi Python, consulte o [Leia-me](https://github.com/Microsoft/microsoft-iot-central-firmware/blob/master/RaspberryPi/README.md) ficheiro no GitHub.

1. Utilize o browser no ambiente de trabalho Raspberry Pi para navegar para o [versões de firmware do Azure IoT Central](https://github.com/Microsoft/microsoft-iot-central-firmware/releases) página.

1. Transfira o ficheiro zip que contém o firmware mais recente para a pasta raiz em Raspberry Pi. O nome de ficheiro aspeto `RaspberryPi-IoTCentral-X.X.X.zip`.

1. Para descomprimir o ficheiro de firmware, utilize o **Gestor ficheiros** no ambiente de trabalho Raspberry Pi. O ficheiro zip com o botão direito e selecione **extrair aqui**. Esta operação cria uma pasta denominada `RaspberryPi-IoTCentral-X.X.X` na pasta raiz.

1. Se não tiver um **sentido Hat** quadro ligado ao seu Raspberry Pi, tem de ativar o emulador:
    1. No **Gestor de ficheiros**, no `RaspberryPi-IoTCentral-X.X.X` pasta, faça duplo clique o **config.iot** de ficheiros e escolha **Editor de texto**.
    1. Altere a linha `"simulateSenseHat": false,` para `"simulateSenseHat": true,`.
    1. Guardar as alterações e fechar **Editor de texto**.

1. Iniciar um **Terminal** sessão e utilizar o `cd` comando para navegar para a pasta que criou no passo anterior.

1. Para iniciar a aplicação de exemplo em execução, escreva `./start.sh` no **Terminal** janela. Se estiver a utilizar o **sentido HAT emulador**, apresenta o GUI. Pode utilizar a GUI para alterar os valores de telemetria enviados para a aplicação do Azure IoT Central.

1. O **Terminal** janela apresenta uma mensagem que se assemelha `Device information being served at http://192.168.0.60:8080`. O URL pode ser diferente no seu ambiente. Copie o URL e utilize o browser para navegar para a página de configuração:

    ![Configurar o dispositivo](media/howto-connect-raspberry-pi-python/configure.png)

1. Introduza a cadeia de ligação do dispositivo que tomou nota do quando adicionar um dispositivo real à sua aplicação do Azure IoT Central. Em seguida, escolha **configurar dispositivo**. Verá uma mensagem **dispositivo configurado, o dispositivo deve começar a enviar dados para o Azure IoT Central momentaneamente**.

1. Na aplicação do Azure IoT Central, verá como o código em execução no Raspberry Pi interage com a aplicação:

    * No **medidas** página para o seu dispositivo real, pode ver a telemetria enviada do Raspberry Pi. Se estiver a utilizar o **sentido HAT emulador**, pode modificar os valores de telemetria no GUI no Raspberry Pi.
    * No **propriedades** página, pode ver o valor a que relatados **Die número** propriedade.
    * No **definições** página, pode alterar várias definições no Raspberry Pi, tais como velocidade tensão e ventoinha. Quando o Raspberry Pi reconhece a alteração, a definição mostra como **sincronizado** no Azure IoT Central.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como ligar um Raspberry Pi a sua aplicação do Azure IoT Central, Eis os passos sugeridos:

* [Ligar uma aplicação de cliente do Node.js genérica para o Azure IoT Central](howto-connect-nodejs.md)