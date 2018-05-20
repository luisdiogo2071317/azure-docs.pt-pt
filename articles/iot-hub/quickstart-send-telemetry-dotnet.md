---
title: 'Guia de Início Rápido: enviar telemetria para o Hub IoT do Azure (C#) | Microsoft Docs'
description: Neste guia de início rápido, irá executar duas aplicações C# de exemplo para enviar telemetria simulada para um hub IoT e ler telemetria do mesmo para processamento na cloud.
services: iot-hub
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: ns
ms.date: 04/30/2018
ms.author: dobett
ms.openlocfilehash: 9feade4407f800312556a1db39ce6c315c9330c2
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-c"></a>Guia de Início Rápido: enviar telemetria a partir de um dispositivo para um hub IoT e ler a telemetria do hub com uma aplicação back-end (C#)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

O Hub IoT é um serviço do Azure que lhe permite ingerir elevados volumes de telemetria dos seus dispositivos IoT para a cloud para armazenamento ou processamento. Neste guia de início rápido, irá enviar telemetria a partir de uma aplicação de dispositivo simulado, através do Hub IoT, para uma aplicação back-end para processamento.

Este guia de início rápido utiliza duas aplicações C# pré-escritas, uma para enviar a telemetria e outra para ler a telemetria a partir do hub. Antes de executar estas duas aplicações, tem de criar um hub IoT e registar um dispositivo no hub.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

As duas aplicações de exemplo que executa neste guia de início rápido são escritas com C#. Precisa do SDK de .NET Core 2.1.0 ou posterior no seu computador de desenvolvimento.

Pode transferir o SDK de .NET Core para múltiplas plataformas em [.NET](https://www.microsoft.com/net/download/all).

Pode verificar qual a versão atual do C# no seu computador de desenvolvimento através do seguinte comando:

```cmd/sh
dotnet --version
```

Transfira o projeto C# de exemplo de https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip e extraia o arquivo ZIP.

## <a name="create-an-iot-hub"></a>Criar um hub IoT

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Registar um dispositivo

É necessário registar um dispositivo no hub IoT antes de o mesmo se poder ligar. Neste início rápido, vai utilizar a CLI do Azure para registar um dispositivo simulado.

1. Adicione a extensão da CLI do Hub IoT e crie a identidade do dispositivo. Substitua `{YourIoTHubName}` pelo nome que escolheu para o seu hub IoT:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyDotnetDevice
    ```

    Se escolher um nome diferente para o seu dispositivo, atualize o nome do dispositivo em aplicações de exemplo antes de executá-las.

1. Execute o seguinte comando para obter a _cadeia de ligação do dispositivo_ do dispositivo que acabou de registar:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyDotnetDevice --output table
    ```

    Anote a cadeia de ligação do dispositivo, que se parece com `Hostname=...=`. Irá utilizar este valor mais adiante no guia de início rápido.

1. Também precisa do _ponto final compatível com hub de eventos_, do _caminho compatível com hub de eventos_ e da _chave primária iothubowner_ do hub IoT para permitir que a aplicação back-end se ligue ao hub IoT e obtenha as mensagens. Os seguintes comandos obtêm estes valores para o hub IoT:

    ```azurecli-interactive
    az iot hub show --query properties.eventHubEndpoints.events.endpoint --name {YourIoTHubName}

    az iot hub show --query properties.eventHubEndpoints.events.path --name {YourIoTHubName}

    az iot hub policy show --name iothubowner --query primaryKey --hub-name {your IoT Hub name}
    ```

    Tome nota destes três valores, que irá utilizar mais tarde neste guia de início rápido.

## <a name="send-simulated-telemetry"></a>Enviar telemetria simulada

A aplicação de dispositivo simulado liga-se a um ponto final específico do dispositivo no seu hub IoT e envia telemetria simulada de humidade e de temperatura.

1. Numa janela de terminal, navegue para a pasta raiz do projeto C# de exemplo. Em seguida, navegue para a pasta **Quickstarts\simulated-device**.

1. Abra o ficheiro **SimulatedDevice.cs** num editor de texto à sua escolha.

    Substitua o valor da variável `connectionString` pela cadeia de ligação do dispositivo que anotou anteriormente. Em seguida, guarde as alterações feitas ao ficheiro **SimulatedDevice.cs**.

1. Na janela de terminal, execute os seguintes comandos para instalar os pacotes necessários para a aplicação de dispositivo simulado:

    ```cmd/sh
    dotnet restore
    ```

1. Na janela de terminal, execute o seguinte comando para compilar e executar a aplicação de dispositivo simulado:

    ```cmd/sh
    dotnet run
    ```

    A seguinte captura de ecrã mostra a saída à medida que a aplicação de dispositivo simulado envia telemetria para o seu hub IoT:

    ![Executar o dispositivo simulado](media/quickstart-send-telemetry-dotnet/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>Ler a telemetria a partir do seu hub

A aplicação back-end liga-se ao ponto final de **eventos** do lado do serviço no seu Hub IoT. A aplicação recebe as mensagens do dispositivo para a cloud enviadas a partir do seu dispositivo simulado. Uma aplicação back-end do Hub IoT é normalmente executada na cloud para receber e processar mensagens do dispositivo para a cloud.

1. Noutra janela de terminal, navegue para a pasta raiz do projeto C# de exemplo. Em seguida, navegue para a pasta **Quickstarts\read-d2c-messages**.

1. Abra o ficheiro **ReadDeviceToCloudMessages.cs** num editor de texto à sua escolha.

    Substitua o valor da variável `eventHubsCompatibleEndpoint` pelo ponto final compatível com hub de eventos que anotou anteriormente.

    Substitua o valor da variável `eventHubsCompatiblePath` pelo caminho compatível com Hub de Eventos que anotou anteriormente.

    Substitua o valor da variável `iotHubSasKey` pela chave primária iothubowner que anotou anteriormente. Em seguida, guarde as alterações feitas ao ficheiro **ReadDeviceToCloudMessages.js**.

1. Na janela de terminal, execute os seguintes comandos para instalar as bibliotecas necessárias para a aplicação back-end:

    ```cmd/sh
    dotnet restore
    ```

1. Na janela de terminal, execute os seguintes comandos para compilar e executar a aplicação back-end:

    ```cmd/sh
    dotnet run
    ```

    A seguinte captura de ecrã mostra a saída à medida que a aplicação back-end recebe telemetria enviada pelo dispositivo simulado para o hub:

    ![Executar a aplicação back-end](media/quickstart-send-telemetry-dotnet/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Limpar recursos

Se tenciona concluir o guia de início rápido seguinte, saia do grupo de recursos e do hub IoT e reutilize-os mais tarde.

Se já não precisar do Hub IoT, elimine-o, bem como ao grupo de recursos, no portal. Para tal, selecione o grupo de recursos **qs-iot-hub-rg** que contém o seu hub IoT e clique em **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Neste guia de início rápido, configurou um hub IoT, registou um dispositivo, enviou telemetria simulada para o hub com uma aplicação C# e leu a telemetria do hub com uma simples aplicação back-end.

Para saber como controlar o seu dispositivo simulado a partir de uma aplicação back-end, continue para o guia de início rápido seguinte.

> [!div class="nextstepaction"]
> [Guia de Início Rápido: controlar um dispositivo ligado a um hub IoT](quickstart-control-device-dotnet.md)