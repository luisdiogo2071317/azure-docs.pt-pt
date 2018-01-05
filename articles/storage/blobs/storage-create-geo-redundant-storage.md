---
title: "Disponibilizar dados da aplicação altamente no Azure | Microsoft Docs"
description: "Utilize o armazenamento georredundante com acesso de leitura para tornar os dados da aplicação de elevada disponibilidade"
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 63ca91c2eadf7b003427e9716d99621fca1b1a19
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>Tornar os dados da aplicação de elevada disponibilidade com armazenamento do Azure

Este tutorial faz parte de um de uma série. Este tutorial mostra como disponibilizar os dados da aplicação altamente no Azure. Quando tiver terminado, tem uma aplicação de consola do .NET core que carrega e obtém um blob para um [acesso de leitura georredundante](../common/storage-redundancy.md#read-access-geo-redundant-storage) conta de armazenamento (RA-GRS). RA-GRS funciona ao replicar as transações do primário para a região secundária. Este processo de replicação garante que os dados na região secundária são eventualmente consistentes. A aplicação utiliza o [disjuntor](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker) padrão para determinar o ponto final para ligar a. A aplicação muda para o ponto final secundário quando uma falha é simulada.

Na parte de uma série, saiba como:

> [!div class="checklist"]
> * Criar uma conta do Storage
> * Transferir o exemplo
> * Definir a cadeia de ligação
> * Executar a aplicação de consola

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

* Instale o [2017 do Visual Studio](https://www.visualstudio.com/downloads/) com as seguintes cargas de trabalho:
  - **Desenvolvimento do Azure**

  ![Desenvolvimento do Azure (em Web & nuvem)](media/storage-create-geo-redundant-storage/workloads.png)

* Transfira e instale [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [Portal do Azure](https://portal.azure.com/).

## <a name="create-a-storage-account"></a>Criar uma conta do Storage

Uma conta do storage fornece um espaço de nomes exclusivo para armazenar e aceder aos seus objetos de dados de armazenamento do Azure.

Siga estes passos para criar uma conta de armazenamento georredundante com acesso de leitura:

1. Selecione o **novo** botão encontrado no canto superior esquerdo do portal do Azure.

2. Selecione **armazenamento** do **novo** página e selecione **conta de armazenamento - BLOBs, ficheiro, tabela, fila** em **em destaque**.
3. Preencha o formulário de conta de armazenamento com as seguintes informações, conforme mostrado na imagem seguinte e selecione **criar**:

   | Definição       | Valor sugerido | Descrição |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Nome** | mystorageaccount | Um valor exclusivo para a sua conta de armazenamento |
   | **Modelo de implementação** | Resource Manager  | Gestor de recursos contém funcionalidades mais recentes.|
   | **Tipo de conta** | Fins gerais | Para obter detalhes sobre os tipos de contas, consulte [tipos de contas de armazenamento](../common/storage-introduction.md#types-of-storage-accounts) |
   | **Desempenho** | Standard | Padrão é suficiente para o cenário de exemplo. |
   | **Replicação**| Armazenamento georredundante com acesso de leitura (RA-GRS) | Isto é necessário para o exemplo funcionar. |
   |**Transferência segura necessária** | Desativado| Transferência segura não é necessária para este cenário. |
   |**Subscrição** | A subscrição |Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições). |
   |**ResourceGroup** | myResourceGroup |Para nomes de grupo de recursos válidos, veja [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições). |
   |**Localização** | EUA Leste | Escolha uma localização. |

![Criar conta de armazenamento](media/storage-create-geo-redundant-storage/figure1.png)

## <a name="download-the-sample"></a>Transferir o exemplo

[Transferir o projeto de exemplo](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip).

Extrair (deszipe) o ficheiro storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip.
O projeto de exemplo contém uma aplicação de consola.

## <a name="set-the-connection-string"></a>Definir a cadeia de ligação

Na aplicação, tem de indicar a cadeia de ligação da sua conta de armazenamento. Recomenda-se para armazenar esta cadeia de ligação dentro de uma variável de ambiente no computador local que executa a aplicação. Siga um dos exemplos abaixo, dependendo do sistema operativo para criar a variável de ambiente.

No portal do Azure, navegue até à sua conta de armazenamento. Selecione **chaves de acesso** em **definições** na sua conta de armazenamento. Copiar o **cadeia de ligação** partir da chave primária ou secundária. Substitua \<yourconnectionstring\> com a ligação real cadeia executando um dos seguintes comandos com base no seu sistema operativo. Este comando guarda uma variável de ambiente para o computador local. No Windows, a variável de ambiente não está disponível até recarregar o **linha de comandos** ou shell estiver a utilizar. Substitua  **\<storageConnectionString\>**  no seguinte exemplo:

### <a name="linux"></a>Linux

```bash
export storageconnectionstring=<yourconnectionstring>
```

### <a name="windows"></a>Windows

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

![ficheiro de configuração de aplicação](media/storage-create-geo-redundant-storage/figure2.png)

## <a name="run-the-console-application"></a>Executar a aplicação de consola

No Visual Studio, prima **F5** ou selecione **iniciar** para iniciar a depuração da aplicação. Visual studio automaticamente restauros em falta pacotes NuGet se configurado, visitam para [instalar e reinstalar pacotes com o restauro de pacote](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) para obter mais informações.

Uma janela da consola é iniciado e começa a aplicação em execução. A aplicação carrega o **HelloWorld.png** imagem da solução para a conta de armazenamento. A aplicação verifica para garantir que a imagem foi replicado para o ponto final RA-GRS secundário. Em seguida, inicia a transferência da imagem 999 vezes. Cada leitura é representada por um **P** ou um **S**. Onde **P** representa o ponto final principal e **S** representa o ponto final secundário.

![Aplicação de consola em execução](media/storage-create-geo-redundant-storage/figure3.png)

No código de exemplo, o `RunCircuitBreakerAsync` de tarefas no `Program.cs` ficheiro é utilizado para transferir uma imagem da conta de armazenamento utilizando o [DownloadToFileAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet) método. Antes da transferência um [OperationContext](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet) está definido. O contexto da operação define processadores de eventos, que são acionados quando uma transferência for concluída com êxito ou se uma transferência falhe e repetir a operação.

### <a name="retry-event-handler"></a>Repita o processador de eventos

O `OperationContextRetrying` processador de eventos é chamado quando a transferência da imagem de falha e é definido para repetir. Se o número máximo de tentativas, o que são definidas na aplicação é atingido, o [LocationMode](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) do pedido é alterado para `SecondaryOnly`. Esta definição força o tentar transferir a imagem do ponto final secundário da aplicação. Esta configuração reduz o tempo decorrido para a imagem de pedidos, como o ponto final principal não for repetido indefinidamente.

```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

### <a name="request-completed-event-handler"></a>Processador de eventos de pedido concluído

O `OperationContextRequestCompleted` processador de eventos é chamado quando a transferência da imagem é efetuada com êxito. Se a aplicação estiver a utilizar o ponto final secundário, a aplicação continua a utilizar este ponto final até 20 vezes. Após 20 vezes, os conjuntos de aplicação a [LocationMode](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) para `PrimaryThenSecondary` e repete as tentativas o ponto final principal. Se um pedido for bem-sucedida, continua a aplicação de ler a partir do ponto final principal.

```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times, 
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

Parte de uma série, aprendeu sobre disponibilizar uma aplicação altamente com contas de armazenamento RA-GRS, tais como:

> [!div class="checklist"]
> * Criar uma conta do Storage
> * Transferir o exemplo
> * Definir a cadeia de ligação
> * Executar a aplicação de consola

Avançar para a parte dois da série para saber como simule uma falha e forçar a aplicação para utilizar o ponto final RA-GRS secundário.

> [!div class="nextstepaction"]
> [Simule uma falha na ligação para o ponto final de armazenamento primário](storage-simulate-failure-ragrs-account-app.md)
