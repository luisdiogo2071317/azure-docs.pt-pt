---
title: Atividade de pesquisa no Azure Data Factory | Documentos da Microsoft
description: Saiba como utilizar a atividade de pesquisa para procurar um valor de uma origem externa. Este resultado pode ser ainda referenciado por atividades subsequentes.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: shlo
ms.openlocfilehash: 25ed439674fcf7136e29034eb97e0652ae9ba111
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38237837"
---
# <a name="lookup-activity-in-azure-data-factory"></a>Atividade de pesquisa no Azure Data Factory

Atividade de pesquisa pode ser usada para recuperar um conjunto de dados a partir de qualquer origem de dados suportados de ADF.  Podem ser utilizada no seguinte cenário:
- Determinar quais os objetos (ficheiros, tabelas, etc.) a trabalhar numa atividade subsequente, em vez de embutir o nome do objeto dinamicamente

Atividade de pesquisa pode ler e retornar o conteúdo de um ficheiro de configuração, uma tabela de configuração ou o resultado da execução de uma consulta ou um procedimento armazenado.  A saída da atividade de pesquisa pode ser usada num subsequente cópia ou a atividade de transformação, se for um valor de singleton ou usada numa atividade ForEach, se for uma matriz de atributos.

## <a name="supported-capabilities"></a>Capacidades suportadas

São suportadas as seguintes origens de dados para a pesquisa. O número máximo de linhas pode ser devolvido pela pesquisa é de atividade **5000**e até **2MB** de tamanho. E atualmente a duração máxima para a atividade de pesquisa antes do tempo limite é de uma hora.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores-for-lookup-activity.md)]

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
conjunto de dados | Fornece a referência de conjunto de dados para a pesquisa. Obter os detalhes da secção "Propriedades do conjunto de dados" em cada artigo conector correspondente. | Par chave/valor | Sim
source | Contém as propriedades da origem de conjunto de dados específicas, o mesmo que a origem de atividade de cópia. Obter os detalhes da secção "Propriedades da atividade Copy" em cada artigo conector correspondente. | Par chave/valor | Sim
firstRowOnly | Indica se deve retornar apenas a primeira linha ou todas as linhas. | Booleano | Não. A predefinição é `true`.

**Tenha em atenção os seguintes pontos:**

1. Coluna de origem com o tipo de ByteArray não é suportada.
2. Estrutura não é suportada na definição do conjunto de dados. Para ficheiros de formato de texto especificamente, pode utilizar a linha de cabeçalho para fornecer o nome da coluna.
3. Se a sua origem de pesquisa é um ficheiro ou ficheiros JSON, o `jsonPathDefinition` definir para voltar a moldar o objeto JSON não é suportada, todo o objetos serão recuperados.

## <a name="use-the-lookup-activity-result-in-a-subsequent-activity"></a>Utilizar o resultado da atividade de pesquisa numa atividade subsequente

É devolvido o resultado de pesquisa no `output` seção a resultado da execução da atividade.

* **Quando `firstRowOnly` está definido como `true` (predefinição)**, o formato de saída é conforme mostrado no código a seguir. O resultado de pesquisa é fixa em `firstRow` chave. Para utilizar o resultado na atividade subsequente, utilize o padrão de `@{activity('MyLookupActivity').output.firstRow.TableName}`.

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "TableName" : "Table1"
        }
    }
    ```

* **Quando `firstRowOnly` está definido como `false`** , o formato de saída é conforme mostrado no código a seguir. R `count` campo indica o número de registos é devolvido e valores detalhadas são apresentadas em fixa `value` matriz. Nesse caso, a atividade de pesquisa geralmente é seguida por uma [atividade Foreach](control-flow-for-each-activity.md). Pode passar o `value` matriz para a atividade ForEach `items` campo ao utilizar o padrão de `@activity('MyLookupActivity').output.value`. Para elementos de acesso a `value` matriz, utilize a seguinte sintaxe: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Eis um exemplo: `@{activity('lookupActivity').output.value[0].tablename}`.

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
Neste exemplo, a atividade de cópia copia dados de uma tabela SQL na sua instância de base de dados do Azure SQL, para armazenamento de Blobs do Azure. O nome da tabela SQL é armazenado num ficheiro JSON no armazenamento de Blobs. A atividade de pesquisa procura o nome da tabela em tempo de execução. Essa abordagem permite que a JSON seja modificado, dinamicamente, sem ter de voltar a implementar pipelines ou conjuntos de dados. 

Este exemplo demonstra uma pesquisa na primeira linha. Para a pesquisa para todas as linhas e encadear os resultados com a atividade ForEach, veja os exemplos [copiar várias tabelas em massa com o Azure Data Factory](tutorial-bulk-copy.md).

### <a name="pipeline"></a>Pipeline
Este pipeline contém duas atividades: *lookup* e *cópia*. 

- A atividade de pesquisa está configurada para utilizar LookupDataset, que se refere a uma localização no armazenamento de Blobs do Azure. A atividade de pesquisa lê o nome da tabela SQL a partir de um ficheiro JSON nesta localização. 
- A atividade de cópia utiliza a saída da atividade de pesquisa (nome da tabela SQL). A propriedade tableName no conjunto de dados de origem (SourceDataset) está configurada para utilizar a saída da atividade lookup. A atividade de cópia copia dados da tabela SQL para uma localização no armazenamento de Blobs do Azure que é especificado pela propriedade SinkDataset. 


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
O conjunto de dados de pesquisa refere-se para o *sourcetable.json* ficheiro na pasta de pesquisa do armazenamento do Azure, que é especificado pelo tipo AzureStorageLinkedService. 

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
O conjunto de dados de origem utiliza a saída da atividade de pesquisa, que é o nome da tabela SQL. A atividade de cópia copia dados desta tabela de SQL para uma localização no armazenamento de Blobs do Azure especificada pelo conjunto de dados de sink. 

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

### <a name="sink-dataset-for-the-copy-activity"></a>Conjunto de dados para a atividade de cópia de sink
A atividade de cópia copia dados da tabela SQL para o *filebylookup.csv* de ficheiros a *csv* pasta no armazenamento do Azure, que é especificado pela propriedade AzureStorageLinkedService. 

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
Esta instância de base de dados do Azure SQL contém os dados para ser copiado para armazenamento de Blobs. 

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
  "tableName": "Table1"
}
{
   "Id": "2",
  "tableName": "Table2"
}
```

#### <a name="array-of-objects"></a>Matriz de objetos

```json
[ 
    {
        "Id": "1",
        "tableName": "Table1"
    },
    {
        "Id": "2",
        "tableName": "Table2"
    }
]
```

## <a name="next-steps"></a>Passos Seguintes
Consulte outras atividades de fluxo de controle que são suportadas pelo Data Factory: 

- [Atividade execute Pipeline](control-flow-execute-pipeline-activity.md)
- [Para cada atividade](control-flow-for-each-activity.md)
- [Atividade obter metadados](control-flow-get-metadata-activity.md)
- [Atividade Web](control-flow-web-activity.md)
