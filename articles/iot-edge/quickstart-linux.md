---
title: Início rápido do Azure IoT Edge + Linux | Microsoft Docs
description: Neste início rápido, saiba como implementar código pré-construído remotamente num dispositivo IoT Edge.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 5346467dff40832aa35799ee3d532e99bf14d569
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38482079"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Início rápido: Implementar o seu primeiro módulo do IoT Edge num dispositivo Linux x64

O Azure IoT Edge move o poder da cloud para os seus dispositivos de Internet das Coisas. Neste guia de início rápido, irá aprender a utilizar a interface da cloud para implementar código pré-construído remotamente num dispositivo IoT Edge.

Neste início rápido, vai aprender a:

1. Criar um Hub IoT.
2. Registar um dispositivo IoT Edge no seu hub IoT.
3. Instalar e iniciar o runtime do IoT Edge no seu dispositivo.
4. Implementar remotamente um módulo num dispositivo IoT Edge.

![Arquitetura do início rápido][2]

Este início rápido transforma o seu computador Linux ou máquina virtual num dispositivo IoT Edge. Em seguida, pode implementar um módulo a partir do portal do Azure no seu dispositivo. O módulo que vai implementar neste início rápido é um sensor simulado que gera dados de temperatura, humidade e pressão. Os outros tutoriais do Azure IoT Edge tiram partido do seu trabalho aqui realizado, ao implementar módulos que analisam os dados simulados de informações empresariais. 

Se não tiver uma subscrição do Azure ativa, crie uma [conta gratuita][lnk-account] antes de começar.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Vai utilizar a CLI do Azure para concluir muitos dos passos neste início rápido e o IoT do Azure tem uma extensão para ativar funcionalidades adicionais. 

Adicione a extensão do IoT do Azure à instância da shell da cloud.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido utiliza uma máquina Linux como um dispositivo IoT Edge. Se não tiver uma disponível para teste, pode criar uma com a CLI do Azure. 

Crie um novo grupo de recursos. Pode utilizar este grupo de recursos para os outros recursos do Azure que criou neste início rápido, para facilitar a gestão.  

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

Criar a máquina virtual. Não precisa de uma máquina virtual muito grande para testar o IoT Edge. Um tamanho como **B1ms** é suficiente.

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username azureuser --generate-ssh-keys --size Standard_B1ms
   ```

## <a name="create-an-iot-hub"></a>Criar um hub IoT

Comece o início rápido ao criar o seu Hub IoT no portal do Azure.
![Criar Hub IoT][3]

O nível gratuito do Hub IoT funciona para este início rápido. Se tiver utilizado o Hub IoT anteriormente e já tiver um hub gratuito criado, pode utilizar esse hub IoT. Cada subscrição só pode ter um Hub IoT gratuito. 

1. No Azure Cloud Shell, crie um grupo de recursos caso ainda não o tenha feito como parte dos pré-requisitos. Ao colocar todos os recursos dos inícios rápidos e os tutoriais num grupo, pode geri-los em conjunto. 

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus
   ```

1. Crie um Hub IoT no seu novo grupo de recursos. O código seguinte cria um hub **F1** gratuito no grupo de recursos **IoTEdgeResources**. Substitua *{hub_name}* por um nome exclusivo para o hub IoT.

   ```azurecli-interactive
   az iot hub create --resource-group TestResources --name {hub_name} --sku F1 
   ```

   Se obtiver um erro porque já existe um hub gratuito na sua subscrição, altere o SKU para **S1**. 

## <a name="register-an-iot-edge-device"></a>Registar um dispositivo do IoT Edge

Registe um dispositivo do IoT Edge no seu Hub IoT recentemente criado.
![Registar um dispositivo][4]

Crie uma identidade de dispositivo para o seu dispositivo simulado para que este consiga comunicar com o seu hub IoT. Uma vez que os dispositivos do IoT Edge se comportam e podem ser geridos de forma diferente dos dispositivos do IoT típicos, declare esta opção como um dispositivo do IoT Edge desde o início. 

1. No Azure Cloud Shell, introduza o comando seguinte para criar um dispositivo com o nome **myEdgeDevice** no seu hub.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
   ```

1. Obtenha a cadeia de ligação para o seu dispositivo, que liga o dispositivo físico à respetiva identidade do Hub IoT. 

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

1. Copie a cadeia de ligação e guarde-a. Irá utilizar este valor para configurar o runtime do IoT Edge na secção seguinte. 


## <a name="install-and-start-the-iot-edge-runtime"></a>Instalar e iniciar o runtime do IoT Edge

Instalar e iniciar o runtime do Azure IoT Edge no seu dispositivo. 
![Registar um dispositivo][5]

O runtime do IoT Edge é implementado em todos os dispositivos do IoT Edge. Tem três componentes. O **daemon de segurança do IoT Edge** é iniciado sempre que um dispositivo Edge arranca e arranca o dispositivo ao iniciar o agente do IoT Edge. O **agente do IoT Edge** facilita a implementação e a monitorização de módulos no dispositivo IoT Edge, incluindo o hub do IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT. 

Conclua os passos seguintes na máquina Linux ou VM que preparou para este início rápido. 

### <a name="register-your-device-to-use-the-software-repository"></a>Registar o dispositivo para utilizar o repositório de software

Os pacotes precisos para executar o runtime do IoT Edge são geridos num repositório de software. Configure o dispositivo IoT Edge para aceder a este repositório. 

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

Instale o **Moby**, um runtime de contentor.

   ```bash
   sudo apt-get install moby-engine
   ```

Instale os comandos da CLI para Moby. 

   ```bash
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

