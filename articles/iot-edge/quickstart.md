---
title: Início rápido do Azure IoT Edge + Windows | Microsoft Docs
description: Experimentar o Azure IoT Edge executando análise num dispositivo Edge simulado
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 05/03/2018
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 2fd16ab4ade61b1a08f93294051f4246e47839b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631739"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-from-the-azure-portal-to-a-windows-device---preview"></a>Início rápido: Implementar o primeiro módulo IoT Edge do portal do Azure para um dispositivo Windows – pré-visualização

Neste guia de início rápido, utilize a interface cloud do Azure IoT Edge para implementar código pré-construído remotamente num dispositivo IoT Edge. Para realizar esta tarefa, primeiro utilize o seu dispositivo do Windows para simular um dispositivo IoT Edge, em seguida, pode implementar um módulo no mesmo.

Se não tiver uma subscrição do Azure ativa, crie uma [conta gratuita][lnk-account] antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial parte do princípio de que está a utilizar um computador ou uma máquina virtual que executa o Windows para simular um dispositivo de Internet das Coisas. Se estiver a executar o Windows numa máquina virtual, ative a [virtualização aninhada][lnk-nested] e aloque, pelo menos, 2 GB de memória. 

1. Certifique-se de que está a utilizar uma versão suportada do Windows:
   * Windows 10 
   * Windows Server
2. Instale o [Docker para Windows][lnk-docker] e certifique-se de que está a ser executado.
3. Instale o [Python no Windows] [ lnk-python] e certifique-se de que pode utilizar o comando do pip. Este início rápido foi testado com as versões do Python >=2.7.9 e >=3.5.4.  
4. Execute o comando seguinte para transferir o script de controlo do IoT Edge.

   ```cmd
   pip install -U azure-iot-edge-runtime-ctl
   ```

> [!NOTE]
> O Azure IoT Edge pode executar contentores do Windows ou do Linux. Para utilizar contentores do Windows, tem de executar:
>    * Windows 10 Fall Creators Update, ou
>    * Windows Server 1709 (Compilação 16299), ou
>    * Windows IoT Core (Compilação 16299) num dispositivo baseado em x64
>
> Para o Windows IoT Core, siga as instruções em [Instalar o runtime do IoT Edge no Windows IoT Core][lnk-install-iotcore]. Caso contrário, basta [configurar o Docker para utilizar contentores Windows][lnk-docker-containers]e, opcionalmente, validar os pré-requisitos com o comando seguinte do PowerShell:
>    ```powershell
>    Invoke-Expression (Invoke-WebRequest -useb https://aka.ms/iotedgewin)
>    ```

## <a name="create-an-iot-hub-with-azure-cli"></a>Criar um Hub IoT com a CLI do Azure

Crie um Hub IoT na sua subscrição do Azure. O nível gratuito do Hub IoT funciona para este início rápido. Se já tiver utilizado o Hub IoT no passado e se já tiver um hub gratuito criado, pode ignorar esta secção e avançar para [Registar um dispositivo IoT Edge][anchor-register]. Cada subscrição só pode ter um Hub IoT gratuito. 

1. Inicie sessão no [Portal do Azure][lnk-portal]. 
1. Selecione o botão do **Cloud Shell**. 

   ![Botão do Cloud Shell][1]

1. Crie um grupo de recursos. O código seguinte cria um grupo de recursos denominado **IoTEdge** na região **E.U.A. Oeste**:

   ```azurecli
   az group create --name IoTEdge --location westus
   ```

1. Crie um Hub IoT no seu novo grupo de recursos. O código seguinte cria um hub **F1** gratuito denominado **MyIotHub** no grupo de recursos **IoTEdge**:

   ```azurecli
   az iot hub create --resource-group IoTEdge --name MyIotHub --sku F1 
   ```

## <a name="register-an-iot-edge-device"></a>Registar um dispositivo do IoT Edge

[!INCLUDE [iot-edge-register-device](../../includes/iot-edge-register-device.md)]

## <a name="configure-the-iot-edge-runtime"></a>Configurar o runtime do IoT Edge

O runtime do IoT Edge é implementado em todos os dispositivos do IoT Edge. É composto por dois módulos. Primeiro, o agente do IoT Edge facilita a implementação e a monitorização de módulos no dispositivo IoT Edge. Segundo, o Hub IoT Edge gere as comunicações entre os módulos no dispositivo IoT Edge e entre o dispositivo e o Hub IoT. 

Configure o runtime com a cadeia de ligação do dispositivo IoT Edge a partir da secção anterior.

```cmd
iotedgectl setup --connection-string "{device connection string}" --nopass
```

Inicie o runtime.

```cmd
iotedgectl start
```

Verifique o Docker para ver se o agente do IoT Edge está a ser executado como um módulo.

```cmd
docker ps
```

![Ver o edgeAgent no Docker](./media/tutorial-simulate-device-windows/docker-ps.png)

## <a name="deploy-a-module"></a>Implementar um módulo

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Ver os dados gerados

Neste início rápido, criou um novo dispositivo IoT Edge e instalou o runtime do IoT Edge no mesmo. Depois, utilizou o portal do Azure para emitir um módulo do IoT Edge para ser executado no dispositivo, sem ter de realizar alterações no dispositivo propriamente dito. Neste caso, o módulo que emitiu cria dados ambientais que pode utilizar para os tutoriais. 

Abra novamente a linha de comandos no computador que está a executar o seu dispositivo simulado. Certifique-se de que o módulo implementado a partir da cloud está em execução no seu dispositivo IoT Edge. 

```cmd
docker ps
```

![Ver três módulos no seu dispositivo](./media/tutorial-simulate-device-windows/docker-ps2.png)

Veja as mensagens que estão a ser enviadas do módulo tempSensor para a cloud. 

```cmd
docker logs -f tempSensor
```

![Ver os dados a partir do seu módulo](./media/tutorial-simulate-device-windows/docker-logs.png)

Também pode ver a telemetria que o dispositivo está a enviar com a [ferramenta do explorador do Hub IoT][lnk-iothub-explorer]. 
## <a name="clean-up-resources"></a>Limpar recursos

Se pretender remover o dispositivo simulado que criou, juntamente com os contentores do Docker que foram iniciados para cada módulo, utilize o comando seguinte: 

```cmd
iotedgectl uninstall
```

Quando já não necessitar do Hub IoT que criou, pode utilizar o comando [az iot hub delete] [ lnk-delete] para remover o recurso e todos os dispositivos associados ao mesmo:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

## <a name="next-steps"></a>Passos seguintes

Aprendeu a implementar um módulo do IoT Edge num dispositivo IoT Edge. Agora, tente implementar tipos diferentes de serviços do Azure como módulos, para poder analisar dados no Edge. 

* [Implementar uma Função do Azure como um módulo](tutorial-deploy-function.md)
* [Implementar o Azure Stream Analytics como um módulo](tutorial-deploy-stream-analytics.md)
* [Implementar o seu próprio código como um módulo](tutorial-csharp-module.md)


<!-- Images -->
[1]: ./media/quickstart/cloud-shell.png

<!-- Links -->
[lnk-docker]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers
[lnk-python]: https://www.python.org/downloads/
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-account]: https://azure.microsoft.com/free
[lnk-portal]: https://portal.azure.com
[lnk-nested]: https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
[lnk-delete]: https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az_iot_hub_delete
[lnk-install-iotcore]: how-to-install-iot-core.md

<!-- Anchor links -->
[anchor-register]: #register-an-iot-edge-device
