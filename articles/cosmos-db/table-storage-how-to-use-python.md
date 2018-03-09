---
title: "Introdução ao Table storage do Azure com o Python | Microsoft Docs"
description: "Armazene dados estruturados na nuvem através do Table Storage do Azure, um arquivo de dados NoSQL."
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/08/2018
ms.author: mimig
ms.openlocfilehash: 455479c9eb77093dd5611263fe5bdcf699b9d026
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="get-started-with-azure-table-storage-using-python"></a>Introdução ao Table storage do Azure com o Python

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

O Armazenamento de tabelas do Azure é um serviço que armazena dados NoSQL estruturados na cloud, fornecendo uma loja de atributos/chaves com um design sem esquemas. Uma vez que o Table Storage não tem esquemas, é fácil adaptar os seus dados à medida que as necessidades da sua aplicação evoluem. O acesso a dados do Armazenamento de tabelas é rápido e rentável para muitos tipos de aplicações, e é normalmente mais económico do que o SQL tradicional para semelhantes volumes de dados.

Pode utilizar o Armazenamento de Tabelas para armazenar conjuntos de dados flexíveis, como os dados do utilizador para aplicações Web, livros de endereços, informações do dispositivo, ou outros tipo de metadados necessários para o seu serviço. Pode armazenar quantas entidades pretender numa tabela e uma conta do Storage pode conter quantas tabelas pretender, até ao limite de capacidade da conta do Storage.

