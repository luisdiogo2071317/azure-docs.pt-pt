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
ms.openlocfilehash: 90837092390cd2550805658471ff7aa884773371
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51239598"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Ligar um Raspberry Pi a sua aplicação do Azure IoT Central (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

Este artigo descreve como, como um desenvolvedor de dispositivo, para ligar um Raspberry Pi a sua aplicação do Microsoft Azure IoT Central com o Python de linguagem de programação.

## <a name="before-you-begin"></a>Antes de começar

Para concluir os passos neste artigo, tem os seguintes componentes:

* Uma aplicação do Azure IoT Central, criada a partir da **Devkits exemplo** o modelo de aplicativo. Para obter mais informações, consulte a [criar um guia de introdução do aplicativo](quick-deploy-iot-central.md).
* Um dispositivo de Raspberry Pi com o sistema de operativo Raspbian. Terá de um monitor, teclado e mouse ligado ao seu Raspberry Pi para aceder ao ambiente de GUI. O Raspberry Pi deve conseguir [ligar à internet](https://www.raspberrypi.org/learning/software-guide/wifi/).
* Opcionalmente, um [sentido Hat](https://www.raspberrypi.org/products/sense-hat/) quadro de suplemento para o Raspberry Pi. Este quadro recolhe dados de telemetria de vários sensores para enviar a sua aplicação do Azure IoT Central. Se não tiver uma **sentido Hat** quadro, pode utilizar um emulador em vez disso (disponível como parte da imagem de Raspberry Pi).

## <a name="sample-devkits-application"></a>**Exemplo Devkits** aplicação

Um aplicativo criado a partir da **Devkits de exemplo** inclui o modelo de aplicativo um **Raspberry Pi** modelo de dispositivo com as seguintes características: 

- Telemetria, que inclui as seguintes medidas que recolherá o dispositivo:
    - Humidade
    - Temperatura
    - Pressão
    - Magnetômetro (X, Y, Z)
    - Acelerômetro (X, Y, Z)
    - Por giroscópio (X, Y, Z)
- Definições
    - Tensão
    - Atual
    - Ventoinha velocidade
    - Botão de alternar do Runtime de integração.
- Propriedades
    - Morrem número propriedade do dispositivo
    - Propriedade de cloud de localização

Para todos os detalhes da configuração do modelo de dispositivo, consulte [detalhes do modelo de dispositivo do Raspberry PI](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details)
    

## <a name="add-a-real-device"></a>Adicionar um dispositivo real

Na aplicação do Azure IoT Central, adicionar um dispositivo real a partir da **Raspberry Pi** track de modelo e continue a dispositivos de detalhes de ligação do dispositivo (**chave primária do ID de âmbito, ID de dispositivo,**). Para obter mais informações, consulte [adicionar um dispositivo real à sua aplicação do Azure IoT Central](tutorial-add-device.md).


### <a name="configure-the-raspberry-pi"></a>Configurar o Raspberry Pi

Os passos seguintes descrevem como transferir e configurar a aplicação de Python de exemplo do GitHub. Este aplicativo de exemplo:

* Envia os valores de propriedade e telemetria para o Azure IoT Central.
* Responde a definir as alterações feitas no Azure IoT Central.

Para configurar o dispositivo, [siga as instruções passo a passo no GitHub.](https://aka.ms/iotcentral-docs-Raspi-releases)


> [!NOTE]
> Para obter mais informações sobre o exemplo de Python do Raspberry Pi, consulte a [Leiame](https://aka.ms/iotcentral-docs-Raspi-releases) ficheiros no GitHub.


1. Depois do dispositivo está configurado, o dispositivo deve começar a enviar dados para o Azure IoT Central em instantes.
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
| IR           | activateIR | ON      | OFF      | Desativado     |

### <a name="properties"></a>Propriedades

| Tipo            | Nome a apresentar | Nome do campo | Tipo de dados |
| --------------- | ------------ | ---------- | --------- |
| Propriedade do dispositivo | Morrem número   | dieNumber  | número    |
| Texto            | Localização     | localização   | N/A       |

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como ligar um Raspberry Pi a sua aplicação do Azure IoT Central, aqui estão os passos sugeridos seguintes:

* [Ligar uma aplicação de cliente genérica do node. js para o Azure IoT Central](howto-connect-nodejs.md)
