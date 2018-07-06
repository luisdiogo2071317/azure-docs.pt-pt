---
title: A criação de partições da Graph API | Documentos da Microsoft
description: Saiba como pode utilizar um gráfico particionado no Azure Cosmos DB.
services: cosmos-db
author: luisbosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: conceptual
ms.date: 02/28/2018
ms.author: lbosq
ms.openlocfilehash: 202c575a917cfb24436d86881e5368b61f216d42
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861889"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Usando um gráfico de particionadas no Azure Cosmos DB

Uma das principais funcionalidades da Graph API no Azure Cosmos DB é a capacidade de processar gráficos em grande escala por meio de escalabilidade horizontal. Este processo é obtido por meio do [criação de partições de recursos no Azure Cosmos DB](partition-data.md#how-does-partitioning-work), que torna utilizar contentores, que pode dimensionar de forma independente em termos de armazenamento e débito. O Azure Cosmos DB suporta os seguintes tipos de contentores em todas as APIs:

- **Contentor fixo**: desses contêineres podem armazenar um gráfico da base de dados até 10 GB de tamanho com um máximo de 10 000 unidades de pedido por segundo atribuída ao mesmo. Para criar um contentor fixo não é necessário especificar uma propriedade de chave de partição de dados.

- **Contentor ilimitado**: desses contêineres podem Dimensionar automaticamente para armazenar um gráfico além do limite de 10 GB por meio da criação de partições horizontais. Cada partição armazenará 10 GB e os dados irão ser automaticamente balanceados com base no **chave de partição especificado**, que será um parâmetro necessário quando utilizar um contentor ilimitado. Esse tipo de contentor pode armazenar um tamanho de dados praticamente ilimitada e pode permitir que até 100 000 unidades de pedido por segundo ou mais [contactando o suporte](https://aka.ms/cosmosdbfeedback?subject=Cosmos%20DB%20More%20Throughput%20Request).

Neste documento, os detalhes sobre como as bases de dados do gráfico são particionados serão descritos juntamente com suas implicações para os vértices (ou nós) e margens.

## <a name="requirements-for-partitioned-graph"></a>Requisitos para o graph particionada

Seguem-se detalhes que precisa de ser entendidos ao criar um contentor de gráficos particionada:
- **Configurar a criação de partições será necessário** se o contentor é esperado que seja mais de 10 GB de tamanho e/ou se a alocação de mais de 10.000 unidades de pedido por segundo (RU/s) será necessária.
- **Vértices e margens são armazenadas como documentos JSON** no back-end de um contentor de Graph API do Azure Cosmos DB.
- **Vértices requerem uma chave de partição**. Esta chave determinará qual partição vértice será armazenado por meio de um algoritmo de hash. O nome desta chave de partição é uma cadeia de palavra única sem espaços ou carateres especiais, e ele é definido durante a criação de um novo contentor utilizando o formato `/partitioning-key-name` no portal.
- **Margens serão armazenadas com seu vértice de origem**. Em outras palavras, para cada vértice respetiva chave de partição definirá onde serão armazenados juntamente com bordas de saída. Isso é feito para evitar consultas entre partições quando utiliza o `out()` cardinalidade em consultas de gráficos.
- **Consultas de gráficos tem de especificar uma chave de partição**. Para aproveitar ao máximo a criação de partições horizontais no Azure Cosmos DB, a chave de partição deve ser especificada quando é selecionado um único vértice, sempre que possível. Seguem-se as consultas para a seleção de vértices de um ou vários num gráfico particionado:

    - Selecionar um vértice por ID, em seguida, **utilizando o `.has()` passo para especificar a propriedade da chave de partição**: 
    
        ```
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - Selecionar um vértice pela **especificando uma tupla, incluindo o valor da chave de partição e ID**: 
    
        ```
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - Especificar um **matriz de cadeias de identificação de valores de chave de partição e IDs**:
    
        ```
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Selecionar um conjunto de vértices e **especificando uma lista de valores de chave de partição**: 
    
        ```
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Melhores práticas quando utilizar um gráfico particionado

Seguem-se diretrizes que devem ser seguidas para garantir o desempenho e escalabilidade mais eficiente ao utilizar gráficos particionados em contentores ilimitados:
- **Sempre especificar o valor da chave de partição ao consultar um vértice**. Obter um vértice a partir de uma partição conhecida é a maneira mais eficiente em termos de desempenho.
- **Utilizar a direção de saída ao consultar as margens, sempre que possível**. Conforme mencionado acima, os limites são armazenados com seus vértices de origem na direção de saída. Isso significa que as possibilidades de recorrer a consultas entre partições são minimizadas quando os dados e as consultas são projetadas com esse padrão em mente.
- **Escolha uma chave de partição que irá distribuir uniformemente os dados em partições**. Essa decisão depende intensamente o modelo de dados da solução. Saiba mais sobre criar uma chave de partição adequado no [criação de partições e dimensionamento no Azure Cosmos DB](partition-data.md).
- **Otimizar as consultas para obter dados dentro dos limites de uma partição sempre que possível**. Uma estratégia de particionamento ideal seria alinhada para os padrões de consultas. Consultas que obtêm dados de uma única partição fornecem o melhor desempenho possível.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, foi fornecida uma visão geral dos conceitos e práticas recomendadas para criação de partições com um Graph API do Azure Cosmos DB. 

* Saiba mais sobre [particionar e dimensionar no Azure Cosmos DB](partition-data.md).
* Saiba mais sobre o [suporte para Gremlin na API do Graph](gremlin-support.md).
* Saiba mais sobre [introdução à Graph API](graph-introduction.md).
