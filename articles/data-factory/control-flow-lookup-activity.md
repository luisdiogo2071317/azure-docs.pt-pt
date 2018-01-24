---
title: Atividade de pesquisa no Azure Data Factory | Microsoft Docs
description: Saiba como utilizar a atividade de pesquisa para procurar um valor de uma origem externa. Este resultado pode ser ainda referenciado por atividades subsequentes.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: shlo
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: spelluru
ms.openlocfilehash: 02e4d7cd062364cae2edad0c76e3a009bb6c1bda
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="lookup-activity-in-azure-data-factory"></a>Atividade de pesquisa no Azure Data Factory
Pode utilizar a atividade de pesquisa para ler ou procure um registo, o nome da tabela ou o valor de uma origem externa. Este resultado pode ser ainda referenciado por atividades subsequentes. 

Atividade de pesquisa é útil quando pretender dinamicamente obter uma lista de ficheiros, registos ou tabelas a partir de um ficheiro de configuração ou de uma origem de dados. A saída da atividade pode ser mais utilizada por outras atividades para efetuar específicas de processamento apenas esses itens.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Azure Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja [Data Factory version 1 documentation](v1/data-factory-introduction.md) (Documentação da versão 1 do Data Factory).

## <a name="supported-capabilities"></a>Capacidades suportadas

As seguintes origens de dados são atualmente suportadas para a pesquisa:
- Ficheiro JSON no Blob storage do Azure
- Ficheiro JSON no sistema de ficheiros
- Base de dados SQL do Azure (convertidos a partir da consulta de dados JSON)
- O Azure SQL Data Warehouse (convertidos a partir da consulta de dados JSON)
- SQL Server (convertidos a partir da consulta de dados JSON)
- Table storage do Azure (convertidos a partir da consulta de dados JSON)

O número máximo de linhas devolvidas pela atividade de pesquisa é **5000**e até **10Mb** de tamanho.

## <a name="syntax"></a>Sintaxe

```json
{
    "name": "LookupActivity",
    "type": "Lookup",
    "typeProperties": {
        "source": {
            "type": "<source type>"
            <additional source specific properties (optional)>
        },
        "dataset": { 
            "referenceName": "<source dataset name>",
            "type": "DatasetReference"
        },
        "firstRowOnly": false
    }
}
```

