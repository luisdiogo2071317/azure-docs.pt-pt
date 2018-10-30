---
title: Início rápido do Azure IoT Edge + Linux | Microsoft Docs
description: Neste início rápido, saiba como implementar código pré-construído remotamente num dispositivo IoT Edge.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 08/14/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: a774873872d4b41c4ef5c005946db6b2a1b4e39e
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49955280"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-linux-x64-device"></a>Início rápido: Implementar o seu primeiro módulo do IoT Edge num dispositivo Linux x64

O Azure IoT Edge move o poder da cloud para os seus dispositivos de Internet das Coisas. Neste guia de início rápido, irá aprender a utilizar a interface da cloud para implementar código pré-construído remotamente num dispositivo IoT Edge.

Neste início rápido, vai aprender a:

1. Criar um Hub IoT.
2. Registar um dispositivo IoT Edge no seu hub IoT.
3. Instalar e iniciar o runtime do IoT Edge no seu dispositivo.
4. Implementar remotamente um módulo num dispositivo IoT Edge.

![Arquitetura do início rápido](./media/quickstart-linux/install-edge-full.png)

Este início rápido transforma o seu computador Linux ou máquina virtual num dispositivo IoT Edge. Em seguida, pode implementar um módulo a partir do portal do Azure no seu dispositivo. O módulo que vai implementar neste início rápido é um sensor simulado que gera dados de temperatura, humidade e pressão. Os outros tutoriais do Azure IoT Edge tiram partido do seu trabalho aqui realizado, ao implementar módulos que analisam os dados simulados de informações empresariais.

Se não tiver uma subscrição ativa do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Vai utilizar a CLI do Azure para concluir muitos dos passos neste início rápido e o IoT do Azure tem uma extensão para ativar funcionalidades adicionais. 

Adicione a extensão do IoT do Azure à instância da shell da cloud.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prerequisites"></a>Pré-requisitos

Recursos da cloud: 

* Um grupo de recursos para gerir todos os recursos que utilizar neste início rápido. 

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

Dispositivo IoT Edge

* Um dispositivo ou máquina virtual do Linux que funcione como o seu dispositivo IoT Edge. Se quiser criar uma máquina virtual no Azure, utilize o seguinte comando para começar rapidamente:

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username azureuser --generate-ssh-keys --size Standard_B1ms
   ```

## <a name="create-an-iot-hub"></a>Criar um hub IoT

Comece o início rápido ao criar o seu Hub IoT com a CLI do Azure.

![Criar o Hub IoT](./media/quickstart-linux/create-iot-hub.png)

O nível gratuito do Hub IoT funciona para este início rápido. Se tiver utilizado o Hub IoT anteriormente e já tiver um hub gratuito criado, pode utilizar esse hub IoT. Cada subscrição só pode ter um Hub IoT gratuito. 

O código seguinte cria um hub **F1** gratuito no grupo de recursos **IoTEdgeResources**. Substitua *{hub_name}* por um nome exclusivo para o hub IoT.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 
   ```

   Se obtiver um erro porque já existe um hub gratuito na sua subscrição, altere o SKU para **S1**.

## <a name="register-an-iot-edge-device"></a>Registar um dispositivo do IoT Edge

Registe um dispositivo do IoT Edge no seu hub IoT recentemente criado.
![Registar um dispositivo](./media/quickstart-linux/register-device.png)

Crie uma identidade de dispositivo para o seu dispositivo simulado para que este consiga comunicar com o seu hub IoT. A identidade do dispositivo reside na cloud e verá uma cadeia de ligação do dispositivo única para associar um dispositivo físico a uma identidade do dispositivo. 

Uma vez que os dispositivos do IoT Edge se comportam e podem ser geridos de forma diferente dos dispositivos do IoT típicos, declare esta opção como um dispositivo do IoT Edge desde o início. 

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

Instalar e iniciar o runtime do Azure IoT Edge no seu dispositivo IoT Edge. 
![Registar um dispositivo](./media/quickstart-linux/start-runtime.png)

O runtime do IoT Edge é implementado em todos os dispositivos do IoT Edge. Tem três componentes. O **daemon de segurança do IoT Edge** é iniciado sempre que um dispositivo Edge arranca e arranca o dispositivo ao iniciar o agente do IoT Edge. O **agente do IoT Edge** facilita a implementação e a monitorização de módulos no dispositivo IoT Edge, incluindo o hub do IoT Edge. O **hub do IoT Edge** gere as comunicações entre os módulos no dispositivo do IoT Edge e entre o dispositivo e o Hub IoT. 

Durante a configuração do runtime, apresenta a cadeia de ligação do dispositivo. Utilize a cadeia que obteve na CLI do Azure. Essa cadeia associa o dispositivo físico à identidade do dispositivo IoT Edge no Azure. 

Conclua os passos seguintes no computador Linux ou na VM que preparou para funcionar como dispositivo IoT Edge. 

### <a name="register-your-device-to-use-the-software-repository"></a>Registar o dispositivo para utilizar o repositório de software

Os pacotes precisos para executar o runtime do IoT Edge são geridos num repositório de software. Configure o dispositivo IoT Edge para aceder a este repositório. 

