---
title: Ligar um Raspberry Pi com C para oferecer suporte a atualizações de firmware do Azure IoT Suite | Documentos da Microsoft
description: Utilize o Microsoft Azure IoT Starter Kit para o Raspberry Pi 3 e Suite IoT do Azure. Utilizar C para ligar o seu Raspberry Pi a solução de monitorização remota, enviar a telemetria de sensores para a cloud e efetuar uma atualização de firmware remoto.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 8160752b0116c3ef3e6b6ab7920bb35e471f180b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38687700"
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-enable-remote-firmware-updates-using-c"></a>Ligar o seu Raspberry Pi 3 a solução de monitorização remota e ativar as atualizações de firmware remoto com C

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Este tutorial mostra-lhe como utilizar o Microsoft Azure IoT Starter Kit para Raspberry Pi 3 para:

* Desenvolva um leitor de temperatura e humidade que possa comunicar com a cloud.
* Ativar e executar um firmware remoto update para atualizar a aplicação de cliente no Raspberry Pi.

O tutorial utiliza:

* Raspbian OS, a linguagem de programação C e o SDK do Microsoft Azure IoT para C para implementar um dispositivo de exemplo.
* O IoT Suite remoto solução pré-configurada de monitorização como o back-end baseado na nuvem.

## <a name="overview"></a>Descrição geral

Neste tutorial, vai concluir os seguintes passos:

* Implemente uma instância da solução pré-configurada de monitorização remota para a sua subscrição do Azure. Este passo automaticamente implementa e configura vários serviços do Azure.
* Configure os dispositivos e sensores para comunicar com o seu computador e solução de monitorização remota.
* Atualizar o código de dispositivo de exemplo para ligar a solução de monitorização remota e enviar telemetria, que pode ver no dashboard da solução.
* Utilize o código de dispositivo de exemplo para atualizar a aplicação cliente.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prerequisites](../../includes/iot-suite-v1-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Solução de monitorização remota aprovisiona um conjunto de serviços do Azure na sua subscrição do Azure. A implementação reflete uma arquitetura empresarial de real. Para evitar encargos de consumo do Azure desnecessários, elimine a instância da solução pré-configurada em azureiotsuite.com quando tiver terminado com ele. Se precisar de novamente a solução pré-configurada, pode recriá-lo facilmente. Para obter mais informações sobre a redução do consumo enquanto é executada a solução de monitorização remota, consulte [soluções para fins de demonstração de pré-configuradas de configurar o Azure IoT Suite][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a>Transferir e configurar o exemplo

Agora pode transferir e configurar a aplicação de cliente de monitorização remota no seu Raspberry Pi.

### <a name="clone-the-repositories"></a>Clonar os repositórios

Se ainda não fez isso, clone os repositórios necessários ao executar os seguintes comandos no seu instalador de plataforma:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>Atualizar a cadeia de ligação do dispositivo

Abra o ficheiro de configuração de exemplo na **nano** editor usando o seguinte comando:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

Substitua os valores de marcador de posição com os ID e o IoT Hub informações do dispositivo que criou e guardou no início deste tutorial.

Quando tiver terminado, o conteúdo do arquivo deviceinfo deve ter um aspeto semelhante ao seguinte exemplo:

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

Guardar as alterações (**Ctrl-S**, **Enter**) e sair do editor (**Ctrl-X**).

## <a name="build-the-sample"></a>Criar o exemplo

Se ainda não o fez, instale os pacotes de pré-requisitos para o SDK de dispositivo do Microsoft Azure IoT para C, executando os seguintes comandos num terminal sobre o Raspberry Pi:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

Agora pode compilar a solução de exemplo no Raspberry Pi:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
```

Agora, pode executar o programa de exemplo no Raspberry Pi. Introduza o comando:

  ```sh
  sudo ~/cmake/remote_monitoring/remote_monitoring
  ```

A saída de exemplo seguinte é um exemplo da saída que vir no prompt de comando no Raspberry Pi:

![Saída da aplicação de Raspberry Pi][img-raspberry-output]

Prima **Ctrl-C** para sair do programa em qualquer altura.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced.md)]

1. No dashboard da solução, clique em **dispositivos** visitar o **dispositivos** página. Selecione o seu Raspberry Pi nas **lista de dispositivos**. Em seguida, escolha **métodos**:

    ![Lista de dispositivos no dashboard][img-list-devices]

1. Sobre o **invocar método** página, selecione **InitiateFirmwareUpdate** no **método** lista pendente.

1. Na **FWPackageURI** , insira **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**. Este ficheiro de arquivo contém a implementação da versão 2.0 do firmware.

1. Escolher **InvokeMethod**. A aplicação no Raspberry Pi envia uma confirmação de volta para o dashboard de solução. Em seguida, inicia o processo de atualização de firmware baixando a nova versão do firmware:

    ![Mostrar histórico do método][img-method-history]

## <a name="observe-the-firmware-update-process"></a>Observe o processo de atualização de firmware

Pode observar que o processo de atualização, enquanto é executada no dispositivo e visualizando as propriedades reportadas no dashboard da solução de firmware:

1. Pode ver o progresso do processo de atualização no Raspberry Pi:

    ![Mostrar Progresso da atualização][img-update-progress]

    > [!NOTE]
    > A aplicação de monitorização remota é reiniciado automaticamente quando a atualização for concluída. Utilize o comando `ps -ef` para verificar se está a ser executado. Se quiser terminar o processo, use o `kill` comando com o id de processo.

1. Pode ver o estado de atualização de firmware, conforme comunicado pelo dispositivo, no portal da solução. Captura de ecrã seguinte mostra o estado e a duração de cada estágio do processo de atualização e a nova versão de firmware:

    ![Mostrar estado da tarefa][img-job-status]

    Se navegar para o dashboard, pode verificar que o dispositivo ainda está a enviar telemetria após a atualização de firmware.

> [!WARNING]
> Se deixar a solução de monitorização remota em execução na sua conta do Azure, é cobrada a hora que é executado. Para obter mais informações sobre a redução do consumo enquanto é executada a solução de monitorização remota, consulte [soluções para fins de demonstração de pré-configuradas de configurar o Azure IoT Suite][lnk-demo-config]. Elimine a solução pré-configurada da sua conta do Azure quando tiver terminado de usá-lo.

## <a name="next-steps"></a>Passos Seguintes

Visite o [Centro de desenvolvimento do Azure IoT](https://azure.microsoft.com/develop/iot/) para obter mais exemplos e documentação sobre IoT do Azure.


[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md