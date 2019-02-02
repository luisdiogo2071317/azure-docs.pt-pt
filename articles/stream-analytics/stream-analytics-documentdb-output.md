---
title: Saída do Azure Stream Analytics para o Cosmos DB
description: Este artigo descreve como utilizar o Azure Stream Analytics para guardar a saída para o Azure Cosmos DB para a saída JSON, para o arquivo de dados e consultas de baixa latência nos dados não estruturados de JSON.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/11/2019
ms.custom: seodec18
ms.openlocfilehash: 4be3de8de4332e8ffb0e88e612a3041829ccd606
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55658577"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Saída do Azure Stream Analytics ao Azure Cosmos DB  
Stream Analytics pode visar [do Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) para saída JSON, permitindo que consultas de arquivamento e baixa latência de dados em dados não estruturados de JSON. Este documento abrange algumas das melhores práticas para implementar esta configuração.

Para aqueles que não estão familiarizados com o Cosmos DB, dê uma olhada [percurso de aprendizagem do Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) para começar a utilizar. 

> [!Note]
> Neste momento, o Azure Stream Analytics suporta apenas a ligação para utilizar o Azure Cosmos DB **API do SQL**.
> Outras APIs do Azure Cosmos DB ainda não são suportados. Se ponto Azure Stream Analytics para as contas do Azure Cosmos DB criado com as outras APIs, os dados poderão não ser corretamente armazenados. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Noções básicas do Cosmos DB como um destino de saída
A saída do Azure Cosmos DB no Stream Analytics permite escrever a sua transmissão em fluxo processando os resultados como saída JSON em suas coleções de Cosmos DB. Stream Analytics não criar coleções na base de dados, em vez disso, que seja necessário criá-los de antemão. Isto é, para que os custos de faturas de coleções do Cosmos DB são controlados por si e para que pode ajustar o desempenho, a consistência e a capacidade de seus conjuntos de diretamente com o [Cosmos DB APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx).

> [!Note]
> Tem de adicionar 0.0.0.0 à lista de IPs permitidos de firewall do Azure Cosmos DB.

Algumas das opções de coleção do Cosmos DB são detalhadas abaixo.

## <a name="tune-consistency-availability-and-latency"></a>Ajustar a consistência, disponibilidade e latência
Para corresponder aos requisitos de aplicação, o Azure Cosmos DB permite-lhe bem otimizar a base de dados e coleções e fazer compensações entre consistência, disponibilidade e latência. Dependendo do que níveis de consistência de leitura suas necessidades de cenário em relação a ler e escrever a latência, que pode escolher um nível de consistência na sua conta de base de dados. Também por predefinição, a Azure Cosmos DB permite a indexação síncrona de cada operação CRUD à sua coleção. Essa é outra opção útil para controlar o desempenho de leitura/escrita no Azure Cosmos DB. Para obter mais informações, reveja os [alterar os níveis de consistência da base de dados e consulta](../cosmos-db/consistency-levels.md) artigo.

## <a name="upserts-from-stream-analytics"></a>Upserts do Stream Analytics
Integração do Stream Analytics com o Azure Cosmos DB permite-lhe inserir ou atualizar registos na sua coleção com base numa determinada coluna de ID do documento. Isso também é referido como um *Upsert*.

Stream Analytics utiliza uma abordagem de upsert otimista, onde as atualizações apenas são efetuadas quando insert falha com um conflito de ID do documento. Esta atualização é executada como um PATCH, para que ele permite que as atualizações parciais para o documento, ou seja, a adição de novas propriedades ou substituir que uma propriedade existente é realizada de forma incremental. No entanto, as alterações nos valores das propriedades de matriz no seu resultado de documento JSON da matriz todo getting substituído, ou seja, a matriz não está intercalado.

Se o documento JSON de entrada tem um campo de ID existente, que o campo é automaticamente utilizado como a coluna de ID do documento no Cosmos DB e quaisquer gravações subseqüentes são tratadas como tal, o que leva a uma das situações:
- IDs exclusivos levam para inserir
- IDs duplicados e de "ID do documento' definido como"ID"leva a upsert
- duplicar IDs e ID do documento não conjunto leva a erro, após o primeiro documento

Se quiser salvar <i>todos os</i> documentos, incluindo as que têm um ID duplicado, mudar o nome do campo de ID na sua consulta (com a palavra-chave AS) e permitir que o Cosmos DB, criar o campo de ID ou substitua o ID de valor de outra coluna (usando a palavra-chave AS ou pelo Utilize a definição de "ID do documento").

## <a name="data-partitioning-in-cosmos-db"></a>Criação de partições de dados no Cosmos DB
O Azure Cosmos DB [ilimitado](../cosmos-db/partition-data.md) contentores são a abordagem recomendada para a partição dos dados, como o Azure Cosmos DB automaticamente dimensiona-se partições com base na carga de trabalho. Ao escrever os contentores ilimitados, o Stream Analytics utiliza os escritores paralelas como anterior consulta passo ou entrada de esquema de partições.
> [!Note]
> Neste momento, o Azure Stream Analytics suporta apenas coleções ilimitadas com chaves de partição no nível superior. Por exemplo, `/region` é suportada. Aninhados chaves de partição (por exemplo, `/region/name`) não são suportadas. 

Para coleções do Azure Cosmos DB fixas, o Stream Analytics não permite nenhuma forma de aumentar vertical ou horizontalmente, uma vez que estão cheias. Eles têm um limite superior de 10 GB e 10 000 de RU/s débito.  Para migrar os dados a partir de um contentor fixo para um contentor ilimitado (por exemplo, um com uma chave de partição e, pelo menos, 1.000 RU/s), tem de utilizar o [ferramenta de migração de dados](../cosmos-db/import-data.md) ou o [biblioteca de feed de alterações](../cosmos-db/change-feed.md).

Gravar em vários contentores fixos está a ser preterida e não é a abordagem recomendada para aumentar horizontalmente a tarefa de Stream Analytics. O artigo [criação de partições e dimensionamento no Cosmos DB](../cosmos-db/sql-api-partition-data.md) fornecem mais detalhes.

## <a name="cosmos-db-settings-for-json-output"></a>Definições de cosmos DB para a saída JSON

Criar o Cosmos DB como uma saída no Stream Analytics gera uma linha de comandos para obter informações, como mostrado abaixo. Esta seção fornece uma explicação sobre a definição de propriedades.

![ecrã de saída do documentdb stream analytics](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

|Campo           | Descrição|
|-------------   | -------------|
|Alias de saída    | Um alias para fazer referência esta saída na sua consulta do ASA.|
|Subscrição    | Escolha a sua subscrição do Azure.|
|ID da Conta      | O nome ou o ponto final do URI de conta do Azure Cosmos DB.|
|Chave de conta     | A chave de acesso partilhado para a conta do Azure Cosmos DB.|
|Base de Dados        | O nome de base de dados do Azure Cosmos DB.|
|Padrão do nome de coleção | O nome da coleção para a coleção a ser utilizado. `MyCollection` é uma entrada válida de exemplo - uma coleção com o nome `MyCollection` tem de existir.  |
|ID do documento     | Opcional. O nome da coluna em eventos de saída utilizado como a chave exclusiva no qual inserção ou atualização de operações devem basear-se. Se deixado em branco, serão possível inserir todos os eventos, sem qualquer opção de atualização.|
