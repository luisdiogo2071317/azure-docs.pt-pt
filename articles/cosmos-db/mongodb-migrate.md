---
title: Migrar os dados da MongoDB para o Azure Cosmos DB com o mongoimport e mongorestore
description: Aprenderá como usar o mongoimport e mongorestore para importar dados para Cosmos DB.
keywords: mongoimport, mongorestore
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: rimman
ms.custom: mvc
Customer intent: As a developer, I want to migrate the data from my existing MongoDB to Cosmos DB.
ms.openlocfilehash: 4cd30c7981cd6807113729292db403a80cbddef0
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793755"
---
# <a name="migrate-your-mongodb-data-to-azure-cosmos-db"></a>Migre os seus dados do MongoDB ao Azure Cosmos DB

 Este tutorial fornece instruções sobre como migrar os dados armazenados no MongoDB ao Azure Cosmos DB configurado para utilizar a API do Cosmos DB para o MongoDB. Se importar dados do MongoDB e pretende utilizá-la com a API de SQL do Azure Cosmos DB, deve utilizar o [ferramenta de migração de dados](import-data.md) para importar os dados.

Neste tutorial, irá:

> [!div class="checklist"]
> * Prepare um plano de migração.
> * Migre os dados utilizando o mongoimport.
> * Migre dados por meio da mongorestore.

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Reveja e conclua os seguintes pré-requisitos antes de iniciar a migração.

### <a name="plan-for-the-migration"></a>Plano para a migração

Esta secção descreve como planear a migração de dados. Vamos estimar os custos de RU, determinar a latência a partir do seu computador para o serviço em nuvem e calcular o tamanho de lote e o número de trabalhadores de inserção.


#### <a name="pre-create-and-scale-your-collections"></a>Pré-criar e dimensionar as suas coleções

