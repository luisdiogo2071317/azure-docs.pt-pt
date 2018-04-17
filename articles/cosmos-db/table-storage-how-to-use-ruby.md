---
title: Como utilizar o Table Storage do Azure e a API de tabela de base de dados do Azure Cosmos com Ruby | Microsoft Docs
description: Armazene dados estruturados na nuvem através do Table Storage do Azure, um arquivo de dados NoSQL.
services: cosmos-db
documentationcenter: ruby
author: SnehaGunda
manager: kfile
editor: ''
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 04/05/2018
ms.author: sngun
ms.openlocfilehash: 19ffdab40b3032421612ef4ba1b840eeb0d2e62b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-use-azure-table-storage-and-azure-cosmos-db-table-api-with-ruby"></a>Como utilizar o Table Storage do Azure e a API de tabela de base de dados do Azure Cosmos com Ruby
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Descrição geral
Este guia mostra como efetuar cenários comuns utilizando o serviço de tabelas do Azure e a API de tabela de base de dados do Azure Cosmos. Os exemplos são escritos no Ruby e utilize o [biblioteca cliente de tabela de armazenamento do Azure para Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table). Os cenários abrangidos incluem **criar e eliminar uma tabela e a inserção e consultar entidades numa tabela**.

## <a name="create-an-azure-service-account"></a>Criar uma conta de serviço do Azure
[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Criar uma conta do Storage do Azure
[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Criar uma conta de API de tabela de base de dados do Azure Cosmos
[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="add-access-to-storage-or-azure-cosmos-db"></a>Adicione o acesso a armazenamento ou base de dados do Azure Cosmos
Para utilizar o Storage do Azure ou a base de dados do Azure Cosmos, tem de transferir e utilizar o pacote de Ruby Azure inclui um conjunto de bibliotecas de conveniência que comunicam com os serviços de REST de tabela.

### <a name="use-rubygems-to-obtain-the-package"></a>Utilizar RubyGems para obter o pacote
1. Utilize uma interface de linha de comandos, como **PowerShell** (Windows), **Terminal** (Mac), ou **Bash** (Unix).
2. Tipo **gem instalar tabela de armazenamento do azure** na janela de comando para instalar o gem e dependências.

### <a name="import-the-package"></a>Importar o pacote
Utilize o editor de texto favorito, adicione o seguinte na parte superior do ficheiro Ruby em que pretende utilizar o armazenamento:

```ruby
require "azure/storage/table"
```

## <a name="add-an-azure-storage-connection"></a>Adicionar uma ligação de armazenamento do Azure
O módulo de armazenamento do Azure lê as variáveis de ambiente **AZURE_STORAGE_ACCOUNT** e **AZURE_STORAGE_ACCESS_KEY** de informações necessárias para ligar à sua conta do Storage do Azure. Se estas variáveis de ambiente não estiver definidas, tem de especificar as informações de conta antes de utilizar **Azure::Storage::Table::TableService** com o seguinte código:

```ruby
Azure.config.storage_account_name = "<your Azure Storage account>"
Azure.config.storage_access_key = "<your Azure Storage access key>"
```

Para obter estes valores a partir de um clássico ou conta de armazenamento do Resource Manager no portal do Azure:

1. Inicie sessão no [Portal do Azure](https://portal.azure.com).
2. Navegue para a conta de armazenamento que pretende utilizar.
3. No painel definições à direita, clique em **chaves de acesso**.
4. No painel de chaves de acesso que aparece, verá a chave de acesso 1 e a chave de acesso 2. Pode utilizar qualquer um destes.
5. Clique no ícone de cópia para copiar a chave para a área de transferência.

## <a name="add-an-azure-cosmos-db-connection"></a>Adicionar uma ligação de base de dados do Azure Cosmos
Para ligar à base de dados do Azure Cosmos, copie a cadeia de ligação principal do portal do Azure e criar um **cliente** objeto utilizando a cadeia de ligação copiado. Pode passar o **cliente** objeto quando cria um **TableService** objeto:

```ruby
common_client = Azure::Storage::Common::Client.create(storage_account_name:'myaccount', storage_access_key:'mykey', storage_table_host:'mycosmosdb_endpoint')
table_client = Azure::Storage::Table::TableService.new(client: common_client)
```

## <a name="create-a-table"></a>Criar uma tabela
O **Azure::Storage::Table::TableService** objeto permite-lhe trabalhar com tabelas e entidades. Para criar uma tabela, utilize o **create_table()** método. O exemplo seguinte cria uma tabela ou imprima o erro se existir alguma.

```ruby
azure_table_service = Azure::Storage::Table::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-to-a-table"></a>Adicionar uma entidade a uma tabela
Para adicionar uma entidade, primeiro crie um objeto de hash, que define as propriedades de entidade. Tenha em atenção que para cada entidade tem de especificar um **PartitionKey** e **RowKey**. Estes são os identificadores exclusivos da sua entidades e valores que podem ser consultados muito mais rapidamente do que as outras propriedades. Storage do Azure utiliza **PartitionKey** para distribuir automaticamente o entidades da tabela através de vários nós de armazenamento. Entidades com o mesmo **PartitionKey** são armazenados no mesmo nó. O **RowKey** é o ID exclusivo da entidade na partição pertence.

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a>Atualizar uma entidade
Existem vários métodos disponíveis para atualizar uma entidade existente:

* **update_entity():** atualizar uma entidade existente, substituindo-lo.
* **merge_entity():** atualiza uma entidade existente, a intercalação novos valores de propriedade para a entidade existente.
* **insert_or_merge_entity():** atualiza uma entidade existente, substituindo-lo. Se nenhuma entidade de existir, será inserido um novo:
* **insert_or_replace_entity():** atualiza uma entidade existente, a intercalação novos valores de propriedade para a entidade existente. Se nenhuma entidade de existir, será inserido um novo.

O exemplo seguinte demonstra a atualizar uma entidade utilizando **update_entity()**:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

Com **update_entity()** e **merge_entity()**, se a entidade que estão a atualizar não existir, a operação de atualização irá falhar. Por conseguinte, se pretender armazenar uma entidade, independentemente de já existir, deve em vez disso, utilizar **insert_or_replace_entity()** ou **insert_or_merge_entity()**.

## <a name="work-with-groups-of-entities"></a>Trabalhar com grupos de entidades
Por vezes, faz sentido para submeter várias operações em conjunto num batch para garantir atómico processamento pelo servidor. Para realizar que, se crie primeiro um **Batch** de objeto e, em seguida, utilizar o **execute_batch()** método no **TableService**. O exemplo seguinte demonstra submeter duas entidades com RowKey 2 e 3 num batch. Tenha em atenção que só funciona para entidades com o mesmo PartitionKey.

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a>Consulta de uma entidade
Para consultar uma entidade numa tabela, utilize o **get_entity()** método, transferindo o nome da tabela, **PartitionKey** e **RowKey**.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Consulta de um conjunto de entidades
Para consultar um conjunto de entidades numa tabela, crie um objeto de hash de consulta e utilize o **query_entities()** método. O exemplo seguinte demonstra a obter todas as entidades com o mesmo **PartitionKey**:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Se o conjunto de resultados é demasiado grande para uma única consulta devolver, um token de continuação é devolvido que pode utilizar para obter as páginas subsequentes.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Consultar um subconjunto de propriedades de entidade
Uma consulta a uma tabela pode obter apenas algumas propriedades de uma entidade. Esta técnica, denominada "projeção," reduz a largura de banda e pode melhorar o desempenho de consulta, especialmente para entidades grandes. Utilize a cláusula select e passar os nomes das propriedades que pretende colocar do cliente.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Eliminar uma entidade
Para eliminar uma entidade, utilize o **delete_entity()** método. Passar no nome da tabela que contém a entidade, o PartitionKey e RowKey da entidade.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Eliminar uma tabela
Para eliminar uma tabela, utilize o **delete_table()** método e passar no nome da tabela que pretende eliminar.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Passos Seguintes

* O [Explorador de Armazenamento do Microsoft Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md) é uma aplicação autónoma e gratuita da Microsoft, que lhe permite trabalhar visualmente com dados do Armazenamento do Azure no Windows, macOS e Linux.
* [Centro de programadores de Ruby](https://azure.microsoft.com/develop/ruby/)
* [Biblioteca de clientes do Microsoft Azure Storage tabela para Ruby](https://github.com/azure/azure-storage-ruby/tree/master/table) 

