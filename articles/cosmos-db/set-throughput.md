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
ms.date: 03/23/2018
ms.author: sngun
ms.openlocfilehash: 0a53bb0a23fae386abbe71de944b073cbb93d502
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers"></a>Definir e obter o débito de base de dados do Azure Cosmos contentores

Pode definir o débito para os contentores de BD do Cosmos do Azure no portal do Azure ou ao utilizar o SDKs do cliente. 

A tabela seguinte lista o débito disponível para contentores:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Contentor de partições únicas</strong></p></td>
            <td valign="top"><p><strong>Contentor particionada</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Débito mínimo</p></td>
            <td valign="top"><p>400 unidades de pedido por segundo</p></td>
            <td valign="top"><p>unidades de pedido de 1000 por segundo</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Débito máximo</p></td>
            <td valign="top"><p>10 000 unidades de pedido por segundo</p></td>
            <td valign="top"><p>Ilimitado</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>Para definir o débito utilizando o portal do Azure

1. Numa nova janela, abra o [portal do Azure](https://portal.azure.com).
2. Na barra da esquerda, clique em **Azure Cosmos DB**, ou clique em **todos os serviços** na parte inferior, em seguida, desloque-se para **bases de dados**e, em seguida, clique em **Azure Cosmos DB**.
3. Selecione a sua conta de base de dados do Cosmos.
4. Na nova janela, clique em **Explorador de dados** no menu de navegação.
5. Na nova janela, expanda a base de dados e o contentor e, em seguida, clique em **definições de dimensionamento &**.
6. Na nova janela, escreva o novo valor de débito no **débito** caixa e, em seguida, clique em **guardar**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>Para definir o débito, utilizando a API de SQL para .NET

O fragmento de código seguinte obtém o débito atual e altera-lo para 500 RU/s. Para o exemplo de código completo, consulte o [CollectionManagement](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L188-L216) projeto no GitHub.

```csharp
// Fetch the offer of the collection whose throughput needs to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 500 request units per second
offer = new OfferV2(offer, 500);

// Now persist these changes to the collection by replacing the original offer resource
await client.ReplaceOfferAsync(offer);
```

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>Para definir o débito, utilizando a API do SQL Server para Java

O fragmento de código seguinte obtém o débito atual e altera-lo para 500 RU/s. Para um exemplo de código completo, consulte o [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) ficheiro no GitHub. 

```Java
// find offer associated with this collection
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

Por exemplo, na Shell do Mongo, execute a operação que pretende verificar a taxa de pedidos.
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

Com isto em mente, um método para estimar a quantidade de débito reservado exigido pela sua aplicação consiste em registar os encargos de unidade de pedido associados a execução de operações típicas num item representativo utilizado pela sua aplicação e, em seguida, estimar o número de operações que antecipa para efetuar a cada segundo.

> [!NOTE]
> Se tiver de tipos de itens que variam significativamente em termos de tamanho e o número de propriedades indexadas, em seguida, registe a taxa de unidade de pedido de operação aplicável associada a cada *tipo* do item típica.
> 
> 

## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>Obtenha o débito, utilizando as métricas de portais de API do MongoDB

A forma mais simples para obter uma boa estimativa do pedido de encargos de unidade para a base de dados de MongoDB API consiste em utilizar o [portal do Azure](https://portal.azure.com) métricas. Com o *número de pedidos* e *pedido encargos* gráficos, pode obter uma estimativa do número de unidades de pedido cada operação está a consumir e unidades de pedido quantos consumirem relativo ao outro.

![Métricas de portais de API do MongoDB][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> Exceder os limites de débito reservado na MongoDB API
As aplicações que excedem o débito aprovisionado para um contentor será limitado taxa até que a taxa de consumo descerem abaixo a taxa de débito aprovisionado. Quando ocorre uma limitação de taxa, o back-end preventivamente vai terminar o pedido com um `16500` código de erro - `Too Many Requests`. Por predefinição, a API do MongoDB tenta automaticamente Repetir até 10 vezes antes de o devolver um `Too Many Requests` código de erro. Se está a receber muitas `Too Many Requests` códigos de erro, poderá considerar a adição de uma lógica de repetição no rotinas de processamento de erros da aplicação ou [aumentar o débito aprovisionado para o contentor](set-throughput.md).

## <a name="throughput-faq"></a>Débito FAQ

**Pode definir o meu débito para menos de 400 RU/s?**

400 RU/s é o débito mínimo disponível nos contentores do Cosmos DB única partição (1000 RU/s é o mínimo para contentores particionadas). O pedido unidades estão definidas em 100 intervalos de RU/s, mas o débito não é possível definir 100 RU/s ou qualquer valor inferior a 400 RU/s. Se estiver à procura de um método económico desenvolver e testar Cosmos DB, pode utilizar o livre [emulador de BD do Azure Cosmos](local-emulator.md), que pode implementar localmente, sem qualquer custo. 

**Como definir o througput utilizando a API do MongoDB**

Não há nenhuma extensão de API do MongoDB para definir o débito. A recomendação é utilizar a API do SQL Server, conforme mostrado no [para definir o débito, utilizando a API de SQL para .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o aprovisionamento e a escala planet contínuo com Cosmos DB, consulte o artigo [divisão em partições e o dimensionamento com Cosmos DB](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png