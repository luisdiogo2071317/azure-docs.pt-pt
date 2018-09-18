---
title: Débito de aprovisionamento para o Azure Cosmos DB | Documentos da Microsoft
description: Saiba como configurar o débito aprovisionado para sua containsers, coleções, gráficos e tabelas do Azure Cosmos DB.
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: andrl
ms.openlocfilehash: 2da00f700f5cc234455cc686377e5863f1c35bdd
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734476"
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers-and-database"></a>Definir e obter o débito de contentores do Azure Cosmos DB e a base de dados

Pode definir o débito para um contentor do Azure Cosmos DB ou um conjunto de contentores com o portal do Azure ou ao utilizar os SDKs do cliente. 

**Débito de aprovisionamento para um contentor individual:** quando aprovisionar o débito para um conjunto de contentores, todos os contentores partilham o débito aprovisionado. Aprovisionamento de débito de contentores individuais irá garantir que a reserva de débito para esse contentor específico. Ao atribuir RU/seg ao nível do contentor individual, os contentores podem ser criados como *fixo* ou *ilimitado*. Os contentores de tamanho fixo têm um limite máximo de 10 GB e débito de 10 000 de RU/s. Para criar um contentor ilimitado, tem de especificar um débito mínimo de 1000 RU/s e uma [chave de partição](partition-data.md). Uma vez que os seus dados poderão ter de ser divididas em várias partições, é necessário escolher uma chave de partição que tenha uma cardinalidade elevada (100 para milhões de valores distintos). Ao selecionar uma chave de partição com muitos valores distintos, garante que seu contentor/tabela/gráfico e os pedidos podem ser dimensionados de maneira uniforme pelo Azure Cosmos DB. 

**Débito de aprovisionamento de um conjunto de contentores ou uma base de dados:** débito de aprovisionamento para uma base de dados permite que Compartilhe a taxa de transferência entre todos os contentores que pertencem a essa base de dados. Dentro de uma base de dados do Azure Cosmos DB, pode ter um conjunto de contentores que compartilha o débito, bem como os contentores, que tem dedicado a taxa de transferência. Ao atribuir RU/seg num conjunto de contentores, os contentores pertencentes a este conjunto são tratados como *ilimitado* contentores e tem de especificar uma chave de partição.

Com base no débito aprovisionado, Azure Cosmos DB irá alocar partições físicas para alojar os dados de contentor (es) e divisões/rebalances entre partições à medida que ele aumenta. Contentor e o aprovisionamento de débito de nível de base de dados são ofertas distintas e alterar entre qualquer um destes requerer a migrar dados de origem para destino. Que significa que precisa criar uma nova base de dados ou uma nova coleção e, em seguida, migrar dados, utilizando [biblioteca de executor em massa](bulk-executor-overview.md) ou [do Azure Data Factory](../data-factory/connector-azure-cosmos-db.md). A imagem seguinte ilustra o aprovisionamento de débito em níveis diferentes:

![Unidades de pedido de aprovisionamento para contentores individuais e o conjunto de contentores](./media/request-units/provisioning_set_containers.png)

As secções seguintes, irá aprender os passos necessários para configurar o débito em diferentes níveis de uma conta do Azure Cosmos DB. 

## <a name="provision-throughput-by-using-azure-portal"></a>Débito de aprovisionamento através do portal do Azure

### <a name="provision-throughput-for-a-container-collectiongraphtable"></a>Débito de aprovisionamento para um contentor (coleção/gráfico/tabela)