### <a name="about-this-tutorial"></a>Acerca deste tutorial
Este tutorial mostra como utilizar o [Azure Cosmos DB tabela SDK para Python](https://pypi.python.org/pypi/azure-cosmosdb-table/) em cenários comuns do armazenamento de tabelas do Azure. Indica o nome do SDK é para utilização com o Azure Cosmos DB, mas funciona com a base de dados do Cosmos ambas do Azure e o armazenamento de tabelas do Azure, cada serviço tem apenas um único ponto de final. Estes cenários são explorou utilizando os exemplos de Python que ilustram como:
* Criar e eliminar tabelas
* Inserir e a consulta de entidades
* Modificar entidades

Ao trabalhar com os cenários neste tutorial, pode querer fazer referência ao [Azure Cosmos DB SDK para referência da API de Python](https://azure.github.io/azure-cosmosdb-python/).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial com êxito, precisa do seguinte:

- [Python](https://www.python.org/downloads/) 2.7, 3.3, 3.4, 3.5 ou 3.6
- [Azure Cosmos DB tabela SDK para Python](https://pypi.python.org/pypi/azure-cosmosdb-table/). Este SDK liga ao Table storage do Azure e a API de tabela de base de dados do Azure Cosmos.
- [Conta de armazenamento do Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account#create-a-storage-account) ou [conta de base de dados do Azure Cosmos](https://azure.microsoft.com/en-us/try/cosmosdb/)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="create-an-azure-service-account"></a>Criar uma conta de serviço do Azure

Pode trabalhar com tabelas utilizando o Table storage do Azure ou a base de dados do Azure Cosmos. Pode saber mais sobre as diferenças entre os serviços ao ler [tabela ofertas](table-introduction.md#table-offerings). Terá de criar uma conta para o serviço que vai utilizar. 

### <a name="create-an-azure-storage-account"></a>Criar uma conta do Storage do Azure
A forma mais fácil de criar a primeira conta de armazenamento do Azure é com o [portal do Azure](https://portal.azure.com). Para saber mais, veja [Criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md#create-a-storage-account).

Também pode criar uma conta de armazenamento do Azure, utilizando [Azure PowerShell](../storage/common/storage-powershell-guide-full.md) ou [CLI do Azure](../storage/common/storage-azure-cli.md).

Se preferir não criar uma conta de armazenamento neste momento, pode também utilizar o emulador de armazenamento do Azure para executar e testar o seu código num ambiente local. Para obter mais informações, veja [Utilizar o Emulador de Armazenamento do Azure para Programação e Teste](../storage/common/storage-use-emulator.md).

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Criar uma conta de API de tabela de base de dados do Azure Cosmos

Para obter instruções sobre como criar uma conta de API de tabela do Azure Cosmos DB, consulte [criar uma conta de API de tabela](create-table-dotnet.md#create-a-database-account).

## <a name="install-the-azure-cosmos-db-table-sdk-for-python"></a>Instalar a tabela de base de dados do Cosmos do Azure SDK para Python

Depois de criar uma conta de armazenamento, o próximo passo é instalar o [Microsoft Azure Cosmos DB tabela SDK para Python](https://pypi.python.org/pypi/azure-cosmosdb-table/). Para obter detalhes sobre como instalar o SDK, consulte o [README.rst](https://github.com/Azure/azure-cosmosdb-python/blob/master/azure-cosmosdb-table/README.rst) ficheiro no SDK da tabela do Cosmos DB para o repositório de Python no GitHub.

## <a name="import-the-tableservice-and-entity-classes"></a>Importar as classes TableService e entidade

Para trabalhar com entidades no serviço de Azure Table no Python, pode utilizar o [TableService](https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html) e [entidade] [ py_Entity] classes. Adicione este código perto da parte superior do ficheiro de Python para importar ambas:

```python
from azure.cosmosdb.table.tableservice import TableService
from azure.cosmosdb.table.models import Entity
```

## <a name="connect-to-azure-table-service"></a>Ligar ao serviço de tabelas do Azure

Para ligar ao serviço de tabela de armazenamento do Azure, crie um [TableService](https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html) de objeto e passar na sua chave de conta e o nome da conta de armazenamento. Substitua `myaccount` e `mykey` com o nome da conta e a chave.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')
```

## <a name="connect-to-azure-cosmos-db"></a>Ligar ao Azure Cosmos DB

Para ligar à base de dados do Azure Cosmos, copie a cadeia de ligação principal do portal do Azure e criar um [TableService](https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html) utilizando a cadeia de ligação copiados de objeto:

```python
table_service = TableService(connection_string='DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;TableEndpoint=myendpoint;)
```

## <a name="create-a-table"></a>Criar uma tabela

Chamar [create_table] [ py_create_table] para criar a tabela.

```python
table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Adicionar uma entidade a uma tabela

Para adicionar uma entidade, tem primeiro de criar um objeto que representa a entidade, em seguida, passa o objeto para o [TableService.insert_entity método][py_TableService]. O objeto de entidade pode ser um dicionário ou um objeto do tipo [entidade][py_Entity]e define os valores e nomes de propriedade da entidade. Cada entidade tem de incluir necessários [PartitionKey e RowKey](#partitionkey-and-rowkey) propriedades, para além de quaisquer outras propriedades a definir para a entidade.

Este exemplo cria um objeto de dicionário que representa uma entidade, em seguida, transmite-as para o [insert_entity] [ py_insert_entity] método para adicioná-lo para a tabela:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

Este exemplo cria um [entidade] [ py_Entity] objeto, em seguida, passa-o para o [insert_entity] [ py_insert_entity] método para adicioná-lo para a tabela:

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a>PartitionKey e RowKey

Tem de especificar um **PartitionKey** e um **RowKey** propriedade para cada entidade. Estes são os identificadores exclusivos da sua entidades, como em conjunto constituem a chave primária de uma entidade. Pode consultar ao utilizar estes valores muito mais rápidos do que pode consultar as propriedades de entidade porque estas propriedades só são indexadas.

As utilizações de serviço tabela **PartitionKey** inteligentemente distribuir as entidades da tabela em nós de armazenamento. Entidades que tenham o mesmo **PartitionKey** são armazenados no mesmo nó. **RowKey** é o ID exclusivo da entidade na partição pertence.

## <a name="update-an-entity"></a>Atualizar uma entidade

Para atualizar todos os valores de propriedade de uma entidade, chame o [update_entity] [ py_update_entity] método. Este exemplo mostra como substituir uma entidade existente com uma versão atualizada:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

Se a entidade que está a ser atualizada já não existir, a operação de atualização irá falhar. Se pretende armazenar uma entidade se existe ou não, utilizar [insert_or_replace_entity][py_insert_or_replace_entity]. No exemplo seguinte, a primeira chamada substituirá a entidade existente. A segunda chamada inserir uma nova entidade, dado que nenhuma entidade com o PartitionKey especificado e RowKey não existe na tabela.

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> O [update_entity] [ py_update_entity] método substitui todas as propriedades e valores de uma entidade existente, também pode utilizar para remover propriedades de uma entidade existente. Pode utilizar o [merge_entity] [ py_merge_entity] método para atualizar uma entidade existente com os valores de propriedade novas ou modificada sem completamente substituir a entidade.

## <a name="modify-multiple-entities"></a>Modificar várias entidades

Para garantir o processamento de um pedido atómico pelo serviço tabela, pode submeter várias operações em conjunto num batch. Em primeiro lugar, utilize o [TableBatch] [ py_TableBatch] classe para adicionar várias operações para um único lote. Em seguida, chame [TableService][py_TableService].[ commit_batch] [ py_commit_batch] para submeter as operações de uma operação atómica. Todas as entidades de ser modificados no batch tem de ser a mesma partição.

Este exemplo adiciona duas entidades em conjunto num batch:

```python
from azure.cosmosdb.table.tablebatch import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

Lotes também podem ser utilizados com a sintaxe do Gestor de contexto:

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a>Consulta de uma entidade

Para consultar uma entidade numa tabela, passar o respetivo PartitionKey e RowKey para o [TableService][py_TableService].[ get_entity] [ py_get_entity] método.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Consulta de um conjunto de entidades

Pode consultar um conjunto de entidades, fornecendo uma cadeia de filtro com o **filtro** parâmetro. Neste exemplo localiza todas as tarefas em Seattle aplicando um filtro em PartitionKey:

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Consultar um subconjunto de propriedades de entidade

Também pode restringir as propriedades que são devolvidas para cada entidade numa consulta. Esta técnica, denominada *projecção*, reduz a largura de banda e pode melhorar o desempenho de consulta, especialmente para entidades grandes ou conjuntos de resultados. Utilize o **selecione** parâmetro e pass, os nomes das propriedades que pretende devolvidos ao cliente.

A consulta no seguinte código devolve apenas as descrições de entidades na tabela.

> [!NOTE]
> O fragmento seguinte funciona apenas com o Storage do Azure. Não é suportada pelo emulador de armazenamento.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Eliminar uma entidade

Eliminar uma entidade transferindo o **PartitionKey** e **RowKey** para o [delete_entity] [ py_delete_entity] método.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Eliminar uma tabela

Se já não necessita de uma tabela ou qualquer uma das entidades dentro da mesma, chame o [delete_table] [ py_delete_table] método para eliminar permanentemente a tabela do Storage do Azure.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Passos Seguintes

* [FAQ – desenvolver com a tabela de API](https://docs.microsoft.com/en-us/azure/cosmos-db/faq#develop-with-the-table-api)
* [SDK de BD do Cosmos do Azure para referência da API de Python](https://azure.github.io/azure-cosmosdb-python/)
* [Centro para Programadores do Python](https://azure.microsoft.com/develop/python/)
* [Explorador de armazenamento do Microsoft Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md): uma aplicação gratuita de várias plataformas para trabalhar visualmente com dados de armazenamento do Azure no Windows, macOS e Linux.
* [Trabalhar com o Python no Visual Studio (Windows)](https://docs.microsoft.com/en-us/visualstudio/python/overview-of-python-tools-for-visual-studio)

[py_commit_batch]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_create_table]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_delete_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_delete_table]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_Entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.models.html
[py_get_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_insert_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_insert_or_replace_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_TableService]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_TableBatch]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tablebatch.html
[py_merge_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_update_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