3. Adicione a cadeia de ligação do dispositivo IoT Edge. Encontre a variável **device_connection_string** e atualize o respetivo valor com a cadeia que copiou depois de registar o seu dispositivo.

4. Guarde e feche o ficheiro. 

   `CTRL + X`, `Y`, `Enter`

4. Reinicie o daemon de segurança do IoT Edge.

   ```bash
   sudo systemctl restart iotedge
   ```

5. Verifique se o Daemon de Segurança do Edge está em execução como serviço de sistema.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Ver o Daemon do Edge em execução como serviço de sistema](./media/quickstart-linux/iotedged-running.png)

   Também pode ver os registos do Daemon de Segurança do Edge ao executar o seguinte comando:

   ```bash
   journalctl -u iotedge
   ```

6. Veja os módulos em execução no seu dispositivo. 

   >[!TIP]
   >Precisa de utilizar o *sudo* para executar `iotedge` comandos primeiro. Termine a sessão do computador e inicie sessão novamente para atualizar as permissões e, em seguida, pode executar comandos `iotedge` sem privilégios elevados. 

   ```bash
   sudo iotedge list
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
   sudo iotedge list
   ```
   Depois de encerrar e iniciar sessão, o *sudo* não é preciso para o comando acima.

   ![Ver três módulos no seu dispositivo](./media/quickstart-linux/iotedge-list-2.png)

Veja as mensagens que estão a ser enviadas do módulo tempSensor:

   ```bash
   sudo iotedge logs tempSensor -f 
   ```
Depois de encerrar e iniciar sessão, o *sudo* não é preciso para o comando acima.

![Ver os dados a partir do seu módulo](./media/quickstart-linux/iotedge-logs.png)

O módulo do sensor de temperatura poderá estar a aguardar pela ligação ao Hub do Edge se a última linha vista no registo for `Using transport Mqtt_Tcp_Only`. Experimente terminar o módulo e permitir que o Agente do Edge o reinicie. Pode terminá-lo com o comando `sudo docker stop tempSensor`.

Também pode ver a telemetria que o dispositivo está a enviar através da ferramenta [Explorador do Hub IoT][lnk-iothub-explorer] ou da [extensão do Toolkit do Azure IoT para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 


## <a name="clean-up-resources"></a>Limpar recursos

Se quiser avançar para os tutoriais do IoT Edge, pode utilizar o dispositivo que registou e configurou neste início rápido. Caso contrário, pode eliminar os recursos do Azure que criou e remover o runtime do IoT Edge do seu dispositivo. 

### <a name="delete-azure-resources"></a>Eliminar recursos do Azure

Se tiver criado a sua máquina virtual e o hub IoT num novo grupo de recursos, pode eliminar esse grupo e todos os recursos associados. Se tiver alguma coisa nesse grupo de recursos que pretende manter, então basta eliminar os recursos individuais que quer limpar. 

Para remover um grupo de recursos, siga estes passos: 

1. Inicie sessão no [Portal do Azure](https://portal.azure.com) e clique em **Grupos de recursos**.
2. Na caixa de texto **Filtrar por nome...**, escreva o nome do grupo de recursos que contém o seu Hub IoT. 
3. À direita do seu grupo de recursos na lista de resultados, clique em **...** e em **Eliminar grupo de recursos**.
4. É-lhe pedido que confirme a eliminação do grupo de recursos. Escreva o nome do grupo de recursos novamente para confirmar e, em seguida, clique em **Eliminar**. Após alguns instantes, o grupo de recursos e todos os recursos contidos no mesmo são eliminados.

### <a name="remove-the-iot-edge-runtime"></a>Remover o runtime do IoT Edge

Se quiser remover as instalações do seu dispositivo, utilize os comandos seguintes.  

Remova o runtime do IoT Edge.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Quando o runtime do IoT Edge é removido, os contentores que ele criou são parados, mas continuam no seu dispositivo. Veja todos os contentores.

   ```bash
   sudo docker ps -a
   ```

Elimine os contentores criados no seu dispositivo pelo runtime do IoT Edge. Altere o nome do contentor tempSensor, se o tiver chamado de outra forma. 

   ```bash
   sudo docker rm -f tempSensor
   sudo docker rm -f edgeHub
   sudo docker rm -f edgeAgent
   ```

Remova o runtime do contentor.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>Passos seguintes

Este início rápido é o pré-requisito para todos os tutoriais do IoT Edge. Pode continuar para qualquer um dos outros tutoriais para saber mais sobre como o Azure IoT Edge o pode ajudar a transformar estes dados em informações empresariais no Edge.

> [!div class="nextstepaction"]
> [Filtrar os dados de sensores com uma Função do Azure](tutorial-deploy-function.md)



<!-- Images -->
[0]: ./media/quickstart-linux/cloud-shell.png
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
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