1. Inicie sessão no [portal do Azure](https://portal.azure.com).  
2. Na navegação à esquerda, selecione **todos os recursos** e localize a conta do Azure Cosmos DB.  
3. Pode configurar o débito ao criar um contentor (collection, gráfico, tabela) ou o débito de atualização para um contentor existente.  
4. Para atribuir o débito ao criar um contentor, abra a **Data Explorer** painel e selecione **nova coleção** (novo gráfico, nova tabela para outras APIs)  
5. Preencha o formulário **adicionar coleção** painel. Campos neste painel são descritos na tabela a seguir:  

   |**Definição**  |**Descrição**  |
   |---------|---------|
   |Id da base de dados  |  Forneça um nome exclusivo para identificar a sua base de dados. Base de dados é um contentor lógico de uma ou mais coleções. Os nomes das bases de dados devem conter de 1 a 255 carateres e não podem conter /, \\, #, ?, ou um espaço à direita. |
   |ID da coleção  | Forneça um nome exclusivo para identificar a sua coleção. Os IDs das coleções têm os mesmos requisitos em termos de carateres do que os nomes das bases de dados. |
   |Capacidade de armazenamento   | Este valor representa a capacidade de armazenamento da base de dados. Quando aprovisionar o débito de uma coleção individual, a capacidade de armazenamento pode ser **fixo (10 GB)** ou **ilimitado**. Capacidade de armazenamento ilimitada requer que defina uma chave de partição para os seus dados.  |
   |Débito   | Cada coleção e a base de dados podem ter débito em unidades de pedido por segundo.  Capacidade de armazenamento fixa, débito mínimo é de 400 unidades de pedido por segundo (RU/s), para armazenamento ilimitado mínimo, capacidade de débito é definido para 1000 RU/s.|

6. Depois de introduzir valores para estes campos, selecione **OK** para guardar as definições.  

   ![Definir débito de uma coleção](./media/set-throughput/set-throughput-for-container.png)

7. Para atualizar a taxa de transferência para um contentor existente, expanda a sua base de dados e o contentor e, em seguida, clique em **definições**. Na nova janela, escreva o novo valor de débito e, em seguida, selecione **guardar**.  

   ![Débito de atualização para uma coleção](./media/set-throughput/update-throughput-for-container.png)

### <a name="provision-throughput-for-a-set-of-containers-or-at-the-database-level"></a>Débito de aprovisionamento de um conjunto de contentores ou ao nível da base de dados

1. Inicie sessão no [portal do Azure](https://portal.azure.com).  
2. Na navegação à esquerda, selecione **todos os recursos** e localize a conta do Azure Cosmos DB.  
3. Pode configurar o débito durante a criação de um débito de base de dados ou a atualização para uma base de dados existente.  
4. Para atribuir a taxa de transferência durante a criação de uma base de dados, abra a **Data Explorer** painel e selecione **nova base de dados**  
5. Preencher o **id de base de dados** valor, verificação **débito aprovisionar** opção e configurar o valor de débito.  

   ![Definir débito com a nova opção de base de dados](./media/set-throughput/set-throughput-with-new-database-option.png)

6. Para atualizar um débito de base de dados existente, expanda a sua base de dados e o contentor e, em seguida, clique em **dimensionamento**. Na nova janela, escreva o novo valor de débito e, em seguida, selecione **guardar**.  

   ![Débito de atualização para uma base de dados](./media/set-throughput/update-throughput-for-database.png)

### <a name="provision-throughput-for-a-set-of-containers-as-well-as-for-an-individual-container-in-a-database"></a>Débito de aprovisionamento de um conjunto de contentores, bem como para um contentor individual numa base de dados

1. Inicie sessão no [portal do Azure](https://portal.azure.com).  
2. Na navegação à esquerda, selecione **todos os recursos** e localize a conta do Azure Cosmos DB.  
3. Crie uma base de dados e atribua o débito ao mesmo. Abra o **Data Explorer** painel e selecione **nova base de dados**  
4. Preencher o **id de base de dados** valor, verificação **débito aprovisionar** opção e configurar o valor de débito.  

   ![Definir débito com a nova opção de base de dados](./media/set-throughput/set-throughput-with-new-database-option.png)

5. Em seguida, crie uma coleção na base de dados que criou nos passos acima. Para criar uma coleção, clique com botão direito na base de dados e selecione **nova coleção**.  

6. Na **adicionar coleção** painel, introduza um nome para a coleção e chave de partição. Opcionalmente, pode aprovisionar o débito para esse contentor específico se optar por não atribuir um valor de débito, o débito atribuído à base de dados é partilhado na coleção.  

   ![Se desejar, defina o débito do contentor](./media/set-throughput/optionally-set-throughput-for-the-container.png)

## <a name="considerations-when-provisioning-throughput"></a>Considerações sobre o quando o aprovisionamento de débito

Seguem-se algumas considerações que ajudarão a decidir sobre a estratégia de reserva de débito.

### <a name="considerations-when-provisioning-throughput-at-the-database-level"></a>Considerações sobre o quando aprovisionar o débito ao nível da base de dados

Considere o aprovisionamento de débito no nível de base de dados (ou seja, para um conjunto de contentores) nos seguintes casos:

* Se tiver mais de uma dúzia ou número de contentores que possa compartilhar a taxa de transferência entre alguns ou todos eles.  

* Quando estiver a migrar de uma base de dados de inquilino único que foi concebido para executar em VMs de IaaS alojado ou no local (por exemplo, NoSQL ou bases de dados relacionais) para o Azure Cosmos DB e número de contentores.  

* Se pretender considerar o aumento não planeados de cargas de trabalho através da utilização de débito agrupado ao nível da base de dados.  

* Em vez de débito de definição num contentor individual, está interessado em obter o débito agregado num conjunto de contentores na base de dados.

### <a name="considerations-when-provisioning-throughput-at-the-container-level"></a>Considerações sobre o quando aprovisionar o débito no nível do contentor

Considere o aprovisionamento de débito com um contentor individual nos seguintes casos:

* Se tiver um número inferior de contentores do Azure Cosmos DB.  

* Se quiser obter a taxa de transferência garantida num contentor especificado apoiado pelo SLA.

<a id="set-throughput-sdk"></a>

## <a name="set-throughput-by-using-sql-api-for-net"></a>Definir débito com a API de SQL para .NET

### <a name="set-throughput-at-the-container-level"></a>Definir débito ao nível do contentor
Este é um trecho de código para criar um contentor com 3 000 unidades de pedido por segundo para um contentor individual com o SDK de .NET da API de SQL:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

### <a name="set-throughput-for-a-set-of-containers-at-the-database-level"></a>Definir débito para um conjunto de contentores ao nível da base de dados

Aqui está um trecho de código para o aprovisionamento de 100.000 unidades de pedido por segundo num conjunto de contentores com o SDK de .NET da API de SQL:

```csharp
// Provision 100,000 RU/sec at the database level. 
// sharedCollection1 and sharedCollection2 will share the 100,000 RU/sec from the parent database
// dedicatedCollection will have its own dedicated 4,000 RU/sec, independant of the 100,000 RU/sec provisioned from the parent database
Database database = await client.CreateDatabaseAsync(new Database { Id = "myDb" }, new RequestOptions { OfferThroughput = 100000 });

DocumentCollection sharedCollection1 = new DocumentCollection();
sharedCollection1.Id = "sharedCollection1";
sharedCollection1.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, sharedCollection1, new RequestOptions())

DocumentCollection sharedCollection2 = new DocumentCollection();
sharedCollection2.Id = "sharedCollection2";
sharedCollection2.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, sharedCollection2, new RequestOptions())

DocumentCollection dedicatedCollection = new DocumentCollection();
dedicatedCollection.Id = "dedicatedCollection";
dedicatedCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, dedicatedCollection, new RequestOptions { OfferThroughput = 4000 )
```

O Azure Cosmos DB opera sobre um modelo de reserva de débito. Ou seja, é-lhe cobrada para a quantidade de débito *reservado*, independentemente de quanto do que o débito é ativamente *utilizado*. Como seu aplicativo da alteração de padrões de carga, dados e da utilização pode facilmente aumentar e diminuir o número de RUs reservadas através de SDKs ou utilizando o [Portal do Azure](https://portal.azure.com).

Cada contentor, ou um conjunto de contentores, que é mapeado para um `Offer` recursos no Azure Cosmos DB, que tem metadados sobre o débito aprovisionado. Pode alterar o débito alocado ao procurar o recurso de oferta correspondente para um contentor, em seguida, atualizá-lo com o novo valor de débito. Aqui está um trecho de código para alterar o débito de um contentor para 5000 unidades de pedido por segundo, utilizando o SDK do .NET. Depois de alterar o débito, deve atualizar quaisquer janelas de portais do Azure existentes para o débito alterado sejam apresentados. 

```csharp
// Fetch the resource to be updated
// For a updating throughput for a set of containers, replace the collection's self link with the database's self link
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set the throughput to 5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

Não há nenhum impacto sobre a disponibilidade do seu contentor, ou um conjunto de contentores, quando altera a taxa de transferência. Normalmente, o novo débito reservado é aplicado dentro de segundos em aplicativos da taxa de transferência nova.

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>Para definir o débito ao utilizar a API de SQL para Java

O fragmento de código seguinte obtém o débito atual e altera-a para 500 RU/s. Para obter um exemplo de código completo, consulte a [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) ficheiros no GitHub. 

```Java
// find offer associated with this collection
// To change the throughput for a set of containers, use the database's resource id instead of the collection's resource id
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 500;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>Obtenção de débito através de métricas do portal de API do MongoDB

A forma mais simples para obter uma boa estimativa do pedido de unidades de cobrança para sua base de dados de API do MongoDB está a utilizar o [portal do Azure](https://portal.azure.com) métricas. Com o *número de pedidos* e *encargos de pedidos* gráficos, pode obter uma estimativa de quantas unidades de pedido cada operação está a consumir e quantas unidades de pedido que consomem em relação ao outro.

![Métricas do portal de API do MongoDB][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> Exceder os limites de débito reservado na API do MongoDB
Aplicações que excedem o débito aprovisionado para um contentor ou um conjunto de contentores será limitado taxa até que a taxa de consumo passa a ser inferior a taxa de débito aprovisionado. Quando ocorre uma limitação de taxa, o back-end irá terminar o pedido com um `16500` código de erro - `Too Many Requests`. Por predefinição, a API do MongoDB repete automaticamente até 10 vezes antes de retornar um `Too Many Requests` código de erro. Se estiver a receber muitas `Too Many Requests` códigos de erro, poderá considerar adicionar a lógica de repetição em rotinas de tratamento de erros do seu aplicativo ou [aumentar o débito aprovisionado para o contentor](set-throughput.md).

## <a id="GetLastRequestStatistics"></a>Obter os encargos de pedidos com o comando de GetLastRequestStatistics da API do MongoDB

A API do MongoDB suporta um comando personalizado, *getLastRequestStatistics*, para recuperar os custos de pedido a uma determinada operação.

Por exemplo, na shell do Mongo, execute a operação que pretende verificar a taxa de pedido.
```
> db.sample.find()
```

Em seguida, execute o comando *getLastRequestStatistics*.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Um método para estimar a quantidade de débito reservado exigida pela sua aplicação é registrar o custo da unidade de pedido associado à execução de operações típicas em relação a um item representativo utilizado pela sua aplicação e, em seguida, calcular o número de operações prevê para executar a cada segundo.

> [!NOTE]
> Se tiver de tipos de item que irão muito diferente em termos de tamanho e o número de propriedades indexadas, em seguida, registe as cobranças de unidades de pedido de operação aplicável associada a cada *tipo* de item de típico.
> 
> 

## <a name="throughput-faq"></a>FAQ de débito

**Pode definir minha taxa de transferência para menos de 400 RU/s?**

400 RU/s é o débito mínimo disponível nos contentores de partição única do Cosmos DB (1000 RU/s é o mínimo para contentores particionadas). Solicite unidades estiverem definidas em 100 intervalos de RU/s, mas a taxa de transferência não pode ser definida para 100 RU/s ou para qualquer valor inferior a 400 RU/s. Se estiver procurando por um método rentável de desenvolver e testar o Cosmos DB, pode utilizar gratuitamente [emulador do Azure Cosmos DB](local-emulator.md), que pode implementar localmente, sem qualquer custo. 

**Como posso definir débito com a API do MongoDB?**

Não existe nenhuma extensão de API do MongoDB para definir a taxa de transferência. A recomendação é usar a API de SQL, conforme mostrado na [para definir o débito ao utilizar a API de SQL para .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre a estimativa de unidades de débito e a pedido, consulte o artigo [solicitar unidades e do débito estimando no Azure Cosmos DB](request-units.md)

* Para saber mais sobre o aprovisionamento e de escala planetária contínuo com o Cosmos DB, veja [criação de partições e dimensionamento com o Cosmos DB](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png
