---
title: Receber eventos com o node. js - Event Hubs do Azure | Documentos da Microsoft
description: Este artigo fornece um passo a passo para criar uma aplicação de node. js que recebe eventos dos Hubs de eventos do Azure.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 55e4ce4a59a498c5c22fe6e701e3140aa65e7a10
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55895249"
---
# <a name="receive-events-from-azure-event-hubs-using-nodejs"></a>Receber eventos dos Hubs de eventos do Azure com node. js

Os Hubs de eventos do Azure é um sistema de gerenciamento de eventos altamente escalável que pode lidar com milhões de eventos por segundo, permitindo que os aplicativos processar e analisar enormes quantidades de dados produzidos pelos dispositivos ligados e outros sistemas. Depois de recolhidos para um hub de eventos, pode receber e lidar com eventos usando manipuladores de dentro do processo ou através do reencaminhamento para outros sistemas de análise. Também pode capturar dados de eventos no armazenamento do Azure ou do Azure Data Lake Store antes de ela é processada.  

Para saber mais sobre os Hubs de eventos, consulte a [descrição geral dos Hubs de eventos](event-hubs-about.md).

Este tutorial mostra como receber eventos de um hub de eventos ao utilizar o Azure [EventProcessorHost](event-hubs-event-processor-host.md) numa aplicação node. js. O EventProcessorHost (EPH) ajuda-o com eficiência receber eventos de um hub de eventos através da criação de recetores todas as partições no grupo de consumidor de um hub de eventos. Ele metadados de pontos de verificação nas mensagens recebidas em intervalos regulares num Blob de armazenamento do Azure. Essa abordagem torna mais fácil continuar a receber mensagens a partir de onde parou num momento posterior.

O código para este início rápido está disponível no [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/processor).

> [!NOTE]
>  Para enviar eventos para Hubs de eventos com o node. js, consulte este artigo: [Enviar eventos para Hubs de eventos do Azure com node. js](event-hubs-node-get-started-send.md). 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa dos seguintes pré-requisitos:

- Versão node. js 8.x e superiores. Transferir a versão mais recente do LTS partir [ https://nodejs.org ](https://nodejs.org). Não utilize a versão mais antiga do LTS do node. js. 
- Uma conta ativa do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita][] antes de começar.

## <a name="create-a-namespace-and-event-hub"></a>Criar um hub de eventos e de espaço de nomes
O primeiro passo consiste em utilizar o portal do Azure para criar um espaço de nomes de Hubs de eventos com um hub de eventos. Se não tiver um já existente, pode criar estas entidades, seguindo as instruções em [criar um espaço de nomes de Hubs de eventos e um hub de eventos com o portal do Azure](event-hubs-create.md).

## <a name="create-a-storage-account-and-container"></a>Criar uma conta de armazenamento e um contentor
Para utilizar o EventProcessorHost, tem de ter uma conta de armazenamento do Azure. As informações de estado como concessões em partições e os pontos de verificação no fluxo de eventos são partilhados entre recetores com um contentor de armazenamento do Azure. Pode criar uma conta de armazenamento do Azure ao seguir as instruções em [este artigo](../storage/common/storage-quickstart-create-account.md).

## <a name="clone-the-git-repository"></a>Clonar o repositório de Git
Transfira ou clone a [exemplo](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples/) do GitHub. 

## <a name="install-the-eventprocessorhost"></a>Instalar o EventProcessorHost
Instale o EventProcessorHost para o módulo de Hubs de eventos. 

```shell
npm install @azure/event-processor-host
```

## <a name="receive-events-using-eventprocessorhost"></a>Receber eventos com o EventProcessorHost
O SDK tem um clone contém vários exemplos que lhe mostram como receber eventos de um hub de eventos com node. js. Neste início rápido, vai utilizar o **singleEPH.js** exemplo. Para observar eventos a ser recebidos, abra outra terminal e enviar eventos com o [enviar exemplo](event-hubs-node-get-started-send.md).

1. Abra o projeto no Visual Studio Code. 
2. Crie um ficheiro denominado **. env** sob a **processador** pasta. Copie e cole as variáveis de ambiente de exemplo do **sample.env** na pasta raiz.
3. Configure a sua cadeia de ligação do hub de eventos, o nome do hub de eventos e o ponto final de armazenamento. Pode copiar a cadeia de ligação para o seu hub de eventos de **primário de cadeia de ligação** chave sob **RootManageSharedAccessKey** na página do Hub de eventos no portal do Azure. Para obter passos detalhados, consulte [obter cadeia de ligação](event-hubs-create.md#create-an-event-hubs-namespace).
4. Sobre a CLI do Azure, navegue para o **processador** caminho da pasta. Instalar pacotes de nó e compile o projeto ao executar os comandos seguintes:

    ```shell
    npm i
    npm run build
    ```
5. Receba eventos com o anfitrião do processador de eventos ao executar o seguinte comando:

    ```shell
    node dist/examples/singleEph.js
    ```

## <a name="review-the-sample-code"></a>Reveja o código de exemplo 
Aqui está o código de exemplo para receber eventos de um hub de eventos com o node. js. Manualmente pode criar um ficheiro de sampleEph.js e execute-o para receber eventos para um hub de eventos. 

  ```javascript
  const { EventProcessorHost, delay } = require("@azure/event-processor-host");

  const path = process.env.EVENTHUB_NAME;
  const storageCS = process.env.STORAGE_CONNECTION_STRING;
  const ehCS = process.env.EVENTHUB_CONNECTION_STRING;
  const storageContainerName = "test-container";
  
  async function main() {
    // Create the Event Processor Host
    const eph = EventProcessorHost.createFromConnectionString(
      EventProcessorHost.createHostName("my-host"),
      storageCS,
      storageContainerName,
      ehCS,
      {
        eventHubPath: path,
        onEphError: (error) => {
          console.log("This handler will notify you of any internal errors that happen " +
          "during partition and lease management: %O", error);
        }
      }
    );
    let count = 0;
    // Message event handler
    const onMessage = async (context/*PartitionContext*/, data /*EventData*/) => {
      console.log(">>>>> Rx message from '%s': '%s'", context.partitionId, data.body);
      count++;
      // let us checkpoint every 100th message that is received across all the partitions.
      if (count % 100 === 0) {
        return await context.checkpoint();
      }
    };
    // Error event handler
    const onError = (error) => {
      console.log(">>>>> Received Error: %O", error);
    };
    // start the EPH
    await eph.start(onMessage, onError);
    // After some time let' say 2 minutes
    await delay(120000);
    // This will stop the EPH.
    await eph.stop();
  }
  
  main().catch((err) => {
    console.log(err);
  });
      
  ```

Não se esqueça de definir as variáveis de ambiente antes de executar o script. Pode configurar esta opção na linha de comandos, conforme mostrado no exemplo seguinte, ou utilizar o [dotenv pacote](https://www.npmjs.com/package/dotenv#dotenv). 

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

Pode encontrar mais exemplos [aqui](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples).


## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, criou uma aplicação node. js que recebeu mensagens a partir de um hub de eventos. Para saber como enviar eventos para um hub de eventos com node. js, veja [enviar eventos do hub de eventos - node. js](event-hubs-node-get-started-send.md).

<!-- Links -->
[conta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
