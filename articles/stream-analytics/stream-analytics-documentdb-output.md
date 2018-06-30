---
title: Saída de Stream Analytics do Azure à base de dados do Cosmos
description: Este artigo descreve como utilizar o Azure Stream Analytics para guardar a saída à base de dados do Azure Cosmos da saída JSON, para o arquivo de dados e consultas de latência baixa em dados não estruturados de JSON.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: ff2071a703b0b5e94cd68122a878b51e9d97669a
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112756"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Saída de Stream Analytics do Azure à base de dados do Azure Cosmos  
Pode visar do Stream Analytics [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) para saída JSON, permitindo consultas de arquivo e baixa latência de dados em dados não estruturados de JSON. Este documento inclui algumas melhores práticas para implementar esta configuração.

Os utilizadores que não está familiarizado com a base de dados do Cosmos, observe [percurso de aprendizagem da BD do Azure Cosmos](https://azure.microsoft.com/documentation/learning-paths/documentdb/) para começar a utilizar. 

> [!Note]
> Neste momento, o Azure Stream Analytics suporta apenas a ligação à base de dados do Azure Cosmos utilizando **API do SQL Server**.
> APIs de BD do Cosmos outros Azure ainda não são suportadas. Se o ponto de Azure Stream Analytics para as contas de base de dados do Azure Cosmos criado com outras APIs, os dados poderão não ser corretamente armazenados. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Noções básicas da BD do Cosmos como um destino de saída
A saída de BD do Cosmos Azure Stream Analytics permite escrita sua transmissão em fluxo processar os resultados como saída JSON na sua collection(s) de BD do Cosmos. Do Stream Analytics não criar coleções na base de dados, em vez disso, exigir a criação de compromisso. Isto é, de modo a que os custos de faturação da BD do Cosmos coleções são controlados por si e para que pode otimizar o desempenho, a consistência e a capacidade do seu coleções diretamente utilizando a [APIs de BD do Cosmos](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

Algumas das opções de recolha de BD do Cosmos detalhadas abaixo.

## <a name="tune-consistency-availability-and-latency"></a>Otimizar a consistência, disponibilidade e a latência
Para fazer corresponder os seus requisitos de aplicações, o Azure Cosmos DB permite-lhe ajustar e otimizar a base de dados e coleções compromissos entre consistência, disponibilidade e a latência. Consoante os níveis de consistência de leitura as necessidades de cenário contra ler e escrever latência, que pode escolher um nível de consistência na sua conta de base de dados. Também por predefinição, a base de dados do Azure Cosmos permite a indexação síncrona de cada operação CRUD à sua coleção. Esta é outra opção útil para controlar o desempenho de leitura/escrita na base de dados do Azure Cosmos. Para obter mais informações, consulte o [alterar os níveis de consistência da base de dados e a consulta](../cosmos-db/consistency-levels.md) artigo.

## <a name="upserts-from-stream-analytics"></a>Upserts a partir do Stream Analytics
Integração de análise de fluxo com base de dados do Azure Cosmos permite-lhe inserir ou atualizar os registos na sua coleção com base numa coluna ID do documento fornecida. Isto também é referido como um *Upsert*.

Do Stream Analytics utiliza uma abordagem de upsert otimista, onde as atualizações apenas são efetuadas quando insert falha com um conflito de ID do documento. Esta atualização é executada como uma correção, pelo que permite que as atualizações parciais o documento, ou seja, a adição de novas propriedades ou substituir que uma propriedade existente é executada de forma incremental. No entanto, as alterações nos valores de propriedades de matriz no seu resultado de documentos JSON na matriz todo obter substituída, ou seja, a matriz não está intercalada.

## <a name="data-partitioning-in-cosmos-db"></a>Criação de partições de dados na base de dados do Cosmos
BD do Azure do Cosmos [ilimitados](../cosmos-db/partition-data.md) são a abordagem recomendada para a criação de partições de dados, como a base de dados do Azure Cosmos automaticamente dimensiona partições com base na sua carga de trabalho. Quando são escritos para contentores ilimitados, o Stream Analytics utiliza tantos escritores paralelas como passo consulta anterior ou introduza a criação de partições de esquema.

Para coleções de base de dados do Azure Cosmos fixas, o Stream Analytics não permite nenhuma forma de aumentar verticalmente ou horizontalmente assim que estiverem completas. Têm um limite superior de 10 GB e 10 000 de RU/s débito.  Para migrar os dados a partir de um contentor fixo para um contentor ilimitado (por exemplo, um com, pelo menos, 1000 RU/s e uma chave de partição), tem de utilizar o [ferramenta de migração de dados](../cosmos-db/import-data.md) ou [alteração feed biblioteca](../cosmos-db/change-feed.md).

Escrever em vários contentores fixos está a ser preterido e não é a abordagem recomendada para aumentar horizontalmente a tarefa de Stream Analytics. O artigo [divisão em partições e o dimensionamento na base de dados do Cosmos](../cosmos-db/sql-api-partition-data.md) fornece mais detalhes.

## <a name="cosmos-db-settings-for-json-output"></a>Definições de cosmos base de dados de saída JSON
Criar BD do Cosmos como uma saída no Stream Analytics gera uma linha de comandos para obter informações, como mostrado abaixo. Esta secção fornece uma explicação da definição de propriedades.


![ecrã de saída do documentdb stream analytics](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

Campo           | Descrição 
-------------   | -------------
Alias de saída    | Um alias para fazer referência este resultado da consulta do ASA   
Nome da Conta    | O nome ou o ponto final o URI da conta de base de dados do Azure Cosmos 
Chave da Conta     | A chave de acesso partilhado para a conta de base de dados do Azure Cosmos
Base de Dados        | O nome de base de dados de base de dados do Azure Cosmos
Nome da Coleção | O nome da coleção para a coleção ser utilizado. `MyCollection` é um exemplo de entrada válido - uma coleção designada `MyCollection` tem de existir.  
ID do documento     | Opcional. O nome de coluna em eventos de saída utilizado como a chave exclusiva no qual insert ou update operações devem basear-se. Se deixado em branco, todos os eventos serão inseridos, sem qualquer opção de atualização.
