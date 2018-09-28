---
title: Início rápido - API REST do Azure SignalR Service | Microsoft Docs
description: Um início rápido para utilizar a API REST do Azure SignalR Service.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET
ms.workload: tbd
ms.date: 06/13/2018
ms.author: zhshang
ms.openlocfilehash: 93c1198ecfba6db809228ed6dcd99c705f53926c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972764"
---
# <a name="quickstart-broadcast-real-time-messages-from-console-app"></a>Início rápido: Transmitir mensagens em tempo real a partir da aplicação de consola

O Azure SignalR Service apresenta a [API REST](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md) para suportar o servidor para cenários de comunicação de cliente, como a difusão. Pode escolher qualquer linguagem de programação que possa fazer a chamada à API REST. Pode publicar mensagens para todos os clientes ligados, um cliente específico por nome ou um grupo de clientes.

Neste início rápido, irá aprender a enviar mensagens a partir de uma aplicação de linha de comandos para aplicações de cliente ligadas em C#.

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido pode ser executado no macOS, Windows ou Linux.
* [SDK .NET Core](https://www.microsoft.com/net/download/core)
* Um editor de texto ou editor de código da sua preferência.


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em <https://portal.azure.com/> com a sua conta do Azure.


[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

## <a name="clone-the-sample-application"></a>Clonar a aplicação de exemplo

Enquanto o serviço está a ser implementado, vamos passar para preparar o código. Clone a [aplicação de exemplo do GitHub](https://github.com/aspnet/AzureSignalR-samples.git), defina a cadeia de ligação do Serviço SignalR e execute a aplicação localmente.

1. Abra uma janela de terminal do git. Alterar para uma pasta em que quer clonar o projeto de exemplo.

1. Execute o seguinte comando para clonar o repositório de exemplo. Este comando cria uma cópia da aplicação de exemplo no seu computador.

    ```bash
    git clone https://github.com/aspnet/AzureSignalR-samples.git
    ```

## <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

Este exemplo é uma aplicação de consola que mostra a utilização do Azure SignalR Service. Ele oferece dois modos:

- Modo de Servidor: utilize comandos simples para chamar a API REST do Azure SignalR Service.
- Modo de Cliente: ligar ao Azure SignalR Service e receber mensagens do servidor.

Também pode encontrar como gerar um token de acesso para autenticar com o Azure SignalR Service.

### <a name="build-the-executable-file"></a>Criar o ficheiro executável

Utilizamos o macOS osx.10.13-x64 como exemplo. Pode encontrar a [referência](https://docs.microsoft.com/dotnet/core/rid-catalog) sobre como criar noutras plataformas.
```bash
cd AzureSignalR-samples/samples/Serverless/

dotnet publish -c Release -r osx.10.13-x64
```

### <a name="start-a-client"></a>Iniciar um cliente

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="start-a-server"></a>Iniciar um servidor

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless server -c "<ConnectionString>" -h <HubName>
```

## <a name="run-the-sample-without-publishing"></a>Executar o exemplo sem publicar

Também pode executar o comando abaixo para iniciar um servidor ou cliente

```bash
# Start a server
dotnet run -- server -c "<ConnectionString>" -h <HubName>

# Start a client
dotnet run -- client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="use-user-secrets-to-specify-connection-string"></a>Utilize segredos do utilizador para especificar a Cadeia de Ligação

Pode executar `dotnet user-secrets set Azure:SignalR:ConnectionString "<ConnectionString>"` no diretório de raiz do exemplo. Depois disso, deixará de precisar da opção `-c "<ConnectionString>"`.

## <a name="usage"></a>Utilização

Depois de o servidor ser iniciado, utilize o comando para enviar mensagem

```
send user <User Id>
send users <User List>
send group <Group Name>
send groups <Group List>
broadcast
```

Pode iniciar vários clientes com nomes de cliente diferentes.


[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]
