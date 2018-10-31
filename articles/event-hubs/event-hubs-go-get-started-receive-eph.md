---
title: Receber eventos dos Hubs de eventos do Azure com Go | Documentos da Microsoft
description: Começar a receber eventos dos Hubs de eventos com Go
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/23/2018
ms.author: shvija
ms.openlocfilehash: 0a8cb349be0ffefecb07bc705391684dc57f1b96
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239496"
---
# <a name="receive-events-from-event-hubs-using-go"></a>Receber eventos dos Hubs de eventos com Go

Os Hubs de eventos do Azure é um sistema de gerenciamento de eventos altamente escalável que pode lidar com milhões de eventos por segundo, permitindo que os aplicativos processar e analisar enormes quantidades de dados produzidos pelos dispositivos ligados e outros sistemas. Depois de recolhidos para um hub de eventos, pode receber e lidar com eventos usando manipuladores de dentro do processo ou através do reencaminhamento para outros sistemas de análise.

Para saber mais sobre os Hubs de eventos, consulte a [descrição geral dos Hubs de eventos][Event Hubs overview].

Este tutorial descreve como receber eventos de um hub de eventos num aplicativo de Go. Para saber como enviar eventos ver [o artigo de envio correspondente](event-hubs-go-get-started-send.md).

Código neste tutorial é obtido a partir [estes exemplos do GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/eventhubs), que pode examinar para ver toda a trabalhar incluindo aplicação importar instruções e declarações de variável.

