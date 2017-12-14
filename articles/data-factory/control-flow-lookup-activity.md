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
ms.date: 12/12/2017
ms.author: spelluru
ms.openlocfilehash: e0a1613f2f820f0c108e97c2c15585a581041181
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="lookup-activity-in-azure-data-factory"></a>Atividade de pesquisa no Azure Data Factory
A atividade de Pesquisa pode ser utilizada para ler ou procurar registos/nomes de tabelas/valores em qualquer origem externa. Este resultado pode ser ainda referenciado por atividades subsequentes. 

Atividade de pesquisa é útil quando pretender dinamicamente obter uma lista de ficheiros / registos/tabelas a partir de um ficheiro de configuração ou de uma origem de dados. A saída da atividade pode ser mais utilizada por outras atividades para efetuar específicas de processamento apenas esses itens.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [documentação do Data Factory V1](v1/data-factory-introduction.md).

## <a name="supported-capabilities"></a>Capacidades suportadas

As seguintes origens de dados são atualmente suportadas para a pesquisa:
- Ficheiro JSON no Blob do Azure
- Ficheiro JSON no sistema de ficheiros
- Base de dados SQL do Azure (convertidos a partir da consulta de dados JSON)
- O Azure SQL Data Warehouse (convertidos a partir da consulta de dados JSON)
- SQL Server (convertidos a partir da consulta de dados JSON)
- Table Storage do Azure (convertidos a partir da consulta de dados JSON)

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
Nome | Descrição | Tipo | Necessário
---- | ----------- | ---- | --------
Conjunto de dados | O atributo de conjunto de dados é fornecer a referência de conjunto de dados para a pesquisa. Atualmente, os tipos de conjunto de dados suportados são:<ul><li>`AzureBlobDataset`para [Blob Storage do Azure](connector-azure-blob-storage.md#dataset-properties) como origem</li><li>`FileShareDataset`para [sistema de ficheiros](connector-file-system.md#dataset-properties) como origem</li><li>`AzureSqlTableDataset`para [SQL Database do Azure](connector-azure-sql-database.md#dataset-properties) ou [Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md#dataset-properties) como origem</li><li>`SqlServerTable`para [do SQL Server](connector-sql-server.md#dataset-properties) como origem</li><li>`AzureTableDataset`para [Table Storage do Azure](connector-azure-table-storage.md#dataset-properties) como origem</li> | Par chave/valor | Sim
origem | Propriedades específicas do conjunto de dados de origem, mesmo que a origem de atividade de cópia. Saiba os detalhes da secção "Copiar propriedades da atividade" cada tópico conector correspondente. | Par chave/valor | Sim
firstRowOnly | Indique se pretende devolver a primeira linha ou todas as linhas. | Valor booleano | Não. Predefinição é `ture`.

## <a name="use-lookup-activity-result-in-subsequent-activity"></a>Utilizar o resultado de atividade de pesquisa na atividade subsequente

O resultado de pesquisa é devolvido no `output` secção o resultado de execução da atividade.

**Quando `firstRowOnly` está definido como `true` (predefinição)**, o formato de saída é o seguinte. O resultado de pesquisa está sob um fixo `firstRow` chave. Para utilizar o resultado na atividade subsequente, utilize o padrão de `@{activity('MyLookupActivity').output.firstRow.TableName}`.

```json
{
    "firstRow":
    {
        "Id": "1",
        "TableName" : "Table1"
    }
}
```

**Quando `firstRowOnly` está definido como `false`** , o foramt de saída é o seguinte. A `count` campo indica o número de registos são devolvidos, e os valores de detalhado estão em fixa `value` matriz. Esse caso, a atividade de pesquisa é, normalmente, seguida de um [atividade Foreach](control-flow-for-each-activity.md), pode passar o `value` matriz para a atividade ForEach `items` campo a utilizar o padrão de `@activity('MyLookupActivity').output.value`.

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
Neste exemplo, a atividade de cópia copia dados de uma tabela SQL na base de dados SQL do Azure para armazenamento de Blobs do Azure. O nome da tabela SQL é armazenado num ficheiro JSON no armazenamento de Blobs. A atividade de pesquisa procura o nome da tabela no tempo de execução. Esta abordagem permite JSON ser modificado dinamicamente se desejar pipelines/conjuntos de dados. 

Este exemplo demostrates procurar apenas a primeira linha. Para ver todas as linhas e cadeia com atividade ForEach, consulte [Tutorial - os dados de cópia em massa](tutorial-bulk-copy.md) exemplo.

### <a name="pipeline"></a>Pipeline
Este pipeline contém duas atividades: **procurar** e **cópia**. 

- A atividade de pesquisa está configurada para utilizar o LookupDataset, que se refere a uma localização de um Blob Storage do Azure. A atividade de pesquisa lê o nome da tabela SQL a partir de um ficheiro JSON nesta localização. 
- A atividade de cópia utiliza a saída da atividade de pesquisa (nome da tabela SQL). TableName no conjunto de dados de origem (SourceDataset) está configurado para utilizar a saída da atividade de pesquisa. A atividade de cópia copia dados da tabela SQL para uma localização de armazenamento de Blobs do Azure que é especificado pelo SinkDataset. 


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
O conjunto de dados de referência refere-se para o ficheiro de sourcetable.json na pasta de pesquisa no Storage do Azure especificado pelo AzureStorageLinkedService. 

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
O conjunto de dados de origem utiliza a saída da atividade de pesquisa, que é o nome da tabela SQL. A atividade de cópia copia os dados desta tabela de SQL para uma localização de armazenamento de Blobs do Azure especificada pelo conjunto de dados sink. 

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
A atividade de cópia copia dados da tabela SQL filebylookup.csv ficheiro na pasta de csv no Storage do Azure especificado pelo AzureStorageLinkedService. 

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

### <a name="azure-storage-linked-service"></a>Serviço Ligado do Storage do Azure
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
Esta base de dados SQL do Azure contém os dados para ser copiado para o armazenamento de Blobs. 

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

### <a name="sourcetablejson"></a>SourceTable.JSON

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

## <a name="next-steps"></a>Passos seguintes
Outras atividades de fluxo de controlo suportadas pela fábrica de dados, consulte: 

- [Atividade Executar Pipeline](control-flow-execute-pipeline-activity.md)
- [Para cada atividade](control-flow-for-each-activity.md)
- [Atividade Obter Metadados](control-flow-get-metadata-activity.md)
- [Atividade de Web](control-flow-web-activity.md)
