---
title: Enviar eventos para Hubs de eventos do Azure com node. js | Documentos da Microsoft
description: Introdução ao envio de eventos para Hubs de eventos com o node. js.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 09/18/2018
ms.author: shvija
ms.openlocfilehash: 3497abdd571282974d85ebaa58d06b47cbe3b159
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368771"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Enviar eventos para Hubs de eventos do Azure com node. js

Os Hubs de eventos do Azure é um sistema de gerenciamento de eventos altamente escalável que pode lidar com milhões de eventos por segundo, permitindo que os aplicativos processar e analisar enormes quantidades de dados produzidos pelos dispositivos ligados e outros sistemas. Depois de recolhidos para um hub de eventos, pode receber e lidar com eventos usando manipuladores de dentro do processo ou através do reencaminhamento para outros sistemas de análise.

Para saber mais sobre os Hubs de eventos, consulte a [descrição geral dos Hubs de eventos](event-hubs-about.md).

Este tutorial descreve como enviar eventos para um hub de eventos de um aplicativo escrito em node. js. Para receber eventos com o pacote de anfitrião do processador de eventos de node. js, veja [o artigo correspondente na receção](event-hubs-node-get-started-receive.md).

Código para este início rápido está disponível no [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client). 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa dos seguintes pré-requisitos:

- Versão node. js 8.x e superiores. Transferir a versão mais recente do LTS partir [ https://nodejs.org ](https://nodejs.org).
- Uma conta ativa do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita][] antes de começar.
- Visual Studio Code (recomendado) ou qualquer outro IDE

## <a name="create-a-namespace-and-event-hub"></a>Criar um hub de eventos e de espaço de nomes
O primeiro passo consiste em utilizar o portal do Azure para criar um espaço de nomes de Hubs de eventos com um hub de eventos. Se não tiver um já existente, pode criar estas entidades, seguindo as instruções em [criar um espaço de nomes de Hubs de eventos e um hub de eventos com o portal do Azure](event-hubs-create.md).

## <a name="clone-the-sample-git-repository"></a>Clonar o repositório de Git de exemplo
Clonar o repositório de Git de exemplo da [Github](https://github.com/Azure/azure-event-hubs-node) no seu computador. 

## <a name="install-nodejs-package"></a>Instalar o pacote de node. js
Instale o pacote de node. js para os Hubs de eventos do Azure no seu computador. 

```nodejs
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>Clonar o repositório de Git
Transfira ou clone a [exemplo](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) do Github. 

## <a name="send-events"></a>Enviar eventos
O SDK tem um clone contém vários exemplos que mostram-lhe como enviar eventos para um hub de eventos com node. js. Neste início rápido, vai utilizar o **simpleSender.js** exemplo. Para observar eventos a ser recebidos, abra outra terminal e receber eventos com o [receber exemplo](event-hubs-node-get-started-receive.md).

1. Abra o projeto no Visual Studio Code. 
2. Crie um ficheiro denominado **. env** sob a **cliente** pasta. Copie e cole as variáveis de ambiente de exemplo do **sample.env** na pasta raiz.
3. Configure a sua cadeia de ligação do hub de eventos, o nome do hub de eventos e o ponto final de armazenamento. Pode copiar a cadeia de ligação para o seu hub de eventos de **primário de cadeia de ligação** chave sob **RootManageSharedAccessKey** na página do Hub de eventos no portal do Azure. Para obter passos detalhados, consulte [obter cadeia de ligação](event-hubs-quickstart-portal.md#create-an-event-hubs-namespace).
4. Sobre a CLI do Azure, navegue para o **cliente** caminho da pasta. Instalar pacotes de nó e compile o projeto ao executar os comandos seguintes:

    ```nodejs
    npm i
    npm run build
    ```
5. Inicie o envio de eventos ao executar o seguinte comando: 

    ```nodejs
    node dist/examples/simpleSender.js
    ```


## <a name="review-the-sample-code"></a>Reveja o código de exemplo 
Aqui está o código de exemplo para enviar eventos para um hub de eventos com node. js. Manualmente pode criar um ficheiro de sampleSender.js e execute-o para enviar eventos para um hub de eventos. 

```nodejs
const { EventHubClient, EventPosition } = require('@azure/event-hubs');

const client = EventHubClient.createFromConnectionString(process.env["EVENTHUB_CONNECTION_STRING"], process.env["EVENTHUB_NAME"]);

async function main() {
    // NOTE: For receiving events from Azure Stream Analytics, please send Events to an EventHub where the body is a JSON object/array.
    // const eventData = { body: { "message": "Hello World" } };
    const data = { body: "Hello World 1" };
    const delivery = await client.send(data);
    console.log("message sent successfully.");
}

main().catch((err) => {
    console.log(err);
});

```

## <a name="next-steps"></a>Passos Seguintes

Consulte os seguintes artigos para saber mais sobre os Hubs de eventos:

* [Receber eventos com o node. js](event-hubs-node-get-started-receive.md)
* [Exemplos no GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

<!-- Links -->
[conta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
