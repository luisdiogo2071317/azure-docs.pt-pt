---
title: Enviar eventos para Hubs de eventos do Azure com Go | Documentos da Microsoft
description: Introdução ao envio de eventos para Hubs de eventos com Go
services: event-hubs
author: joshgav
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/23/2018
ms.author: joshgav
ms.openlocfilehash: de454e7c94cb89a44f681fa1301bb5109ced03fd
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39717030"
---
# <a name="send-events-to-event-hubs-using-go"></a>Enviar eventos para Hubs de eventos com Go

Os Hubs de eventos do Azure é um sistema de gerenciamento de eventos altamente escalável que pode lidar com milhões de eventos por segundo, permitindo que os aplicativos processar e analisar enormes quantidades de dados produzidos pelos dispositivos ligados e outros sistemas. Depois de recolhidos para um hub de eventos, pode receber e lidar com eventos usando manipuladores de dentro do processo ou através do reencaminhamento para outros sistemas de análise.

Para saber mais sobre os Hubs de eventos, consulte a [descrição geral dos Hubs de eventos][Event Hubs overview].

Este tutorial descreve como enviar eventos para um hub de eventos de um aplicativo escrito em Go. Para receber eventos, utilize o **ir eph** pacote (anfitrião do processador de eventos), conforme descrito na [o artigo correspondente na receção](event-hubs-go-get-started-receive-eph.md).

O código neste tutorial é obtido a partir [estes exemplos do GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), que pode examinar para ver toda a aplicação de trabalho, incluindo instruções de importação e declarações de variável.

Outros exemplos estão disponíveis [de eventos dos Hubs de empacotar repositório](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa dos seguintes pré-requisitos:

* Vá instalado localmente. Siga [estas instruções](https://golang.org/doc/install) se necessário.
* Uma conta ativa do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita][] antes de começar.
* Os Hubs de eventos espaço de nomes e o event hub existente. Pode criar estas entidades, seguindo as instruções em [este artigo](event-hubs-create.md).

## <a name="install-go-package"></a>Instalar o pacote do Go

Obter o pacote do Go para os Hubs de eventos com `go get` ou `dep`. Por exemplo:

```bash
go get -u github.com/Azure/azure-event-hubs-go
go get -u github.com/Azure/azure-amqp-common-go/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
```

## <a name="import-packages-in-your-code-file"></a>Importar pacotes em seu arquivo de código

Para importar os pacotes de Go, utilize o seguinte exemplo de código:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
)
```

## <a name="create-service-principal"></a>Criar um principal de serviço

Crie um novo principal de serviço ao seguir as instruções em [criar um Azure principal de serviço com a CLI 2.0 do Azure](/cli/azure/create-an-azure-service-principal-azure-cli). Guarde as credenciais fornecidas no seu ambiente com os seguintes nomes. O Azure SDK para Go e os pacotes de Hubs de eventos estão pré-configuradas para procurar por estes nomes de variáveis:

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Agora, crie um fornecedor de autorização para o seu cliente de Hubs de eventos que utiliza estas credenciais:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

## <a name="create-event-hubs-client"></a>Crie os Hubs de eventos do cliente

O código a seguir cria um cliente dos Hubs de eventos:

```go
hub, err := eventhubs.NewHub("namespaceName", "hubName", tokenProvider)
ctx := context.WithTimeout(context.Background(), 10 * time.Second)
defer hub.Close(ctx)
if err != nil {
    log.Fatalf("failed to get hub %s\n", err)
}
```

## <a name="send-messages"></a>Enviar mensagens

No seguinte fragmento, utilize (1) para enviar mensagens interativamente a partir de um terminal ou (2) para enviar mensagens dentro de seu programa:

```go
// 1. send messages at the terminal
ctx = context.Background()
reader := bufio.NewReader(os.Stdin)
for {
    fmt.Printf("Input a message to send: ")
    text, _ := reader.ReadString('\n')
    hub.Send(ctx, eventhubs.NewEventFromString(text))
}

// 2. send messages within program
ctx = context.Background()
hub.Send(ctx, eventhubs.NewEventFromString("hello Azure!")
```

## <a name="extras"></a>Extras

Obter os IDs das partições no seu hub de eventos:

```go
info, err := hub.GetRuntimeInformation(ctx)
if err != nil {
    log.Fatalf("failed to get runtime info: %s\n", err)
}
log.Printf("got partition IDs: %s\n, info.PartitionIDs)
```

## <a name="next-steps"></a>Passos Seguintes

Visite as páginas seguintes para saber mais sobre os Hubs de eventos:

* [Receber eventos com o EventProcessorHost](event-hubs-go-get-started-receive-eph.md)
* [Descrição geral dos Hubs de Eventos][Event Hubs overview]
* [Criar um hub de eventos](event-hubs-create.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[conta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
