---
title: Desenvolver com a API de tabela com o .NET SDK
titleSuffix: Azure Cosmos DB
description: Aprenda a desenvolver com a API de tabela no Azure Cosmos DB com o .NET SDK
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: f10cb17f0300b6bd21d17b1e2ff204d57e4a7988
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53251186"
---
# <a name="develop-with-azure-cosmos-dbs-table-api-using-net-sdk"></a>Desenvolver com a API de tabela do Azure Cosmos DB com o .NET SDK

O Azure Cosmos DB é um serviço de base de dados com vários modelos e de distribuição global da Microsoft. Pode criar e consultar rapidamente o documento, a chave/valor e as bases de dados de gráficos, que beneficiam de capacidades de escalamento horizontal e distribuição global no centro do Azure Cosmos DB.

Este tutorial abrange as seguintes tarefas: 

> [!div class="checklist"] 
> * Criar uma conta do Azure Cosmos DB 
> * Ativar a funcionalidade no ficheiro app.config 
> * Criar uma tabela com a [API de Tabelas](table-introduction.md)
> * Adicionar uma entidade a uma tabela 
> * Inserir um lote de entidades 
> * Obter uma única entidade 
> * Consultar entidades mediante a utilização de índices secundários automáticos 
> * Substituir uma entidade 
> * Eliminar uma entidade 
> * Eliminar uma tabela
 
## <a name="tables-in-azure-cosmos-db"></a>Tabelas no Azure Cosmos DB 

O Azure Cosmos DB disponibiliza a [API de Tabelas](table-introduction.md) para as aplicações que precisam de um arquivo de chaves-valores com um design sem esquema. Tanto a API de Tabelas do Azure Cosmos DB, como o [armazenamento de Tabelas do Azure](../storage/common/storage-introduction.md), suportam agora os mesmos SDKs e APIs REST. Pode utilizar o Azure Cosmos DB para criar tabelas com requisitos de débito elevado.

Este tutorial é para os programadores que estão familiarizados com o SDK de armazenamento de Tabelas do Azure e que querem utilizar as funcionalidades premium disponíveis no Azure Cosmos DB. Baseia-se em [Get Started with Azure Table storage using .NET](table-storage-how-to-use-dotnet.md) (Introdução ao armazenamento de Tabelas do Azure com .NET) e mostra como tirar partido de capacidades adicionais, como índices secundários, débito aprovisionado e multi-homing. Este tutorial descreve como utilizar o portal do Azure para criar uma conta do Azure Cosmos DB e, em seguida, criar e implementar uma aplicação de API de Tabelas. Também vamos analisar exemplos de .NET para criar e eliminar uma tabela e inserir, atualizar, eliminar e consultar dados de tabelas. 

Se utilizar atualmente o armazenamento de Tabelas do Azure, beneficia das vantagens seguintes com a API de Tabelas do Azure Cosmos DB:

