---
title: Guia de início rápido sobre como enviar telemetria para o Hub IoT do Azure (Python) | Microsoft Docs
description: Neste guia de início rápido, irá executar um exemplo de aplicação Python para enviar telemetria simulada para um hub IoT e utilizar um utilitário para ler telemetria do mesmo.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/30/2018
ms.author: dobett
ms.openlocfilehash: 7d4d29b7502f081de8385c7d88687ece4905b02b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38688391"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-python"></a>Guia de Início Rápido: Enviar telemetria a partir de um dispositivo para um hub IoT e ler a telemetria do hub com uma aplicação back-end (Python)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

O Hub IoT é um serviço do Azure que lhe permite ingerir elevados volumes de telemetria dos seus dispositivos IoT para a cloud para armazenamento ou processamento. Neste guia de início rápido, irá enviar telemetria a partir de uma aplicação de dispositivo simulado, através do Hub IoT, para uma aplicação back-end para processamento.

Este guia de início rápido utiliza uma aplicação Python pré-escrita para enviar a telemetria e um utilitário de CLI para lê-la a partir do hub. Antes de executar estas duas aplicações, tem de criar um hub IoT e registar um dispositivo no hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

As duas aplicações de exemplo que executa neste guia de início rápido são escritas com Python. Precisa do Python 2.7.X ou 3.5.X no seu computador de desenvolvimento.

Pode transferir o Python para múltiplas plataformas em [Python.org](https://www.python.org/downloads/).

Pode verificar qual a versão atual do Python no seu computador de desenvolvimento através de um dos seguintes comandos:

```python
python --version
```

```python
python3 --version
```

Transfira o projeto Python de exemplo a partir de https://github.com/Azure-Samples/azure-iot-samples-python/archive/master.zip e extraia o arquivo ZIP.

Para instalar o utilitário de CLI que lê a telemetria do hub IoT, instale primeiro o Node.js v4.X.X ou versão posterior no seu computador de desenvolvimento. Pode transferir o Node.js para múltiplas plataformas em [nodejs.org](https://nodejs.org).

Pode verificar qual a versão atual do Node.js no seu computador de desenvolvimento através do seguinte comando:

```cmd/sh
node --version
```

Para instalar o utilitário de CLI `iothub-explorer`, execute o seguinte comando:

```cmd/sh
npm install -g iothub-explorer
```

## <a name="create-an-iot-hub"></a>Criar um hub IoT

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Registar um dispositivo

É necessário registar um dispositivo no hub IoT antes de o mesmo se poder ligar. Neste início rápido, vai utilizar a CLI do Azure para registar um dispositivo simulado.

1. Adicione a extensão da CLI do Hub IoT e crie a identidade do dispositivo. Substitua `{YourIoTHubName}` pelo nome que escolheu para o seu hub IoT:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyPythonDevice
    ```

    Se escolher um nome diferente para o seu dispositivo, atualize o nome de dispositivo a aplicação de exemplo antes de executá-la.

1. Execute o seguinte comando para obter a _cadeia de ligação do dispositivo_ do dispositivo que acabou de registar:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyPythonDevice --output table
    ```

    Anote a cadeia de ligação do dispositivo, que se parece com `Hostname=...=`. Irá utilizar este valor mais adiante no guia de início rápido.

1. Também precisa de uma _cadeia de ligação do serviço_ para permitir que o utilitário de CLI `iothub-explorer` se ligue ao seu hub IoT e obtenha as mensagens. O seguinte comando obtém a cadeia de ligação do serviço do seu hub IoT:

    ```azurecli-interactive
    az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
    ```

    Anote a cadeia de ligação do serviço, que se parece com `Hostname=...=`. Irá utilizar este valor mais adiante no guia de início rápido. A cadeia de ligação do serviço é diferente da cadeia de ligação de dispositivo.

## <a name="send-simulated-telemetry"></a>Enviar telemetria simulada

A aplicação de dispositivo simulado liga-se a um ponto final específico do dispositivo no seu hub IoT e envia telemetria simulada de humidade e de temperatura.

1. Numa janela de terminal, navegue para a pasta raiz do projeto Python de exemplo. Em seguida, navegue para a pasta **iot-hub\Quickstarts\simulated-device**.

1. Abra o ficheiro **SimulatedDevice.py** num editor de texto à sua escolha.

    Substitua o valor da variável `CONNECTION_STRING` pela cadeia de ligação do dispositivo que anotou anteriormente. Em seguida, guarde as alterações feitas ao ficheiro **SimulatedDevice.py**.

1. Na janela de terminal, execute os seguintes comandos para instalar as bibliotecas necessárias para a aplicação de dispositivo simulado:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Na janela de terminal, execute os seguintes comandos para executar a aplicação de dispositivo simulado:

    ```cmd/sh
    python SimulatedDevice.py
    ```

    A seguinte captura de ecrã mostra a saída à medida que a aplicação de dispositivo simulado envia telemetria para o seu hub IoT:

    ![Executar o dispositivo simulado](media/quickstart-send-telemetry-python/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>Ler a telemetria a partir do seu hub

O utilitário da CLI `iothub-explorer` liga-se ao ponto final **Events** do lado do serviço no Hub IoT. O utilitário recebe as mensagens dispositivo para cloud enviadas a partir do seu dispositivo simulado. Uma aplicação back-end do Hub IoT é normalmente executada na cloud para receber e processar mensagens do dispositivo para a cloud.

Noutra janela de terminal, execute os seguintes comandos e substitua `{your hub service connection string}` pela cadeia de ligação do serviço que anotou anteriormente:

```cmd/sh
iothub-explorer monitor-events MyPythonDevice --login {your hub service connection string}
```

A seguinte captura de ecrã mostra a saída à medida que o utilitário recebe telemetria enviada pelo dispositivo simulado para o hub:

![Executar a aplicação back-end](media/quickstart-send-telemetry-python/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Passos seguintes

Neste guia de início rápido, configurou um hub IoT, registou um dispositivo, enviou telemetria simulada para o hub com uma aplicação Python e leu a telemetria do hub com uma simples aplicação back-end.

Para saber como controlar o seu dispositivo simulado a partir de uma aplicação back-end, continue para o guia de início rápido seguinte.

> [!div class="nextstepaction"]
> [Guia de Início Rápido: controlar um dispositivo ligado a um hub IoT](quickstart-control-device-python.md)