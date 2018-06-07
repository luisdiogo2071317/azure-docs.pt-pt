---
title: Utilizar a biblioteca BulkExecutor Java para efetuar operações em massa na base de dados do Azure Cosmos | Microsoft Docs
description: Utilize a biblioteca de BulkExecutor Java da BD do Azure Cosmos em massa importar e atualizar os documentos para coleções de base de dados do Azure Cosmos.
keywords: Executor do Java em massa
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 77135ec5f62636d1dd634361da345b00d98ad918
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34610247"
---
# <a name="use-bulkexecutor-java-library-to-perform-bulk-operations-on-azure-cosmos-db-data"></a>Utilizar a biblioteca BulkExecutor Java para efetuar operações em massa na base de dados do Azure Cosmos dados

Este tutorial fornece instruções sobre como utilizar a biblioteca de Java de executor de em massa do Cosmos BD do Azure para importar e atualizar a base de dados do Azure Cosmos documentos. Para saber mais sobre a biblioteca de BulkExecutor e como ajuda a tirar partido do débito intenso e armazenamento, consulte [descrição geral da biblioteca de BulkExecutor](bulk-executor-overview.md) artigo. Neste tutorial, criar uma aplicação de Java que gera documentos aleatórios e são em massa importá-lo para uma coleção de BD do Cosmos do Azure. Depois de importar, será em massa algumas propriedades de um documento de atualização. 

## <a name="prerequisites"></a>Pré-requisitos

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.  