Os passos nesta secção destinam-se a dispositivos x64 com o **Ubuntu 16.04**. Para aceder ao repositório de software noutras versões do Linux ou arquiteturas de dispositivo, veja [Instalar o runtime do Azure IoT Edge no Linux (x64)](how-to-install-iot-edge-linux.md) ou [Instalar o runtime do Azure IoT Edge no Linux (ARM32v7/armhf)](how-to-install-iot-edge-linux-arm.md).

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

1. Atualize **apt-get**.

   ```bash
   sudo apt-get update
   ```

2. Instale o **Moby**, um runtime de contentor.

   ```bash
   sudo apt-get install moby-engine
   ```

3. Instale os comandos da CLI para Moby. 

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

3. Adicione a cadeia de ligação do dispositivo IoT Edge. Encontre a variável **device_connection_string** e atualize o respetivo valor com a cadeia que copiou depois de registar o seu dispositivo. Esta cadeia de ligação associa o dispositivo físico à identidade do dispositivo que criou no Azure.

4. Guarde e feche o ficheiro. 

   `CTRL + X`, `Y`, `Enter`

5. Reinicie o daemon de segurança do IoT Edge para aplicar as suas alterações.

   ```bash
   sudo systemctl restart iotedge
   ```

>[!TIP]
>Precisa de privilégios elevados para executar os comandos `iotedge`. Depois de terminar sessão do seu computador e iniciar sessão novamente pela primeira vez depois de instalar o runtime do IoT Edge, as suas permissões são atualizadas automaticamente. Até lá, utilize o **sudo** à frente dos comandos. 

### <a name="view-the-iot-edge-runtime-status"></a>Ver o estado de runtime do IoT Edge

Verifique se o runtime foi instalado e configurado corretamente.

1. Verifique se o Daemon de Segurança do Edge está em execução como serviço de sistema.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Ver o Daemon do Edge em execução como serviço de sistema](./media/quickstart-linux/iotedged-running.png)

2. Se precisar de resolver problemas relacionados com o serviço, obtenha os registos do serviço. 

   ```bash
   journalctl -u iotedge
   ```

3. Veja os módulos em execução no seu dispositivo. 

   ```bash
   sudo iotedge list
   ```

   ![Ver um módulo no seu dispositivo](./media/quickstart-linux/iotedge-list-1.png)

O seu dispositivo IoT Edge está agora configurado. Está pronto para executar módulos implantados na cloud. 

## <a name="deploy-a-module"></a>Implementar um módulo

Gira o seu dispositivo Azure IoT Edge a partir da cloud para implementar um módulo que irá enviar dados telemétricos para o Hub IoT.
![Registar um dispositivo](./media/quickstart-linux/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Ver os dados gerados

Neste início rápido, criou um novo dispositivo IoT Edge e instalou o runtime do IoT Edge no mesmo. Depois, utilizou o portal do Azure para emitir um módulo do IoT Edge para ser executado no dispositivo, sem ter de realizar alterações no dispositivo propriamente dito. Neste caso, o módulo que emitiu cria dados ambientais que pode utilizar para os tutoriais.

Abra a linha de comandos no seu dispositivo IoT Edge novamente. Certifique-se de que o módulo implementado a partir da cloud está em execução no seu dispositivo do IoT Edge:

   ```bash
   sudo iotedge list
   ```

   ![Ver três módulos no seu dispositivo](./media/quickstart-linux/iotedge-list-2.png)

Veja as mensagens que estão a ser enviadas do módulo tempSensor:

   ```bash
   sudo iotedge logs tempSensor -f
   ```

![Ver os dados a partir do seu módulo](./media/quickstart-linux/iotedge-logs.png)

O módulo do sensor de temperatura poderá estar a aguardar pela ligação ao Hub do Edge se a última linha vista no registo for `Using transport Mqtt_Tcp_Only`. Experimente terminar o módulo e permitir que o Agente do Edge o reinicie. Pode terminá-lo com o comando `sudo docker stop tempSensor`.

Também pode ver a telemetria à medida que chega ao hub IoT com a [extensão Azure IoT Toolkit para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit). 

## <a name="clean-up-resources"></a>Limpar recursos

Se quiser avançar para os tutoriais do IoT Edge, pode utilizar o dispositivo que registou e configurou neste início rápido. Caso contrário, pode eliminar os recursos do Azure que criou e remover o runtime do IoT Edge do seu dispositivo.

### <a name="delete-azure-resources"></a>Eliminar recursos do Azure

Se tiver criado a sua máquina virtual e o hub IoT num novo grupo de recursos, pode eliminar esse grupo e todos os recursos associados. Se tiver alguma coisa nesse grupo de recursos que pretende manter, então basta eliminar os recursos individuais que quer limpar. 

Remova o grupo de **IoTEdgeResources**.

   ```azurecli-interactive
   az group delete --name IoTEdgeResources 
   ```

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
   sudo apt-get remove --purge moby-cli
   sudo apt-get remove --purge moby-engine
   ```

## <a name="next-steps"></a>Passos seguintes

Este início rápido é o pré-requisito para todos os tutoriais do IoT Edge. Pode continuar para qualquer um dos outros tutoriais para saber mais sobre como o Azure IoT Edge o pode ajudar a transformar estes dados em informações empresariais no Edge.

> [!div class="nextstepaction"]
> [Filtrar os dados de sensores com uma Função do Azure](tutorial-deploy-function.md)
