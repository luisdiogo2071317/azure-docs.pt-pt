---
title: Expirar dados no Azure Cosmos DB com TTL | Documentos da Microsoft
description: Com o valor de TTL, o Microsoft Azure Cosmos DB fornece a capacidade de ter documentos automaticamente removidos do sistema após um período de tempo.
services: cosmos-db
keywords: TTL
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 08/29/2017
ms.author: sngun
ms.openlocfilehash: 49f6d6ee65ffae71cba8c73301355bfe2bdcd1d6
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480561"
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-to-live"></a>Expirar dados nas coleções do Azure Cosmos DB automaticamente com o tempo de duração
Aplicações podem produzir e armazenar grandes quantidades de dados. Alguns destes dados, como o computador gerado dados, registos e utilizador a sessão do evento informações só são úteis para um período finito de tempo. Depois dos dados se torna surplus às necessidades do aplicativo, é seguro remover estes dados e reduzir as necessidades de armazenamento de um aplicativo.

Com "time to live" ou o valor de TTL, o Microsoft Azure Cosmos DB fornece a capacidade de ter documentos automaticamente removidos da base de dados após um período de tempo. O tempo de duração predefinido pode ser definido ao nível da coleção e substituído numa base por documento. Depois do TTL é definido como um padrão de coleção ou num nível de documento, o Cosmos DB removerá automaticamente documentos existentes após esse período de tempo, em segundos, uma vez que eles foram modificado pela última vez.

TTL no Azure Cosmos DB utiliza um desvio em relação a quando o documento foi modificado pela última vez. Para fazer isso, ele usa o `_ts` campo, o que existe em cada documento. O campo TS é um carimbo de "Epoch" estilo unix, que representa a data e hora. O `_ts` campo é atualizado sempre que um documento é modificado. 

## <a name="ttl-behavior"></a>Comportamento TTL
A funcionalidade de valor de TTL é controlada por propriedades TTL em dois níveis - o nível de coleção e o nível de documento. Os valores são definidos em segundos e são tratados como um delta do `_ts` que o documento foi última modificação às.

1. DefaultTTL para a coleção
   
   * Se estiver em falta (ou definida como nula), documentos não são eliminados automaticamente.
   * Se o momento presente e o valor está definido como "-1" = infinito – documentos não expiram por predefinição
   * Se presente e o valor está definido como um número ("n") – documentos expirarem "n" segundos após a última modificação
2. Valor de TTL para os documentos: 
   
   * A propriedade é aplicável apenas se DefaultTTL está presente para a coleção principal.
   * Substitui o valor de DefaultTTL para a coleção principal.

Assim que o documento expirou (`ttl`  +  `_ts` < = hora atual do servidor), o documento está marcado como "expirada". Nenhuma operação será permitida nestes documentos após esse período e eles serão excluídos dos resultados de todas as consultas executadas. Os documentos são eliminados fisicamente no sistema e são eliminados em segundo plano oportunisticamente num momento posterior. Isso não consuma qualquer [unidades de pedido (RUs)](request-units.md) do orçamento de coleção.

A lógica acima pode ser apresentada na matriz seguinte:

|  | DefaultTTL em falta ou não definido na coleção de | DefaultTTL = -1 na coleção | DefaultTTL = "n" na coleção |
| --- |:--- |:--- |:--- |
| TTL em falta no documento |Nada para substituir no nível de documento, uma vez que o documento e a coleção não tem nenhum conceito de TTL. |Não existem documentos nesta coleção irão expirar. |Os documentos nesta coleção irão expirar quando o período de intervalo n. |
| TTL = -1 no documento |Nada para substituir a nível de documento, uma vez que a coleção não define a propriedade de DefaultTTL que pode substituir um documento. TTL num documento é não interpretado pelo sistema. |Não existem documentos nesta coleção irão expirar. |O documento com valor de TTL =-1 nesta coleção nunca irá expirar. Todos os outros documentos irão expirar após o intervalo de "n". |
| TTL = n no documento |Nada para substituir a nível de documento. TTL num documento é não interpretado pelo sistema. |O documento com valor de TTL = n irá expirar após n de intervalo, em segundos. Outros documentos irão herdar intervalo de -1 e nunca expirar. |O documento com valor de TTL = n irá expirar após n de intervalo, em segundos. Outros documentos irão herdar intervalo de "n" da coleção. |

## <a name="configuring-ttl"></a>Configurar o valor de TTL
Por predefinição, o TTL é desativado por predefinição em todas as coleções do Cosmos DB e em todos os documentos. Valor de TTL pode ser definido por meio de programação ou através do portal do Azure. Utilize os seguintes passos para configurar o TTL a partir do portal do Azure:

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) e navegue para a sua conta do Azure Cosmos DB.  

2. Navegue para a coleção que pretende definir o valor de TTL, abra a **dimensionamento e definições** painel. Pode ver que o tempo TTL é por padrão, definido como **desativar**. Pode alterá-lo para **ativada (sem predefinição)** ou **no**.

   **desativar** -documentos não são eliminados automaticamente.  
   **no (não padrão)** -esta opção define o valor de TTL como "-1" (infinito) que significa que documentos não expiram por predefinição.  
   **no** -documentos expirarem "n" segundos após a última modificação.  

   ![Tempo de duração](./media/time-to-live/set-ttl-in-portal.png)