* Pode [Experimentar o Azure Cosmos DB gratuitamente](https://azure.microsoft.com/try/cosmosdb/) sem uma subscrição do Azure, sem encargos e compromissos. Em alternativa, pode utilizar o [emulador de BD do Azure Cosmos](https://docs.microsoft.com/azure/cosmos-db/local-emulator) com o `https://localhost:8081` URI. A chave primária é fornecida na [autenticar pedidos](local-emulator.md#authenticating-requests).  

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  
  - No Ubuntu, execute `apt-get install default-jdk` para instalar o JDK.  

  - Certifique-se de que define a variável de ambiente JAVA_HOME para apontar para a pasta onde está instalado o JDK.

* [Transferir](http://maven.apache.org/download.cgi) e [instalar](http://maven.apache.org/install.html) um arquivo binário [Maven](http://maven.apache.org/)  
  
  - No Ubuntu, pode executar `apt-get install maven` para instalar o Maven.

* Criar uma conta de API do SQL Server de base de dados do Azure Cosmos utilizando os passos descritos no [criar conta de base de dados](create-sql-api-java.md#create-a-database-account) secção do artigo de início rápido de Java.

## <a name="clone-the-sample-application"></a>Clonar a aplicação de exemplo

Agora vamos mudar para trabalhar com o código ao transferir um exemplo de aplicação Java a partir do GitHub. Esta aplicação executa operações em massa na base de dados do Azure Cosmos dados. Para clonar a aplicação, abra uma linha de comandos, navegue para o diretório onde pretende copiar a aplicação e execute o seguinte comando:

```
 git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started.git 
```

O repositório clonado contém dois exemplos "bulkimport" e "bulkupdate" relativo à pasta de "\azure-cosmosdb-bulkexecutor-java-getting-started\samples\bulkexecutor-sample\src\main\java\com\microsoft\azure\cosmosdb\bulkexecutor". A aplicação "bulkimport" gera documentos aleatórios e importa-los à base de dados do Azure Cosmos. A aplicação "bulkupdate" atualiza alguns documentos do BD Azure Cosmos. As secções seguintes, irá rever o código em cada uma destas aplicações de exemplo. 

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Dados de importação em volume à base de dados do Azure Cosmos

1. Cadeias de ligação do Cosmos BD do Azure são lidos como argumentos e atribuídas para as variáveis definidas no ficheiro CmdLineConfiguration.java.  

2. Em seguida o DocumentClient objecto foi inicializado, utilizando as seguintes instruções:  

   ```java
   ConnectionPolicy connectionPolicy = new ConnectionPolicy();
   connectionPolicy.setMaxPoolSize(1000);
   DocumentClient client = new DocumentClient(
      HOST,
      MASTER_KEY, 
      connectionPolicy,
      ConsistencyLevel.Session)
   ```

3. O objeto de DocumentBulkExecutor é inicializado com um valores de tentativa elevada para o tempo de espera e limitadas pedidos. E, em seguida, estão definidos para 0 para passar o controlo de congestionamento para DocumentBulkExecutor para o seu período de duração.  

   ```java
   // Set client's retry options high for initialization
   client.getConnectionPolicy().getRetryOptions().setMaxRetryWaitTimeInSeconds(30);
   client.getConnectionPolicy().getRetryOptions().setMaxRetryAttemptsOnThrottledRequests(9);

   // Builder pattern
   Builder bulkExecutorBuilder = DocumentBulkExecutor.builder().from(
     client,
     DATABASE_NAME,
     COLLECTION_NAME,
     collection.getPartitionKey(),
     offerThroughput) // throughput you want to allocate for bulk import out of the collection's total throughput

   // Instantiate DocumentBulkExecutor
   DocumentBulkExecutor bulkExecutor = bulkExecutorBuilder.build()

   // Set retries to 0 to pass complete control to bulk executor
   client.getConnectionPolicy().getRetryOptions().setMaxRetryWaitTimeInSeconds(0);
   client.getConnectionPolicy().getRetryOptions().setMaxRetryAttemptsOnThrottledRequests(0);
```

4. Chame o importAll API que gera aleatórios documentos em massa importar uma coleção de BD do Cosmos do Azure. Pode configurar as configurações de linha de comandos no ficheiro CmdLineConfiguration.java.

   ```java
   BulkImportResponse bulkImportResponse = bulkExecutor.importAll(documents, false, true, null);
```
   A API de importação em massa aceita uma coleção de documentos JSON serializado e tem a seguinte sintaxe, para obter mais detalhes, consulte o [documentação da API](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor):

   ```java
   public BulkImportResponse importAll(
        Collection<String> documents,
        boolean isUpsert,
        boolean disableAutomaticIdGeneration,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;   
   ```

   O método importAll aceita os seguintes parâmetros:
 
   |**Parâmetro**  |**Descrição**  |
   |---------|---------|
   |isUpsert    |   Um sinalizador para ativar upsert dos documentos. Se um documento com o ID já existe, é atualizado.  |
   |disableAutomaticIdGeneration     |   Um sinalizador para desativar a geração automática do ID. Por predefinição, está definida como true.   |
   |maxConcurrencyPerPartitionRange    |  O nível máximo de concorrência por intervalo de chaves de partição. O valor predefinido é 20.  |

   **Em massa a definição de objeto de resposta de importação** o resultado da chamada de API de importação em massa contém os métodos get seguinte:

   |**Parâmetro**  |**Descrição**  |
   |---------|---------|
   |Int getNumberOfDocumentsImported()  |   O número total de documentos que foram importados com êxito sem os documentos fornecidos para o volume de importar a chamada à API.      |
   |getTotalRequestUnitsConsumed() duplo   |  As unidades de pedido total (RU) consumidas pelo volume de importar a chamada à API.       |
   |Duração getTotalTimeTaken()   |    O tempo total que a importação em massa chamada da API para concluir a execução.     |
   |Lista<Exception> getErrors() |  Obtém a lista de erros se alguns documentos fora do batch fornecido para o volume de importar a falha ao obter inserido na chamada à API.       |
   |Lista<Object> getBadInputDocuments()  |    A lista de documentos de formato incorrecto que não foram importados com êxito no volume importar chamada à API. Utilizador deve corrigir os documentos devolvidos e repita a importação. Documentos com formato incorrecto incluem documentos cujo valor de ID não é uma cadeia (nulo ou qualquer outro tipo de dados é considerado inválido).     |

5. Depois de ter o volume de importar aplicação prontos, construir a ferramenta de linha de comandos a partir da origem, utilizando o comando 'mvn limpa pacote'. Este comando gera um ficheiro jar na pasta de destino:  

   ```java
   mvn clean package
   ```

6. Depois das dependências de destino são geradas, pode invocar a aplicação do importador de volume usando o seguinte comando:  

   ```java
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint *<Fill in your Azure Cosmos DB’s endpoint URI>*  -masterKey *<Fill in your Azure Cosmos DB’s master key>* -databaseId bulkImportDb -collectionId bulkImportColl -operation import -shouldCreateCollection -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

   O importador de em massa cria uma nova base de dados e uma coleção com o nome de base de dados, o nome da coleção e a valores de débito especificados no ficheiro App. config. 

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Dados de atualização em massa na base de dados do Azure Cosmos

Pode atualizar documentos existentes, utilizando a API de BulkUpdateAsync. Neste exemplo, irá definir o campo de nome para um novo valor e remover o campo de descrição do documentos existentes. Para o conjunto completo de campo suportado operações de atualização, consulte [documentação da API](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor). 

1. Define os itens de atualização, juntamente com as operações de atualização de campo correspondente. Neste exemplo, irá utilizar SetUpdateOperation para atualizar o campo de nome e UnsetUpdateOperation para remover o campo de descrição de todos os documentos. Também pode efetuar outras operações como incremento um campo de documento por um valor específico, enviar valores específicos para um campo de matriz ou remover um valor específico de um campo de matriz. Para saber mais sobre os diferentes métodos que forneceu a API de atualização em massa, consulte o [documentação da API](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor).  

   ```java
   SetUpdateOperation<String> nameUpdate = new SetUpdateOperation<>("Name","UpdatedDocValue");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   ArrayList<UpdateOperationBase> updateOperations = new ArrayList<>();
   updateOperations.add(nameUpdate);
   updateOperations.add(descriptionUpdate);

   List<UpdateItem> updateItems = new ArrayList<>(cfg.getNumberOfDocumentsForEachCheckpoint());
   IntStream.range(0, cfg.getNumberOfDocumentsForEachCheckpoint()).mapToObj(j -> {                      
    return new UpdateItem(Long.toString(prefix + j), Long.toString(prefix + j), updateOperations);
    }).collect(Collectors.toCollection(() -> updateItems));
   ```

2. Chame o updateAll API que gera aleatórios documentos, em seguida, ser em massa importá-lo para uma coleção de BD do Cosmos do Azure. Pode configurar as configurações da linha de comandos a ser transmitido nas CmdLineConfiguration.java ficheiro.

   ```java
   BulkUpdateResponse bulkUpdateResponse = bulkExecutor.updateAll(updateItems, null)
   ```

   A API de atualização em massa aceita uma coleção de itens a serem atualizados. Cada item de atualização Especifica a lista de operações de atualização de campo para ser efetuada num documento identificado por um ID e um valor de chave de partição. Para obter mais detalhes, consulte o [documentação da API](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor):

   ```java
   public BulkUpdateResponse updateAll(
        Collection<UpdateItem> updateItems,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;
   ```

   O método updateAll aceita os seguintes parâmetros:

   |**Parâmetro** |**Descrição** |
   |---------|---------|
   |maxConcurrencyPerPartitionRange   |  O nível máximo de concorrência por intervalo de chaves de partição. O valor predefinido é 20.  |
 
   **Em massa a definição de objeto de resposta de importação** o resultado da chamada de API de importação em massa contém os métodos get seguinte:

   |**Parâmetro** |**Descrição**  |
   |---------|---------|
   |Int getNumberOfDocumentsUpdated()  |   O número total de documentos que foram atualizadas com êxito sem os documentos fornecidos para a chamada de API de atualização em massa.      |
   |getTotalRequestUnitsConsumed() duplo |  As unidades de pedido total (RU) consumidos pelos próprios a atualização em massa na chamada à API.       |
   |Duração getTotalTimeTaken()  |   O tempo total que o volume de chamada da API para concluir a execução de atualização.      |
   |Lista<Exception> getErrors()   |     Obtém a lista de erros se alguns documentos fora do lote fornecido para a chamada de API de atualização em massa não foi possível obter inserir.      |

3. Depois de ter o volume de atualizar a aplicação prontos, construir a ferramenta de linha de comandos a partir da origem, utilizando o comando 'mvn limpa pacote'. Este comando gera um ficheiro jar na pasta de destino:  

   ```
   mvn clean package
   ```

4. Depois das dependências de destino são geradas, pode invocar a aplicação de atualização em massa, utilizando o seguinte comando:

   ```
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint **<Fill in your Azure Cosmos DB’s endpoint URI>* -masterKey **<Fill in your Azure Cosmos DB’s master key>* -databaseId bulkUpdateDb -collectionId bulkUpdateColl -operation update -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

## <a name="performance-tips"></a>Sugestões de desempenho 

Considere os seguintes pontos para um melhor desempenho ao utilizar a biblioteca de BulkExecutor:

* Para melhor desempenho, execute a aplicação a partir de uma VM do Azure na mesma região que a região de escrita da conta de base de dados do Cosmos.  
* Para alcançar um maior débito:  

   * Defina o tamanho de área dinâmica para dados das JVM para um número suficientemente grande para evitar qualquer problema de memória no processamento elevado número de documentos. Sugestões de tamanho da pilha: máximo (3GB, 3 * sizeof (todos os documentos transmitidos para em massa importar API em um lote)).  
   * Há uma hora preprocessing, devido à qual irá obter um maior débito ao realizar operações em massa com um grande número de documentos. Por isso, se pretender importar 10,000,000 documentos, execução de importação em volume 10 vezes em 10 em massa documentos cada tamanho 1,000,000 é preferível que executar importação em volume de 100 vezes em 100 em massa documentos cada um dos documentos de 100 000 de tamanho.  

* Recomenda-se ao instanciar um objeto de DocumentBulkExecutor único para a aplicação completa numa única máquina virtual que corresponde a uma coleção de BD do Cosmos de Azure específica.  

* Uma vez que uma execução de API de operação em massa única consome um segmento grande de CPU e da rede e/s a máquina cliente. Isto acontece por gerar várias tarefas internamente, evite gerar várias tarefas simultâneas dentro do seu processo de aplicação que chama de cada API de operação em massa em execução. Se uma chamada de API de operação em massa única em execução numa única máquina virtual não é possível consumir o débito da sua coleção completa (se débito > 1 a coleção milhões RU/s), é preferível para criar máquinas virtuais separadas para executar em simultâneo em massa chamadas de operação de API.

    
## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre os detalhes do pacote maven e as notas da biblioteca BulkExecutor Java, consulte[detalhes BulkExecutor SDK](sql-api-sdk-bulk-executor-java.md).


