---
title: Utilizar a biblioteca BulkExecutor .NET para efetuar operações em massa na base de dados do Azure Cosmos | Microsoft Docs
description: Utilize a biblioteca de BulkExecutor .NET da BD do Azure Cosmos em massa importar e atualizar os documentos para coleções de base de dados do Azure Cosmos.
keywords: Executor em massa de .net
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.workload: data-services
ms.topic: article
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 608551090ce10e08ba517def644c72186a6f25e1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="using-bulkexecutor-net-library-to-perform-bulk-operations-in-azure-cosmos-db"></a>Utilizar a biblioteca BulkExecutor .NET para efetuar operações em massa na base de dados do Azure Cosmos

Este tutorial fornece instruções sobre como utilizar a biblioteca de BulkExecutor .NET do Cosmos BD do Azure para importar e atualizar os documentos para coleções de base de dados do Azure Cosmos. Para saber mais sobre a biblioteca de BulkExecutor e como ajuda a tirar partido do débito intenso e armazenamento, consulte [descrição geral da biblioteca de BulkExecutor](bulk-executor-overview.md) artigo. Este tutorial irá guiá-lo através de uma aplicação .NET de exemplo que em massa importa documentos gerados aleatoriamente para uma coleção de base de dados do Azure Cosmos. Depois de importar, mostra como pode em massa atualizar os dados importados especificando patches como efetuar nos campos de documentos específico das operações.

## <a name="prerequisites"></a>Pré-requisitos

