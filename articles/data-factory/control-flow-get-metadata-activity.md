---
title: Obter os metadados atividade no Azure Data Factory | Microsoft Docs
description: Saiba como pode utilizar a atividade de procedimento armazenado do SQL Server para invocar um procedimento armazenado numa SQL Database do Azure ou do Azure SQL Data Warehouse a partir de um pipeline do Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: shlo
ms.openlocfilehash: c07199887faf073d19007f1ef410c193bbdbf3ee
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049371"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Obter os metadados atividade no Azure Data Factory
GetMetadata atividade pode ser utilizada para obter **metadados** de quaisquer dados no Azure Data Factory. Esta atividade pode ser utilizada nos seguintes cenários:

- Validar as informações de metadados de quaisquer dados
- Acionar um pipeline, quando os dados pronto / disponíveis

A seguinte funcionalidade está disponível no fluxo de controlo:

- A saída da atividade de GetMetadata pode ser utilizada em expressões condicionais para efetuar a validação.
- Um pipeline pode ser acionado quando a condição é satisfeita através de efetue-até ciclo

## <a name="supported-capabilities"></a>Capacidades suportadas

A atividade de GetMetadata demora um conjunto de dados como uma entrada necessária e produz informações de metadados disponíveis como saída da atividade. Atualmente, os conectores com meatadata recuperável correspondente que se seguem são suportados:

>[!NOTE]
>Se executar a atividade de GetMetadata num tempo de execução de integração Self-hosted, a capacidade mais recente é suportada na versão 3.6 ou superior. 

### <a name="supported-connectors"></a>Conectores suportados

**Armazenamento de ficheiros:**

| Conector/metadados | itemName<br>(ficheiro/pasta) | itemType<br>(ficheiro/pasta) | tamanho<br>(ficheiro) | criado<br>(ficheiro/pasta) | lastModified<br>(ficheiro/pasta) |childItems<br>(pasta) |contentMD5<br>(ficheiro) | estrutura<br/>(ficheiro) | columnCount<br>(ficheiro) | existe<br>(ficheiro/pasta) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Amazon S3 | √/√ | √/√ | √ | x/x | √ / √ * | √ | x | √ | √ | √ / √ * |
| Blob do Azure | √/√ | √/√ | √ | x/x | √ / √ * | √ | √ | √ | √ | √/√ |
| Azure Data Lake Store | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| Armazenamento de Ficheiros do Azure | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| Sistema de Ficheiros | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| SFTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| FTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |

- Para o Amazon S3, o `lastModified` aplica-se ao registo e a chave, a pasta não virtual; mas; e o `exists` aplica-se ao registo e chave, mas não para o prefixo ou pasta virtual.
- Para o Blob do Azure, o `lastModified` aplica-se ao contentor e BLOBs mas pasta não virtual.

**Base de dados relacional:**

| Conector/metadados | estrutura | columnCount | existe |
|:--- |:--- |:--- |:--- |
| Base de Dados SQL do Azure | √ | √ | √ |
| Azure SQL Data Warehouse | √ | √ | √ |
| SQL Server | √ | √ | √ |

### <a name="metadata-options"></a>Opções de metadados

Os seguintes tipos de metadados podem ser especificados na lista de campos de atividade GetMetadata obter:

| Tipo de metadados | Descrição |
|:--- |:--- |
| itemName | Nome do ficheiro ou pasta. |
| itemType | Tipo de ficheiro ou pasta. O valor de saída é `File` ou `Folder`. |
| tamanho | Tamanho do ficheiro em bytes. Aplicável apenas de ficheiros. |
| criado | Criado datetime do ficheiro ou pasta. |
| lastModified | Última modificação datetime do ficheiro ou pasta. |
| childItems | Lista de subpastas e ficheiros nessa pasta especificada. Aplicável apenas a pasta. O valor de saída é uma lista de nome e tipo de cada item subordinado. |
| contentMD5 | MD5 do ficheiro. Aplicável apenas de ficheiros. |
| estrutura | Estrutura de dados dentro do ficheiro ou uma tabela de base de dados relacional. O valor de saída é uma lista de nomes de coluna e tipo de coluna. |
| columnCount | Número de colunas dentro do ficheiro ou tabela relacional. |
| existe| Se um ficheiro/pasta/tabela existe ou não. Tenha em atenção que "existe" seja especificado na lista de campos de GetaMetadata, a atividade não irá falhar, mesmo quando o item (ficheiro/pasta/tabela) não existe; em vez disso, devolve `exists: false` na saída. |

>[!TIP]
>Especificar quando pretende validar se um ficheiro/pasta/tabela existe ou não, `exists` na lista de campos de atividade GetMetadata, em seguida, pode verificar o `exists: true/false` resultar da saída da atividade. Se `exists` não está configurada na lista de campos, GetMetadata atividade irão falhar quando o objeto não foi encontrado.

## <a name="syntax"></a>Sintaxe

**GetMetadata atividade:**

```json
{
    "name": "MyActivity",
    "type": "GetMetadata",
    "typeProperties": {
        "fieldList" : ["size", "lastModified", "structure"],
        "dataset": {
            "referenceName": "MyDataset",
            "type": "DatasetReference"
        }
    }
}
```

**Conjunto de dados:**

```json
{
    "name": "MyDataset",
    "properties": {
    "type": "AzureBlob",
        "linkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath":"container/folder",
            "filename": "file.json",
            "format":{
                "type":"JsonFormat"
            }
        }
    }
}
```

## <a name="type-properties"></a>Propriedades do tipo

Atualmente GetMetadata atividade pode obter os seguintes tipos de informações de metadados.

Propriedade | Descrição | Necessário
-------- | ----------- | --------
fieldList | Lista os tipos de informações de metadados necessários. Consulte os detalhes na [opções de metadados](#metadata-options) secção nos metadados suportados. | Sim 
Conjunto de dados | O conjunto de dados de referência, cujo metadados da atividade é possível obter pela atividade GetMetadata. Consulte [capacidades suportadas](#supported-capabilities) secção no conectores suportados e consulte a tópico conector nos detalhes de sintaxe do conjunto de dados. | Sim

## <a name="sample-output"></a>Resultado da amostra

O resultado de GetMetadata é apresentado na saída da atividade. Seguem-se dois exemplos com opções de metadados exaustiva selecionados na lista de campos como referência. Para utilizar o resultado na atividade subsequente, utilize o padrão de `@{activity('MyGetMetadataActivity').output.itemName}`.

### <a name="get-a-files-metadata"></a>Obter os metadados de um ficheiro

```json
{
  "exists": true,
  "itemName": "test.csv",
  "itemType": "File",
  "size": 104857600,
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "contentMD5": "cMauY+Kz5zDm3eWa9VpoyQ==",
  "structure": [
    {
        "name": "id",
        "type": "Int64"
    },
    {
        "name": "name",
        "type": "String"
    }
  ],
  "columnCount": 2
}
```

### <a name="get-a-folders-metadata"></a>Obter os metadados de uma pasta

```json
{
  "exists": true,
  "itemName": "testFolder",
  "itemType": "Folder",
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "childItems": [
    {
      "name": "test.avro",
      "type": "File"
    },
    {
      "name": "folder hello",
      "type": "Folder"
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes
Outras atividades de fluxo de controlo suportadas pela fábrica de dados, consulte: 

- [Atividade Executar Pipeline](control-flow-execute-pipeline-activity.md)
- [Para cada atividade](control-flow-for-each-activity.md)
- [Atividade de Pesquisa](control-flow-lookup-activity.md)
- [Atividade de Web](control-flow-web-activity.md)
