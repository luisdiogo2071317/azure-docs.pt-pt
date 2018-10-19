---
title: Migre os dados para a conta da API do Cassandra do Azure Cosmos DB
description: Saiba como utilizar o comando CQL Copy e o Spark para copiar dados do Apache Cassandra para a API para Cassandra do Azure Cosmos DB.
services: cosmos-db
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.author: govindk
ms.topic: tutorial
ms.date: 09/24/2018
ms.reviewer: sngun
ms.openlocfilehash: f73a201a25bb2f975e8a261a6c21aa7b066c3a7c
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48247855"
---
# <a name="migrate-your-data-to-azure-cosmos-db-cassandra-api-account"></a>Migre os dados para a conta da API do Cassandra do Azure Cosmos DB

Este tutorial fornece instruções sobre como migrar dados do Apache Cassandra para a API para Cassandra do Azure Cosmos DB. 

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Planear a migração
> * Pré-requisitos de migração
> * Migrar dados com o comando cqlsh COPY
> * Migrar dados com o Spark 

## <a name="plan-for-migration"></a>Planear a migração

Antes de migrar dados para a API para Cassandra do Azure Cosmos DB, deve determinar as necessidades de débito da sua carga de trabalho. Em geral, recomenda-se começar com o débito médio necessário para as operações CRUD e, em seguida, incluir o débito adicional necessário para operações ETL (Extrair Transformar Carregar) ou grandes. Precisa dos seguintes detalhes para planear a migração: 

* **Tamanho dos dados existentes ou o tamanho estimado dos dados:** define o requisito de tamanho e débito de base de dados mínimos. Se estiver a calcular o tamanho dos dados para uma nova aplicação, pode pressupor que os dados são distribuídos uniformemente nos registos e calcular o valor ao multiplicar o tamanho de dados. 

* **Débito necessário:** taxa aproximada do débito de leitura (consulta/obter) e escrita (atualizar/eliminar/inserir). Este valor é necessário para calcular as unidades de pedido necessárias, juntamente com o tamanho de dados no estado estável.  

* **Obter o esquema:** ligue ao seu cluster do Cassandra existente através do cqlsh e exporte o esquema do Cassandra: 

  ```bash
  cqlsh [IP] "-e DESC SCHEMA" > orig_schema.cql
  ```

Depois de identificar os requisitos da carga de trabalho existente, deve criar uma conta, uma base de dados e contentores do Azure Cosmos DB, de acordo com os requisitos de débito recolhidos.  

* **Determinar os custos de RU de uma operação:** pode determinar os RUs com o SDK da API para Cassandra do Azure Cosmos DB à sua escolha. Este exemplo mostra a versão do .NET dos encargos de obter RU.

  ```csharp
  var tableInsertStatement = table.Insert(sampleEntity);
  var insertResult = await tableInsertStatement.ExecuteAsync();

  foreach (string key in insertResult.Info.IncomingPayload)
    {
       byte[] valueInBytes = customPayload[key];
       string value = Encoding.UTF8.GetString(valueInBytes);
       Console.WriteLine($"CustomPayload:  {key}: {value}");
    }
  ```

* **Alocar o débito necessário:** o Azure Cosmos DB pode dimensionar automaticamente o armazenamento e débito à medida que os requisitos aumentam. Pode calcular as necessidades de débito com a [Calculadora de unidades de pedido do Azure Cosmos DB](https://www.documentdb.com/capacityplanner). 

## <a name="prerequisites-for-migration"></a>Pré-requisitos de migração

* **Criar tabelas na conta da API para Cassandra do Azure Cosmos DB:** antes de começar a migração de dados, crie previamente todas as tabelas do portal do Azure ou do cqlsh. Se estiver a migrar para uma conta do Azure Cosmos DB com débito ao nível da base de dados, confirme que proporciona uma chave de partição quando criar os contentores do Azure Cosmos DB.

* **Aumentar o débito:** a duração da migração de dados depende da quantidade de débito que aprovisionou para as tabelas no Azure Cosmos DB. Aumente o débito durante o período de migração. Com o débito mais elevado, pode evitar limitações de velocidade e realizar a migração em menos tempo. Após concluir a migração, reduza o débito para reduzir os custos. Para obter mais informações sobre como aumentar o débito, veja [configurar o débito](set-throughput.md) para contentores do Azure Cosmos DB. É também recomendado ter a conta do Azure Cosmos DB na mesma região da base de dados de origem. 

* **Ativar o SSL:** o Azure Cosmos DB tem requisitos e normas de segurança estritos. Lembre-se de ativar o SSL quando interagir com a sua conta. Quando utilizar o CQL com SSH, tem a opção de fornecer informações SSL.

## <a name="options-to-migrate-data"></a>Opções de migração de dados

Pode mover dados de cargas de trabalho existentes do Cassandra para o Azure Cosmos DB com as seguintes opções:

* [Com o comando cqlsh COPY](#using-cqlsh-copy-command)  
* [Com o Spark](#using-spark) 

## <a name="migrate-data-using-cqlsh-copy-command"></a>Migrar dados com o comando cqlsh COPY

O [comando CQL COPY](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) é utilizado para copiar dados locais para a conta da API para Cassandra do Azure Cosmos DB. Utilize os seguintes passos para copiar dados:

1. Obtenha as informações da cadeia de ligação da conta da API para Cassandra:

   * Inicie sessão no [portal do Azure](https://portal.azure.com) e navegue até à sua conta do Azure Cosmos DB.

   * Abra o painel **Cadeia de Ligação** que contém todas as informações necessárias para ligar à sua conta da API para Cassandra a partir do cqlsh.

2. Inicie a sessão no cqlsh com as informações de ligação do portal.

3. Utilize o comando CQL COPY para copiar dados locais para a conta da API para Cassandra.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="migrate-data-using-spark"></a>Migrar dados com o Spark 

Utilize os seguintes passos para migrar dados para a API para Cassandra do Azure Cosmos DB com o Spark:

- Aprovisionar um [Azure Databricks](cassandra-spark-databricks.md) ou um [cluster do HDInsight](cassandra-spark-hdinsight.md) 

- Mover dados para o ponto final da API para Cassandra de destino com uma [operação de cópia de tabela](cassandra-spark-table-copy-ops.md) 

A migração de dados com tarefas do Spark é uma opção recomendada se tiver dados que residem num cluster existente em máquinas virtuais do Azure ou qualquer outra cloud. Isto requer que o Spark esteja configurado como um intermediário para uma ocasião ou para ingestão regular. Pode acelerar a migração com a conectividade do expressroute entre o local e o Azure. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como migrar os dados de exemplo para a conta da API para Cassandra do Azure Cosmos DB. Agora, pode avançar para a secção Conceitos para obter mais informações sobre o Azure Cosmos DB. 

> [!div class="nextstepaction"]
> [Tunable data consistency levels in Azure Cosmos DB](../cosmos-db/consistency-levels.md) (Níveis de consistência de dados ajustáveis no Azure Cosmos DB)


