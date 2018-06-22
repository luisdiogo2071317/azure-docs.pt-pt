---
title: Simular o Azure IoT Edge no Linux | Microsoft Docs
description: Instalar o runtime do Azure IoT Edge num dispositivo simulado no Linux e implementar o seu primeiro módulo
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: elioda
ms.date: 01/11/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0b8b2658af9173cea6a7cdcb0147c7b0dc13a455
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631001"
---
# <a name="deploy-azure-iot-edge-on-a-simulated-device-in-linux-or-macos---preview"></a>Implementar o Azure IoT Edge num dispositivo simulado no Linux ou MacOS – pré-visualização

O Azure IoT Edge permite-lhe realizar o processamento de dados e de análise nos seus dispositivos, em vez de ter de solicitar todos os dados para a cloud. Os tutoriais do IoT Edge demonstram como implementar diferentes tipos de módulos, criados a partir de serviços do Azure ou código personalizado, mas primeiro precisa de um dispositivo para testar. 

Neste tutorial, ficará a saber como:

1. Criar um Hub IoT
2. Registar um dispositivo do IoT Edge
3. Iniciar o runtime do IoT Edge
4. Implementar um módulo

![Arquitetura do tutorial][2]

O dispositivo simulado que criar neste tutorial é um monitor que gera dados de temperatura, humidade e pressão. Os outros tutoriais do Azure IoT Edge tiram partido do seu trabalho aqui realizado, ao implementar módulos que analisam os dados de informações empresariais. 

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial utiliza o seu computador ou máquina virtual como um dispositivo de Internet das Coisas. Para ativar o seu computador para um dispositivo do IoT Edge, são precisos os seguintes serviços:

* Pip do Python, para instalar o runtime do IoT Edge.
   * Linux: `sudo apt-get install python-pip`.
     * _Tenha em atenção que em determinadas distribuições (por exemplo, Raspbian), também poderá ter de atualizar determinados pacotes do pip e instalar dependências adicionais:_
     ```
     sudo pip install --upgrade setuptools pip
     
     sudo apt-get install python2.7-dev libffi-dev libssl-dev
     ```
   * MacOS: `sudo easy_install pip`.
* Docker, para executar os módulos do IoT Edge
   * [Instale o Docker para Linux][lnk-docker-ubuntu] e certifique-se de que está a ser executado. 
   * [Instale o Docker para Mac][lnk-docker-mac] e certifique-se de que está a ser executado. 

## <a name="create-an-iot-hub"></a>Criar um hub IoT

Comece o tutorial ao criar o seu Hub IoT.
![Criar Hub IoT][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registar um dispositivo do IoT Edge

Registe um dispositivo do IoT Edge no seu Hub IoT recentemente criado.
![Registar um dispositivo][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="install-and-start-the-iot-edge-runtime"></a>Instalar e iniciar o runtime do IoT Edge

Instalar e iniciar o runtime do Azure IoT Edge no seu dispositivo. 
![Registar um dispositivo][5]

O runtime do IoT Edge é implementado em todos os dispositivos do IoT Edge. É composto por dois módulos. O **agente do IoT Edge** facilita a implementação e monitorização de módulos no dispositivo do IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT. Quando configura o runtime no seu dispositivo novo, ao princípio apenas o agente do IoT Edge irá iniciar. O hub do IoT Edge aparece mais tarde quando implementar um módulo. 

No computador onde irá executar o dispositivo do IoT Edge, transfira o script de controlo do IoT Edge:
```cmd
sudo pip install -U azure-iot-edge-runtime-ctl
```

Configure o runtime com a cadeia de ligação do dispositivo do IoT Edge a partir da secção anterior:
```cmd
sudo iotedgectl setup --connection-string "{device connection string}" --nopass
```

Inicie o runtime:
```cmd
sudo iotedgectl start
```

Verifique o Docker para ver se o agente do IoT Edge está a ser executado como um módulo:
```cmd
sudo docker ps
```

![Ver edgeAgent no Docker](./media/tutorial-simulate-device-linux/docker-ps.png)

## <a name="deploy-a-module"></a>Implementar um módulo

Gira o seu dispositivo do Azure IoT Edge a partir da cloud para implementar um módulo que irá enviar dados telemétricos para o Hub IoT.
![Registar um dispositivo][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Ver os dados gerados

Neste tutorial, criou um novo dispositivo do IoT Edge e instalou o runtime do IoT Edge no mesmo. Em seguida, utilizou o portal do Azure para emitir um módulo do IoT Edge para ser executado no dispositivo, sem ter de realizar alterações no dispositivo propriamente dito. Neste caso, o módulo que emitiu cria dados ambientais que pode utilizar para os tutoriais. 

Abra novamente a linha de comandos no computador que está a executar o seu dispositivo simulado. Certifique-se de que o módulo implementado a partir da cloud está em execução no seu dispositivo do IoT Edge:

```cmd
sudo docker ps
```

![Ver três módulos no seu dispositivo](./media/tutorial-simulate-device-linux/docker-ps2.png)

Veja as mensagens que estão a ser enviadas do módulo tempSensor para a cloud:

```cmd
sudo docker logs -f tempSensor
```

![Ver os dados a partir do seu módulo](./media/tutorial-simulate-device-linux/docker-logs.png)

Também pode ver a telemetria que o dispositivo está a enviar com a [ferramenta do explorador do Hub IoT][lnk-iothub-explorer]. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou um novo dispositivo do IoT Edge e utilizou a interface da cloud do Azure IoT Edge para implementar código no dispositivo. Agora tem um dispositivo simulado a gerar dados não processados sobre o seu ambiente. 

Este tutorial é o pré-requisito para todos os outros tutoriais do IoT Edge. Pode continuar para qualquer um dos outros tutoriais para saber mais sobre como o Azure IoT Edge o pode ajudar a transformar estes dados em informações empresariais no Edge.

> [!div class="nextstepaction"]
> [Programar e implementar código C# como módulo](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/tutorial-install-iot-edge/view-module.png
[2]: ./media/tutorial-install-iot-edge/install-edge-full.png
[3]: ./media/tutorial-install-iot-edge/create-iot-hub.png
[4]: ./media/tutorial-install-iot-edge/register-device.png
[5]: ./media/tutorial-install-iot-edge/start-runtime.png
[6]: ./media/tutorial-install-iot-edge/deploy-module.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-docker-mac]: https://docs.docker.com/docker-for-mac/install/
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
