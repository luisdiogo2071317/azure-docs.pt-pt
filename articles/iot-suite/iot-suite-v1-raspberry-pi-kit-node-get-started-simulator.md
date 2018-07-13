---
title: Ligar um Raspberry Pi com node. js com telemetria simulada do Azure IoT Suite | Documentos da Microsoft
description: Utilize o Microsoft Azure IoT Starter Kit para o Raspberry Pi 3 e Suite IoT do Azure. Utilizar o node. js para ligar o seu Raspberry Pi a solução de monitorização remota, enviar telemetria simulada para a cloud e responder a métodos invocados a partir do dashboard da solução.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-suite
ms.devlang: node.js
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 53297049fd36eae3839c6a8146afc336b8f5cd02
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38676492"
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a>Ligar o seu Raspberry Pi 3 a solução de monitorização remota e enviar telemetria simulada com node. js

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Este tutorial mostra-lhe como utilizar o Raspberry Pi 3 para simular dados de temperatura e humidade para enviar para a cloud. O tutorial utiliza:

- Raspbian OS, a linguagem de programação do node. js e o SDK do Microsoft Azure IoT para node. js para implementar um dispositivo de exemplo.
- O IoT Suite remoto solução pré-configurada de monitorização como o back-end baseado na nuvem.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Solução de monitorização remota aprovisiona um conjunto de serviços do Azure na sua subscrição do Azure. A implementação reflete uma arquitetura empresarial de real. Para evitar encargos de consumo do Azure desnecessários, elimine a instância da solução pré-configurada em azureiotsuite.com quando tiver terminado com ele. Se precisar de novamente a solução pré-configurada, pode recriá-lo facilmente. Para obter mais informações sobre a redução do consumo enquanto é executada a solução de monitorização remota, consulte [soluções para fins de demonstração de pré-configuradas de configurar o Azure IoT Suite][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-the-sample"></a>Transferir e configurar o exemplo

Agora pode transferir e configurar a aplicação de cliente de monitorização remota no seu Raspberry Pi.

### <a name="install-nodejs"></a>Instalar o Node.js

Se ainda não fez isso, instale o node. js no seu Raspberry Pi. O SDK de IoT para node. js requer a versão 0.11.5 do node. js ou posterior. Os passos seguintes mostram-lhe como instalar o node. js v6.10.2 em seu Raspberry Pi:

1. Utilize o seguinte comando para atualizar o seu Raspberry Pi:

    ```sh
    sudo apt-get update
    ```

1. Utilize o seguinte comando para transferir os binários de node. js para o seu Raspberry Pi:

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Utilize o seguinte comando para instalar os binários:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Utilize o seguinte comando para verificar se o que ter instalado o node. js v6.10.2 com êxito:

    ```sh
    node --version
    ```

### <a name="clone-the-repositories"></a>Clonar os repositórios

Se ainda não o fez, clone os repositórios necessários ao executar os seguintes comandos num terminal no seu instalador de plataforma:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>Atualizar a cadeia de ligação do dispositivo

Abra o ficheiro de origem de exemplo na **nano** editor usando o seguinte comando:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

Localize a linha:

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

Substitua os valores de marcador de posição com os dispositivos e as informações do IoT Hub que criou e guardou no início deste tutorial. Guardar as alterações (**Ctrl-S**, **Enter**) e sair do editor (**Ctrl-X**).

## <a name="run-the-sample"></a>Executar o exemplo

Execute os seguintes comandos para instalar os pacotes de pré-requisitos para o exemplo:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

Agora, pode executar o programa de exemplo no Raspberry Pi. Introduza o comando:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

A saída de exemplo seguinte é um exemplo da saída que vir no prompt de comando no Raspberry Pi:

![Saída da aplicação de Raspberry Pi][img-raspberry-output]

Prima **Ctrl-C** para sair do programa em qualquer altura.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Passos Seguintes

Visite o [Centro de desenvolvimento do Azure IoT](https://azure.microsoft.com/develop/iot/) para obter mais exemplos e documentação sobre IoT do Azure.

[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
