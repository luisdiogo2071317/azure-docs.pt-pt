---
title: Importar dados do Cassandra para o Azure Cosmos DB | Microsoft Docs
description: Aprenda como utilizar o comando CQL Copy para copiar dados do Cassandra para o Azure Cosmos DB.
services: cosmos-db
author: govindk
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: govindk
ms.custom: mvc
ms.openlocfilehash: 73c9f1fc26f5cb36cc475a66b67705c6177bebf8
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37095677"
---
# <a name="azure-cosmos-db-import-cassandra-data"></a>Azure Cosmos DB: importar dados do Cassandra

Este tutorial dá instruções sobre como importar dados do Cassandra para o Cosmos DB ao utilizar o comando COPY do Cassandra Query Language (CQL). 

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * A obter a sua cadeia de ligação
> * Importar dados ao utilizar o comando cqlsh COPY
> * Importar com o conector Spark 

# <a name="prerequisites"></a>Pré-requisitos

* Instale o [Apache Cassandra](http://cassandra.apache.org/download/) e certifique-se especificamente que o *cqlsh* está presente.
* Débito aumentado: a duração da migração de dados depende da quantidade de débito que aprovisionou para as suas Tabelas. Aumente o débito para migrações de dados maiores. Após concluir a migração, reduza o débito para reduzir os custos. Para obter mais informações sobre como aumentar o débito no [portal do Azure](https://portal.azure.com), veja [Configurar débito para contentores do Azure Cosmos DB](set-throughput.md).
* Ativar o SSL: o Azure Cosmos DB tem requisitos e normas de segurança estritos. Lembre-se de ativar o SSL quando interagir com a sua conta. Quando utilizar o CQL com SSH, tem a opção de fornecer informações SSL. 

## <a name="find-your-connection-string"></a>Encontrar a sua cadeia de ligação

1. No [Portal do Azure](https://portal.azure.com), na parte da esquerda, clique em **Azure Cosmos DB**.

2. No painel **Subscrições**, selecione o nome da sua conta.

3. Clique em **Cadeia de Ligação**. O painel do lado direito contém todas as informações de que precisa para se ligar com êxito à sua conta.

    ![Página de cadeia de ligação](./media/cassandra-import-data/keys.png)

## <a name="use-cqlsh-copy"></a>Utilizar o cqlsh COPY

Para importar os dados do Cassandra para o Azure Cosmos DB para utilizar com a API para Cassandra, utilize as seguintes diretrizes:

1. Inicie a sessão no cqhsh ao utilizar as informações de ligação do portal.
2. Utilize o [comando CQL COPY](http://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) para copiar dados locais para o ponto final API para Apache Cassandra. Certifique-se de que a origem e o destino estão no mesmo datacenter para minimizar os problemas de latência.

### <a name="guide-for-moving-data-with-cqlsh"></a>Guia para mover dados com o cqlsh

1. Crie previamente e dimensione a sua tabela:
    * Por predefinição, o Azure Cosmos DB aprovisiona uma nova tabela do API para Cassandra com 1.000 unidades de pedido por segundo (RU/s) (a criação baseada em CQL é aprovisionada com 400 RU/s). Antes de iniciar a migração ao utilizar cqlsh, pré-crie todas as suas tabelas do [portal do Azure](https://portal.azure.com) ou a partir de cqlsh. 

    * Do [portal do Azure](https://portal.azure.com), aumente o débito das tabelas de débito predefinido (400 ou 1000 RU/s) para 10 000 RU/s para a duração da migração. Com o débito mais elevado, pode evitar limitações de velocidade e realizar a migração em menos tempo. Com a faturação à hora no Azure Cosmos DB, pode reduzir o débito imediatamente após a migração, para reduzir os custos.

2. Determine os encargos de RU durante uma operação. Pode fazê-lo ao utilizar o SDK da API do Azure Cosmos DB Cassandra à sua escolha. Este exemplo mostra a versão do .NET dos encargos de obter RU. 

    ```csharp
    var tableInsertStatement = table.Insert(sampleEntity);
    var insertResult = await tableInsertStatement.ExecuteAsync();

    foreach (string key in insertResult.Info.IncomingPayload)
            {
                byte[] valueInBytes = customPayload[key];
                string value = Encoding.UTF8.GetString(valueInBytes);
                Console.WriteLine($“CustomPayload:  {key}: {value}”);
            }
 
    ``` 

3. Determine a latência do seu computador para o serviço cloud do Azure Cosmos DB. Se estiver dentro de um datacenter do Azure, a latência pode ser um número de milissegundos baixo de um algarismo. Se estiver fora do Datacenter do Azure, pode utilizar psping ou azurespeed.com para obter a latência aproximada da sua localização.   

4. Calcule os valores certos para os parâmetros (NUMPROCESS, INGESTRATE, MAXBATCHSIZE ou MINBATCHSIZE) que forneçam um bom desempenho. 

5. Execute o comando final de migração. Executar este comando assume que iniciou cqlsh com as informações de cadeia de ligação.

   ```
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="use-spark-to-import-data"></a>Utilize o Spark para importar dados

Para os dados num cluster existente nas máquinas virtuais do Azure, importar dados com o Spark também é uma opção exequível. Isto requer que o Spark esteja configurado como um intermediário para uma ocasião ou para ingestão regular. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu a concluir as seguintes tarefas:

> [!div class="checklist"]
> * Obter a sua cadeia de ligação
> * Importar dados com o comando de cópia cql
> * Importar com o conector Spark 

Agora, pode avançar para a secção Conceitos para obter mais informações sobre o Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Tunable data consistency levels in Azure Cosmos DB](../cosmos-db/consistency-levels.md) (Níveis de consistência de dados ajustáveis no Azure Cosmos DB)