- [Distribuição global](distribute-data-globally.md) chave na mão com multi-homing e [ativações pós-falha automáticas e manuais](high-availability.md)
- Suporte para indexação automática e independente de esquema relativamente a todas as propriedades (“índices secundários”) e consultas rápidas 
- Suporte para [dimensionamento independente de armazenamento e débito](partition-data.md), em qualquer número de regiões
- Suporte para [débito dedicado por tabela](request-units.md) que pode ser dimensionado de centenas para milhões de pedidos por segundo
- Suporte para [cinco níveis de consistência ajustáveis](consistency-levels.md) para alternar entre disponibilidade, latência e consistência com base nas necessidades da sua aplicação.
- 99,99% de disponibilidade numa única região e capacidade de adicionar mais regiões para maior disponibilidade e [SLAs abrangentes líderes da indústria](https://azure.microsoft.com/support/legal/sla/cosmos-db/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) em disponibilidade geral
- Trabalhar com o SDK do .NET do armazenamento do Azure já existente e ausência de alterações ao código da aplicação

Este tutorial abrange a API de Tabelas do Azure Cosmos DB com o SDK do .NET. Pode transferir [Azure Cosmos DB Table API .NET SDK](https://aka.ms/tableapinuget) (SDK do .NET da API de Tabelas do Azure Cosmos DB) do NuGet.

Para saber mais sobre as tarefas de armazenamento de tabelas do Azure complexas, veja:

* A [Introdução à API de Tabelas do Azure Cosmos DB](table-introduction.md)
* A documentação de referência do serviço Tabelas para obter detalhes completos sobre as APIs disponíveis [Azure Cosmos DB Table API .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)

### <a name="about-this-tutorial"></a>Acerca deste tutorial
Este tutorial é para os programadores que estão familiarizados com o SDK de armazenamento de Tabelas do Azure e que querem utilizar as funcionalidades premium disponíveis com a utilização do Azure Cosmos DB. Baseia-se em [Get Started with Azure Table storage using .NET](table-storage-how-to-use-dotnet.md) (Introdução ao armazenamento de Tabelas do Azure com .NET) e mostra como tirar partido de capacidades adicionais, como índices secundários, débito aprovisionado e multi-homing. Descreve como utilizar o portal do Azure para criar uma conta do Azure Cosmos DB e, em seguida, criar e implementar uma aplicação de Tabelas. Também vamos analisar exemplos de .NET para criar e eliminar uma tabela e inserir, atualizar, eliminar e consultar dados de tabelas. 

Se ainda não tiver o Visual Studio 2017 instalado, pode transferir e utilizar a [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) **gratuita**. Confirme que ativa o **desenvolvimento do Azure** durante a configuração do Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Criar uma conta de base de dados

Comecemos por criar uma conta do Azure Cosmos DB no portal do Azure.  
 
> [!IMPORTANT]  
> Tem de criar uma nova conta da API de Tabela para trabalhar com os SDKs de API de Tabela geralmente disponíveis. As contas de API de Tabela criadas durante a pré-visualização não são suportadas pelos SDKs geralmente disponíveis. 
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-the-sample-application"></a>Clonar a aplicação de exemplo

Agora, vamos clonar uma aplicação de Tabela a partir do GitHub, definir a cadeia de ligação e executá-la. Vai ver como é fácil trabalhar com dados programaticamente. 

1. Abra uma janela de terminal do git, como o git bash e utilize o comando `cd` para alterar para uma pasta e instalar a aplicação de exemplo. 

    ```bash
    cd "C:\git-samples"
    ```

2. Execute o seguinte comando para clonar o repositório de exemplo. Este comando cria uma cópia da aplicação de exemplo no seu computador. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Em seguida, abra o ficheiro da solução no Visual Studio. 

## <a name="update-your-connection-string"></a>Atualizar a cadeia de ligação

Agora, regresse ao portal do Azure para obter as informações da cadeia de ligação e copie-as para a aplicação. Isto permite à aplicação comunicar com a base de dados alojada. 

1. No [portal do Azure](https://portal.azure.com/), clique em **Cadeia de ligação**. 

    Utilize os botões de cópia à direita do ecrã, para copiar a CADEIA DE LIGAÇÃO PRIMÁRIA.

    ![Ver e copiar a CADEIA DE LIGAÇÃO no painel Cadeia de Ligação](./media/create-table-dotnet/connection-string.png)

2. No Visual Studio, abra o ficheiro app.config. 

3. Anule o comentário StorageConnectionString na linha 8 e comente em StorageConnectionString na linha 7, uma vez que este tutorial não utiliza o Emulador de Armazenamento. As linhas 7 e 8 devem agora ter o seguinte aspeto:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. Cole a CADEIA DE LIGAÇÃO PRIMÁRIA do portal no valor de StorageConnectionString na linha 8. Cole a cadeia dentro de aspas.
   
    > [!IMPORTANT]
    > Se o Ponto final utilizar documents.azure.com, significa que tem uma conta de pré-visualização, e tem de criar uma [nova conta de API de Tabela](#create-a-database-account) para trabalhar com o SDK de API de Tabela geralmente disponível. 
    >

    A linha 8 deve ter agora um aspeto semelhante a:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

5. Guarde o ficheiro app.config.

Atualizou agora a sua aplicação com todas as informações necessárias para comunicar com o Azure Cosmos DB. 

## <a name="azure-cosmos-db-capabilities"></a>Capacidades do Azure Cosmos BD
O Azure Cosmos DB suporta várias capacidades que não estão disponíveis na API de armazenamento de Tabelas do Azure. 

Algumas funcionalidades são acedidas através de novas sobrecargas para o elemento CreateCloudTableClient que permitem aos utilizadores especificar a política de ligação e o nível de consistência.

| Definições de Ligação da Tabela | Descrição |
| --- | --- |
| Modo da Ligação  | O Azure Cosmos DB suporta dois modos de conectividade No modo `Gateway`, os pedidos são sempre feitos para o gateway do Azure Cosmos DB, que os encaminha para as partições de dados correspondentes. No modo de conectividade `Direct`, o cliente obtém o mapeamento das tabelas para partições e os pedidos são feitos diretamente nas partições de dados. Recomendamos `Direct`, o predefinido.  |
| Protocolo da Ligação | O Azure Cosmos DB suporta dois protocolos de ligação, `Https` e `Tcp`. `Tcp` é o predefinido e o recomendado porque é mais leve. |
| Localizações Preferenciais | Uma lista separada por vírgulas das localizações preferenciais (multi-homing) para leituras. Cada conta do Azure Cosmos DB pode ser associada a entre 1 e 30 ou mais regiões. Cada instância do cliente pode especificar um subconjunto dessas regiões pela ordem preferencial para leituras de latência baixa. Os nomes das regiões devem utilizar os respetivos [nomes a apresentar](https://msdn.microsoft.com/library/azure/gg441293.aspx), como, por exemplo, `West US`. Veja também [Multi-homing APIs](tutorial-global-distribution-table.md) (APIs de multi-homing). |
| Nível de Consistência | Pode fazer compensações entre latência, consistência e disponibilidade ao escolher entre cinco níveis de consistência bem definidos: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix` e `Eventual`. A predefinição é `Session`. A escolha do nível de consistência faz uma diferença significativa quanto ao desempenho nas configurações de várias regiões. Veja [Consistency levels](consistency-levels.md) (Níveis de consistência) para obter detalhes. |

Podem ser ativadas outras funcionalidades com os valores de configuração `appSettings` seguintes.

| Chave | Descrição |
| --- | --- |
| TableQueryMaxItemCount | Configurar o número máximo de itens devolvidos por consulta de tabela num único percurso de ida e volta. A predefinição é `-1`, que permite que o Azure Cosmos DB determine dinamicamente o valor no runtime. |
| TableQueryEnableScan | Se a consulta não puder utilizar o índice para algum filtro, executá-la de qualquer forma através de uma análise. A predefinição é `false`.|
| TableQueryMaxDegreeOfParallelism | O grau de paralelismo para a execução de uma consulta entre partições. `0` é um valor em série sem obtenção prévia, `1` é um valor em série com obtenção prévia e valores mais altos aumentam a taxa de paralelismo. A predefinição é `-1`, que permite que o Azure Cosmos DB determine dinamicamente o valor no runtime. |

Para alterar o valor predefinido, abra o ficheiro `app.config` no Explorador de Soluções do Visual Studio. Adicione o conteúdo do elemento `<appSettings>` mostrado abaixo. Substitua `account-name` pelo nome da sua conta de armazenamento e `account-key` pela chave de acesso da conta. 

```xml
<configuration>
    ...
    <appSettings>
      <!-- Client options -->
      <add key="CosmosDBStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://account-name.table.cosmosdb.azure.com" />
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Vamos fazer uma breve revisão do que está a acontecer à aplicação. Abra o ficheiro `Program.cs` e verá que estas linhas de código criam os recursos da Tabela. 

## <a name="create-the-table-client"></a>Criar o cliente da tabela
Para ligar à conta da tabela, inicialize `CloudTableClient`.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Este cliente é iniciado com os valores de configuração `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel` e `TablePreferredLocations`, se estiverem especificados nas definições da aplicação.

## <a name="create-a-table"></a>Criar uma tabela

Em seguida, vai criar uma tabela com `CloudTable`. As tabelas do Azure Cosmos DB podem ser dimensionadas de forma independente em termos de armazenamento e débito e o serviço cria automaticamente as partições. 

```csharp
CloudTable table = tableClient.GetTableReference("people");
400
table.CreateIfNotExists(throughput: 800);
```

Há uma diferença importante na forma como as tabelas são criadas. O Azure Cosmos DB reserva débito, ao contrário do modelo baseado no consumo do armazenamento do Azure para transações. O débito é dedicado/reservado, pelo que nunca se verá limitado se a taxa de pedidos estiver no débito aprovisionado ou abaixo.

Pode configurar o débito predefinido ao incluí-lo como um parâmetro de CreateIfNotExists.

A leitura de uma entidade de 1 KB é normalizada como 1 RU e as outras operações são normalizadas para um valor de RU fixo com base na CPU, na memória e no consumo de IOPS. Saiba mais sobre [Request units in Azure Cosmos DB](request-units.md) (Unidades de pedido no Azure Cosmos DB) e, mais especificamente, [key value stores](key-value-store-cost.md) (Arquivos de chaves-valores).

Em seguida, vamos analisar as operações simples de leitura e escrita (CRUD) ao utilizar o SDK de armazenamento de Tabelas do Azure. Este tutorial demonstra latências baixas previsíveis, em milissegundos de um só dígito, e consultas rápidas fornecidas pelo Azure Cosmos DB.

## <a name="add-an-entity-to-a-table"></a>Adicionar uma entidade a uma tabela
As entidades no armazenamento de Tabelas do Azure partem da classe `TableEntity` e têm de ter as propriedades `PartitionKey` e `RowKey`. Segue-se uma definição de exemplo para uma entidade de cliente.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

O fragmento seguinte mostra como inserir uma entidade com o SDK de armazenamento do Azure. O Azure Cosmos DB foi concebido para latência baixa garantida em qualquer escala em todo o mundo.

As escritas são concluídas em <15 ms em p99 e ~6 ms em p50 nas aplicações que são executadas na mesma região que a conta do Azure Cosmos DB. E esta duração tem em conta o facto de as escritas serem reconhecidas no cliente só após serem replicadas de forma síncrona, consolidadas de forma durável e depois de todo o conteúdo ter sido indexado.


```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Inserir um lote de entidades
O armazenamento de Tabelas do Azure suporte uma API de operação em lote, que lhe permite combinar atualizações, eliminações e inserções na mesma operação em lote.

```csharp
// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a>Obter uma única entidade
As obtenções (GETs) no Azure Cosmos DB demoram <10 ms em p99 e ~1 ms em p50 na mesma região do Azure. Pode adicionar tantas regiões quantas pretender à sua conta para leituras de latência baixa e implementar aplicações para que leiam a partir da respetiva região local ("multi-homed") ao definir `TablePreferredLocations`. 

Pode utilizar o fragmento abaixo para obter uma entidade única:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> Saiba mais sobre o multi-homing das APIs em [Developing with multiple regions](tutorial-global-distribution-table.md) (Desenvolver com várias regiões)
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Consultar entidades mediante a utilização de índices secundários automáticos
As tabelas podem ser consultadas com a classe `TableQuery`. O Azure Cosmos DB tem um motor de bases de dados otimizado para escritas que indexa automaticamente todas as colunas das tabelas. A indexação do Azure Cosmos DB é independente do esquema. Por conseguinte, mesmo que o esquema seja diferente entre as linhas que evolua ao longo do tempo, é indexado automaticamente. Uma vez que o Azure Cosmos DB suporta índices secundários automáticos, as consultas a qualquer propriedade podem utilizar o índice e ser satisfeitas de forma eficiente.

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

O Azure Cosmos DB suporta a mesma funcionalidade de consultas que o armazenamento de Tabelas do Azure para a API de Tabelas. Também suporta ordenação, agregações, consultas geoespaciais, hierarquias e um vasto conjunto de funções incorporadas. Veja [Azure Cosmos DB query](how-to-sql-query.md) (Consulta do Azure Cosmos DB) para obter uma descrição geral destas capacidades. 

## <a name="replace-an-entity"></a>Substituir uma entidade
Para atualizar uma entidade, recupere-a do serviço Tabela, modifique o objeto de entidade e, em seguida, guarde as alterações novamente no serviço Tabela. O código seguinte altera o número de telefone de um cliente existente. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
Da mesma forma, pode realizar operações `InsertOrMerge` ou `Merge`.  

## <a name="delete-an-entity"></a>Eliminar uma entidade
Pode facilmente eliminar uma entidade depois de a ter obtido através do mesmo padrão mostrado para atualizar uma entidade. O código seguinte obtém e elimina uma entidade de cliente.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Eliminar uma tabela
Finalmente, o seguinte exemplo de código elimina uma tabela a partir de uma conta do Storage. Pode eliminar e recriar uma tabela imediatamente com o Azure Cosmos DB.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, mostrámos como começar a utilizar o Azure Cosmos DB com a API de Tabelas e fizemos o seguinte: 

> [!div class="checklist"] 
> * Criámos uma conta do Azure Cosmos DB 
> * Ativámos funcionalidades no ficheiro app.config 
> * Criámos uma tabela 
> * Adicionámos uma entidade a uma tabela 
> * Inserimos um lote de entidades 
> * Obtivemos uma entidade individual 
> * Utilizámos índices secundários automáticos para consultar entidades 
> * Substituímos uma entidade 
> * Eliminámos uma entidade 
> * Eliminámos uma tabela  

Agora, pode avançar para o próximo tutorial e saber mais sobre como consultar dados de tabelas. 

> [!div class="nextstepaction"]
> [Query with the Table API](tutorial-query-table.md) (Consultar com a API de Tabelas)
