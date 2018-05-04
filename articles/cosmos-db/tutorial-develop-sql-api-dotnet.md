---
title: 'Azure Cosmos DB: Desenvolver com a API de SQL em .NET | Microsoft Docs'
description: Saiba como desenvolver com a API de SQL do Azure Cosmos DB mediante a utilização de .NET
services: cosmos-db
documentationcenter: ''
author: rafats
manager: kfile
editor: ''
tags: ''
ms.assetid: ''
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 05/10/2017
ms.author: rafats
ms.custom: mvc
ms.openlocfilehash: a6ed74de159593003e8a18daefce2eb9a5945481
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="azure-cosmos-db-develop-with-the-sql-api-in-net"></a>Azure Cosmos DB: Desenvolver com a API de SQL em .NET

O Azure Cosmos DB é um serviço de base de dados com vários modelos e de distribuição global da Microsoft. Pode criar e consultar rapidamente o documento, a chave/valor e as bases de dados de gráficos, que beneficiam de capacidades de escalamento horizontal e distribuição global no centro do Azure Cosmos DB.

Este tutorial demonstra como criar uma conta do Azure Cosmos DB com o Portal do Azure e, em seguida, criar uma base de dados de documentos e uma coleção com uma [chave de partição](sql-api-partition-data.md#partition-keys) mediante a utilização da [API .NET de SQL](sql-api-introduction.md). Ao definir uma chave de partição quando cria uma coleção, a sua aplicação estará preparada para se dimensionar facilmente à medida que os seus dados aumentam.

Este tutorial abrange as seguintes tarefas mediante a utilização da [API .NET de SQL](sql-api-sdk-dotnet.md):

> [!div class="checklist"]
> * Criar uma conta do Azure Cosmos DB
> * Criar uma base de dados e uma coleção com uma chave de partição
> * Criar documentos JSON
> * Atualizar um documento
> * Consultar as coleções particionadas
> * Executar procedimentos armazenados
> * Eliminar um documento
> * Eliminar uma base de dados

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar, certifique-se de que tem o seguinte:

* Acesso a uma conta do Azure Cosmos DB

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

  Também pode utilizar a sua própria subscrição do Azure ao inscrever-se para obter uma [conta do Azure gratuita](https://azure.microsoft.com/free/). A partir daí, pode [Criar uma conta do Azure Cosmos DB](create-sql-api-dotnet.md#create-a-database-account).

* Se ainda não tiver o Visual Studio 2017 instalado, pode transferir e utilizar a [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) **gratuita**. Confirme que ativa o **desenvolvimento do Azure** durante a configuração do Visual Studio.


> [!TIP]
> * Se optar por utilizar o Emulador do Azure Cosmos DB, siga os passos em [Emulador do Azure Cosmos DB](local-emulator.md) para configurar o emulador
>
>


## <a id="SetupVS"></a>Configurar a sua solução Visual Studio
1. Abra o **Visual Studio** no seu computador.
2. No menu **Ficheiro**, selecione **Novo**, e, em seguida, escolha **Projeto**.
3. Na caixa de diálogo **New Project** (Novo Projeto), selecione **Templates** (Modelos) / **Visual C#** / **Console App (.NET Framework)** (Aplicação de Consola [.NET Framework]), dê um nome ao projeto e clique em **OK**.
   ![Captura de ecrã da janela Novo Projeto](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-new-project-2.png)

4. No **Explorador de Soluções**, clique com o botão direito do rato na sua nova aplicação de consola, que está sob a sua solução Visual Studio e, em seguida, clique em **Gerir Pacotes NuGet...**

    ![Captura de ecrã do Menu com Botão do Lado Direito para o Projeto](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. No separador **NuGet**, clique em **Procurar** e escreva **documentdb** na caixa de pesquisa.
<!---stopped here--->
6. Nos resultados, localize **Microsoft.Azure.DocumentDB** e clique em **Instalar**.
   O ID do pacote da Biblioteca de Cliente do Azure Cosmos DB é [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).
   ![Captura de ecrã do Menu NuGet para encontrar o SDK de Cliente do Azure Cosmos DB](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)

    Se obtiver uma mensagem sobre a revisão das alterações para a solução, clique em **OK**. Se obtiver uma mensagem sobre a aceitação de licença, clique em **Aceito**.

## <a id="Connect"></a>Adicionar referências ao seu projeto
Os restantes passos deste tutorial disponibilizam os fragmentos de código da API de SQL necessários para criar e atualizar recursos do Azure Cosmos DB no seu projeto.

Primeiro, adicione estas referências à sua aplicação.
<!---These aren't added by default when you install the pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <a id="add-references"></a>Ligar a aplicação

Em seguida, adicione estas duas constantes e a variável *client* na sua aplicação.

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

Depois, regresse ao [Portal do Azure](https://portal.azure.com) para obter o URL do ponto final e a chave primária. O URL de ponto final e a chave primária são necessários para que a sua aplicação saiba onde ligar e para que o Azure Cosmos DB confie na ligação da sua aplicação.

No portal do Azure, navegue até à sua conta do Azure Cosmos DB. No menu do lado esquerdo, selecione **Chaves** e, em seguida, selecione **Chaves de leitura/escrita**.

Copie o URI a partir do portal e cole-o em `<your endpoint URL>`, no ficheiro program.cs. Em seguida, copie a CHAVE PRIMÁRIA a partir do portal e cole-a em `<your primary key>`. Não se esqueça de remover `<` e `>` dos valores.

![Captura de ecrã do Portal do Azure utilizado pelo tutorial NoSQL para criar uma aplicação de consola C#. Mostra uma conta do Azure Cosmos DB, com as CHAVES realçadas na secção da conta do Azure Cosmos DB e os valores URI e CHAVE PRIMÁRIA realçados na secção Chaves](./media/tutorial-develop-sql-api-dotnet/nosql-tutorial-keys.png)

## <a id="instantiate"></a>Instanciar o DocumentClient

Agora, crie uma instância nova do **DocumentClient**.

```csharp
DocumentClient client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
```

## <a id="create-database"></a>Criar uma Base de Dados

Em seguida, crie uma [base de dados](sql-api-resources.md#databases) do Azure Cosmos DB com o métodos [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) ou [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) da classe **DocumentClient** do [SDK .NET de SQL](sql-api-sdk-dotnet.md). Uma base de dados é o contentor lógico do armazenamento de documentos JSON particionado em coleções.

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a>Escolher uma chave de partição

As coleções são contentores para armazenar documentos. São recursos lógicos e podem [abranger uma ou mais partições físicas](partition-data.md). As [chaves de partições](sql-api-partition-data.md) são uma propriedade (ou caminho) dentro dos documentos utilizada para distribuir os seus dados entre os servidores ou partições. Todos os documentos com a mesma chave de partição são armazenadas na mesma partição.

É importante determinar uma chave de partição antes de criar uma coleção. As chaves de partições são uma propriedade (ou caminho) dentro dos documentos que o Azure Cosmos DB pode utilizar para distribuir os seus dados entre vários servidores ou partições. O Cosmos DB codifica o valor da chave de partição e utiliza o resultado codificado para determinar a partição na qual armazenar o documento. Todos os documentos com a mesma chave de partição são armazenados na mesma partição e as chaves de partição não podem ser alteradas após as coleções terem sido criadas.

Neste tutorial, tem de definir a chave de partição como `/deviceId`, de modo a que todos os dados de um dispositivo individual sejam armazenados numa única partição. O mais indicado é escolher uma chave de partição que tenha muitos valores, cada um desses utilizado com a mesma frequência para garantir que o Cosmos DB consegue balancear a carga à medida que os seus dados aumentam e obter todo o débito da coleção.

Para obter mais informações sobre a criação de partições veja [How to partition and scale in Azure Cosmos DB?](partition-data.md) (Como criar partições e dimensionar no Azure Cosmos DB?)

## <a id="CreateColl"></a>Criar uma coleção

Com a chave de partição, `/deviceId`, pode criar uma [coleção](sql-api-resources.md#collections) com o método [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) ou o método [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) da classe **DocumentClient**. As coleções são contentores de documentos JSON e das lógicas das aplicações JavaScript associadas.

> [!WARNING]
> Criar uma coleção tem implicações ao nível dos preços, pois está a reservar débito para a aplicação comunicar com o Azure Cosmos DB. Para obter mais detalhes, visite a [página de preços](https://azure.microsoft.com/pricing/details/cosmos-db/)
>
>

```csharp
// Collection for device telemetry. Here the JSON property deviceId is used  
// as the partition key to spread across partitions. Configured for 2500 RU/s  
// throughput and an indexing policy that supports sorting against any  
// number or string property. .
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 2500 });
```

Este método faz uma chamada de API REST para o Azure Cosmos DB e o serviço aprovisiona um número de partições com base no débito pedido. Pode alterar o débito de uma coleção se tiver de aumentar o seu desempenho com o SDK ou o [Portal do Azure](set-throughput.md).

## <a id="CreateDoc"></a>Criar documentos JSON
Vamos inserir alguns documentos JSON no Azure Cosmos DB. Pode criar um [documento](sql-api-resources.md#documents) utilizando o método [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) da classe **DocumentClient**. Os documentos são conteúdos JSON (arbitrários) definidos pelo utilizador. Esta classe de exemplo contém uma leitura de dispositivo e uma chamada para CreateDocumentAsync para inserir uma leitura de dispositivo nova numa coleção.

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted
// as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```
## <a name="read-data"></a>Ler dados

Vamos ler o documento pela respetiva chave de partição e ID com o método ReadDocumentAsync. Tenha em atenção que as leituras incluem um valor de PartitionKey (correspondente ao cabeçalho do pedido `x-ms-documentdb-partitionkey` na API REST).

```csharp
// Read document. Needs the partition key and the Id to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"),
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a>Atualizar dados

Agora, vamos atualizar alguns dados através do método ReplaceDocumentAsync.

```csharp
// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"),
  reading);
```

## <a name="delete-data"></a>Eliminar dados

Agora, vamos eliminar um documento por chave de partição e ID através do método DeleteDocumentAsync.

```csharp
// Delete a document. The partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"),
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a>Consultar as coleções particionadas

Quando consulta dados em coleções particionadas, o Azure Cosmos DB encaminha automaticamente a consulta para as partições que correspondem aos valores das chaves de partições especificados no filtro (se existirem). Por exemplo, esta consulta é encaminhada para a partição que contém a chave de partição "XMS-0001".

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

A seguinte consulta não tem um filtro para a chave de partição (DeviceId) e é distribuída para todas as partições onde é executada no índice da partição. Tenha em atenção que tem de especificar EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` na API REST) para que o SDK execute uma consulta nas várias partições.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a>Execução paralela de consultas
Os SDKs de SQL 1.9.0 e acima do Azure Cosmos BD suportam opções de execução paralela de consultas, que lhe permitem fazer consultas de latência baixo em coleções particionadas, mesmo quando têm de alcançar um grande número de partições. Por exemplo, a seguinte consulta está configurada para ser executada em paralelo em várias partições.

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

Pode gerir a execução paralela da consulta ao otimizar os parâmetros abaixo:

* Ao definir `MaxDegreeOfParallelism`, pode controlar o nível de paralelismo, ou seja, o número máximo de ligações de rede simultâneas  às partições da coleção. Se definir este parâmetro como -1, o nível de paralelismo é gerido pelo SDK. Se `MaxDegreeOfParallelism` não for especificado ou for definido como 0, que é o valor predefinido, existirá uma única ligação de rede às partições da coleção.
* Ao definir `MaxBufferedItemCount`, pode compensar a latência da consulta e a utilização da memória do lado do cliente. Se omitir este parâmetro ou o definir como -1, o número de itens colocados em memória intermédia durante a execução paralela da consulta é gerido pelo SDK.

Tendo em conta o mesmo estado da coleção, uma consulta paralela devolverá resultados pela mesma ordem do que numa execução em série. Quando realizar uma consulta em várias partições que inclua ordenação (ORDER BY e/ou TOP), o SDK de SQL emite a consulta em paralelo para as várias partições e combina os resultados ordenados parcialmente no lado do cliente, de modo a produzir resultados ordenados globalmente.

## <a name="execute-stored-procedures"></a>Executar procedimentos armazenados
Por fim pode executar transações atómicas em documentos com o mesmo ID de dispositivo, por exemplo, se quiser manter agregados ou o último estado de um dispositivo num documento individual. Para tal, basta adicionar o código seguinte ao seu projeto.

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") },
    "XMS-001-FE24C");
```

E já está! São estes os principais componentes de uma aplicação do Azure Cosmos DB que utiliza uma chave de partição para dimensionar eficientemente a distribuição de dados em várias partições.  

## <a name="clean-up-resources"></a>Limpar recursos

Se não pretender continuar a utilizar esta aplicação, elimine todos os recursos criados neste tutorial no Portal do Azure com os seguintes passos:

1. No menu do lado esquerdo do Portal do Azure, clique em **Grupos de recursos** e, em seguida, clique no nome exclusivo do recurso que criou.
2. Na página do grupo de recursos, clique em **Eliminar**, escreva o nome do recurso a eliminar na caixa de texto e, em seguida, clique em **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, fez o seguinte:

> [!div class="checklist"]
> * Criámos uma conta do Azure Cosmos DB
> * Criou uma base de dados e uma coleção com uma chave de partição
> * Criou documentos JSON
> * Atualizou um documento
> * Consultou coleções particionadas
> * Executou um procedimento armazenado
> * Eliminou um documento
> * Eliminou uma base de dados

Agora, pode avançar para o próximo tutorial e importar dados adicionais para sua conta do Cosmos DB.

> [!div class="nextstepaction"]
> [Import data into Azure Cosmos DB](import-data.md) (Importar dados para o Azure Cosmos DB).