## <a name="enabling-ttl"></a>Ativar o valor de TTL
Para ativar o TTL uma coleção ou documentos dentro de uma coleção, terá de definir a propriedade de DefaultTTL de uma coleção como -1 ou um número positivo diferente de zero. Definir o DefaultTTL como-1 significa que por padrão, todos os documentos na coleção irão durar para sempre, mas o serviço Cosmos DB, deverá monitorizar esta coleção para documentos que tenham substituído esta predefinição.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a>Configurar o TTL predefinido numa coleção
É possível configurar um tempo de duração num nível de coleção predefinido. Para definir o valor de TTL numa coleção, tem de fornecer um número positivo diferente de zero que indica o período de, em segundos, para expirar todos os documentos na coleção após a última modificação timestamp do documento (`_ts`). Em alternativa, pode definir o padrão como -1, o que implica que todos os documentos inseridos na coleção residirá indefinidamente por predefinição.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a>Definição TTL de um documento
Além de definir um TTL predefinido numa coleção, pode definir o TTL específico num nível de documento. Isso irá substituir a predefinição da coleção.

* Para definir o valor de TTL num documento, tem de fornecer um número positivo diferente de zero, que indica o período de, em segundos, para o documento de expirar após a última modificação timestamp do documento (`_ts`).
* Se um documento não tiver nenhum campo de valor de TTL, o padrão da coleção será aplicada.
* Se o valor de TTL é desativado ao nível da coleção, o campo de valor de TTL do documento será ignorado enquanto o TTL é novamente ativado na coleção.

Este é um trecho de código que mostra como definir a expiração do TTL num documento:

    // Include a property that serializes to "ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used to set expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set the value to the expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a>Estendendo o TTL num documento existente
Pode repor o valor de TTL num documento ao realizar qualquer operação de escrita no documento. Fazer isso definirá os `_ts` para a hora atual e a contagem regressiva para a expiração de documentos, conforme definido pelo `ttl`, começarão de novo. Se pretender alterar o `ttl` de um documento, pode atualizar o campo como pode fazer com qualquer outro campo definível.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time to live
    
    response = await client.ReplaceDocumentAsync(readDocument);

## <a name="removing-ttl-from-a-document"></a>Remover o valor de TTL de um documento
Se um valor de TTL foi definida num documento e que já não quer esse documento para expirar, em seguida, pode obter o documento, remova o campo de valor de TTL e substituir o documento no servidor. Quando o campo de valor de TTL é removido do documento, a predefinição da coleção será aplicada. Para parar um documento de expirar e não herdar da coleção, em seguida, terá de definir o valor TTL como -1.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit the default TTL of the collection
    
    response = await client.ReplaceDocumentAsync(readDocument);

## <a name="disabling-ttl"></a>Desativar o valor de TTL
Para desativar o valor de TTL inteiramente numa coleção e parar o processo de plano de fundo à procura de documentos expirados, que a propriedade DefaultTTL na coleção deve ser eliminada. A eliminar esta propriedade é diferente de defini-la como -1. Definição para-1 significa que os novos documentos adicionado à coleção irá durar para sempre, mas pode substituí-lo em documentos específicos na coleção. Esta propriedade a remover totalmente a partir da coleção significa que que não existem documentos irão expirar, mesmo se existem documentos que tenham substituído explicitamente o padrão anterior.

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);

<a id="ttl-and-index-interaction"></a> 
## <a name="ttl-and-index-interaction"></a>Interação de TTL e índice
Adicionar ou alterar a definição do valor de TTL numa coleção altera o índice subjacente. Quando o valor de TTL é alterado de desativado para ativado, a coleção é reindexada. Ao fazer alterações à política de indexação quando o modo de indexação é consistente, os usuários não notará uma alteração para o índice. Quando o modo de indexação é definido como lento, o índice é sempre capturando cópia de segurança e se o valor TTL é alterado, o índice é recriado do zero. Quando o valor TTL é alterado e o modo de índice é definido como lento, consultas feitas durante a recriação de índice não devolveu resultados completos ou corretos.

Se precisar de exatamente os dados retornados, não altere o valor de TTL quando o modo de indexação é definido como lento. O ideal é que o índice consistente deve ser selecionado para garantir que os resultados da consulta consistente. 

## <a name="faq"></a>FAQ
**O valor de TTL custará-me?**

Não existe nenhum custo adicional para definir um valor de TTL num documento.

**Quanto tempo levará para eliminar o meu documento depois do TTL é de cópia de segurança?**

Os documentos são a expirou imediatamente depois do valor de TTL está a funcionar e não estará acessível via CRUD ou APIs de consulta. 

**TTL num documento terá qualquer impacto sobre os custos de RU?**

Não, haverá nenhum impacto sobre os custos de RU de eliminações de expirada documentos por meio do TTL no Cosmos DB.

**É a funcionalidade de valor de TTL apenas aplicável a todos os documentos ou pode expirar, os valores de propriedade de documentos individuais?**

TTL se aplica a todo o documento. Se quiser expirar apenas uma parte de um documento, em seguida, recomenda-se que extraia a parte do documento principal para um documento separado de "ligado" e, em seguida, utilize o TTL nesse documento extraídos.

**A funcionalidade de valor de TTL tem quaisquer requisitos de indexação específicos?**

Sim. A coleção tem de ter [conjunto de políticas de indexação](indexing-policies.md) para consistência ou Lazy. Tentar definir DefaultTTL numa coleção com indexação definido como None resultará num erro, assim como a tentar desativar a indexação numa coleção que tenha um DefaultTTL já definido.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o Azure Cosmos DB, consulte o serviço [ *documentação* ](https://azure.microsoft.com/documentation/services/cosmos-db/) página.

