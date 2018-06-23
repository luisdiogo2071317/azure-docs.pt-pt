---
title: Copiar dados de MySQL utilizando o Azure Data Factory | Microsoft Docs
description: Saiba mais sobre o conector de MySQL no Azure Data Factory que lhe permite copiar dados de uma base de dados MySQL para um arquivo de dados suportado como um sink.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/23/2018
ms.author: jingwang
ms.openlocfilehash: 43a27b98d8b53523bee8694ed3071e65a03355a6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335878"
---
# <a name="copy-data-from-mysql-using-azure-data-factory"></a>Copiar dados de MySQL utilizando o Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - GA](v1/data-factory-onprem-mysql-connector.md)
> * [Versão 2 - Pré-visualização](connector-mysql.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de uma base de dados MySQL. Baseia-se no [copiar descrição geral da atividade](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [conector MySQL no V1](v1/data-factory-onprem-mysql-connector.md).

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados da base de dados MySQL para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector MySQL suporta MySQL **versão 5.1 e acima**.

## <a name="prerequisites"></a>Pré-requisitos

Se a base de dados MySQL não está acessível publicamente, terá de configurar um tempo de execução de integração Self-hosted. Para saber mais sobre tempos de execução automática alojada integração, consulte [Self-hosted integração Runtime](create-self-hosted-integration-runtime.md) artigo. O tempo de execução de integração fornece um controlador MySQL incorporado a partir da versão 3.7, por conseguinte, não precisa de instalar manualmente quaisquer controladores.

Para a versão de resposta a incidentes Self-hosted menor 3.7, tem de instalar o [MySQL conector/Net para Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) versão entre 6.6.5 e 6.10.7 na máquina de tempo de execução de integração. Este controlador de 32 bits é compatível com IR. de 64 bits

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o conector de MySQL.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço de MySQL ligada:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida: **MySql** | Sim |
| connectionString | Especifique as informações necessárias para estabelecer ligação à base de dados do Azure para a instância de MySQL. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Sim |
| connectVia | O [integração Runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar um tempo de execução de Self-hosted integração (se o arquivo de dados esteja localizado numa rede privada) ou o Runtime de integração do Azure. Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

Uma cadeia de ligação típico é `Server=<server>;Port=<port>;Database=<database>;UID=<username>;PWD=<password>`. Propriedades que pode ser definidas por seu incidente:

| Propriedade | Descrição | Opções | Necessário |
|:--- |:--- |:--- |:--- |:--- |
| SSLMode | Esta opção especifica se o controlador utiliza encriptação SSL e verificação ao ligar a MySQL. Por exemplo, `SSLMode=<0/1/2/3/4>`| DESATIVADO (0) / preferencial (1) **(predefinida)** / necessária (2) / VERIFY_CA (3) / VERIFY_IDENTITY (4) | Não |
| useSystemTrustStore | Esta opção especifica se pretende utilizar um certificado de AC do arquivo de confiança de sistema ou de um ficheiro PEM especificado. Por exemplo, `UseSystemTrustStore=<0/1>;`| (1) de ativado / desativado (0) **(predefinida)** | Não |

**Exemplo:**

```json
{
    "name": "MySQLLinkedService",
    "properties": {
        "type": "MySql",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Port=<port>;Database=<database>;UID=<username>;PWD=<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Se estava a utilizar o serviço de MySQL ligado com o seguinte payload, ainda é suportado como-é, enquanto são sugeridos para utilizar um novo passa.

**Payload anterior:**

```json
{
    "name": "MySQLLinkedService",
    "properties": {
        "type": "MySql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o artigo de conjuntos de dados. Esta secção fornece uma lista de propriedades suportadas por conjunto de dados MySQL.

Para copiar dados de MySQL, defina a propriedade de tipo do conjunto de dados para **RelationalTable**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida: **RelationalTable** | Sim |
| tableName | Nome da tabela na base de dados MySQL. | Não (se for especificada "consulta" na origem de atividade) |

**Exemplo**

```json
{
    "name": "MySQLDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<MySQL linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportadas por origem MySQL.

### <a name="mysql-as-source"></a>MySQL como origem

Para copiar dados de MySQL, defina o tipo de origem na atividade de cópia para **RelationalSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida: **RelationalSource** | Sim |
| consulta | Utilize a consulta SQL personalizada para ler os dados. Por exemplo: `"SELECT * FROM MyTable"`. | Não (se for especificado "tableName" no conjunto de dados) |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromMySQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MySQL input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "RelationalSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-mysql"></a>Tipo de dados de mapeamento para o MySQL

Quando copiar dados de MySQL, os seguintes mapeamentos são utilizados MySQL tipos de dados para tipos de dados intermédio do Azure Data Factory. Consulte [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md) para saber mais sobre como atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink.

| Tipo de dados MySQL | Tipo de dados intermédio de fábrica de dados |
|:--- |:--- |
| `bigint` |`Int64` |
| `bigint unsigned` |`Decimal` |
| `bit` |`Decimal` |
| `blob` |`Byte[]` |
| `bool` |`Boolean` |
| `char` |`String` |
| `date` |`Datetime` |
| `datetime` |`Datetime` |
| `decimal` |`Decimal` |
| `double` |`Double` |
| `double precision` |`Double` |
| `enum` |`String` |
| `float` |`Single` |
| `int` |`Int32` |
| `int unsigned` |`Int64`|
| `integer` |`Int32` |
| `integer unsigned` |`Int64` |
| `long varbinary` |`Byte[]` |
| `long varchar` |`String` |
| `longblob` |`Byte[]` |
| `longtext` |`String` |
| `mediumblob` |`Byte[]` |
| `mediumint` |`Int32` |
| `mediumint unsigned` |`Int64` |
| `mediumtext` |`String` |
| `numeric` |`Decimal` |
| `real` |`Double` |
| `set` |`String` |
| `smallint` |`Int16` |
| `smallint unsigned` |`Int32` |
| `text` |`String` |
| `time` |`TimeSpan` |
| `timestamp` |`Datetime` |
| `tinyblob` |`Byte[]` |
| `tinyint` |`Int16` |
| `tinyint unsigned` |`Int16` |
| `tinytext` |`String` |
| `varchar` |`String` |
| `year` |`Int` |


## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia no Azure Data Factory, consulte [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
