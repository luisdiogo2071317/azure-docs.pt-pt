---
title: Ligar um dispositivo de Windows Core de IoT a sua aplicação do Azure IoT Central | Microsoft Docs
description: Como um programador de dispositivo, saiba como ligar um dispositivo MXChip IoT DevKit à sua aplicação do Azure IoT Central.
author: miriamb
ms.author: mriamb
ms.date: 04/09/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 55c0d9c26cd4e7f8ae9f6ff2359e0f2d2a9970c9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629148"
---
# <a name="connect-a-windows-iot-core-device-to-your-azure-iot-central-application"></a>Ligar um dispositivo de Windows Core de IoT a sua aplicação do Azure IoT Central

Este artigo descreve como, como um programador de dispositivo, para ligar um dispositivo de Windows Core de IoT a sua aplicação do Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de começar

Para executar os passos descritos neste artigo é necessário o seguinte:

1. Uma aplicação do Azure IoT Central criada a partir de **exemplo Devkits** modelo de aplicação. Para obter mais informações, consulte [criar a sua aplicação Central do Azure IoT](howto-create-application.md).
2. Um dispositivo com o sistema operativo do Windows 10 IoT Core. Nestas instruções, iremos utilizar um Raspberry Pi

Uma aplicação criada a partir de **Devkits de exemplo** modelo de aplicação inclui um **Windows IoT Core** modelo de dispositivo com as seguintes características:

### <a name="telemetry-measurements"></a>Medidas de telemetria

| Nome do campo     | Unidades  | Mínimo | Máximo | Casas decimais |
| -------------- | ------ | ------- | ------- | -------------- |
| humidade       | %      | 0       | 100     | 0              |
| Temp           | ° C     | -40     | 120     | 0              |
| pressure       | hPa    | 260     | 1260    | 0              |

### <a name="settings"></a>Definições

Definições de um valor numérico

| Nome a apresentar | Nome do campo | Unidades | Casas decimais | Mínimo | Máximo | Inicial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Ventoinha velocidade    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |


### <a name="properties"></a>Propriedades

| Tipo            | Nome a apresentar | Nome do campo | Tipo de dados |
| --------------- | ------------ | ---------- | --------- |
| Propriedade do dispositivo | Die número   | dieNumber  | número    |
| Texto            | Localização     | localização   | N/A       |

## <a name="add-a-real-device"></a>Adicionar um dispositivo real

Na aplicação do Azure IoT Central, adicionar um dispositivo real do **Windows IoT Core** modelo do dispositivo e tome nota da cadeia de ligação de dispositivo. Para obter mais informações, consulte [adicionar um dispositivo real à sua aplicação do Azure IoT Central](tutorial-add-device.md).

### <a name="prepare-the-windows-iot-core-device"></a>Preparar o dispositivo de Windows Core de IoT

Para configurar um dispositivo de Windows Core de IoT siga o guia passo a passo, [configurar um dispositivo de Windows Core de IoT] (https://github.com/Microsoft/microsoft-iot-central-firmware/tree/master/WindowsIoT#setup-a-physical-device).

### <a name="add-a-real-device"></a>Adicionar um dispositivo real

Na aplicação do Azure IoT Central, adicionar um dispositivo real do **Windows IoT Core** modelo do dispositivo e tome nota da cadeia de ligação de dispositivo. Para obter mais informações, consulte [adicionar um dispositivo real à sua aplicação do Azure IoT Central](tutorial-add-device.md).

## <a name="prepare-the-windows-10-iot-core-device"></a>Preparar o dispositivo Windows 10 IoT Core

### <a name="what-youll-need"></a>O que necessita

Para configurar um dispositivo Windows 10 IoT núcleos físico, terá de ter primeiro um dispositivo com o Windows 10 IoT Core. Saiba como configurar um dispositivo Windows 10 IoT Core [aqui](https://developer.microsoft.com/en-us/windows/iot/getstarted/prototype/setupdevice).

Também precisa de uma aplicação de cliente que possa comunicar com o Azure IoT Central. Pode criar a sua própria aplicação personalizada utilizando o SDK do Azure e implementá-la para o seu dispositivo com o Visual Studio, ou pode transferir um [exemplo pré-criadas](https://developer.microsoft.com/en-us/windows/iot/samples) simplesmente implementar e executá-la no dispositivo. 

### <a name="deploying-the-sample-client-application"></a>Implementar a aplicação de cliente de exemplo

Para implementar a aplicação de cliente do passo anterior para o seu dispositivo Windows 10 IoT para preparar:

**Certifique-se de que a cadeia de ligação é armazenada no dispositivo para a aplicação de cliente para utilizar**
* No ambiente de trabalho, guarde a cadeia de ligação num ficheiro de texto com o nome connection.string.iothub.
* Copie o ficheiro de texto para a pasta de documentos do dispositivo: `[device-IP-address]\C$\Data\Users\DefaultAccount\Documents\connection.string.iothub`

Assim que tiver feito, terá de abrir o [Portal do dispositivo Windows](https://docs.microsoft.com/en-us/windows/iot-core/manage-your-device/deviceportal) escrevendo na http://[device-IP-address]:8080 em qualquer browser.

Há e, como apresentado no se abaixo, poderá ser útil para:
1. Expanda o nó de "Aplicações" no lado esquerdo.
2. Clique em "executar rápida amostras".
3. Clique em "Cliente do IoT Hub do Azure".
4. Clique em "Implementar e executar".

![GIF do cliente do IoT Hub do Azure no Portal de dispositivo do Windows](./media/howto-connect-windowsiotcore/iothubapp.gif)

Quando tiver êxito, a aplicação iniciar no dispositivo e este aspeto:

![Captura de ecrã do Hub IoT do Azure aplicação cliente](./media/howto-connect-windowsiotcore/IoTHubForegroundClientScreenshot.png)

No Azure IoT Central, pode ver como o código em execução no Raspberry Pi interage com a aplicação:

* No **medidas** página para o seu dispositivo real, pode ver a telemetria.
* No **propriedades** página, pode ver o valor da propriedade de número Die comunicado.
* No **definições** página, pode alterar várias definições no Raspberry Pi, tais como velocidade tensão e ventoinha.

## <a name="download-the-source-code"></a>Transferir o código de origem

Se pretender que a explorar e modificar o código de origem para a aplicação de cliente, poderá transferi-lo a partir do GitHub [aqui](https://github.com/Microsoft/Windows-iotcore-samples/tree/develop/Samples/Azure/IoTHubClients). Se pretender modificar o código, deve seguir estas instruções no ficheiro Leia-me [aqui](https://github.com/Microsoft/Windows-iotcore-samples) para o seu sistema de operativo ambiente de trabalho.

> [!NOTE]
> Se **git** não está instalado no seu ambiente de desenvolvimento, poderá transferi-lo de [ https://git-scm.com/download ](https://git-scm.com/download).