Antes de migrar com mongoimport ou mongorestore, pré-criar todas as suas coleções a partir da [portal do Azure](https://portal.azure.com) ou a partir de controladores do MongoDB e ferramentas. 

Partir do [portal do Azure](https://portal.azure.com), aumentar o débito de coleções para a migração. Com um débito mais elevado, pode evitar que está a ser limitada de taxa e migrar em menos tempo. Pode reduzir o débito imediatamente após a migração, para reduzir os custos.

Para além do débito de aprovisionamento com um nível de coleção, também pode de aprovisionar o débito ao nível da base de dados num conjunto de coleções para partilhar o débito aprovisionado. Terá de criar previamente as coleções e a base de dados e definir uma chave de partição horizontal para cada coleção da base de dados da taxa de transferência partilhada.

Pode criar coleções em partição horizontal com sua ferramenta preferencial, o driver ou o SDK. Neste exemplo, utilizamos a shell do Mongo para criar uma coleção fragmentada:

```bash
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
O comando devolve os seguintes resultados:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

#### <a name="calculate-the-approximate-ru-charge-for-a-single-document-write"></a>Calcular o custo de RU aproximado de escrita de um único documento

A partir da Shell de MongoDB, ligue à sua conta de Cosmos configurada para utilizar a API do Cosmos DB para MongoDB. Pode encontrar instruções em [ligar uma aplicação do MongoDB para o Cosmos DB](connect-mongodb-account.md).

Em seguida, execute um comando de inserção de exemplo utilizando um dos seus documentos de exemplo:
   
```bash
db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
```
        
Execute o comando `db.runCommand({getLastRequestStatistics: 1})`.

Receber uma resposta como a seguinte saída:
     
```bash
globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "insert",
    "RequestCharge": 10,
    "RequestDurationInMilliSeconds": NumberLong(50)
}
```
        
Tome nota do custo do pedido.
    
#### <a name="determine-the-latency-from-your-machine-to-cosmos-db"></a>Determinar a latência da sua máquina ao Cosmos DB
    
Ativar o registo verboso a partir da Shell de MongoDB com o comando `setVerboseShell(true)`.
    
Executar uma consulta básica na base de dados com o comando `db.coll.find().limit(1)`.

Receber uma resposta como a seguinte saída:

```bash
Fetched 1 record(s) in 100(ms)
```
        
Antes de executar a migração, remova o documento inserido para se certificar de que documentos não duplicado. Pode remover documentos com o comando `db.coll.remove({})`.

#### <a name="calculate-the-approximate-values-for-the-batchsize-and-numinsertionworkers-properties"></a>Calcular os valores aproximados para as propriedades batchSize e numInsertionWorkers

Para o **batchSize** propriedade, divisão total aprovisionado (RUs/s) de débito pelos RUs consumidos para escrita um único documento, como concluída na seção "Determinar a latência da sua máquina ao Cosmos DB." Se o valor calculado é inferior ou igual a 24, use esse número como o valor da propriedade. Se o valor calculado for superior a 24, defina o valor da propriedade para 24.
    
O valor do **numInsertionWorkers** propriedade, utilize esta equação:

`numInsertionWorkers = (Provisioned RUs throughput * Latency in seconds) / (batchSize * Consumed RUs for a single write)`

Podemos usar os seguintes valores para calcular um valor para o **numInsertionWorkers** propriedade:

| Propriedade | Valor |
|--------|-----|
| **batchSize** | 24 |
| RUs aprovisionados | 10,000 |
| Latência | 0,100 s |
| RUs consumidas | 10 RUs |
| **numInsertionWorkers** | (10 000 RUs x 0.100 s) / (24 x 10 RUs) = **4.1666** |

Executar o **monogoimport** comando de migração. Os parâmetros de comando são descritos neste artigo.

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
```

Também pode utilizar o **monogorestore** comando. Certifique-se de todas as coleções têm o débito definido no ou acima do número de Urs usado nos cálculos de anteriores.
   
```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
```

### <a name="complete-the-prerequisites"></a>Concluir os pré-requisitos

Depois de planear para a migração, conclua os seguintes passos: 

* **Obter dados de exemplo**: Certifique-se de que tem alguns dados de exemplo, antes de iniciar a migração. 

* **Aumentar o débito**: A duração da sua migração de dados depende da quantidade de débito que Aprovisiona para uma coleção individual ou a base de dados. Aumente o débito para migrações de dados maiores. Depois de concluir a migração, reduzir a taxa de transferência para reduzir os custos. 

* **Ativar o SSL**:  O cosmos DB tem requisitos de segurança rigorosas e padrões. Certifique-se de que ativar o SSL quando interage com a sua conta do Cosmos. Os procedimentos neste artigo incluem como ativar o SSL para os comandos mongoimport e mongorestore.

* **Criar recursos do Cosmos DB**: Antes de começar a migração, pré-crie todas as suas coleções a partir do portal do Azure. Se migrar para uma conta do Cosmos com débito aprovisionado de nível de base de dados, certifique-se fornecer uma chave de partição quando cria as coleções.

* **Obter a cadeia de ligação**: Na [portal do Azure](https://portal.azure.com), selecione a **do Azure Cosmos DB** entrada à esquerda. Sob **subscrições**, selecione o nome da sua conta. Sob **cadeia de ligação**, selecione **cadeia de ligação**. À direita do portal mostra as informações tem de se ligar à sua conta:

    ![Informações da Cadeia de Ligação](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="use-mongoimport"></a>Utilizar o mongoimport

Para importar dados para a sua conta do Cosmos, utilize o modelo seguinte.

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Substitua a \<your_hostname >, \<your_username >, e \<your_password > parâmetros com os valores específicos para a sua conta. No exemplo a seguir, usamos **sampleDB** como o valor de \<your_database >, e **sampleColl** como o valor de \<your_collection >:

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="use-mongorestore"></a>Utilizar mongorestore

Para restaurar dados para a sua conta do Cosmos configurada com a API do Cosmos DB para o MongoDB, usam o modelo seguinte para executar a importação.

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Substitua a \<your_hostname >, \<your_username >, e \<your_password > parâmetros com os valores específicos para a sua conta. No exemplo a seguir, usamos **./dumps/dump-2016-12-07** como o valor de \<path_to_backup >:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --db mydatabase --collection mycollection --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não precisar dos recursos, pode eliminar o grupo de recursos, conta do Cosmos e todos os recursos relacionados. Utilize os seguintes passos para eliminar o grupo de recursos:

1. Vá para o grupo de recursos onde criou a conta do Cosmos.
1. Selecione **Eliminar grupo de recursos**.
1. Confirme o nome do grupo de recursos a eliminar e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Continue para o próximo tutorial para saber como consultar dados com a API do Azure Cosmos DB do MongoDB. 

> [!div class="nextstepaction"]
> [Como consultar dados do MongoDB](../cosmos-db/tutorial-query-mongodb.md)
