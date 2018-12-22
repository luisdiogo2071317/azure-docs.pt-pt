---
title: 'Tutorial: Utilizar mongoimport e mongorestore com a API do Azure Cosmos DB para MongoDB | Microsoft Docs'
description: Neste tutorial, saiba como utilizar o mongoimport e mongorestore para importar dados para uma API para uma conta do MongoDB.
keywords: mongoimport, mongorestore
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: sngun
ms.custom: mvc
Customer intent: As a developer, I want to migrate the data from my existing MongoDB workloads to a MongoDB API account in Azure Cosmos DB, so that the overhead to manage resources is handled by Azure Cosmos DB.
ms.openlocfilehash: b40ca7cf62127a95fe277e205e5a5c5d36618828
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753851"
---
# <a name="tutorial-migrate-your-data-to-a-mongodb-api-account-in-azure-cosmos-db"></a>Tutorial: Migre os seus dados para uma conta de API do MongoDB no Azure Cosmos DB

Como desenvolvedor, pode ter aplicações que utilizam dados de documentos NoSQL. Pode utilizar uma conta de API do MongoDB no Azure Cosmos DB para armazenar e aceder a estes dados de documento. Também pode migrar dados de seus aplicativos existentes para a API do MongoDB. Este tutorial fornece instruções sobre como migrar os dados armazenados no MongoDB para uma conta de API do MongoDB no Azure Cosmos DB. Se importar dados do MongoDB e pretende utilizá-la com a API de SQL do Azure Cosmos DB, deve utilizar o [ferramenta de migração de dados](import-data.md) para importar os dados.

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

Por predefinição, o Azure Cosmos DB aprovisiona uma coleção do MongoDB nova com mil unidades de pedido por segundo (RU/seg.). Antes de migrar com mongoimport ou mongorestore, pré-criar todas as suas coleções a partir da [portal do Azure](https://portal.azure.com) ou a partir de controladores do MongoDB e ferramentas. Se o tamanho dos dados for superior a 10 GB, crie uma coleção particionada com uma chave de partição horizontal adequada.

Partir do [portal do Azure](https://portal.azure.com), aumentar o débito de coleções para a migração de 1000 RUs/seg para uma coleção de partição única e 2.500 RUs/seg para uma coleção em partição horizontal. Com o débito mais elevado, pode evitar limitações de velocidade e realizar a migração em menos tempo. Pode reduzir o débito imediatamente após a migração, para reduzir os custos.

Além de aprovisionamento RUs/seg ao nível da coleção, também pode aprovisionar RUs/seg para um conjunto de coleções no nível de base de dados principal. No nível principal, tem de pré-criar a base de dados e coleções e definir uma chave de partição horizontal para cada coleção.

Pode criar coleções em partição horizontal, utilizando a ferramenta preferencial, o driver ou o SDK. Neste exemplo, utilizamos a shell do Mongo para criar uma coleção fragmentada:

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

A partir da Shell de MongoDB, ligue à sua conta de API do MongoDB no Azure Cosmos DB. Pode encontrar instruções em [Connect a MongoDB application to Azure Cosmos DB](connect-mongodb-account.md) (Ligar uma aplicação MongoDB ao Azure Cosmos DB).

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
    
#### <a name="determine-the-latency-from-your-machine-to-the-azure-cosmos-db-cloud-service"></a>Determinar a latência a partir do seu computador para o serviço de cloud do Azure Cosmos DB
    
Ativar o registo verboso a partir da Shell de MongoDB com o comando `setVerboseShell(true)`.
    
Executar uma consulta básica na base de dados com o comando `db.coll.find().limit(1)`.

Receber uma resposta como a seguinte saída:

```bash
Fetched 1 record(s) in 100(ms)
```
        
Antes de executar a migração, remova o documento inserido para se certificar de que documentos não duplicado. Pode remover documentos com o comando `db.coll.remove({})`.

#### <a name="calculate-the-approximate-values-for-the-batchsize-and-numinsertionworkers-properties"></a>Calcular os valores aproximados para as propriedades batchSize e numInsertionWorkers

Para o **batchSize** propriedade, divisão total aprovisionado RUs pelos RUs consumidos de escrita seu único documento, como concluída na seção "Determinar a latência a partir do seu computador para o serviço de cloud do Azure Cosmos DB." Se o valor calculado é inferior ou igual a 24, use esse número como o valor da propriedade. Se o valor calculado for superior a 24, defina o valor da propriedade para 24.
    
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

* **Obter dados de exemplo**: Certifique-se de que tem alguns dados do MongoDB de exemplo, antes de iniciar a migração. Se não tiver uma base de dados do MongoDB de exemplo, pode transferir e instalar o [servidor de Comunidade do MongoDB](https://www.mongodb.com/download-center), criar uma base de dados de exemplo e utilizar o mongoimport.exe ou mongorestore.exe para carregar dados de exemplo. 

* **Aumentar o débito**: A duração da sua migração de dados depende da quantidade de débito que configura para uma coleção individual ou um conjunto de coleções. Aumente o débito para migrações de dados maiores. Depois de concluir a migração, reduzir a taxa de transferência para reduzir os custos. 

* **Ativar o SSL**: Azure Cosmos DB tem requisitos de segurança rigorosas e padrões. Lembre-se de ativar o SSL quando interagir com a sua conta. Os procedimentos neste artigo incluem como ativar o SSL para os comandos mongoimport e mongorestore.

* **Criar recursos do Azure Cosmos DB**: Antes de começar a migração, pré-crie todas as suas coleções a partir do portal do Azure. Se migrar para uma conta do Azure Cosmos DB com o débito de nível de base de dados, certifique-se fornecer uma chave de partição quando cria as coleções do Azure Cosmos DB.

* **Obter a cadeia de ligação**: Na [portal do Azure](https://portal.azure.com), selecione a **do Azure Cosmos DB** entrada à esquerda. Sob **subscrições**, selecione o nome da sua conta. Sob **cadeia de ligação**, selecione **cadeia de ligação**. À direita do portal mostra as informações tem de se ligar à sua conta:

    ![Informações da Cadeia de Ligação](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="use-mongoimport"></a>Utilizar o mongoimport

Para importar dados para a sua conta do Azure Cosmos DB, utilize o modelo seguinte.

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

Substitua a \<your_hostname >, \<your_username >, e \<your_password > parâmetros com os valores específicos para a sua conta. No exemplo a seguir, usamos **sampleDB** como o valor de \<your_database >, e **sampleColl** como o valor de \<your_collection >:

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="use-mongorestore"></a>Utilizar mongorestore

Para restaurar dados para a sua API para a conta do MongoDB, utilize o modelo seguinte para executar a importação.

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

Substitua a \<your_hostname >, \<your_username >, e \<your_password > parâmetros com os valores específicos para a sua conta. No exemplo a seguir, usamos **./dumps/dump-2016-12-07** como o valor de \<path_to_backup >:

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --db mydatabase --collection mycollection --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não precisar dos recursos, pode eliminar o grupo de recursos, conta do Azure Cosmos DB e todos os recursos relacionados. Utilize os seguintes passos para eliminar o grupo de recursos:

1. Vá para o grupo de recursos onde criou a conta do Azure Cosmos DB.
1. Selecione **Eliminar grupo de recursos**.
1. Confirme o nome do grupo de recursos a eliminar e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Continue para o próximo tutorial para saber como consultar dados do MongoDB com o Azure Cosmos DB. 

> [!div class="nextstepaction"]
> [Como consultar dados do MongoDB](../cosmos-db/tutorial-query-mongodb.md)
