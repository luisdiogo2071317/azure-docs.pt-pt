---
title: Enviar eventos com o node. js - Event Hubs do Azure | Documentos da Microsoft
description: Este artigo fornece um passo a passo para criar uma aplicação node. js para enviar eventos de Hubs de eventos do Azure.
services: event-hubs
author: spelluru
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 02/19/2019
ms.author: spelluru
ms.openlocfilehash: ec3182d11f1b2ffa31acd05fa1f2db695f3f2cf7
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447723"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Enviar eventos para Hubs de eventos do Azure com node. js

Os Hubs de eventos do Azure é um grandes volumes de dados de transmissão em fluxo plataforma e o serviço de ingestão de eventos que pode receber e processar milhões de eventos por segundo. Os Hubs de Eventos podem processar e armazenar eventos, dados ou telemetria produzidos por dispositivos e software distribuído. Os dados enviados para um hub de eventos podem ser transformados e armazenados em qualquer fornecedor de análise em tempo real ou adaptadores de armazenamento/criação de batches. Para uma descrição geral detalhada dos Hubs de Eventos, veja [Descrição geral dos Hubs de Eventos](event-hubs-about.md) e [Funcionalidades dos Hubs de Eventos](event-hubs-features.md).

Este tutorial descreve como enviar eventos para um hub de eventos de um aplicativo escrito em node. js.

> [!NOTE]
> Pode transferir este início rápido como uma amostra a partir do [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client), substituir as cadeias de carateres `EventHubConnectionString` e `EventHubName` pelos seus valores de hub de eventos e executá-la. Em alternativa, pode seguir os passos neste tutorial para criar a sua própria.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa dos seguintes pré-requisitos:

- Versão node. js 8.x e superiores. Transferir a versão mais recente do LTS partir [ https://nodejs.org ](https://nodejs.org).
- Visual Studio Code (recomendado) ou qualquer outro IDE

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Criar um espaço de nomes de Hubs de Eventos e um hub de eventos
O primeiro passo consiste em utilizar o [portal do Azure](https://portal.azure.com) para criar um espaço de nomes do tipo Hubs de Eventos e obter as credenciais de gestão de que a sua aplicação precisa para comunicar com o hub de eventos. Para criar um espaço de nomes e um hub de eventos, siga o procedimento [este artigo](event-hubs-create.md), em seguida, prossiga com os seguintes passos neste tutorial.

Obter a cadeia de ligação para o espaço de nomes do hub de eventos ao seguir as instruções do artigo: [Obter cadeia de ligação](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Irá utilizar a cadeia de ligação mais tarde neste tutorial.

## <a name="clone-the-sample-git-repository"></a>Clonar o repositório de Git de exemplo
Clonar o repositório de Git de exemplo da [GitHub](https://github.com/Azure/azure-event-hubs-node) no seu computador. 

## <a name="install-nodejs-package"></a>Instalar o pacote de node. js
Instale o pacote de node. js para os Hubs de eventos do Azure no seu computador. 

```shell
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>Clonar o repositório de Git
Transfira ou clone a [exemplo](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) do GitHub. 

## <a name="send-events"></a>Enviar eventos
O SDK tem um clone contém vários exemplos que mostram-lhe como enviar eventos para um hub de eventos com node. js. Neste início rápido, vai utilizar o **simpleSender.js** exemplo. Para observar eventos a ser recebidos, abra outra terminal e receber eventos com o [receber exemplo](event-hubs-node-get-started-receive.md).

1. Abra o projeto no Visual Studio Code. 
2. Crie um ficheiro denominado **. env** sob a **cliente** pasta. Copie e cole as variáveis de ambiente de exemplo do **sample.env** na pasta raiz.
3. Configure a sua cadeia de ligação do hub de eventos, o nome do hub de eventos e o ponto final de armazenamento. Para obter instruções sobre como obter uma cadeia de ligação para um hub de eventos [obter cadeia de ligação](event-hubs-create.md#create-an-event-hubs-namespace).
4. Sobre a CLI do Azure, navegue para o **cliente** caminho da pasta. Instalar pacotes de nó e compile o projeto ao executar os comandos seguintes:

    ```shell
    npm i
    npm run build
    ```
5. Inicie o envio de eventos ao executar o seguinte comando: 

    ```shell
    node dist/examples/simpleSender.js
    ```


## <a name="review-the-sample-code"></a>Reveja o código de exemplo 
Rever o código de exemplo no arquivo simpleSender.js para enviar eventos para um hub de eventos.

```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
const lib_1 = require("../lib");
const dotenv = require("dotenv");
dotenv.config();
const connectionString = "EVENTHUB_CONNECTION_STRING";
const entityPath = "EVENTHUB_NAME";
const str = process.env[connectionString] || "";
const path = process.env[entityPath] || "";

async function main() {
    const client = lib_1.EventHubClient.createFromConnectionString(str, path);
    const data = {
        body: "Hello World!!"
    };
    const delivery = await client.send(data);
    console.log(">>> Sent the message successfully: ", delivery.tag.toString());
    console.log(delivery);
    console.log("Calling rhea-promise sender close directly. This should result in sender getting reconnected.");
    await Object.values(client._context.senders)[0]._sender.close();
    // await client.close();
}

main().catch((err) => {
    console.log("error: ", err);
});

```

Não se esqueça de definir as variáveis de ambiente antes de executar o script. Pode configurá-las na linha de comandos, conforme mostrado no exemplo seguinte, ou utilizar o [dotenv pacote](https://www.npmjs.com/package/dotenv#dotenv). 

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

## <a name="next-steps"></a>Passos Seguintes
Neste início rápido, enviou mensagens para um hub de eventos com node. js. Para saber como receber eventos de um hub de eventos com node. js, veja [receber eventos do hub de eventos - node. js](event-hubs-node-get-started-receive.md)

Verifique outras amostras de node. js para os Hubs de eventos no [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/).
