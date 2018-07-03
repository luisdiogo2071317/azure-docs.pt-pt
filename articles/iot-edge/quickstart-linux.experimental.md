---
title: Simular o Azure IoT Edge no Linux | Microsoft Docs
description: Neste início rápido, saiba como implementar código pré-construído remotamente num dispositivo IoT Edge.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 6b63c10a8c092d6568f8caf9842f007a5dc9c027
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049167"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Início rápido: Implementar o seu primeiro módulo do IoT Edge num dispositivo Linux x64

O Azure IoT Edge permite-lhe realizar o processamento de dados e de análise nos seus dispositivos, em vez de ter de solicitar todos os dados para a cloud. Os tutoriais do IoT Edge demonstram como implementar diferentes tipos de módulos, mas primeiro precisa de um dispositivo para testar. 

Neste início rápido, vai aprender a:

1. Criar um Hub IoT.
2. Registar um dispositivo IoT Edge no seu hub IoT.
3. Iniciar o runtime do IoT Edge.
4. Implementar remotamente um módulo num dispositivo IoT Edge.

![Arquitetura do tutorial][2]

Este início rápido transforma o seu computador Linux ou máquina virtual num dispositivo IoT Edge. Em seguida, pode implementar um módulo a partir do portal do Azure no seu dispositivo. O módulo que vai implementar neste início rápido é um sensor simulado que gera dados de temperatura, humidade e pressão. Os outros tutoriais do Azure IoT Edge tiram partido do seu trabalho aqui realizado, ao implementar módulos que analisam os dados simulados de informações empresariais. 

Se não tiver uma subscrição do Azure ativa, crie uma [conta gratuita][lnk-account] antes de começar.

## <a name="create-an-iot-hub"></a>Criar um hub IoT

Comece o início rápido ao criar o seu Hub IoT no portal do Azure.
![Criar Hub IoT][3]

[!INCLUDE [iot-hub-create-hub](../../includes/iot-hub-create-hub.md)]

## <a name="register-an-iot-edge-device"></a>Registar um dispositivo do IoT Edge

Registe um dispositivo do IoT Edge no seu Hub IoT recentemente criado.
![Registar um dispositivo][4]

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]


## <a name="install-and-start-the-iot-edge-runtime"></a>Instalar e iniciar o runtime do IoT Edge

Instalar e iniciar o runtime do Azure IoT Edge no seu dispositivo. 
![Registar um dispositivo][5]

O runtime do IoT Edge é implementado em todos os dispositivos do IoT Edge. Tem três componentes. O **daemon de segurança do IoT Edge** é iniciado sempre um dispositivo Edge arranca limite e arranca o dispositivo ao iniciar o agente do IoT Edge. O **agente do IoT Edge** facilita a implementação e a monitorização de módulos no dispositivo IoT Edge, incluindo o hub do IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT. 

### <a name="register-your-device-to-use-the-software-repository"></a>Registar o dispositivo para utilizar o repositório de software

Os pacotes necessários para executar o runtime do IoT Edge são geridos num repositório de software. Configure o dispositivo IoT Edge para aceder a este repositório. 

Os passos nesta secção destinam-se a dispositivos com o **Ubuntu 16.04**. Para aceder ao repositório de software noutras versões do Linux, veja [Instalar o runtime do Azure IoT Edge no Linux (x64)](how-to-install-iot-edge-linux.md) ou [Instalar o runtime do Azure IoT Edge no Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md).

1. No computador que estiver a utilizar como um dispositivo IoT Edge, instale a configuração do repositório.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
   sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/
   ```

2. Instale uma chave pública para aceder ao repositório.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/
   ```

### <a name="install-a-container-runtime"></a>Instalar um runtime de contentor

O runtime do IoT Edge é um conjunto de contentores e a lógica que implementa no seu dispositivo IoT Edge é empacotada como contentores. Prepare o seu dispositivo para estes componentes através da instalação de um runtime de contentor.

Atualize **apt-get**.

   ```bash
   sudo apt-get update
   ```

Instale o Moby, um runtime de contentor, e os respetivos comandos da CLI. 

   ```bash
   sudo apt-get install moby-engine
   sudo apt-get install moby-cli   
   ```

### <a name="install-and-configure-the-iot-edge-security-daemon"></a>Instalar e configurar o daemon de segurança do IoT Edge

O daemon de segurança é instalado como um serviço de sistema para que o runtime do IoT Edge seja iniciado sempre que o seu dispositivo arranca. A instalação também inclui uma versão do **hsmlib** que permite ao daemon de segurança interagir com a segurança de hardware do dispositivo. 

1. Transfira e instale o Daemon de Segurança do IoT Edge. 

   ```bash
   sudo apt-get update
   sudo apt-get install iotedge
   ```

2. Abra o ficheiro de configuração do IoT Edge. É um ficheiro protegido, pelo que poderá ter de utilizar privilégios elevados para aceder ao mesmo.
   
   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

