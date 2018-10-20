---
title: Enviar eventos para Hubs de eventos do Azure com node. js | Documentos da Microsoft
description: Introdução ao envio de eventos para Hubs de eventos com o node. js.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 10/18/2018
ms.author: shvija
ms.openlocfilehash: 14ea98b9d31bee08b962e8b3801ed507472ba692
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49455798"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Enviar eventos para Hubs de eventos do Azure com node. js

Os Hubs de Eventos do Azure são uma plataforma de fluxo de Macrodados e um serviço de ingestão de eventos capaz de receber e processar milhões de eventos por segundo. Os Hubs de Eventos podem processar e armazenar eventos, dados ou telemetria produzidos por dispositivos e software distribuído. Os dados enviados para um hub de eventos podem ser transformados e armazenados em qualquer fornecedor de análise em tempo real ou adaptadores de armazenamento/criação de batches. Para uma visão geral detalhada dos Hubs de eventos, consulte [descrição geral dos Hubs de eventos](event-hubs-about.md) e [funcionalidades dos Hubs de eventos](event-hubs-features.md).

Este tutorial descreve como enviar eventos para um hub de eventos de um aplicativo escrito em node. js.

> [!NOTE]
> Pode transferir este guia de introdução como uma amostra a partir do [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client), substitua `EventHubConnectionString` e `EventHubName` cadeias de caracteres com os seus valores de hub de eventos, e executá-lo. Em alternativa, pode seguir os passos neste tutorial para criar o seu próprio.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa dos seguintes pré-requisitos:

- Versão node. js 8.x e superiores. Transferir a versão mais recente do LTS partir [ https://nodejs.org ](https://nodejs.org).
- Visual Studio Code (recomendado) ou qualquer outro IDE

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Criar um espaço de nomes de Hubs de Eventos e um hub de eventos
O primeiro passo consiste em utilizar o [portal do Azure](https://portal.azure.com) para criar um espaço de nomes do tipo Hubs de Eventos e obter as credenciais de gestão de que a sua aplicação precisa para comunicar com o hub de eventos. Para criar um espaço de nomes e um hub de eventos, siga o procedimento [este artigo](event-hubs-create.md), em seguida, continuar com os seguintes passos neste tutorial.

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
3. Configure a sua cadeia de ligação do hub de eventos, o nome do hub de eventos e o ponto final de armazenamento. Pode copiar a cadeia de ligação para o seu hub de eventos de **primário de cadeia de ligação** chave sob **RootManageSharedAccessKey** na página do Hub de eventos no portal do Azure. Para obter passos detalhados, consulte [obter cadeia de ligação](event-hubs-create.md#create-an-event-hubs-namespace).
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

Não se esqueça de definir as variáveis de ambiente antes de executar o script. Pode configurar esta opção na linha de comandos, conforme mostrado no exemplo seguinte, ou utilizar o [dotenv pacote](https://www.npmjs.com/package/dotenv#dotenv). 

```
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
