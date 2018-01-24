---
title: "Débito de aprovisionamento para a base de dados do Azure Cosmos | Microsoft Docs"
description: "Saiba como definir o débito de base de dados do Azure Cosmos containsers, coleções, gráficos e tabelas."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: mimig
ms.openlocfilehash: 8797910651c54baa3529b015d4195cf2a5c06ece
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Definir o débito para contentores de base de dados do Azure Cosmos

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
2. Na barra da esquerda, clique em **Azure Cosmos DB**, ou clique em **mais serviços** na parte inferior, em seguida, desloque-se para **bases de dados**e, em seguida, clique em **Azure Cosmos DB**.
3. Selecione a sua conta de base de dados do Cosmos.
4. Na nova janela, clique em **Explorador de dados** no menu de navegação.
5. Na nova janela, expanda a base de dados e o contentor e, em seguida, clique em **definições de dimensionamento &**.
6. Na nova janela, escreva o novo valor de débito no **débito** caixa e, em seguida, clique em **guardar**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>Para definir o débito, utilizando a API de SQL para .NET

```csharp
// Fetch the offer of the collection whose throughput needs to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

// Now persist these changes to the collection by replacing the original offer resource
await client.ReplaceOfferAsync(offer);
```

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>Para definir o débito, utilizando a API do SQL Server para Java

É retirado este fragmento do ficheiro OfferCrudSamples.java no [azure-documentdb-java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) repositório. 

```Java
// find offer associated with this collection
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 10300;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a name="throughput-faq"></a>Débito FAQ

**Pode definir o meu débito para menos de 400 RU/s?**

400 RU/s é o débito mínimo disponível nos contentores do Cosmos DB única partição (1000 RU/s é o mínimo para contentores particionadas). O pedido unidades estão definidas em 100 intervalos de RU/s, mas o débito não é possível definir 100 RU/s ou qualquer valor inferior a 400 RU/s. Se estiver à procura de um método económico desenvolver e testar Cosmos DB, pode utilizar o livre [emulador de BD do Azure Cosmos](local-emulator.md), que pode implementar localmente, sem qualquer custo. 

**Como definir o througput utilizando a API do MongoDB**

Não há nenhuma extensão de API do MongoDB para definir o débito. A recomendação é utilizar a API do SQL Server, conforme mostrado no [para definir o débito, utilizando a API de SQL para .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o aprovisionamento e a escala planet contínuo com Cosmos DB, consulte o artigo [divisão em partições e o dimensionamento com Cosmos DB](partition-data.md).
