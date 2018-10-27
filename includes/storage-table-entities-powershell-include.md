---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: e8c5bf8e3c4cd63b7eec278c480527e95455140d
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165378"
---
<!--created by Robin Shahan to go in the articles for table storage w/powershell.
    There is one for Azure Table Storage and one for Azure Cosmos DB Table API -->

## <a name="managing-table-entities"></a>Gerir entidades da tabela

Agora que tem uma tabela, vamos analisar como gerir entidades ou linhas na tabela. 

Uma entidade pode ter até 255 propriedades, incluindo 3 Propriedades do sistema: **PartitionKey**, **RowKey**, e **Timestamp**. É responsável por inserir e atualizar os valores dos **PartitionKey** e **RowKey**. O servidor gere o valor de **Timestamp**, que não pode ser modificado. Em conjunto a **PartitionKey** e **RowKey** identificam todas as entidades dentro de uma tabela.

* **PartitionKey**: determina a partição que a entidade é armazenada no.
* **RowKey**: identifica de forma exclusiva a entidade na partição.

Pode definir até 252 propriedades personalizadas para uma entidade. 

### <a name="add-table-entities"></a>Adicionar entidades de tabelas

Adicionar entidades a uma tabela com **Add-StorageTableRow**. Estes exemplos utilizam chaves de partição com valores "partition1" e "partition2" e chaves de linha iguais a abreviaturas de estado. As propriedades de cada entidade que são o ID de utilizador e nome de utilizador. 

```powershell
$partitionKey1 = "partition1"
$partitionKey2 = "partition2"

# add four rows 
Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey1 `
    -rowKey ("CA") -property @{"username"="Chris";"userid"=1}

Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey2 `
    -rowKey ("NM") -property @{"username"="Jessie";"userid"=2}

Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey1 `
    -rowKey ("WA") -property @{"username"="Christine";"userid"=3}

Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey2 `
    -rowKey ("TX") -property @{"username"="Steven";"userid"=4}
```

### <a name="query-the-table-entities"></a>Consultar as entidades da tabela

Existem várias formas diferentes de consultar as entidades numa tabela.

#### <a name="retrieve-all-entities"></a>Obter todas as entidades

Para obter todas as entidades, utilize **Get-AzureStorageTableRowAll**.

```powershell
Get-AzureStorageTableRowAll -table $storageTable | ft
```

Este comando produz resultados semelhantes a tabela seguinte:

| ID de utilizador | o nome de utilizador | partição | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Partition1 | AC |
| 3 | Christine | Partition1 | WA |
| 2 | Jessie | Partition2 | NM |
| 4 | Steven | Partition2 | TX |

#### <a name="retrieve-entities-for-a-specific-partition"></a>Obter entidades para uma partição específica

Para obter todas as entidades numa partição específica, utilize **Get-AzureStorageTableRowByPartitionKey**.

```powershell
Get-AzureStorageTableRowByPartitionKey -table $storageTable -partitionKey $partitionKey1 | ft
```
Os resultados ter um aspeto semelhantes a tabela seguinte:

| ID de utilizador | o nome de utilizador | partição | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Partition1 | AC |
| 3 | Christine | Partition1 | WA |

#### <a name="retrieve-entities-for-a-specific-value-in-a-specific-column"></a>Obter entidades para um valor específico numa coluna de específico

Para obter entidades em que o valor numa coluna específica é igual a um determinado valor, utilize **Get-AzureStorageTableRowByColumnName**.

```powershell
Get-AzureStorageTableRowByColumnName -table $storageTable `
    -columnName "username" `
    -value "Chris" `
    -operator Equal
```

Esta consulta devolve um registo.

|Campo|valor|
|----|----|
| ID de utilizador | 1 |
| o nome de utilizador | Chris |
| PartitionKey | Partition1 |
| RowKey      | AC |

#### <a name="retrieve-entities-using-a-custom-filter"></a>Obter entidades utilizando um filtro personalizado 

Para obter as entidades com um filtro personalizado, utilize **Get-AzureStorageTableRowByCustomFilter**.

```powershell
Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter "(userid eq 1)"
```

Esta consulta devolve um registo.

|Campo|valor|
|----|----|
| ID de utilizador | 1 |
| o nome de utilizador | Chris |
| PartitionKey | Partition1 |
| RowKey      | AC |

### <a name="updating-entities"></a>A atualização de entidades 

Existem três passos para a atualização de entidades. Em primeiro lugar, obter a entidade para alterar. Em segundo lugar, efetue a alteração. Em terceiro lugar, consolidar a alteração usando **AzureStorageTableRow atualização**.

Atualizar a entidade com o nome de utilizador = Jessie ter o nome de utilizador = 'Jessie2'. Este exemplo também mostra outra forma de criar um filtro personalizado usando tipos .NET. 

```powershell
# Create a filter and get the entity to be updated.
[string]$filter = `
    [Microsoft.WindowsAzure.Storage.Table.TableQuery]::GenerateFilterCondition("username",`
    [Microsoft.WindowsAzure.Storage.Table.QueryComparisons]::Equal,"Jessie")
$user = Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter $filter

# Change the entity.
$user.username = "Jessie2" 

# To commit the change, pipe the updated record into the update cmdlet.
$user | Update-AzureStorageTableRow -table $storageTable 

# To see the new record, query the table.
Get-AzureStorageTableRowByCustomFilter -table $storageTable `
    -customFilter "(username eq 'Jessie2')"
```

Os resultados mostram o registo de Jessie2.

|Campo|valor|
|----|----|
| ID de utilizador | 2 |
| o nome de utilizador | Jessie2 |
| PartitionKey | Partition2 |
| RowKey      | NM |

### <a name="deleting-table-entities"></a>A eliminação de entidades da tabela

Pode eliminar uma entidade ou todas as entidades na tabela.

#### <a name="deleting-one-entity"></a>A eliminar uma entidade

Para eliminar uma entidade única, obter uma referência a essa entidade e encaminhá-la em **Remove-AzureStorageTableRow**.

```powershell
# Set filter.
[string]$filter = `
  [Microsoft.WindowsAzure.Storage.Table.TableQuery]::GenerateFilterCondition("username",`
  [Microsoft.WindowsAzure.Storage.Table.QueryComparisons]::Equal,"Jessie2")

# Retrieve entity to be deleted, then pipe it into the remove cmdlet.
$userToDelete = Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter $filter
$userToDelete | Remove-AzureStorageTableRow -table $storageTable 

# Retrieve entities from table and see that Jessie2 has been deleted.
Get-AzureStorageTableRowAll -table $storageTable | ft
```

#### <a name="delete-all-entities-in-the-table"></a>Eliminar todas as entidades da tabela 

Para eliminar todas as entidades na tabela, recuperá-los e encaminhar os resultados para o cmdlet remove. 

```powershell
# Get all rows and pipe the result into the remove cmdlet.
Get-AzureStorageTableRowAll `
    -table $storageTable | Remove-AzureStorageTableRow -table $storageTable 

# List entities in the table (there won't be any).
Get-AzureStorageTableRowAll -table $storageTable | ft
```
