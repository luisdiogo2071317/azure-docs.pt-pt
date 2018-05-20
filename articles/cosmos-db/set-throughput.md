---
title: Débito de aprovisionamento para a base de dados do Azure Cosmos | Microsoft Docs
description: Saiba como definir o débito de base de dados do Azure Cosmos containsers, coleções, gráficos e tabelas.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: sngun
ms.openlocfilehash: 925167c6b4a7f173726ec094c2847a16ca3d0ef4
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers-and-database"></a>Definir e obter débito para contentores de base de dados do Azure Cosmos e base de dados

Pode definir débito para um contentor de base de dados do Azure Cosmos ou um conjunto de contentores utilizando o portal do Azure ou ao utilizar o SDKs do cliente. Quando aprovisionar débito para um conjunto de contentores, todos os contentores desses partilham o débito aprovisionado. Débito de aprovisionamento para contentores individuais irá garantir que a reserva de débito para esse contentor específico. Por outro lado, débito para uma base de dados de aprovisionamento permite-lhe partilhar o débito entre todos os contentores que pertencem a essa base de dados. Dentro de uma base de dados do Azure Cosmos DB, pode ter um conjunto de contentores que partilham o débito, bem como contentores, que tem dedicado débito. 

Com base no débito aprovisionado, BD do Cosmos Azure irá alocar partições físicas para alojar os dados de contentores e divisões/rebalances em partições que cresce-lo.

Ao atribuir RU/seg ao nível do contentor individuais, os contentores podem ser criados como *fixo* ou *ilimitados*. Os contentores de tamanho fixo têm um limite máximo de 10 GB e débito de 10 000 de RU/s. Para criar um contentor ilimitado, tem de especificar um débito mínimo de 1.000 RU/s e uma [chave de partição](partition-data.md). Uma vez que os dados poderão ter para serem divididas entre várias partições, é necessário escolher uma chave de partição tem uma cardinalidade elevada (100 para milhões de valores distintos). Ao selecionar uma chave de partição com vários valores distintos, certifique-se de que a tabela/contentor/gráfico e pedidos podem ser escalados uniformemente por base de dados do Azure Cosmos. 

Ao atribuir RU/seg através de um conjunto de contentores, os contentores que pertencem a este conjunto são tratados como *ilimitados* contentores e tem de especificar uma chave de partição.

![Unidades de pedido de contentores individuais e o conjunto de contentores de aprovisionamento](./media/request-units/provisioning_set_containers.png)

Este artigo explica os passos necessários para configurar o débito de diferentes níveis de uma conta de base de dados do Azure Cosmos. 

## <a name="provision-throughput-by-using-azure-portal"></a>Débito de aprovisionar utilizando o portal do Azure

### <a name="provision-throughput-for-a-container-collectiongraphtable"></a>Débito de aprovisionar para um contentor (coleção/gráfico/tabela)