Outros exemplos estão disponíveis [de eventos dos Hubs de empacotar repositório](https://github.com/Azure/azure-event-hubs-go/tree/master/_examples).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisará os seguintes pré-requisitos:

* Vá instalado localmente. Siga [estas instruções](https://golang.org/doc/install) se necessário.
* Uma conta ativa do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita][] antes de começar.
* Para receber mensagens, deve haver mensagens no hub de eventos de destino. Saiba como enviar mensagens [enviar tutorial](event-hubs-go-get-started-send.md).
* Hub de eventos existente (consulte a secção seguinte).
* Uma conta de armazenamento e existentes contentor (consulte a seção depois da secção seguinte).

### <a name="create-an-event-hub"></a>Criar um hub de eventos

Este tutorial começa com um hub de eventos e de espaço de nomes de Hubs de eventos existente. Pode criar estas entidades, seguindo as instruções em [este artigo](event-hubs-create.md).

### <a name="create-a-storage-account-and-container"></a>Criar uma conta de armazenamento e um contentor

Estado, como de concessões em partições e os pontos de verificação de evento stream são partilhados entre recetores com um contentor de armazenamento do Azure. Pode criar uma conta de armazenamento e um contentor com o SDK Go, mas também pode criar uma ao seguir as instruções em [sobre as contas de armazenamento](../storage/common/storage-create-storage-account.md).

Exemplos para criar artefactos de armazenamento com o SDK Go estão disponíveis no [repositório de amostras do Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) e no exemplo correspondente a este tutorial.

## <a name="receive-messages"></a>Receber mensagens

Para receber as mensagens, obter os pacotes de ir para os Hubs de eventos com `go get` ou `dep`:

```bash
go get -u github.com/Azure/azure-event-hubs-go/...
go get -u github.com/Azure/azure-amqp-common-go/...
go get -u github.com/Azure/go-autorest/...

# or

dep ensure -add github.com/Azure/azure-event-hubs-go
dep ensure -add github.com/Azure/azure-amqp-common-go
dep ensure -add github.com/Azure/go-autorest
```

## <a name="import-packages-in-your-code-file"></a>Importar pacotes em seu arquivo de código

Para importar os pacotes de Go, utilize o seguinte exemplo de código:

```go
import (
    aad "github.com/Azure/azure-amqp-common-go/aad"
    eventhubs "github.com/Azure/azure-event-hubs-go"
    eph "github.com/Azure/azure-event-hubs-go/eph"
    storageLeaser "github.com/Azure/azure-event-hubs-go/storage"
    azure "github.com/Azure/go-autorest/autorest/azure"
)
```

## <a name="create-service-principal"></a>Criar um principal de serviço

Crie um novo principal de serviço ao seguir as instruções em [criar um Azure principal de serviço com a CLI 2.0 do Azure](/cli/azure/create-an-azure-service-principal-azure-cli). Guardar as credenciais fornecidas no seu ambiente com os seguintes nomes: O Azure SDK para Go e os Hubs de eventos de pacote estão pré-configuradas para procurar por estes nomes de variáveis.

```bash
export AZURE_CLIENT_ID=
export AZURE_CLIENT_SECRET=
export AZURE_TENANT_ID=
export AZURE_SUBSCRIPTION_ID= 
```

Em seguida, crie um fornecedor de autorização para o seu cliente de Hubs de eventos que utiliza estas credenciais:

```go
tokenProvider, err := aad.NewJWTProvider(aad.JWTProviderWithEnvironmentVars())
if err != nil {
    log.Fatalf("failed to configure AAD JWT provider: %s\n", err)
}
```

## <a name="get-metadata-struct"></a>Obter a estrutura de metadados

Obtenha um struct com metadados sobre o seu ambiente do Azure com o SDK do Go do Azure. Operações posteriores usam este struct para localizar pontos de extremidade corretos.

```go
azureEnv, err := azure.EnvironmentFromName("AzurePublicCloud")
if err != nil {
    log.Fatalf("could not get azure.Environment struct: %s\n", err)
}
```

## <a name="create-credential-helper"></a>Criar programa auxiliar de credencial 

Crie um credencial do programa auxiliar que utiliza as credenciais do Azure Active Directory (AAD) anterior para criar uma credencial de assinatura de acesso partilhado (SAS) para armazenamento. O último parâmetro indica este construtor para utilizar as variáveis de ambiente mesmo como utilizado anteriormente:

```go
cred, err := storageLeaser.NewAADSASCredential(
    subscriptionID,
    resourceGroupName,
    storageAccountName,
    storageContainerName,
    storageLeaser.AADSASCredentialWithEnvironmentVars())
if err != nil {
    log.Fatalf("could not prepare a storage credential: %s\n", err)
}
```

## <a name="create-checkpointer-and-leaser"></a>Criar Checkpointer e Leaser 

Criar uma **Leaser**, responsável pela alocação de uma partição para um recetor específico e um **Checkpointer**, responsáveis pela escrita de pontos de verificação para o fluxo de mensagens, de modo a que podem começar a outros recetores ler a partir do desvio correto.

Atualmente, uma única **StorageLeaserCheckpointer** está disponível que utiliza o mesmo contentor de armazenamento para gerir as concessões e pontos de verificação. Além dos nomes de conta e contentor de armazenamento, o **StorageLeaserCheckpointer** precisa a credencial que criou no passo anterior e a estrutura de ambiente do Azure para aceder corretamente ao contentor.

```go
leaserCheckpointer, err := storageLeaser.NewStorageLeaserCheckpointer(
    cred,
    storageAccountName,
    storageContainerName,
    azureEnv)
if err != nil {
    log.Fatalf("could not prepare a storage leaserCheckpointer: %s\n", err)
}
```

## <a name="construct-event-processor-host"></a>Construir o anfitrião do processador de eventos

Tem agora as partes necessárias para construir um EventProcessorHost, da seguinte forma. O mesmo **StorageLeaserCheckpointer** é utilizado como um Leaser e Checkpointer, conforme descrito anteriormente:

```go
ctx := context.Background()
p, err := eph.New(
    ctx,
    nsName,
    hubName,
    tokenProvider,
    leaserCheckpointer,
    leaserCheckpointer)
if err != nil {
    log.Fatalf("failed to create EPH: %s\n", err)
}
defer p.Close(context.Background())
```

## <a name="create-handler"></a>Criar o manipulador 

Agora, crie um manipulador e registá-lo com o anfitrião do processador de eventos. Quando o anfitrião é iniciado, ele aplica-se este e outros manipuladores especificados a mensagens de entrada:

```go
handler := func(ctx context.Context, event *eventhubs.Event) error {
    fmt.Printf("received: %s\n", string(event.Data))
    return nil
}

// register the handler with the EPH
_, err := p.RegisterHandler(ctx, handler)
if err != nil {
    log.Fatalf("failed to register handler: %s\n", err)
}
```

## <a name="receive-messages"></a>Receber mensagens

Com tudo configurado, pode iniciar o anfitrião do processador de eventos com `Start(context)` mantê-lo permanentemente em execução ou com `StartNonBlocking(context)` apenas executar, desde que as mensagens estão disponíveis.

Este tutorial inicia e executa os seguintes; Veja o exemplo do GitHub para um exemplo usando `StartNonBlocking`:

```go
ctx := context.Background()
err = p.Start()
if err != nil {
    log.Fatalf("failed to start EPH: %s\n", err)
}
```

## <a name="notes"></a>Notas

Este tutorial utiliza uma única instância do **EventProcessorHost**. Para aumentar o débito e a confiabilidade, deve executar várias instâncias **EventProcessorHost** em sistemas diferentes. Leaser sistema garante que apenas um recetor está associado e recebe mensagens de uma partição especificada num tempo especificado.

## <a name="next-steps"></a>Passos Seguintes
Neste início rápido, criou uma aplicação Go que recebeu mensagens a partir de um hub de eventos. Para saber como enviar eventos para um hub de eventos através de Go, veja [enviar eventos do hub de eventos - Go](event-hubs-go-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[conta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
