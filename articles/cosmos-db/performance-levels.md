---
title: Extinto níveis de desempenho de base de dados do Azure Cosmos | Microsoft Docs
description: Saiba mais sobre os níveis de desempenho S1, S2 e S3 anteriormente disponíveis do BD Azure Cosmos.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: sngun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d1bb7551e6dfb6c42853ab95096f17f5285c69c1
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796653"
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>Extinguir os níveis de desempenho S1, S2 e S3

> [!IMPORTANT] 
> Os níveis de desempenho S1, S2 e S3 abordados neste artigo estão a ser extinto e já não estão disponíveis para novas contas de base de dados do Azure Cosmos.
>

Este artigo fornece uma descrição geral dos níveis de desempenho S1, S2 e S3 e descreve a forma como as coleções que utilizam estes níveis de desempenho podem ser migradas para o único coleções particionadas. Depois de ler este artigo, poderá responder às seguintes questões:

- [Por que razão são os níveis de desempenho S1, S2 e S3 é vai ser reformado?](#why-retired)
- [Como coleções de partições únicas e coleções particionadas comparar com S1, S2, níveis de desempenho de S3?](#compare)
- [O que é necessário para garantir acesso ininterrupto aos meus dados?](#uninterrupted-access)
- [Como o meu coleção será alterado após a migração?](#collection-change)
- [Como os meus faturação será alterado depois de posso estou migrado para coleções de partições únicas?](#billing-change)
- [E se for necessário mais de 10 GB de armazenamento?](#more-storage-needed)
- [Posso alterar entre o S1, S2 e S3 níveis de desempenho antes da migração planeada?](#change-before)
- [Como posso migrar de S1, S2, níveis de desempenho de S3 para coleções de partições únicas no meu próprio?](#migrate-diy)
- [Como estou posso afetado se sou um cliente EA?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>Por que razão são o desempenho S1, S2 e S3 níveis vai ser reformado?

Os níveis de desempenho S1, S2 e S3 não oferecem flexibilidade que fornece a oferta da base de dados do Azure Cosmos padrão. Com S1, S2, S3 níveis de desempenho, capacidade de débito e de armazenamento foram previamente definido e não foi possível oferecem elasticidade. BD do Azure do Cosmos oferece a capacidade para personalizar o débito e armazenamento, oferta muito mais flexibilidade na sua capacidade para dimensionar conforme alterar às suas necessidades.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Como coleções de partições únicas e coleções particionadas comparar com S1, S2, níveis de desempenho de S3?

A tabela seguinte compara as opções de débito e armazenamento disponíveis em coleções de partições únicas, coleções particionadas e S1, S2, níveis de desempenho de S3. Eis um exemplo de região dos EUA Leste 2:

|   |Coleção particionada|Coleção de partições únicas|S1|S2|S3|
|---|---|---|---|---|---|
|Débito máximo|Ilimitado|10 mil RU/s|250 RU/s|1 mil RU/s|2.5 mil RU/s|
|Débito mínimo|2.5 mil RU/s|400 RU/s|250 RU/s|1 mil RU/s|2.5 mil RU/s|
|Armazenamento máximo|Ilimitado|10 GB|10 GB|10 GB|10 GB|
|Preço (mensalmente)|Débito: $6 / 100 RU/s<br><br>Armazenamento: $0.25/ GB|Débito: $6 / 100 RU/s<br><br>Armazenamento: $0.25/ GB|$25 EUR|$50 EUR|$100 EUR|

Tem a um cliente EA? Se Sim, consulte [como estou posso afetado os se sou um cliente EA?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>O que é necessário para garantir acesso ininterrupto aos meus dados?

Se tiver uma coleção S1, S2 ou S3, deve migrar a coleção para uma coleção de partições únicas programaticamente [utilizando o SDK .NET](#migrate-diy). 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Como o meu coleção será alterado após a migração?

Se tiver uma coleção de S1, pode migrá-los para uma coleção de partições únicas com débito do 400 RU/s. 400 RU/s é o débito mais baixo disponível com coleções de partições únicas. No entanto, o custo de 400 RU/s de uma coleção de partições únicas é aproximadamente idêntica à foram pagante com a coleção de S1 e 250 RU/s –, pelo que não são pagar para o extra 150 RU/s disponíveis para si.

Se tiver uma coleção de S2, pode migrá-los para uma coleção de partições únicas com 1 mil RU/s. Não verá nenhuma alteração para o nível de débito.

Se tiver uma coleção de S3, pode migrá-los para uma coleção de partições únicas com 2,5 mil RU/s. Não verá nenhuma alteração para o nível de débito.

Em cada um nestes casos, depois de migrar a coleção, poderá personalizar o nível de débito ou dimensioná-lo e reduzir verticalmente conforme necessário para fornecer acesso de latência baixa aos seus utilizadores. 

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-i-migrated-to-the-single-partition-collections"></a>Como os meus faturação será alterado depois de posso migrar para as coleções de partições únicas?

Partindo do princípio de tem 10 S1 coleções, 1 GB de armazenamento para cada região dos EUA leste, e migrar destas coleções S1 10 para 10 coleções de partições únicas em 400 RU/seg (o nível mínimo). A fatura irão ter o seguinte aspeto se mantiver as 10 coleções de partições únicas durante um mês completo:

![Como S1 preços para 10 coleções com 10 coleções utilizando preços para uma coleção de partições únicas](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>E se for necessário mais de 10 GB de armazenamento?

Se tem uma coleção com o nível de desempenho S1, S2 ou S3, ou se tem uma coleção de partições únicas, dos quais têm 10 GB de armazenamento disponível, que pode utilizar a ferramenta de migração de dados do Azure Cosmos DB para migrar os dados para uma coleção particionada com virtualmente armazenamento ilimitado. Para obter informações sobre as vantagens de uma coleção particionada, consulte [divisão em partições e o dimensionamento do BD Azure Cosmos](sql-api-partition-data.md). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-the-planned-migration"></a>Posso alterar entre o S1, S2 e S3 níveis de desempenho antes da migração planeada?

Apenas as contas existentes com um desempenho S1, S2 e S3 podem ser alteradas e alterar camadas de nível de desempenho através de programação [utilizando o SDK .NET](#migrate-diy). Se alterar do S1, S3 ou S3 para uma coleção de partições únicas, não é possível regressar para os níveis de desempenho S1, S2 ou S3.

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>Como posso migrar de S1, S2, níveis de desempenho de S3 para coleções de partições únicas no meu próprio?

Pode migrar de níveis de desempenho S1, S2 e S3 para coleções de partições únicas programaticamente [utilizando o SDK .NET](#migrate-diy). Pode fazê-lo no seu próprio antes da migração planeada para tirar partido das opções disponíveis com coleções de partições únicas débito flexível.

### <a name="migrate-to-single-partition-collections-by-using-the-net-sdk"></a>Migrar para coleções de partições únicas, utilizando o SDK .NET

Esta secção abrange apenas a alterar o desempenho de uma coleção de nível de utilização a [.NET API do SQL Server](sql-api-sdk-dotnet.md), mas o processo é semelhante para os nossos SDKs outros.

Eis um fragmento de código para alterar o débito de coleção para 5000 unidades de pedido por segundo:
    
```csharp
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

Visite [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) para ver exemplos adicionais e saber mais sobre a nossa oferta métodos:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Como estou posso afetado se sou um cliente EA?

Clientes EA estarão preços protegidos até ao fim do respetivo contrato atual.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre preços e gestão de dados com o Azure Cosmos DB, explore estes recursos:

1.  [A criação de partições de dados na base de dados do Cosmos](sql-api-partition-data.md). Compreenda a diferença entre o contentor de partições únicas e contentores particionadas, bem como sugestões na implementação de uma estratégia de criação de partições de dimensionamento de forma totalmente integrada.
2.  [Preços do cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/). Saiba mais sobre os custos de aprovisionamento de débito e o consumo de armazenamento.
3.  [Unidades de pedido](request-units.md). Compreenda o consumo de débito para tipos de operação diferentes, por exemplo, leitura, escrita, consulta.