1. Inicie sessão no [portal do Azure](https://portal.azure.com).  
2. Na navegação esquerda, selecione **todos os recursos** e localize a conta de base de dados do Azure Cosmos.  
3. Pode configurar o débito ao criar um contentor (coleção, gráfico, tabela) ou o débito de atualização para um contentor existente.  
4. Para atribuir débito ao criar um contentor, abra o **Explorador de dados** painel e selecione **nova coleção** (novo gráfico, nova tabela para outras APIs)  
5. Preencha o formulário **adicionar coleção** painel. Campos neste painel são descritos na tabela seguinte:  

   |**Definição**  |**Descrição**  |
   |---------|---------|
   |Id da base de dados  |  Forneça um nome exclusivo para identificar a sua base de dados. Base de dados é um contentor lógico de uma ou mais coleções. Os nomes das bases de dados devem conter de 1 a 255 carateres e não podem conter /, \\, #, ?, ou um espaço à direita. |
   |ID da coleção  | Forneça um nome exclusivo para identificar a sua coleção. Os IDs das coleções têm os mesmos requisitos em termos de carateres do que os nomes das bases de dados. |
   |Capacidade de armazenamento   | Este valor representa a capacidade de armazenamento da base de dados. Quando aprovisionar débito para uma coleção individuais, capacidade de armazenamento pode ser **fixo (10 GB)** ou **ilimitada**. Capacidade de armazenamento ilimitado requer que defina uma chave de partição para os seus dados.  |
   |Débito   | Cada coleção e a base de dados podem ter débito em unidades de pedido por segundo.  Fixo capacidade de armazenamento, débito mínimo é de 400 unidades de pedido por segundo (RU/s), para armazenamento ilimitado, débito de capacidade mínimo está definido como 1000 RU/s.|

6. Depois de introduzir valores para estes campos, selecione **OK** para guardar as definições.  

   ![Débito do conjunto de uma coleção](./media/set-throughput/set-throughput-for-container.png)

7. Para atualizar o débito para um contentor existente, expanda a base de dados e o contentor e, em seguida, clique em **definições**. Na nova janela, escreva o novo valor de débito e, em seguida, selecione **guardar**.  

   ![Débito de atualização para uma coleção](./media/set-throughput/update-throughput-for-container.png)

### <a name="provision-throughput-for-a-set-of-containers-or-at-the-database-level"></a>Débito de aprovisionamento de um conjunto de contentores ou ao nível da base de dados

1. Inicie sessão no [portal do Azure](https://portal.azure.com).  
2. Na navegação esquerda, selecione **todos os recursos** e localize a conta de base de dados do Azure Cosmos.  
3. Pode configurar o débito durante a criação de um débito de base de dados ou a atualização para uma base de dados existente.  
4. Para atribuir débito durante a criação de uma base de dados, abra o **Explorador de dados** painel e selecione **nova base de dados**  
5. Preencher o **id de base de dados** valor, verificação **débito aprovisionar** opção e configura o valor de débito. Uma base de dados pode ser aprovisionado com o valor de débito mínimos 50 000 RU/s.  

   ![Débito do conjunto com a nova opção de base de dados](./media/set-throughput/set-throughput-with-new-database-option.png)

6. Para atualizar o débito de base de dados existente, expanda a base de dados e o contentor e, em seguida, clique em **escala**. Na nova janela, escreva o novo valor de débito e, em seguida, selecione **guardar**.  

   ![Débito de atualização para uma base de dados](./media/set-throughput/update-throughput-for-database.png)

### <a name="provision-throughput-for-a-set-of-containers-as-well-as-for-an-individual-container-in-a-database"></a>Débito de aprovisionamento de um conjunto de contentores, bem como para um contentor individuais numa base de dados

1. Inicie sessão no [portal do Azure](https://portal.azure.com).  
2. Na navegação esquerda, selecione **todos os recursos** e localize a conta de base de dados do Azure Cosmos.  
3. Crie uma base de dados e atribua o débito ao mesmo. Abra o **Explorador de dados** painel e selecione **nova base de dados**  
4. Preencher o **id de base de dados** valor, verificação **débito aprovisionar** opção e configura o valor de débito. Uma base de dados pode ser aprovisionado com o valor de débito mínimos 50 000 RU/s.  

   ![Débito do conjunto com a nova opção de base de dados](./media/set-throughput/set-throughput-with-new-database-option.png)

5. Em seguida, crie uma coleção na base de dados que criou nos passos acima. Para criar uma coleção, clique com botão direito na base de dados e selecione **nova coleção**.  

6. No **adicionar coleção** painel, introduza um nome para a coleção e chave de partição. Opcionalmente, pode aprovisionar débito para esse contentor específico, se optar por não atribuir um valor de débito, o débito atribuído à base de dados é partilhado na coleção.  

   ![Opcionalmente, defina débito para o contentor](./media/set-throughput/optionally-set-throughput-for-the-container.png)

## <a name="considerations-when-provisioning-throughput"></a>Considerações quando aprovisionar débito

Seguem-se algumas considerações que o ajudam a escolher uma estratégia de reserva de débito.

Considere o aprovisionamento de débito no nível de base de dados (que é de conjunto de contentores) nos seguintes casos:

* Se tiver uma dúzia ou mais número de contentores que pode partilhar o débito em alguns ou todos eles.  

* Quando estiver a migrar de uma base de dados de inquilino único que foi concebido para executar em VMs de IaaS alojado ou no local (por exemplo, NoSQL ou bases de dados relacionais) à base de dados do Azure Cosmos e ter muitos contentores.  

* Se pretender considerar picos não planeados em cargas de trabalho utilizando o débito agrupado ao nível da base de dados.  

* Em vez de débito definição um contentor individuais, que está interessado em obter o débito agregado através de um conjunto de contentores na base de dados.

Considere o aprovisionamento de débito num contentor individuais nos seguintes casos:

* Se tiver o menor número de contentores de BD do Cosmos do Azure.  

* Se pretender obter o débito garantido num determinado contentor por SLA.

## <a name="throughput-ranges"></a>Intervalos de débito

A tabela seguinte lista o débito disponível para contentores:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Contentor de partições únicas</strong></p></td>
            <td valign="top"><p><strong>Contentor particionada</strong></p></td>
            <td valign="top"><p><strong>Conjunto de contentores</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Débito mínimo</p></td>
            <td valign="top"><p>400 unidades de pedido por segundo</p></td>
            <td valign="top"><p>1000 unidades de pedido por segundo</p></td>
            <td valign="top"><p>50 000 unidades de pedido por segundo</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Débito máximo</p></td>
            <td valign="top"><p>10 000 unidades de pedido por segundo</p></td>
            <td valign="top"><p>Ilimitado</p></td>
            <td valign="top"><p>Ilimitado</p></td>
        </tr>
    </tbody>
</table>

<a id="set-throughput-sdk"></a>

## <a name="set-throughput-by-using-sql-api-for-net"></a>Débito do conjunto, utilizando a API de SQL para .NET

Eis um fragmento de código para criar um contentor com 3,000 unidades de pedido por segundo para um contentor individuais utilizando o SDK do .NET a API SQL Server:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

Eis um fragmento de código para aprovisionamento 100 000 unidades por segundo de pedido através de um conjunto de contentores com o SDK do .NET a API SQL Server:

```csharp
// Provision 100,000 RU/sec at the database level. 
// sharedCollection1 and sharedCollection2 will share the 100,000 RU/sec from the parent database
// dedicatedCollection will have its own dedicated 4,000 RU/sec, independant of the 100,000 RU/sec provisioned from the parent database
Database database = client.CreateDatabaseAsync(new Database { Id = "myDb" }, new RequestOptions { OfferThroughput = 100000 }).Result;

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

BD do Azure do Cosmos funciona um modelo de reserva para um débito. Ou seja, é-lhe faturado para a quantidade de débito *reservado*, independentemente da quantidade de débito de que está ativamente *utilizado*. Como a aplicação da alteração de padrões de carga, dados e a utilização, pode dimensionar facilmente para cima e baixo o número de reservado RUs através de SDKs ou utilizando o [Portal do Azure](https://portal.azure.com).

Cada contentor, ou um conjunto de contentores, está mapeado para um `Offer` recursos no Azure Cosmos DB, que tem metadados sobre o débito aprovisionado. Pode alterar o débito alocado ao procurar o recurso de oferta correspondente para um contentor, em seguida, atualizá-la com o novo valor de débito. Eis um fragmento de código para alterar o débito de um contentor para 5000 unidades de pedido por segundo, utilizando o SDK .NET:

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

Não há nenhum impacto sobre a disponibilidade do seu contentor ou conjunto de contentores, quando altera o débito. Normalmente, o débito reservado nova é eficaz dentro de segundos na aplicação de débito de novo.

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>Para definir o débito, utilizando a API do SQL Server para Java

O fragmento de código seguinte obtém o débito atual e altera-lo para 500 RU/s. Para um exemplo de código completo, consulte o [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) ficheiro no GitHub. 

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

## <a id="GetLastRequestStatistics"></a>Obter o débito através GetLastRequestStatistics comando da API do MongoDB

A API do MongoDB suporta um comando personalizado, *getLastRequestStatistics*, para obter os encargos de pedido para uma operação indicada.

Por exemplo, na shell do Mongo, execute a operação que pretende verificar a taxa de pedidos.
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

Um método para estimar a quantidade de débito reservado exigido pela sua aplicação está a registar os encargos de unidade de pedido associados a execução de operações típicas num item representativo utilizado pela sua aplicação e, em seguida, calcule o número de operações que antecipa para efetuar a cada segundo.

> [!NOTE]
> Se tiver de tipos de itens que variam significativamente em termos de tamanho e o número de propriedades indexadas, em seguida, registe a taxa de unidade de pedido de operação aplicável associada a cada *tipo* do item típica.
> 
> 

## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>Obtenha o débito, utilizando as métricas de portais de API do MongoDB

A forma mais simples para obter uma boa estimativa do pedido de encargos de unidade para a base de dados de MongoDB API consiste em utilizar o [portal do Azure](https://portal.azure.com) métricas. Com o *número de pedidos* e *pedido encargos* gráficos, pode obter uma estimativa do número de unidades de pedido cada operação está a consumir e unidades de pedido quantos consumirem relativo ao outro.

![Métricas de portais de API do MongoDB][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> Exceder os limites de débito reservado na MongoDB API
As aplicações que excedem o débito aprovisionado para um contentor ou um conjunto de contentores será limitado taxa até que a taxa de consumo descerem abaixo a taxa de débito aprovisionado. Quando ocorre uma limitação de taxa, o back-end vai terminar o pedido com um `16500` código de erro - `Too Many Requests`. Por predefinição, a API do MongoDB tenta automaticamente Repetir até 10 vezes antes de o devolver um `Too Many Requests` código de erro. Se está a receber muitas `Too Many Requests` códigos de erro, poderá considerar a adição de uma lógica de repetição no rotinas de processamento de erros da aplicação ou [aumentar o débito aprovisionado para o contentor](set-throughput.md).

## <a name="throughput-faq"></a>Débito FAQ

**Pode definir o meu débito para menos de 400 RU/s?**

400 RU/s é o débito mínimo disponível nos contentores do Cosmos DB única partição (1000 RU/s é o mínimo para contentores particionadas). O pedido unidades estão definidas em 100 intervalos de RU/s, mas o débito não é possível definir 100 RU/s ou qualquer valor inferior a 400 RU/s. Se estiver à procura de um método económico desenvolver e testar Cosmos DB, pode utilizar o livre [emulador de BD do Azure Cosmos](local-emulator.md), que pode implementar localmente, sem qualquer custo. 

**Como definir o débito utilizando a API do MongoDB**

Não há nenhuma extensão de API do MongoDB para definir o débito. A recomendação é utilizar a API do SQL Server, conforme mostrado no [para definir o débito, utilizando a API de SQL para .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como fazer uma estimativa de unidades de débito e a pedido, consulte o artigo [pedido unidades & calcular o débito do BD Azure Cosmos](request-units.md)

* Para saber mais sobre o aprovisionamento e a escala planet contínuo com Cosmos DB, consulte o artigo [divisão em partições e o dimensionamento com Cosmos DB](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png