## <a name="type-properties"></a>Propriedades do tipo
Nome | Descrição | Tipo | Necessário?
---- | ----------- | ---- | --------
Conjunto de dados | Fornece a referência de conjunto de dados para a pesquisa. Atualmente, os tipos de conjunto de dados suportados são:<ul><li>`AzureBlobDataset`para [Blob storage do Azure](connector-azure-blob-storage.md#dataset-properties) como origem</li><li>`FileShareDataset`para [sistema de ficheiros](connector-file-system.md#dataset-properties) como origem</li><li>`AzureSqlTableDataset`para [SQL Database do Azure](connector-azure-sql-database.md#dataset-properties) ou [Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#dataset-properties) como origem</li><li>`SqlServerTable`para [do SQL Server](connector-sql-server.md#dataset-properties) como origem</li><li>`AzureTableDataset`para [Table storage do Azure](connector-azure-table-storage.md#dataset-properties) como origem</li> | Par chave/valor | Sim
origem | Contém propriedades específicas do conjunto de dados de origem, o mesmo que a origem de atividade de cópia. Obter os detalhes da secção "Copiar propriedades da atividade" cada artigo conector correspondente. | Par chave/valor | Sim
firstRowOnly | Indica se deve devolver a primeira linha ou todas as linhas. | Booleano | Não. Predefinição é `true`.

## <a name="use-the-lookup-activity-result-in-a-subsequent-activity"></a>Utilizar o resultado de atividade de pesquisa numa atividade subsequente

O resultado de pesquisa é devolvido no `output` secção o resultado de execução da atividade.

* **Quando `firstRowOnly` está definido como `true` (predefinição)**, o formato de saída é conforme mostrado no seguinte código. O resultado de pesquisa está sob um fixo `firstRow` chave. Para utilizar o resultado na atividade subsequente, utilize o padrão de `@{activity('MyLookupActivity').output.firstRow.TableName}`.

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "TableName" : "Table1"
        }
    }
    ```

* **Quando `firstRowOnly` está definido como `false`** , o formato de saída é conforme mostrado no seguinte código. A `count` campo indica o número de registos são devolvidos e valores detalhados são apresentados em fixa `value` matriz. Nesse caso, a atividade de pesquisa é, normalmente, seguida de um [atividade Foreach](control-flow-for-each-activity.md). Pode passar o `value` matriz para a atividade ForEach `items` campo utilizando o padrão de `@activity('MyLookupActivity').output.value`. Elementos de acesso no `value` matriz, use a seguinte sintaxe: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Eis um exemplo: `@{activity('lookupActivity').output.value[0].tablename}`.

    ```json
    {
        "count": "2",
        "value": [
            {
                "Id": "1",
                "TableName" : "Table1"
            },
            {
                "Id": "2",
                "TableName" : "Table2"
            }
        ]
    } 
    ```

## <a name="example"></a>Exemplo
Neste exemplo, a atividade de cópia copia os dados de uma tabela SQL na instância SQL Database do Azure para armazenamento de Blobs do Azure. O nome da tabela SQL é armazenado num ficheiro JSON no Blob storage. A atividade de pesquisa procura o nome da tabela no tempo de execução. Esta abordagem permite JSON ser modificado dinamicamente sem ter de voltar a implementar pipelines ou conjuntos de dados. 

Este exemplo demonstra pesquisa para a primeira linha. Para a pesquisa para todas as linhas e encadear os resultados com atividade ForEach, consulte exemplos [copiar várias tabelas em massa através do Azure Data Factory](tutorial-bulk-copy.md).

### <a name="pipeline"></a>Pipeline
Este pipeline contém duas atividades: *pesquisa* e *cópia*. 

- A atividade de pesquisa está configurada para utilizar LookupDataset, que se refere a uma localização de armazenamento de Blobs do Azure. A atividade de pesquisa lê o nome da tabela SQL a partir de um ficheiro JSON nesta localização. 
- A atividade de cópia utiliza a saída da atividade de pesquisa (nome da tabela SQL). A propriedade tableName no conjunto de dados de origem (SourceDataset) está configurada para utilizar a saída da atividade de pesquisa. A atividade de cópia copia dados da tabela SQL para uma localização de armazenamento de Blobs do Azure que é especificado pela propriedade SinkDataset. 


```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "dataset": { 
                        "referenceName": "LookupDataset", 
                        "type": "DatasetReference" 
                    }
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "typeProperties": {
                    "source": { 
                        "type": "SqlSource", 
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.firstRow.tableName}" 
                    },
                    "sink": { 
                        "type": "BlobSink" 
                    }
                },                
                "dependsOn": [ 
                    { 
                        "activity": "LookupActivity", 
                        "dependencyConditions": [ "Succeeded" ] 
                    }
                 ],
                "inputs": [ 
                    { 
                        "referenceName": "SourceDataset", 
                        "type": "DatasetReference" 
                    } 
                ],
                "outputs": [ 
                    { 
                        "referenceName": "SinkDataset", 
                        "type": "DatasetReference" 
                    } 
                ]
            }
        ]
    }
}
```

### <a name="lookup-dataset"></a>Conjunto de dados de pesquisa
O conjunto de dados de referência refere-se ao *sourcetable.json* ficheiro na pasta de pesquisa de armazenamento do Azure que é especificado pelo tipo AzureStorageLinkedService. 

```json
{
    "name": "LookupDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "lookup",
            "fileName": "sourcetable.json",
            "format": {
                "type": "JsonFormat",
                "filePattern": "SetOfObjects"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="source-dataset-for-the-copy-activity"></a>Conjunto de dados de origem para a atividade de cópia
O conjunto de dados de origem utiliza a saída da atividade de pesquisa, que é o nome da tabela SQL. A atividade de cópia copia dados a partir desta tabela SQL para uma localização no armazenamento de Blobs do Azure que é especificado pelo conjunto de dados do sink. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "type": "AzureSqlTable",
        "typeProperties":{
            "tableName": "@{activity('LookupActivity').output.firstRow.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-the-copy-activity"></a>Sink do conjunto de dados para a atividade de cópia
A atividade de cópia copia dados a partir da tabela de SQL Server para o *filebylookup.csv* ficheiros o *csv* pasta no armazenamento do Azure que é especificado pela propriedade AzureStorageLinkedService. 

```json
{
    "name": "SinkDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "csv",
            "fileName": "filebylookup.csv",
            "format": {
                "type": "TextFormat"                                                                    
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Serviço ligado do Storage do Azure
Esta conta de armazenamento contém o ficheiro JSON com os nomes das tabelas SQL. 

```json
{
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<StorageAccountKey>",
                "type": "SecureString"
            }
        }
    },
        "name": "AzureStorageLinkedService"
}
```

### <a name="azure-sql-database-linked-service"></a>Serviço ligado da Base de Dados SQL do Azure
Esta instância de base de dados do Azure SQL contém os dados para ser copiado para o Blob storage. 

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": {
                "value": "Server=<server>;Initial Catalog=<database>;User ID=<user>;Password=<password>;",
                "type": "SecureString"
            }
        }
    }
}
```

### <a name="sourcetablejson"></a>sourcetable.json

#### <a name="set-of-objects"></a>Conjunto de objetos

```json
{
  "Id": "1",
  "tableName": "Table1",
}
{
   "Id": "2",
  "tableName": "Table2",
}
```

#### <a name="array-of-objects"></a>Matriz de objetos

```json
[ 
    {
        "Id": "1",
          "tableName": "Table1",
    }
    {
        "Id": "2",
        "tableName": "Table2",
    }
]
```

## <a name="next-steps"></a>Passos Seguintes
Outras atividades de fluxo de controlo que são suportadas pela fábrica de dados, consulte: 

- [Executar a atividade do Pipeline](control-flow-execute-pipeline-activity.md)
- [Para cada atividade](control-flow-for-each-activity.md)
- [Obter os metadados atividade](control-flow-get-metadata-activity.md)
- [Atividade Web](control-flow-web-activity.md)