3. Adicione a cadeia de ligação do dispositivo IoT Edge que copiou quando registou o seu dispositivo. Substitua o valor da variável **device_connection_string** que copiou anteriormente neste início rápido.

4. Reinicie o Daemon de Segurança do Edge:

   ```bash
   sudo systemctl restart iotedge
   ```

5. Verifique se o Daemon de Segurança do Edge está em execução como serviço de sistema:

   ```bash
   sudo systemctl status iotedge
   ```

   ![Ver o Daemon do Edge em execução como serviço de sistema](./media/quickstart-linux/iotedged-running.png)

   Também pode ver os registos do Daemon de Segurança do Edge ao executar o seguinte comando:

   ```bash
   journalctl -u iotedge
   ```

6. Veja os módulos em execução no seu dispositivo: 

   ```bash
   iotedge list
   ```

   ![Ver um módulo no seu dispositivo](./media/quickstart-linux/iotedge-list-1.png)

## <a name="deploy-a-module"></a>Implementar um módulo

Gira o seu dispositivo Azure IoT Edge a partir da cloud para implementar um módulo que irá enviar dados telemétricos para o Hub IoT.
![Registar um dispositivo][6]

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]


## <a name="view-generated-data"></a>Ver os dados gerados

Neste início rápido, criou um novo dispositivo IoT Edge e instalou o runtime do IoT Edge no mesmo. Depois, utilizou o portal do Azure para emitir um módulo do IoT Edge para ser executado no dispositivo, sem ter de realizar alterações no dispositivo propriamente dito. Neste caso, o módulo que emitiu cria dados ambientais que pode utilizar para os tutoriais. 

Abra novamente a linha de comandos no computador que está a executar o seu dispositivo simulado. Certifique-se de que o módulo implementado a partir da cloud está em execução no seu dispositivo do IoT Edge:

   ```bash
   iotedge list
   ```

   ![Ver três módulos no seu dispositivo](./media/quickstart-linux/iotedge-list-2.png)

Veja as mensagens que estão a ser enviadas do módulo tempSensor:

   ```bash
   iotedge logs tempSensor -f 
   ```

![Ver os dados a partir do seu módulo](./media/quickstart-linux/iotedge-logs.png)

O módulo do sensor de temperatura poderá estar a aguardar pela ligação ao Hub do Edge se a última linha vista no registo for `Using transport Mqtt_Tcp_Only`. Experimente terminar o módulo e permitir que o Agente do Edge o reinicie. Pode terminá-lo com o comando `sudo docker stop tempSensor`.

Também pode ver a telemetria que o dispositivo está a enviar através da ferramenta [Explorador do Hub IoT][lnk-iothub-explorer] ou da [extensão do Toolkit do Azure IoT para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 

## <a name="clean-up-resources"></a>Limpar recursos

Se quiser avançar para os tutoriais do IoT Edge, pode utilizar o dispositivo que registou e configurou neste início rápido. Se quiser remover as instalações do seu dispositivo, utilize os comandos seguintes.  

Remova o runtime do IoT Edge.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Elimine os contentores criados no seu dispositivo. 

   ```bash
   sudo docker rm -f $(sudo docker ps -aq)
   ```

Remova o runtime do contentor.

   ```bash
   sudo apt-get remove --purge moby
   ```

Quando já não precisar do hub IoT do Azure ou do dispositivo IoT Edge que criou neste início rápido, pode eliminá-los no portal do Azure. Navegue para a página de descrição geral do hub IoT e selecione **Eliminar**. 

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou um novo dispositivo IoT Edge e utilizou a interface da cloud do Azure IoT Edge para implementar código no dispositivo. Agora tem um dispositivo simulado a gerar dados não processados sobre o seu ambiente. 

Este início rápido é o pré-requisito para todos os tutoriais do IoT Edge. Pode continuar para qualquer um dos tutoriais para saber mais sobre como o Azure IoT Edge o pode ajudar a transformar estes dados em informações empresariais no Edge.

> [!div class="nextstepaction"]
> [Filtrar os dados de sensores com uma Função do Azure](tutorial-deploy-function.md)


<!-- Images -->
[1]: ./media/quickstart-linux/view-module.png
[2]: ./media/quickstart-linux/install-edge-full.png
[3]: ./media/quickstart-linux/create-iot-hub.png
[4]: ./media/quickstart-linux/register-device.png
[5]: ./media/quickstart-linux/start-runtime.png
[6]: ./media/quickstart-linux/deploy-module.png
[7]: ./media/quickstart-linux/iotedged-running.png
[8]: ./media/tutorial-simulate-device-linux/running-modules.png
[9]: ./media/tutorial-simulate-device-linux/sensor-data.png

<!-- Links -->
[lnk-docker-ubuntu]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/ 
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
