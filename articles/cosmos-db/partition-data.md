---
title: Criação de partições e o dimensionamento horizontal do BD Azure Cosmos | Microsoft Docs
description: Saiba mais sobre funciona como criação de partições de BD do Cosmos do Azure, como configurar a criação de partições e chaves de partição e como escolher a chave de partição adequado para a sua aplicação.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3fe2dbab876d1ef55ff05315cf7c823d0444663a
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808677"
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Partição e o dimensionamento do BD Azure Cosmos

[BD do Azure do Cosmos](https://azure.microsoft.com/services/cosmos-db/) é um serviço de base de dados globalmente distribuída e múltiplos modelo concebido para ajudar a alcançar um desempenho previsível e rápido. -Dimensiona de forma totalmente integrada, juntamente com a sua aplicação à medida que o que aumenta. Este artigo fornece uma descrição geral de como funciona para todos os dados de criação de partições modelos do BD Azure Cosmos. Também descreve como pode configurar a base de dados do Azure Cosmos contentores de forma eficaz dimensionar as suas aplicações.

Divisão em partições e chaves de partição são debatidas neste vídeo:

> [!VIDEO https://www.youtube.com/embed/SS6WrQ-HJ30]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>A criação de partições no Azure Cosmos DB
BD do Cosmos do Azure, pode armazenar e consultar dados sem esquema com uma latência de milissegundo único dígito em qualquer escala. BD do Azure do Cosmos fornece contentores para armazenar dados denominado *coleções* (para documentos), *gráficos*, ou *tabelas*. 

Contentores são recursos lógicos e podem abranger uma ou mais partições físicas ou servidores. O número de partições é determinado pela base de dados do Cosmos do Azure com base no tamanho de armazenamento e débito aprovisionado para um contentor ou um conjunto de contentores. 

A *físico* partição é uma quantidade fixa de armazenamento SSD segurança reservada combinada com variável quantidade de recursos de computação (CPU e memória). Cada partição física é replicada para elevada disponibilidade. Cada conjunto de contentores pode partilhar uma ou mais partições físicas. Gestão de partição física é completamente geridos pela base de dados do Azure Cosmos e não tem de escrever código complexo ou gerir as partições. Contentores do Cosmos BD do Azure são ilimitados em termos de armazenamento e débito. 

A *lógica* é uma partição dentro de uma partição física que armazena todos os dados associados a um valor de chave de partição única. Várias partições lógicas podem ficar na mesma partição física. O diagrama a seguir, um único contentor tem três partições lógicas. Cada partição lógica armazena os dados para uma chave de partição, LAX, AMS e MEL respetivamente. Cada uma das partições lógicas LAX, AMS e MEL não é possível crescer além do limite a partição lógica máximo de 10 GB. 

![A criação de partições de recursos](./media/introduction/azure-cosmos-db-partitioning.png) 

Quando um contentor cumpre o [pré-requisitos de criação de partições](#prerequisites), criação de partições é completamente transparente para a aplicação. BD do Azure do Cosmos suporta rápidas leituras e escritas, consultas, lógica transacional, níveis de consistência e controlo de acesso detalhada através de métodos/APIs a um recurso de contentor único. O serviço processa distribui dados em partições físicas e lógicas e o encaminhamento de pedidos de consulta para a partição à direita. 

## <a name="how-does-partitioning-work"></a>Como funciona a criação de partições

Como funciona a criação de partições? Cada item tem de ter um *chave de partição* e um *chave de linha*, que identificá-lo exclusivamente. A chave de partição atua como uma partição lógica para os seus dados e fornece BD do Cosmos do Azure com um limite natural para distribui dados em partições físicas. Os dados para uma única partição lógica têm de residir no interior de uma única partição física, mas a gestão de partição física é gerida pelo Azure Cosmos DB. 

No brief, eis como criação de partições funciona do BD Azure Cosmos:

* Aprovisionar um conjunto de contentores de base de dados do Azure Cosmos com **T** débito RU/s (pedidos por segundo).
* Nos bastidores, a base de dados do Azure Cosmos Aprovisiona partições físicas necessárias para servir **T** pedidos por segundo. Se **T** é superior ao débito máximo por partição física **t**, em seguida, a base de dados do Azure Cosmos Aprovisiona **N = T/t** partições físicas. O valor de débito máximo por partition(t) é configurado pelo Azure Cosmos DB, este valor está atribuído com base no débito aprovisionado total e a configuração de hardware utilizada. 
* BD do Azure do Cosmos aloca uniformemente across hashes de chaves de espaço de chave de partição a **N** partições físicas. Assim, cada físico anfitriões de partição **1/N** (partições lógicas) de valores de chave de partição.
* Quando uma partição física **p** atingir o limite de armazenamento, base de dados do Azure Cosmos perfeitamente divide **p** em duas partições físicas novo, **p1** e **p2**. Distribui-lo valores que correspondem a aproximadamente metade das chaves para cada uma das partições físicas novo. Esta operação de divisão é completamente invisível para a aplicação. Se uma partição física atinge o limite de armazenamento e todos os dados na partição física pertence a mesma chave de partição lógica, a operação de divisão não ocorrer. Isto acontece porque todos os dados para uma chave de partição lógica única tem de residir na mesma partição física. Neste caso, deve ser utilizada uma estratégia de chave de partição diferentes.
* Quando aprovisionar o débito superior **t * N**, base de dados do Azure Cosmos divide uma ou mais das suas partições físicas para suportar um maior débito.

A semântica para as chaves de partição é ligeiramente diferente para corresponder a semântica de cada API, conforme mostrado na seguinte tabela:

| API | Chave de partição | Chave da fila |
| --- | --- | --- |
| SQL | caminho da chave de partição personalizado | `id` corrigido | 
| MongoDB | chave de partição horizontal personalizado  | `_id` corrigido | 
| Gremlin | propriedade de chave de partição personalizado | `id` corrigido | 
| Tabela | `PartitionKey` corrigido | `RowKey` corrigido | 

BD do Azure do Cosmos utiliza a criação de partições com base em hash. Quando escreve um item, base de dados do Azure Cosmos codifica o valor da chave de partição e utiliza o resultado com hash para determinar que partição que pretende armazenar no item. BD do Azure do Cosmos armazena todos os itens com a mesma chave de partição na mesma partição física. 

A escolha da chave de partição é uma decisão importante que terá de fazer no momento da concepção. Escolha um nome de propriedade que tenha uma vasta gama de valores e tem o mesmo padrões de acesso. É uma melhor prática de ter uma chave de partição com um grande número de valores distintos (por exemplo, centenas ou milhares). Permite-lhe distribuir a carga de trabalho uniformemente em todos estes valores. Uma chave de partição ideal é aquele que aparece frequentemente como um filtro em consultas e tem cardinalidade suficiente para garantir a sua solução escalável.

Se uma partição física atinge o limite de armazenamento e os dados na partição com a mesma chave de partição, base de dados do Azure Cosmos devolve o *"chave de partição atingido o tamanho máximo de 10 GB"* mensagem e a partição não é dividida. Escolher uma chave de partição é uma decisão muito importante. As partições são um conceito interno da base de dados do Azure Cosmos e são transitórias. Não é bastante correto pressuposto de partições quantos são alocadas um débito de determinadas. BD do Azure do Cosmos será automaticamente dimensionado partições com base na sua carga de trabalho. Para que não deve corelate a estrutura de base de dados com base no número de partições em vez disso, deve certificar-se escolher a chave de partição adequado. 

Escolha uma chave de partição que:

* A distribuição de dados é mesmo em todas as chaves.
* A carga de trabalho é mesmo em todas as chaves.
* É preferencial para ter um conjunto de chaves como chaves de partição que uma chave única.  Número mais elevado de chaves de fazer uma distribuição de carga de trabalho mesmo.

Ao escolher uma chave de partição com acima considerações, não terá de se preocupar com o número de partições ou que débito atribuído por partição física, à medida que dimensionamos cada partição de forma independente e de forma linear conforme necessário.

Contentores do Cosmos BD do Azure podem ser criados como *fixo* ou *ilimitados* no portal do Azure. Os contentores de tamanho fixo têm um limite máximo de 10 GB e débito de 10 000 de RU/s. Para criar um contentor como ilimitado, tem de especificar uma chave de partição e um débito mínimo de 1.000 RU/s. Contentores do Cosmos BD do Azure também podem ser configurados para partilhar débito entre um conjunto de contentores, em que cada contentor deve specificy uma partição de chave e pode crescer ilimitado.

É uma boa ideia confirmar a forma como os seus dados são distribuídos em partições. Para verificar a distribuição de dados no portal, aceda à sua conta de base de dados do Azure Cosmos e clique em **métricas** no **monitorização** secção e, em seguida, clique em **armazenamento** separador para ver como o dados estão particionados em partições físicas diferentes.

![A criação de partições de recursos](./media/partition-data/partitionkey-example.png)

A imagem à esquerda acima mostra o resultado de uma chave de partição inválido e a imagem correta acima mostra o resultado quando uma chave de partição foi escolhida. A imagem à esquerda, pode ver que os dados não são distribuídos uniformemente entre as partições. Deve esforçar-nos escolher uma chave de partição que distribui os seus dados para procura semelhante para a imagem correta.

<a name="prerequisites"></a>
## <a name="prerequisites-for-partitioning"></a>Pré-requisitos para a criação de partições

Para partições físicas a divisão de automático para **p1** e **p2** conforme descrito em [como funciona o trabalho de criação de partições](#how-does-partitioning-work), o contentor tem de ser criado com um débito de 1.000 RU/s ou mais (ou partilha de débito por um conjunto de contentores), e uma chave de partição tem de ser fornecida. Ao criar um contentor (por exemplo, uma coleção, um gráfico ou uma tabela) no portal do Azure, selecione o **ilimitada** opção de capacidade de armazenamento para tirar partido do dimensionamento ilimitado. 

Se criou um contentor no portal do Azure ou através de programação e o débito inicial foi 1.000 RU/s ou mais e forneceu uma chave de partição, pode tirar partido de dimensionamento ilimitado sem alterações para o contentor. Isto inclui **Fixed** contentores, por isso, desde o contentor inicial foi criado com, pelo menos, 1000 RU/s de débito e uma chave de partição não está especificada.

Todos os contentores configurados para partilhar o débito como parte de um conjunto de contentores são tratados como **ilimitada** contentores.

Se tiver criado uma **Fixed** contentor com nenhuma chave de partição ou débito com menos de 1000 RU/s, o contentor serão não dimensionamento automático, tal como descrito neste artigo. Para migrar os dados a partir de um contentor fixo para um contentor ilimitado (por exemplo, um com, pelo menos, 1000 RU/s e uma chave de partição), tem de utilizar o [ferramenta de migração de dados](import-data.md) ou [biblioteca de Feeds de alteração](change-feed.md). 

## <a name="partitioning-and-provisioned-throughput"></a>Débito aprovisionado e criação de partições
BD do Azure do Cosmos foi concebida para um desempenho previsível. Quando cria um contentor ou um conjunto de contentores, reservar o débito em termos de  *[unidades de pedido](request-units.md) (RU) por segundo*. Cada pedido faz com que um custo de RU é proporcional para a quantidade de recursos do sistema, tais como CPU, memória e consumidas pela operação de e/s. Uma leitura de um documento de 1 KB com consistência de sessão consome 1 RU. Uma leitura é 1 RU independentemente do número de itens armazenados ou o número de pedidos simultâneos em execução ao mesmo tempo. Itens de maior requerem RUs superiores, dependendo do tamanho. Se souber o tamanho do seu entidades e o número de leituras que tiver de suportar para a sua aplicação, pode aprovisionar a quantidade exata de débito necessário para as necessidades da sua aplicação. 

> [!NOTE]
> Para utilizar totalmente débito aprovisionado para um contentor ou um conjunto de contentores, tem de escolher uma chave de partição permite-lhe distribuir uniformemente pedidos entre todos os valores de chave de partição distintos.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="work-with-the-azure-cosmos-db-apis"></a>Trabalhar com o Azure Cosmos APIs de base de dados
Pode utilizar o portal do Azure ou a CLI do Azure para criar contentores e dimensioná-los em qualquer altura. Esta secção mostra como criar contentores e especificar a chave de partição e débito aprovisionada com cada API.


### <a name="sql-api"></a>SQL API
O exemplo seguinte mostra como criar um contentor (uma coleção) utilizando a API do SQL Server. 

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

Pode ler um item (documento), utilizando o `GET` método a REST API ou utilizar `ReadDocumentAsync` dos SDKs.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

Para obter mais informações, consulte [criação de partições na base de dados do Cosmos do Azure utilizando a API do SQL Server](sql-api-partition-data.md).

### <a name="mongodb-api"></a>API do MongoDB
Com a API do MongoDB, pode criar uma coleção em partição horizontal através da sua ferramenta favorita, controlador ou SDK. Neste exemplo, utilizamos a Shell do Mongo para criar uma coleção.

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

Para criar uma tabela utilizando a API de tabela, utilize o `CreateIfNotExists` método. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

Débito aprovisionado está definido como um argumento de `CreateIfNotExists`. A chave de partição implicitamente é criada como o `PartitionKey` valor. 

Pode obter uma única entidade utilizando o seguinte código:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Para obter mais informações, consulte [desenvolver com a API de tabela](tutorial-develop-table-dotnet.md).

### <a name="gremlin-api"></a>API do Gremlin

Com a API de Gremlin, pode utilizar o portal do Azure ou a CLI do Azure para criar um contentor que representa um gráfico. Em alternativa, como base de dados do Azure Cosmos é múltiplos modelo, pode utilizar uma das outros APIs para criar e dimensionar o contentor do gráfico.

Pode ler qualquer vértice ou o Microsoft edge, utilizando a chave de partição e o ID no Gremlin. Por exemplo, para um gráfico com a região ("EUA") como a chave de partição e "Seattle" como a chave de linha, pode encontrar um vértice utilizando a seguinte sintaxe:

```
g.V(['USA', 'Seattle'])
```

Pode referenciar um limite utilizando a chave de partição e a chave de linha.

```
g.E(['USA', 'I5'])
```

Para obter mais informações, consulte [através de um gráfico particionado do BD Azure Cosmos](graph-partitioning.md).


<a name="designing-for-scale"></a>
## <a name="design-for-scale"></a>Estrutura de escala
Para dimensionadas de forma eficaz com BD do Cosmos do Azure, terá de escolher uma chave de partição ao criar o contentor. Existem duas principais considerações sobre a escolha de uma chave de partição:

* **Consultar o limite e transações**. À sua escolha de chave de partição deve balancear a necessidade de utilizar as transações com o requisito para distribuir as entidades por várias chaves de partição para garantir uma solução dimensionável. Em uma Alpine, pode definir a mesma chave de partição para todos os seus itens, mas esta opção pode limitar a escalabilidade da sua solução. Em outra Alpine, pode atribuir uma chave de partição exclusiva para cada item. Esta opção é altamente dimensionável, mas impede-o de utilizando transações entre documentos através de acionadores e procedimentos armazenados. Uma chave de partição ideal permite-lhe utilizar consultas eficiente e tem cardinalidade suficiente para garantir a sua solução escalável. 
* **Nenhum congestionamentos de desempenho e armazenamento**. É importante escolher uma propriedade que lhe permite escritas ser distribuída em vários valores distintos. Pedidos para a mesma chave de partição não podem exceder o débito aprovisionado atribuído a uma partição e serão limitado de taxa. Por isso, é importante escolher uma chave de partição não resulta num "frequente oportunidades" na sua aplicação. Uma vez que todos os dados para uma chave de partição única tem de ser armazenados dentro de uma partição, deve evitar chaves de partição com elevada volumes de dados para o mesmo valor. 

Vamos ver alguns cenários no mundo real e chaves de partição para cada:
* Se estiver a implementar um back-end de perfil de utilizador, o *ID de utilizador* é uma boa opção para uma chave de partição.
* Se estiver a armazenar dados de IoT, por exemplo, estado do dispositivo, um *ID de dispositivo* é uma boa opção para uma chave de partição.
* Se estiver a utilizar o Azure Cosmos DB para o registo de dados de séries de tempo, o *hostname* ou *processar o ID de* é uma boa opção para uma chave de partição.
* Se tiver uma arquitetura multi-inquilino, a *ID de inquilino* é uma boa opção para uma chave de partição.

Em alguns casos, como perfis de utilizador e de IoT de utilização, a chave de partição pode ser idêntica à sua *ID* (chave do documento). Em outras pessoas, como os dados de séries de tempo, poderá ter uma chave de partição que é diferente do *ID*.

### <a name="partitioning-and-loggingtime-series-data"></a>Data de criação de partições e de registo /-série de tempo da
Um dos casos de utilização comuns do BD Azure Cosmos é o registo de telemetria. É importante escolher uma chave de partição neste cenário, porque poderá ter de leitura/escrita vasta volumes de dados. A escolha de uma chave de partição depende as taxas de leitura e escrita e os tipos de consultas que pretende executar. Eis algumas sugestões sobre como escolher uma chave de partição:

* Se o caso de utilização envolve uma taxa pequena de gravações que acumular durante muito tempo e tem de consultar por intervalos de carimbos com outros filtros, utilize um rollup do carimbo. Por exemplo, uma boa abordagem é utilizar o data como uma chave de partição. Com esta abordagem, pode consultar ao longo de todos os dados para uma determinada data a partir de uma única partição. 
* Se a carga de trabalho é escrita extremamente encriptados, que é muito comum neste cenário, utilize uma chave de partição não é baseada no carimbo. Como tal, base de dados do Azure Cosmos pode distribuir e aumentar uniformemente escritas em várias partições. Aqui um *hostname*, *processar o ID de*, *ID da atividade*, ou outra propriedade com cardinalidade elevada é uma boa opção. 
* Outra abordagem é uma abordagem de híbrida, onde tem vários contentores, um para cada dia/mês, e a chave de partição é uma propriedade mais granular como *hostname*. Esta abordagem tem a vantagem que pode definir débito diferente para cada contentor ou um conjunto de contentores com base na janela de tempo e as necessidades de dimensionamento e desempenho. Por exemplo, um contentor para o mês atual pode ser aprovisionado com um débito mais elevado, porque desempenha leituras e escritas. Meses anteriores podem ser aprovisionados com um débito inferior, porque apenas servem leituras.

### <a name="partitioning-and-multitenancy"></a>Criação de partições e arquitetura "multitenancy"
Se estiver a implementar uma aplicação multi-inquilino através da base de dados do Azure Cosmos, existem dois estruturas populares a ter em consideração: *chave de uma partição por inquilino* e *um contentor por inquilino*. Seguem-se a profissionais e contras para cada:

* **Chave de uma partição por inquilino**. Neste modelo, os inquilinos são colocalizados dentro de um único contentor. As consultas e inserções para um único inquilino podem executar uma única partição. Também pode implementar a lógica transacional em todos os itens que pertençam a um inquilino. Porque um contentor de partilha de múltiplos inquilinos, melhor pode utilizar armazenamento e débito aprovisionado pelo agrupamento de recursos para todos os inquilinos dentro de um único contentor, em vez de aprovisionamento para cada inquilino. A desvantagem é que não tem o isolamento de desempenho por inquilino. Débito crescente para garantir o desempenho será aplicada para o contentor inteiro com todos os inquilinos versus aumenta direcionado para um inquilino individual.
* **Um contentor por inquilino**. Neste modelo, cada inquilino tem a suas próprias contentor e pode reservar débito com um desempenho garantido por inquilino. Este modelo é mais económico para aplicações multi-inquilino com alguns inquilinos.

Também pode utilizar uma abordagem de híbridos que colocates inquilinos pequenos em conjunto e isola inquilinos maior para os seus próprios contentores.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, fornecemos-lhe uma descrição geral dos conceitos e procedimentos recomendados para dimensionamento e criação de partições do BD Azure Cosmos. 

* Saiba mais sobre [débito aprovisionado do BD Azure Cosmos](request-units.md).
* Saiba mais sobre [distribuição global do BD Azure Cosmos](distribute-data-globally.md).



