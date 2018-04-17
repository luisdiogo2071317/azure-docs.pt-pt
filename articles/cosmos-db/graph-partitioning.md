---
title: A criação de partições de Graph API | Microsoft Docs
description: Saiba como pode utilizar um gráfico particionado do BD Azure Cosmos.
services: cosmos-db
author: luisbosquez
documentationcenter: ''
manager: kfile
ms.assetid: ''
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/28/2018
ms.author: lbosq
ms.openlocfilehash: db41efeee467d2cda89f62e0a18cf89cec2d9e63
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Utilizar um gráfico particionado do BD Azure Cosmos

Uma das principais funcionalidades da Graph API do BD Azure Cosmos é a capacidade de processar em grande escala gráficos através de escalabilidade horizontal. Este processo é conseguido através de [capacidades do BD Azure Cosmos de criação de partições](partition-data.md#how-does-partitioning-work), que se a utilização de coleções, também referidas como contentores, que podem ser dimensionados de forma independente em termos de armazenamento e débito. BD do Cosmos do Azure suporta os seguintes tipos de contentores através de todos os APIs:

- **Corrigido coleção**: destas coleções podem armazenar um gráfico da base de dados até 10 GB de tamanho com um máximo de 10 000 unidades de pedido por segundo atribuída ao mesmo. Para criar uma coleção fixa não é necessário especificar uma propriedade de chave de partição nos dados.

- **Coleção ilimitada**: estas coleções, podem Dimensionar para armazenar um gráfico para além do limite de 10 GB através da criação de partições horizontais. Cada partição armazenará 10 GB e os dados irão ser automaticamente balanceados com base no **chave de partição especificado**, que será um parâmetro necessário quando utilizar uma coleção ilimitada. Este tipo de contentor pode armazenar um tamanho de dados virtualmente ilimitados e podem permitir que até 100 000 unidades de pedido por segundo ou mais [contactando o suporte](https://aka.ms/cosmosdbfeedback?subject=Cosmos%20DB%20More%20Throughput%20Request).

Neste documento, as especificações no modo como as bases de dados do gráfico são particionadas serão descritas mais juntamente com o respetivas implicações de ambos vértices (ou nós) e contornos.

## <a name="requirements-for-partitioned-graph"></a>Requisitos para gráfico particionado

Seguem-se detalhes que precisam de ser compreendidos ao criar um contentor de gráfico particionada:
- **Como configurar a criação de partições será necessário** se a coleção é mais de 10 GB num tamanho de e/ou se alocar mais de 10 000 unidades de pedido por segundo (RU/s) será necessária.
- **Vértices e margens são armazenadas como documentos JSON** no back-end de um contentor do Azure Cosmos DB Graph API.
- **Vértices requerem uma chave de partição**. Esta chave determinará a partição do vértice será armazenado através de um algoritmo de hashing. O nome desta chave de partição é uma cadeia de palavra única sem espaços ou carateres especiais e está definido quando criar uma nova coleção com o formato `/partitioning-key-name` no portal.
- **Margens serão armazenadas com as respetivas vértice origem**. Por outras palavras, para cada vértice a chave de partição vai definir onde serão armazenadas, juntamente com o respetivos contornos de saída. Isto é feito para evitar consultas de partição cruzada ao utilizar o `out()` cardinalidade em consultas de gráfico.
- **Consultas de gráfico tem de especificar uma chave de partição**. Para tirar partido da criação de partições horizontais do BD Azure Cosmos, a chave de partição deve ser especificada quando é selecionado um único vértice, sempre que possível. Seguem-se as consultas para selecionar um ou vários vértices no gráfico de particionada:

    - A seleção de um vértice por ID, em seguida, **utilizando o `.has()` passo para especificar a propriedade de chave de partição**: 
    
        ```
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - A seleção de um vértice por **especificar uma cadeia de identificação, incluindo o valor da chave de partição e o ID**: 
    
        ```
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - Especificar um **matriz de cadeias de identificação de IDs e valores de chave de partição**:
    
        ```
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Selecionar um conjunto de vértices e **especificar uma lista dos valores de chave de partição**: 
    
        ```
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Melhores práticas quando utilizar um gráfico particionado

Seguem-se diretrizes que devem ser seguidas para garantir o desempenho e escalabilidade mais eficiente ao utilizar gráficos particionados em coleções ilimitadas:
- **Sempre especificar o valor da chave de partição ao consultar um vértice**. Obtenção de um vértice de uma partição conhecida é a forma mais eficiente em termos de desempenho.
- **Utilizar a direção de saída ao consultar contornos sempre que possível**. Tal como mencionado acima, margens são armazenadas com as respetivas vértices de origem na direção de saída. Isto significa que as possibilidades de resorting para consultas de partição cruzada são minimizadas quando os dados e as consultas estão concebidas com este padrão em mente.
- **Escolha uma chave de partição que será distribuir uniformemente dados em partições**. Esta decisão depende descontos elevados no modelo de dados da solução. Saiba mais sobre a criação de uma chave de partição adequado na [Partitining e escala do BD Azure Cosmos](partition-data.md).
- **Otimizar as consultas para obter dados dentro dos limites de uma partição sempre que possível**. Uma estratégia de criação de partições ideal seria serem alinhada com os padrões de consultas. As consultas que obtêm dados de uma única partição proporcionar o melhor desempenho possível.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, foi fornecida uma descrição geral dos conceitos e procedimentos recomendados para a criação de partições, com um Azure Cosmos DB Graph API. 

* Saiba mais sobre [partição e o dimensionamento do BD Azure Cosmos](partition-data.md).
* Saiba mais sobre o [suporte Gremlin no Graph API](gremlin-support.md).
* Saiba mais sobre [introdução a Graph API](graph-introduction.md).
