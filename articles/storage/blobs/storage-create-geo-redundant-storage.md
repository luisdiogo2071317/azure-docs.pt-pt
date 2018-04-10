---
title: Tornar os dados das aplicações altamente disponíveis no Azure | Microsoft Docs
description: Utilize o armazenamento georredundante de acesso de leitura para tornar os dados das suas aplicações altamente disponíveis
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: web
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: 6226fea5001d19a6f0e1f6700d90ea2b9481d43c
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>Tornar os dados das suas aplicações altamente disponíveis com o Armazenamento do Azure

Este tutorial é a primeira parte de uma série, que mostra como tornar os dados das suas aplicações altamente disponíveis no Azure. Quando tiver terminado, terá uma aplicação de consola que carrega e obtém um blob para uma conta de armazenamento [geograficamente redundante de acesso de leitura](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS). O RA-GRS funciona ao replicar as transações da região primária para a secundária. Este processo de replicação garante que os dados na região secundária acabam por ser consistentes. A aplicação utiliza o padrão [Circuit Breaker](/azure/architecture/patterns/circuit-breaker) para determinar a que ponto final se ligar. A aplicação muda para o ponto final secundário quando uma falha é simulada.

Na primeira parte da série, saiba como:

> [!div class="checklist"]
> * Create a storage account
> * Transferir o exemplo
> * Definir a cadeia de ligação
> * Executar a aplicação de consola

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:
 
# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)
* Instale o [2017 do Visual Studio](https://www.visualstudio.com/downloads/) com as seguintes cargas de trabalho:
  - **Desenvolvimento do Azure**

  ![Desenvolvimento do Azure (na Web e na Cloud)](media/storage-create-geo-redundant-storage/workloads.png)

* (Opcional) Transferir e instalar o [Fiddler](https://www.telerik.com/download/fiddler)
 
# <a name="python-tabpython"></a>[Python] (#tab/python) 

* Instalar o [Python](https://www.python.org/downloads/)
* Transfira e instale o [SDK de Armazenamento do Azure para Python](https://github.com/Azure/azure-storage-python).
* (Opcional) Transferir e instalar o [Fiddler](https://www.telerik.com/download/fiddler)

---

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [Portal do Azure](https://portal.azure.com/).

## <a name="create-a-storage-account"></a>Create a storage account

Uma conta de armazenamento fornece um espaço de nomes exclusivo para armazenar e aceder aos objetos de dados de armazenamento do Azure.

Siga estes passos para criar uma conta de armazenamento georredundante com acesso de leitura:

1. Selecione o botão **Criar um recurso**, no canto superior esquerdo do portal do Azure.

2. Selecione **Armazenamento** na página **Novo** e selecione **Conta de armazenamento - blob, ficheiro, tabela, fila** em **Em Destaque**.
3. Preencha o formulário da conta de armazenamento com as seguintes informações, conforme mostrado na imagem abaixo e selecione **Criar**:

   | Definição       | Valor sugerido | Descrição |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Nome** | mystorageaccount | Um valor exclusivo para a conta de armazenamento |
   | **Deployment model** (Modelo de implementação) | Resource Manager  | O Resource Manager contém as funcionalidades mais recentes.|
   | **Account kind** (Tipo de conta) | StorageV2 | Para obter detalhes sobre os tipos de contas, veja [Tipos de contas de armazenamento](../common/storage-introduction.md#types-of-storage-accounts) |
   | **Performance** (Desempenho) | Standard | O desempenho standard é suficiente para este cenário de exemplo. |
   | **Replicação**| Armazenamento georredundante com acesso de leitura (RA-GRS) | É necessário para o exemplo funcionar. |
   |**Secure transfer required** (Transferência segura necessária) | Desativado| A transferência segura não é necessária neste cenário. |
   |**Subscrição** | A sua subscrição |Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições). |
   |**ResourceGroup** | myResourceGroup |Para nomes de grupo de recursos válidos, veja [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições). |
   |**Localização** | EUA Leste | Escolher uma localização. |

![criar conta de armazenamento](media/storage-create-geo-redundant-storage/createragrsstracct.png)

## <a name="download-the-sample"></a>Transferir o exemplo

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

[Transfira o projeto de exemplo](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) e extraia (deszipe) o ficheiro storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip. Também pode utilizar o [git](https://git-scm.com/) para transferir uma cópia da aplicação para o seu ambiente de desenvolvimento. O projeto de exemplo contém uma aplicação de consola.

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.git 
```
# <a name="python-tabpython"></a>[Python] (#tab/python) 

[Transfira o projeto de exemplo](https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) e extraia (deszipe) o ficheiro storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.zip. Também pode utilizar o [git](https://git-scm.com/) para transferir uma cópia da aplicação para o seu ambiente de desenvolvimento. O projeto de exemplo contém uma aplicação Python básica.

```bash
git clone https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```
---


## <a name="set-the-connection-string"></a>Definir a cadeia de ligação

Na aplicação, tem de indicar a cadeia de ligação da sua conta de armazenamento. É recomendado armazenar esta cadeia de ligação numa variável de ambiente no computador local que está a executar a aplicação. Siga um dos exemplos abaixo, consoante o Sistema Operativo para criar a variável de ambiente.

No portal do Azure, navegue para a sua conta de armazenamento. Selecione **Chaves de acesso**, em **Definições**, na conta de armazenamento. Copie a **cadeia de ligação** da chave primária ou secundária. Substitua \<yourconnectionstring\> pela cadeia de ligação real ao executar um dos comandos seguintes, consoante o seu Sistema Operativo. Este comando guarda uma variável de ambiente no computador local. No Windows, a variável de ambiente não está disponível enquanto não recarregar a **Linha de Comandos** ou o shell que está a utilizar. Substitua **\<storageConnectionString\>** no exemplo seguinte:

# <a name="linux-tablinux"></a>[Linux] (#tab/linux) 
export storageconnectionstring=\<yourconnectionstring\> 

# <a name="windows-tabwindows"></a>[Windows] (#tab/windows) 
setx storageconnectionstring "\<yourconnectionstring\>"

---


## <a name="run-the-console-application"></a>Executar a aplicação de consola

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)
No Visual Studio, prima **F5** ou selecione **Start** (Iniciar) para iniciar a depuração da aplicação. O Visual Studio restaura automaticamente os pacotes NuGet em falta, se estiverem configurados. Para saber mais, aceda a [Installing and reinstalling packages with package restore](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) (Instalar e reinstalar pacotes com o restauro de pacotes).

É iniciada uma janela de consola e a aplicação começa a ser executada. A aplicação carrega a imagem **HelloWorld.png** da solução para a conta de armazenamento. A aplicação verifica para garantir que a imagem foi replicada para o ponto final de RA-GRS secundário. Em seguida, começa a transferir a imagem até 999 vezes. Cada leitura é representada por um **P** ou um **S**, em que **P** representa o ponto final primário e **S** o secundário.

![Aplicação de consola em execução](media/storage-create-geo-redundant-storage/figure3.png)

No código de exemplo, a tarefa `RunCircuitBreakerAsync` no ficheiro `Program.cs` é utilizada para transferir uma imagem da conta de armazenamento através do método [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.downloadtofileasync?view=azure-dotnet). Antes da transferência, é definido um [OperationContext](/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet). O contexto da operação define os processadores de eventos que são acionados se uma transferência for concluída com êxito ou se falhar e estiver a repetir a operação.

# <a name="python-tabpython"></a>[Python] (#tab/python) 
Para executar a aplicação num terminal ou numa linha de comandos, aceda ao diretório **circuitbreaker.py** e introduza `python circuitbreaker.py`. A aplicação carrega a imagem **HelloWorld.png** da solução para a conta de armazenamento. A aplicação verifica para garantir que a imagem foi replicada para o ponto final de RA-GRS secundário. Em seguida, começa a transferir a imagem até 999 vezes. Cada leitura é representada por um **P** ou um **S**, em que **P** representa o ponto final primário e **S** o secundário.

![Aplicação de consola em execução](media/storage-create-geo-redundant-storage/figure3.png)

No código de exemplo, o método `run_circuit_breaker` no ficheiro `circuitbreaker.py` é utilizado para transferir uma imagem da conta de armazenamento através do método [get_blob_to_path](https://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html). 

A função de repetição do objeto de armazenamento está definida como uma política de repetição linear. A função de repetição determina se deve repetir um pedido e especifica o número de segundos a aguardar antes da repetição. Defina o valor **retry\_to\_secondary** como verdadeiro se pretender repetir o pedido para o ponto final secundário, caso o pedido inicial para o primário falhe. A aplicação de exemplo, é definida uma política de repetição personalizada na função `retry_callback` do objeto de armazenamento.
 
Antes da transferência, são definidas as funções [retry_callback](https://docs.microsoft.com/en-us/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) e [response_callback](https://docs.microsoft.com/en-us/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) do objeto Serviço. Estas funções definem os processadores de eventos que são acionados se uma transferência for concluída com êxito ou se falhar e estiver a repetir a operação.  

---

### <a name="retry-event-handler"></a>Processador de eventos de repetição

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

O processador de eventos `OperationContextRetrying` é chamado quando a transferência da imagem falha e está definida para repetir. Se for atingido o número máximo de repetições definidas na aplicação, [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) do pedido é alterado para `SecondaryOnly`. Esta definição força a aplicação a tentar transferir a imagem do ponto final secundário. Esta configuração reduz o tempo que demora a pedir a imagem, porque o ponto final primário não é repetido indefinidamente.

No código de exemplo, a tarefa `RunCircuitBreakerAsync` no ficheiro `Program.cs` é utilizada para transferir uma imagem da conta de armazenamento através do método [DownloadToFileAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet). Antes da transferência, é definido um [OperationContext](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet). O contexto da operação define os processadores de eventos que são acionados se uma transferência for concluída com êxito ou se falhar e estiver a repetir a operação.
 
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

# <a name="python-tabpython"></a>[Python] (#tab/python) 
O processador de eventos `retry_callback` é chamado quando a transferência da imagem falha e está definida para repetir. Se for atingido o número máximo de repetições definidas na aplicação, [LocationMode](https://docs.microsoft.com/en-us/python/api/azure.storage.common.models.locationmode?view=azure-python) do pedido é alterado para `SECONDARY`. Esta definição força a aplicação a tentar transferir a imagem do ponto final secundário. Esta configuração reduz o tempo que demora a pedir a imagem, porque o ponto final primário não é repetido indefinidamente.  

```python
def retry_callback(retry_context):
    global retry_count
    retry_count = retry_context.count
    sys.stdout.write("\nRetrying event because of failure reading the primary. RetryCount= {0}".format(retry_count))
    sys.stdout.flush()

    # Check if we have more than n-retries in which case switch to secondary
    if retry_count >= retry_threshold:

        # Check to see if we can fail over to secondary.
        if blob_client.location_mode != LocationMode.SECONDARY:
            blob_client.location_mode = LocationMode.SECONDARY
            retry_count = 0
        else:
            raise Exception("Both primary and secondary are unreachable. "
                            "Check your application's network connection.")
```

---


### <a name="request-completed-event-handler"></a>Processador de eventos de pedido concluído
 
# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

O processador de eventos `OperationContextRequestCompleted` é chamado quando a transferência da imagem é bem-sucedida. Se a aplicação estiver a utilizar o ponto final secundário, continua a utilizar este ponto final até 20 vezes. Ao fim dessas 20 vezes, a aplicação define [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) novamente como `PrimaryThenSecondary` e repete o ponto final primário. Se um pedido for bem-sucedido, a aplicação continua a ler a partir do ponto final primário.
 
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

# <a name="python-tabpython"></a>[Python] (#tab/python) 

O processador de eventos `response_callback` é chamado quando a transferência da imagem é bem-sucedida. Se a aplicação estiver a utilizar o ponto final secundário, continua a utilizar este ponto final até 20 vezes. Ao fim dessas 20 vezes, a aplicação define [LocationMode](https://docs.microsoft.com/en-us/python/api/azure.storage.common.models.locationmode?view=azure-python) novamente como `PRIMARY` e repete o ponto final primário. Se um pedido for bem-sucedido, a aplicação continua a ler a partir do ponto final primário.

```python
def response_callback(response):
    global secondary_read_count
    if blob_client.location_mode == LocationMode.SECONDARY:

        # You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        # then switch back to the primary and see if it is available now.
        secondary_read_count += 1
        if secondary_read_count >= secondary_threshold:
            blob_client.location_mode = LocationMode.PRIMARY
            secondary_read_count = 0
```

---

## <a name="next-steps"></a>Passos seguintes

Na parte um da série, aprendeu a tornar uma aplicação altamente disponível com contas de armazenamento RA-GRS, nomeadamente a:

> [!div class="checklist"]
> * Create a storage account
> * Transferir o exemplo
> * Definir a cadeia de ligação
> * Executar a aplicação de consola

Avance para a parte dois da série para saber como simular uma falha e forçar a aplicação a utilizar o ponto final RA-GRS secundário.

> [!div class="nextstepaction"]
> [Simulate a failure in connection to your primary storage endpoint](storage-simulate-failure-ragrs-account-app.md) (Simular uma falha associada ao ponto final de armazenamento primário)
