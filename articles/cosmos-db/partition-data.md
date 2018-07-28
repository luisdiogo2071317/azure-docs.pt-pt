---
title: Criação de partições e dimensionamento horizontal no Azure Cosmos DB | Documentos da Microsoft
description: Saiba mais sobre a criação de partições como funciona no Azure Cosmos DB, como configurar a criação de partições e chaves de partição e como escolher a chave de partição correta para a sua aplicação.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/26/2018
ms.author: rimman
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ba9982d1f63345db394f1803c31d4246cfac499c
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39309175"
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Particionar e dimensionar no Azure Cosmos DB

[O Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) é um serviço de base de dados com múltiplos modelos distribuída globalmente, concebido para ajudar a alcançar um desempenho rápido e previsível. Dimensiona-se perfeitamente, juntamente com a sua aplicação. Este artigo fornece uma visão geral de como funciona para todos os dados de criação de partições modelos no Azure Cosmos DB. Também descreve como configurar os contentores do Azure Cosmos DB para dimensionar as suas aplicações com eficiência.

Criação de partições e chaves de partição são discutidas neste vídeo:

> [!VIDEO https://www.youtube.com/embed/SS6WrQ-HJ30]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Criação de partições no Azure Cosmos DB
O Azure Cosmos DB fornece contentores para armazenar dados designados coleções (para documentos), gráficos ou tabelas. Os contentores são recursos lógicos e podem abranger uma ou mais partições físicas ou servidores. O número de partições é determinado pelo Azure Cosmos DB com base no tamanho de armazenamento e débito aprovisionado para um contentor ou um conjunto de contentores. 


### <a name="physical-partition"></a>Partição física

R *físico* partição é uma quantidade fixa de armazenamento reservada apoiadas por SSD combinada com a variável quantidade de recursos de computação (CPU e memória). Cada partição física é replicada para elevada disponibilidade. Cada conjunto de contentores pode partilhar uma ou mais partições físicas. Gestão de partição física totalmente gerido pelo Azure Cosmos DB, e não precisa escrever código complexo ou gerenciar suas partições. Contentores do Azure Cosmos DB são ilimitadas em termos de armazenamento e débito. Partições físicas são um conceito interno do Azure Cosmos DB e são transitórias. O Azure Cosmos DB irá dimensionar automaticamente o número de partições físicas com base na carga de trabalho. Portanto, não deve corelate seu design de banco de dados com base no número de partições físicas em vez disso, deve verificar se escolher a chave de partição correta que determina as partições lógicas. 

### <a name="logical-partition"></a>Partição lógica

R *lógico* partição é uma partição dentro de uma partição física que armazena todos os dados associados a um valor de chave de partição única. Várias partições lógicas podem ser armazenados na mesma partição física. O diagrama a seguir, um único contentor tem três partições lógicas. Cada partição lógica armazena os dados para uma chave de partição, LAX AMS e MEL, respetivamente. Cada uma das partições lógicas LAX AMS e MEL não pode crescer além do limite de partição lógica máximo de 10 GB. 

![A criação de partições de recursos](./media/introduction/azure-cosmos-db-partitioning.png) 

Quando um contentor cumpre os [pré-requisitos de criação de partições](#prerequisites), criação de partições é completamente transparente para seu aplicativo. O Azure Cosmos DB processa a distribuição de dados entre partições físicas e lógicas e o encaminhamento de pedidos de consulta para a partição correta. 

## <a name="how-does-partitioning-work"></a>Como funciona a criação de partições

Cada documento tem de ter uma *chave de partição* e uma *chave de linha*, que identifica de forma. A chave de partição age como uma partição lógica para os seus dados e fornece o Azure Cosmos DB com um limite de natural para a distribuição de dados pelas partições físicas. **Os dados para uma única partição lógica tem de residir no interior de uma única partição física e gestão de partição física é gerida pelo Azure Cosmos DB**. 

Em resumo, eis como criação de partições funciona no Azure Cosmos DB:

* Aprovisiona um conjunto de contentores do Azure Cosmos DB com **T** débito de RU/s (solicitações por segundo).  
* Em segundo plano, o Azure Cosmos DB Aprovisiona as partições físicas necessárias para atender **T** pedidos por segundo. Se **T** é maior do que o débito máximo por partição física **t**, em seguida, o Azure Cosmos DB provisões **N = T/t** partições físicas. O valor de débito máximo por partition(t) é configurado pelo Azure Cosmos DB, este valor é atribuído com base no débito aprovisionado total e a configuração de hardware utilizada.  
* O Azure Cosmos DB aloca uniformemente na chave hashes de espaço de chave de partição a **N** partições físicas. Para isso, o número de partições lógicas é de anfitriões de cada partição física **1/N** * número de valores de chave de partição.  
* Quando uma partição física **p** atingir o limite de armazenamento do Azure Cosmos DB perfeitamente divide **p** em duas novas partições físicas, **p1** e **p2**. Distribui valores que correspondem a aproximadamente metade das chaves para cada uma das novas partições físicas. Esta operação de divisão é completamente invisível para seu aplicativo. Se uma partição física atinge o limite de armazenamento e todos os dados na partição física pertence a mesma chave de partição lógica, a operação de divisão não ocorre. Isso ocorre porque todos os dados de uma chave única partição lógica tem de residir na mesma partição física. Neste caso, deve ser empregada uma estratégia de chave de partição diferente.  
* Quando aprovisionar o débito superior **t * N**, Azure Cosmos DB divide uma ou mais das suas partições físicas para suportar o débito mais elevado.

A semântica de chaves de partição é ligeiramente diferentes de acordo com a semântica de cada API, conforme mostrado na tabela a seguir:

| API | Chave de partição | Chave da fila |
| --- | --- | --- |
| SQL | Caminho da chave de partição personalizada | `id` corrigido | 
| MongoDB | Chave de partição horizontal personalizado  | `_id` corrigido | 
| Gremlin | Propriedade de chave de partição personalizada | `id` corrigido | 
| Tabela | `PartitionKey` corrigido | `RowKey` corrigido | 

O Azure Cosmos DB utiliza a criação de partições com base em hash. Quando escreve um item, o Azure Cosmos DB codifica o valor da chave de partição e utiliza o resultado codificado para determinar qual partição para armazenar o item. 

> [!NOTE]
> O Azure Cosmos DB armazena todos os itens com a mesma chave de partição na mesma partição física. 

## <a name="best-practices-when-choosing-a-partition-key"></a>Melhores práticas ao escolher uma chave de partição

A escolha da chave de partição é uma decisão importante que tenha de fazer em tempo de design. Escolha um nome de propriedade que tem uma vasta gama de valores e tem até mesmo os padrões de acesso. É recomendável ter uma chave de partição com um grande número de valores distintos (por exemplo, centenas ou milhares). Permite-lhe distribuir a carga de trabalho uniformemente por estes valores. Uma chave de partição ideal é aquele que aparece, com frequência, como um filtro em suas consultas e tem a cardinalidade suficiente para garantir a que sua solução é escalável.

Se uma partição física atinge o limite de armazenamento e os dados na partição tem a mesma chave de partição, o Azure Cosmos DB devolve os *"chave de partição atingido o tamanho máximo de 10 GB"* mensagem e a partição não está dividido. Escolher uma chave de partição seja segura é uma decisão muito importante. 

Escolha uma chave de partição, de modo que:

* A distribuição de armazenamento é mesmo entre todas as chaves.  
* A distribuição de volume de pedidos num determinado ponto no tempo é mesmo entre todas as chaves.  

  É uma boa idéia verificar a forma como os seus dados são distribuídos em várias partições. Para verificar a distribuição de dados no portal, aceda à sua conta do Azure Cosmos DB e clique em **métricas** na **monitorização** secção e, em seguida, clique em **armazenamento** guia para ver como seu os dados são particionados pelas partições físicas diferentes.

  ![A criação de partições de recursos](./media/partition-data/partitionkey-example.png)

  A imagem à esquerda acima mostra o resultado de uma chave de partição inválido e a imagem correta acima mostra o resultado, quando uma chave de partição seja segura foi escolhida. Imagem da esquerda, pode ver que os dados não são distribuídos uniformemente entre as partições. Deve se esforçar para escolher uma chave de partição que distribui os dados para que ele se parece com a imagem correta.

* Consultas que são invocadas com alta simultaneidade podem ser encaminhadas com eficiência, incluindo a chave de partição no predicado de filtro.  
* Escolher uma chave de partição com cardinalidade superior é geralmente preferido – becaue ele produz, tipicamente, melhor distribuição e a escalabilidade. Por exemplo, uma chave composta pode ser formada pela concatenação de valores de várias propriedades para aumentar a cardinalidade.  

Quando escolher uma chave de partição com acima considerações, não precisa se preocupar sobre o número de partições ou o débito é alocado por partição física, como o Azure Cosmos DB aumenta horizontalmente o número de partições físicas e também pode aumentar verticalmente a partições individuais conforme necessário.

<a name="prerequisites"></a>
## <a name="prerequisites-for-partitioning"></a>Pré-requisitos para a criação de partições

Contentores do Azure Cosmos DB podem ser criados como fixas ou ilimitado no portal do Azure. Os contentores de tamanho fixo têm um limite máximo de 10 GB e débito de 10 000 de RU/s. Para criar um contentor como ilimitado, tem de especificar uma chave de partição e um débito mínimo de 1000 RU/s. Contentores do Azure Cosmos DB também podem ser configurados para partilhar a taxa de transferência entre um conjunto de contentores, em que cada contentor deve especificar uma partição da chave e pode chegar ilimitado. Seguem-se os pré-requisitos a serem considerados para a criação de partições e dimensionamento:

* Ao criar um contentor (por exemplo, uma coleção, um gráfico ou uma tabela) no portal do Azure, selecione o **ilimitado** opção de capacidade de armazenamento para tirar partido do dimensionamento ilimitado. Para as partições físicas para auto-divisão em **p1** e **p2** conforme descrito na [como funciona a criação de partições](#how-does-partitioning-work), o contentor tem de ser criado com um débito de 1000 RU/s ou mais (ou partilha de débito num conjunto de contentores), e uma chave de partição tem de ser fornecida. 

* Se criou um contentor no portal do Azure ou através de programação e a taxa de transferência inicial foi 1000 RU/s ou mais e forneceu uma chave de partição, pode tirar partido do dimensionamento ilimitado sem alterações ao seu contentor. Isto inclui **Fixed** contentores, desde que o contentor inicial foi criado com, pelo menos, 1000 RU/s de débito e uma chave de partição for especificada.

* Todos os contentores configurados para partilhar a taxa de transferência como parte de um conjunto de contentores são tratados como **ilimitado** contentores.

Se tiver criado uma **Fixed** contentor sem qualquer partição chave ou débito menos de 1000 RU/s, o contentor será não de dimensionamento automático. Para migrar os dados a partir de um contentor fixo para um contentor ilimitado, tem de utilizar o [ferramenta de migração de dados](import-data.md) ou o [biblioteca de Feed de alterações](change-feed.md). 

## <a name="partitioning-and-provisioned-throughput"></a>Débito aprovisionado e criação de partições
O Azure Cosmos DB foi concebido para um desempenho previsível. Quando cria um contentor ou um conjunto de contentores, reservar o débito em termos de  *[unidades de pedido](request-units.md) (RU) por segundo*. Cada solicitação faz uma cobrança de RU é proporcional à quantidade de recursos do sistema, como CPU, memória e e/s consumida pela operação. Uma leitura de um documento de 1 KB com consistência de sessão consome 1 RU. Uma leitura é 1 RU, independentemente do número de itens armazenados ou o número de pedidos simultâneos em execução ao mesmo tempo. Itens maiores exigem RUs superior, dependendo do tamanho. Se souber o tamanho de suas entidades e o número de leituras, que precisa oferecer suporte para a sua aplicação, pode aprovisionar a quantidade exata de débito necessário para as necessidades da sua aplicação. 

> [!NOTE]
> Para utilizar totalmente o débito aprovisionado para um contentor ou um conjunto de contentores, tem de escolher uma chave de partição que permite-lhe distribuir uniformemente os pedidos em todos os valores de chave de partição diferente.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="create-partition-key"></a>Criar chave de partição 
Pode utilizar o portal do Azure ou a CLI do Azure para criar contentores e dimensione-os em qualquer altura. Esta secção mostra como criar contentores e especificar a chave de partição e o débito aprovisionada com cada API.


### <a name="sql-api"></a>SQL API
O exemplo a seguir mostra como criar um contentor (uma coleção) com a API de SQL. 

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Pode ler um item (documento), utilizando o `GET` método na API do REST ou utilizar `ReadDocumentAsync` em um dos SDKs.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

Para obter mais informações, consulte [criação de partições no Azure Cosmos DB com a API SQL](sql-api-partition-data.md).

### <a name="mongodb-api"></a>API do MongoDB
Com a API de MongoDB, pode criar uma coleção em partição horizontal através da sua ferramenta favorita, o driver ou o SDK. Neste exemplo, utilizamos o Shell de Mongo para criar uma coleção.

Na Shell do Mongo:

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Resultados:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a>API de Tabela

Para criar uma tabela com a API de tabela, utilize o `CreateIfNotExists` método. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

Débito aprovisionado é definido como um argumento de `CreateIfNotExists`. A chave de partição é criada implicitamente como o `PartitionKey` valor. 

Pode recuperar uma única entidade usando o seguinte código:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Para obter mais informações, consulte [desenvolver com a API de tabela](tutorial-develop-table-dotnet.md).

### <a name="gremlin-api"></a>API do Gremlin

Com a API do Gremlin, pode utilizar o portal do Azure ou a CLI do Azure para criar um contentor que representa um gráfico. Em alternativa, como o Azure Cosmos DB é com múltiplos modelo, pode utilizar uma das outros APIs para criar e dimensionar o seu contentor de gráficos.

É possível ler qualquer vértice ou de extremidade usando a chave de partição e ID no Gremlin. Por exemplo, para um gráfico com a região ("USA") como a chave de partição e o "Seattle" como a chave de linha, pode encontrar um vértice, utilizando a seguinte sintaxe:

```
g.V(['USA', 'Seattle'])
```

Pode referenciar uma extremidade com a chave de partição e a chave de linha.

```
g.E(['USA', 'I5'])
```

Para obter mais informações, consulte [usando um gráfico de particionadas no Azure Cosmos DB](graph-partitioning.md).

## <a name="composite-partition-key"></a>Chave de partição compostos

Pode formar uma chave de partição compostos por concatenação e preenchimento vários valores de propriedade numa propriedade de único artificiais "partitionKey" do item.

Por exemplo, tem um documento que é semelhante a:

```json
{
"deviceId": "abc-123",
"date": 2018
}
```

Uma opção é definir partitionKey /deviceId ou /date. Se desejar formar uma chave composta no id de dispositivo e data. Concatenar esses dois valores numa propriedade de artificial "partitionKey" e defina a chave de partição para /partitionKey.

```json
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

Em cenários em tempo real pode ter milhares de documentos para que deve definir a lógica do lado do cliente para concatenar os valores numa chave composta, inserir a chave composta os documentos e, em seguida, utilizá-la para especificar a chave de partição.

<a name="designing-for-scale"></a>
## <a name="design-for-scale"></a>Conceba para reduzir horizontalmente
Para dimensionar com eficiência com o Azure Cosmos DB, terá de escolher uma chave de partição seja segura quando criar o seu contentor. Existem duas considerações principais para escolher uma chave de partição seja segura:

* **Consultar os limites e transações**. À sua escolha da chave de partição deve equilibrar a necessidade de usar transações de acordo com o requisito para distribuir as suas entidades através de várias chaves de partição para garantir uma solução dimensionável. De um lado, pode definir a mesma chave de partição para todos os seus itens, mas esta opção pode limitar a escalabilidade da sua solução. No outro extremo, pode atribuir uma chave de partição exclusiva para cada item. Esta opção é altamente dimensionável, mas ele impede a utilização de transações entre documentos por meio de procedimentos armazenados e acionadores. Uma chave de partição ideal permite-lhe utilizar consultas eficientes e tem a cardinalidade suficiente para garantir a que sua solução é escalável. 
* **Não existem afunilamentos de desempenho e armazenamento**. É importante escolher uma propriedade que permite escritas ser distribuídos por vários valores distintos. Pedidos para a mesma chave de partição não podem exceder o débito aprovisionado alocado a uma partição e serão limitado de taxa. Portanto, é importante escolher uma chave de partição não resulta na "pontos de acesso" na sua aplicação. Uma vez que todos os dados de uma chave de partição única devem ser armazenados dentro de uma partição, deve evitar chaves de partição com grandes volumes de dados para o mesmo valor. 

Vamos dar uma olhada em alguns cenários reais e chaves de partição seja segura para cada:
* Se estiver a implementar um back-end de perfil de utilizador, o *ID de utilizador* é uma boa opção para uma chave de partição.
* Se estiver armazenando dados de IoT, por exemplo, estado do dispositivo, uma *ID de dispositivo* é uma boa opção para uma chave de partição.
* Se estiver a utilizar o Azure Cosmos DB para registro em log os dados de séries de tempo, o *hostname* ou *processar o ID de* é uma boa opção para uma chave de partição.
* Se tiver uma arquitetura de multi-inquilino, o *ID de inquilino* é uma boa opção para uma chave de partição.

Em alguns casos, como perfis de utilizador e de IoT de utilização, a chave de partição pode ser o mesmo que sua *ID* (chave de documento). Em outros, como os dados de séries de tempo, pode ter uma chave de partição diferente da *ID*.

### <a name="partitioning-and-loggingtime-series-data"></a>Dados de criação de partições e de registo/séries de tempo
Um dos casos de utilização comuns no Azure Cosmos DB é o Registro em log e telemetria. É importante escolher uma chave de partição boa neste cenário, uma vez que poderá ter de grandes volumes de dados de leitura/escrita. A opção para uma chave de partição depende das tarifas de leitura e escrita e os tipos de consultas que devem ser executados. Eis algumas sugestões sobre como escolher uma chave de partição seja segura:

* Se seu caso de utilização envolve uma pequena taxa de gravações que acumular durante muito tempo e precisa consultar por intervalos de carimbos de data / com outros filtros, utilize um rollup do período de tempo. Por exemplo, uma boa abordagem é usar a data como uma chave de partição. Com esta abordagem, pode consultar ao longo de todos os dados para uma determinada data de uma única partição. 
* Se a carga de trabalho é escrita intensiva, que é muito comum neste cenário, utilize uma chave de partição que não se baseia o carimbo de hora. Como tal, o Azure Cosmos DB pode distribuir e dimensionar escritas de forma uniforme entre várias partições. Aqui uma *hostname*, *processar o ID*, *ID de atividade*, ou outra propriedade com cardinalidade elevada é uma boa opção. 
* Outra abordagem é uma abordagem híbrida, em que tem vários contentores, um para cada dia/mês, e a chave de partição é uma propriedade mais granular, como *hostname*. Esta abordagem tem o benefício que pode definir o débito diferente para cada contentor ou um conjunto de contentores com base na janela de tempo e as necessidades de dimensionamento e desempenho. Por exemplo, um contentor para o mês atual pode ser aprovisionado com um débito mais elevado, uma vez que ele serve leituras e escritas. Meses anteriores podem ser aprovisionados com um débito mais baixo, porque eles apenas servem leituras.

### <a name="partitioning-and-multitenancy"></a>Criação de partições e de arquitetura "multitenancy"
Se estiver a implementar uma aplicação multi-inquilino com o Azure Cosmos DB, existem dois designs populares a serem considerados: *chave de uma partição por inquilino* e *um contentor por inquilino*. Aqui estão os prós e contras de cada:

* **Chave de uma partição por inquilino**. Nesse modelo, os inquilinos são colocalizados dentro de um único contentor. Consultas e inserções de um único inquilino podem ser executadas em relação a uma única partição. Também pode implementar a lógica transacional em todos os itens que pertencem a um inquilino. Uma vez que vários inquilinos partilham um contentor, pode utilizar melhor armazenamento e débito aprovisionado agrupamento de recursos para todos os inquilinos dentro de um único contentor, em vez de aprovisionamento para cada inquilino. A desvantagem é que não tem de isolamento de desempenho por inquilino. Cada vez maior débito para garantir o desempenho será aplicada para o contentor inteiro com todos os inquilinos versus aumentos de destinados para um inquilino individual.
* **Um contentor por inquilino**. Nesse modelo, cada inquilino tem seu próprio contentor e, pode reservar débito com desempenho garantido por inquilino. Este modelo é mais rentável para aplicações multi-inquilino com alguns inquilinos.

Também pode usar uma abordagem híbrida que colocates inquilinos pequenos em conjunto e isola as maiores inquilinos para os seus próprios contentores.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, fornecemos uma visão geral dos conceitos e práticas recomendadas para o dimensionamento e a criação de partições no Azure Cosmos DB. 

* Saiba mais sobre [débito aprovisionado no Azure Cosmos DB](request-units.md).
* Saiba mais sobre [distribuição global no Azure Cosmos DB](distribute-data-globally.md).