* Se ainda não tiver o Visual Studio 2017, instalado, pode transferir e utilizar o [edição de Comunidade de 2017 do Visual Studio](https://www.visualstudio.com/downloads/). Certifique-se de que permitem a programação do Azure durante a configuração do Visual Studio.

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar. 

* Pode [Experimentar o Azure Cosmos DB gratuitamente](https://azure.microsoft.com/try/cosmosdb/) sem uma subscrição do Azure, sem encargos e compromissos. Em alternativa, pode utilizar o [emulador de BD do Azure Cosmos](https://docs.microsoft.com/azure/cosmos-db/local-emulator) com o `https://localhost:8081` URI. A chave primária é fornecida na [autenticar pedidos](local-emulator.md#authenticating-requests).

* Criar uma conta de API do SQL Server de base de dados do Azure Cosmos utilizando os passos descritos no [criar conta de base de dados](create-sql-api-dotnet.md#create-a-database-account) secção do artigo de início rápido do .NET. 

## <a name="clone-the-sample-application"></a>Clonar a aplicação de exemplo

Agora vamos mudar para trabalhar com o código transferindo algumas aplicações de .NET de exemplo a partir do GitHub. Estas aplicações efetuar operações em massa na base de dados do Azure Cosmos dados. A clonagem as aplicações, abra uma linha de comandos, navegue para o diretório onde pretende copiar o-os e execute o seguinte comando:

```
git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started.git
```

O repositório clonado contém dois exemplos "BulkImportSample" e "BulkUpdateSample". Pode abrir uma das aplicações de exemplo, Atualize as cadeias de ligação no ficheiro App. config com cadeias de ligação da sua conta de base de dados do Azure Cosmos, compilar a solução e executá-la. 

A aplicação "BulkImportSample" gera documentos aleatórios e em massa importa-los à base de dados do Azure Cosmos. O volume de aplicação "BulkUpdateSample" atualiza os documentos importados especificando patches como efetuar nos campos de documentos específico das operações. Nas secções seguintes, irá rever o código em cada uma destas aplicações de exemplo.

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Dados de importação em volume à base de dados do Azure Cosmos

1. Navegue para a pasta "BulkImportSample" e abra o ficheiro "BulkImportSample.sln".  

2. Cadeias de ligação do Cosmos BD do Azure são obtidas a partir do ficheiro App. config conforme mostrado no seguinte código:  

   ```csharp
   private static readonly string EndpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
   private static readonly string AuthorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];
   private static readonly string DatabaseName = ConfigurationManager.AppSettings["DatabaseName"];
   private static readonly string CollectionName = ConfigurationManager.AppSettings["CollectionName"];
   private static readonly int CollectionThroughput = int.Parse(ConfigurationManager.AppSettings["CollectionThroughput"]);
   ```

   O importador de em massa cria uma nova base de dados e uma coleção com o nome de base de dados, o nome da coleção e a valores de débito especificados no ficheiro App. config. 

3. Em seguida o DocumentClient objecto foi inicializado com o modo de ligação de TCP direta:  

   ```csharp
   ConnectionPolicy connectionPolicy = new ConnectionPolicy
   {
      ConnectionMode = ConnectionMode.Direct,
      ConnectionProtocol = Protocol.Tcp
   };
   DocumentClient client = new DocumentClient(new Uri(endpointUrl),authorizationKey,
   connectionPolicy)
   ```

4. O objeto de BulkExecutor é inicializado com um valores de tentativa elevada para o tempo de espera e limitadas pedidos. E, em seguida, estão definidos para 0 para passar o controlo de congestionamento para BulkExecutor para o seu período de duração.  

   ```csharp
   // Set retry options high during initialization (default values).
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 30;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 9;

   IBulkExecutor bulkExecutor = new BulkExecutor(client, dataCollection);
   await bulkExecutor.InitializeAsync();

   // Set retries to 0 to pass complete control to bulk executor.
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 0;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 0;
   ```

5. A aplicação invoca a API de BulkImportAsync. A biblioteca .NET fornece dois sobrecargas do volume de importar a API - um que aceita uma lista de documentos JSON serializados e outros aceita uma lista de serialização anuladas POCO documentos. Para saber mais sobre as definições de cada um destes métodos sobrecarregados, consulte [documentação da API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkimportasync?view=azure-dotnet).

   ```csharp
   BulkImportResponse bulkImportResponse = await bulkExecutor.BulkImportAsync(
     documents: documentsToImportInBatch,
     enableUpsert: true,
     disableAutomaticIdGeneration: true,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```
   **Método de BulkImportAsync aceita os seguintes parâmetros:**
   
   |**Parâmetro**  |**Descrição** |
   |---------|---------|
   |enableUpsert    |   Um sinalizador para ativar upsert dos documentos. Se um documento com o id já existe, é atualizado. Por predefinição, está definido como false.      |
   |disableAutomaticIdGeneration    |    Um sinalizador para desativar a geração automática do ID. Por predefinição, está definida como true.     |
   |maxConcurrencyPerPartitionKeyRange    | O nível máximo de concorrência por intervalo de chaves de partição, definição para nulo fará com que a biblioteca a utilizar um valor predefinido de 20. |
   |maxInMemorySortingBatchSize     |  O número máximo de documentos retirado do enumerador do documento que é transmitido para a API de chamada, em cada fase.  Para dentro da memória previamente processamento ordenação fase antes da importação em volume, definição para nulo fará com que a biblioteca a utilizar o valor predefinido de min (documents.count, 1000000).       |
   |cancellationToken    |    O token de cancelamento para lidar graciosamente sair importação em volume.     |

   **Em massa a definição de objeto de resposta de importação** o resultado da chamada de API de importação em massa contém os seguintes atributos:

   |**Parâmetro**  |**Descrição**  |
   |---------|---------|
   |NumberOfDocumentsImported (longa)   |  O número total de documentos que foram importados com êxito sem os documentos fornecidos para o volume de importar a chamada à API.       |
   |TotalRequestUnitsConsumed (double)   |   As unidades de pedido total (RU) consumidas pelo volume de importar a chamada à API.      |
   |TotalTimeTaken (TimeSpan)    |   O tempo total que a importação em massa chamada da API para concluir a execução.      |
   |BadInputDocuments (lista<object>)   |     A lista de documentos de formato incorrecto que não foram importados com êxito no volume importar chamada à API. Utilizador deve corrigir os documentos devolvidos e repita a importação. Documentos com formato incorrecto incluem documentos cujo valor de ID não é uma cadeia (nulo ou qualquer outro tipo de dados é considerado inválido).    |

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Dados de atualização em massa na base de dados do Azure Cosmos

Pode atualizar documentos existentes, utilizando a API de BulkUpdateAsync. Neste exemplo, irá definir o campo de nome para um novo valor e remover o campo de descrição do documentos existentes. Para o conjunto completo de campo suportado operações de atualização, consulte [documentação da API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet). 

1. Navegue para a pasta "BulkUpdateSample" e abra o ficheiro "BulkUpdateSample.sln".  

2. Defina os itens de atualização, juntamente com as operações de atualização de campo correspondente. Neste exemplo, irá utilizar SetUpdateOperation para atualizar o campo de nome e UnsetUpdateOperation para remover o campo de descrição de todos os documentos. Também pode efetuar outras operações como incremento um campo de documento por um valor específico, enviar valores específicos para um campo de matriz ou remover um valor específico de um campo de matriz. Para saber mais sobre os diferentes métodos que forneceu a API de atualização em massa, consulte o [documentação da API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet).

   ```csharp
   SetUpdateOperation<string> nameUpdate = new SetUpdateOperation<string>("Name", "UpdatedDoc");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   List<UpdateOperation> updateOperations = new List<UpdateOperation>();
   updateOperations.Add(nameUpdate);
   updateOperations.Add(descriptionUpdate);

   List<UpdateItem> updateItems = new List<UpdateItem>();
   for (int i = 0; i < 10; i++)
   {
    updateItems.Add(new UpdateItem(i.ToString(), i.ToString(), updateOperations));
   }
   ```

3. A aplicação invoca a API de BulkUpdateAsync. Para saber mais sobre a definição do método BulkUpdateAsync, consulte [documentação da API](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.ibulkexecutor.bulkupdateasync?view=azure-dotnet).  

   ```csharp
   BulkUpdateResponse bulkUpdateResponse = await bulkExecutor.BulkUpdateAsync(
     updateItems: updateItems,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```  
   **Método de BulkUpdateAsync aceita os seguintes parâmetros:**

   |**Parâmetro**  |**Descrição** |
   |---------|---------|
   |maxConcurrencyPerPartitionKeyRange    |   O nível máximo de concorrência por intervalo de chaves de partição, definição para nulo fará com que a biblioteca a utilizar o valor predefinido de 20.   |
   |maxInMemorySortingBatchSize    |    O número máximo de itens de atualização retirado do enumerador de itens de atualização transferido para a chamada de API em cada fase de ordenação de fase previamente processamento dentro da memória antes da atualização em massa, definição para nulo fará com que biblioteca a utilizar o valor predefinido de min (updateItems.count, 1000000).     |
   | cancellationToken|O token de cancelamento para sair da atualização em massa corretamente. |

   **Efetuar a definição de objeto de resposta de atualização em massa** o resultado da atualização em massa na chamada à API contém os seguintes atributos:

   |**Parâmetro**  |**Descrição** |
   |---------|---------|
   |NumberOfDocumentsUpdated (longa)    |   O número total de documentos que foram atualizadas com êxito dos fornecido para a chamada de API de atualização em massa.      |
   |TotalRequestUnitsConsumed (double)   |    As unidades de pedido total (RU) consumidos pelos próprios a atualização em massa na chamada à API.    |
   |TotalTimeTaken (TimeSpan)   | O tempo total que o volume de chamada da API para concluir a execução de atualização. |
    
## <a name="performance-tips"></a>Sugestões de desempenho 

Considere os seguintes pontos para um melhor desempenho ao utilizar a biblioteca de BulkExecutor:

* Para melhor desempenho, execute a aplicação a partir de uma máquina virtual do Azure que se encontra na mesma região que a região de escrita da conta de base de dados do Cosmos.  

* Recomenda-se ao instanciar um objeto de BulkExecutor único para toda a aplicação numa única máquina virtual correspondente a uma coleção específica da base de dados do Cosmos.  

* Uma vez que uma execução de API de operação em massa única consome um segmento grande de CPU e da rede e/s a máquina cliente. Isto acontece por gerar várias tarefas internamente, evite gerar várias tarefas simultâneas dentro do seu processo de aplicação que chama de cada API de operação em massa em execução. Se uma chamada de API de operação em massa única em execução numa única máquina virtual não é possível consumir o débito da sua coleção completa (se débito > 1 a coleção milhões RU/s), preferível criar máquinas virtuais separadas para executar em simultâneo em massa chamadas de operação de API.  

* Certifique-se de que é invocada InitializeAsync() após instanciar um objeto de BulkExecutor ao obter o mapa de partição de coleção de Cosmos BD de destino.  

* Na Config sua aplicação, certifique-se **gcServer** está ativada para um melhor desempenho
  ```xml  
  <runtime>
    <gcServer enabled="true" />
  </runtime>
  ```
* A biblioteca emite os rastreios que podem ser recolhidos para um ficheiro de registo ou na consola. Para ativar a ambos, adicione o seguinte ao App. config da sua aplicação.

  ```xml
  <system.diagnostics>
    <trace autoflush="false" indentsize="4">
      <listeners>
        <add name="logListener" type="System.Diagnostics.TextWriterTraceListener" initializeData="application.log" />
        <add name="consoleListener" type="System.Diagnostics.ConsoleTraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
```

## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre os detalhes do pacote Nuget e as notas da biblioteca de BulkExecutor .net, consulte[detalhes BulkExecutor SDK](sql-api-sdk-bulk-executor-dot-net.md). 
