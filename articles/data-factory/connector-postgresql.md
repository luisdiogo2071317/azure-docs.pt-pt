---
title: Copiar dados de PostgreSQL utilizando o Azure Data Factory | Microsoft Docs
description: Saiba como copiar dados de PostgreSQL aos arquivos de dados dependente suportados através da utilização de uma atividade de cópia no pipeline Azure Data Factory.
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
ms.openlocfilehash: 436725e54e197e021f088e0fd0cd75fc944983a3
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751382"
---
# <a name="copy-data-from-postgresql-by-using-azure-data-factory"></a>Copiar dados de PostgreSQL através da utilização do Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - GA](v1/data-factory-onprem-postgresql-connector.md)
> * [Versão 2 - Pré-visualização](connector-postgresql.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de uma base de dados PostgreSQL. Baseia-se no [copiar descrição geral da atividade](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.


> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [conector PostgreSQL no V1](v1/data-factory-onprem-postgresql-connector.md).

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de base de dados PostgreSQL para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector PostgreSQL suporta PostgreSQL **versão 7.4 e acima**.

## <a name="prerequisites"></a>Pré-requisitos

Se a base de dados PostgreSQL não está acessível publicamente, terá de configurar um tempo de execução de integração Self-hosted. Para saber mais sobre tempos de execução automática alojada integração, consulte [Self-hosted integração Runtime](create-self-hosted-integration-runtime.md) artigo. O tempo de execução de integração fornece um controlador PostgreSQL incorporado a partir da versão 3.7, por conseguinte, não precisa de instalar manualmente quaisquer controladores.

Para a versão de resposta a incidentes Self-hosted menor 3.7, tem de instalar o [Ngpsql data provider para PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) com a versão entre 2.0.12 e 3.1.9 na máquina de tempo de execução de integração.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o conector PostgreSQL.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço de PostgreSQL ligada:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida: **PostgreSql** | Sim |
| connectionString | Uma cadeia de ligação de ODBC para ligar à base de dados do Azure para PostgreSQL. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Sim |
| connectVia | O [integração Runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração Self-hosted ou Runtime de integração do Azure (se o arquivo de dados acessível publicamente). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

Uma cadeia de ligação típico é `Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`. Propriedades que pode ser definidas por seu incidente:

| Propriedade | Descrição | Opções | Necessário |
|:--- |:--- |:--- |:--- |:--- |
| EncryptionMethod (IT)| Utiliza o método de controlador para encriptar os dados enviados entre o controlador e o servidor de base de dados. Por exemplo, `ValidateServerCertificate=<0/1/6>;`| 0 (sem encriptação) **(predefinida)** / 1 (SSL) / 6 (RequestSSL) | Não |
| ValidateServerCertificate (VSC) | Determina se o controlador valida o certificado que é enviado pelo servidor de base de dados quando estiver ativada a encriptação SSL (método de encriptação = 1). Por exemplo, `ValidateServerCertificate=<0/1>;`| 0 (desativado) **(predefinida)** / 1 (ativado) | Não |

**Exemplo:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Se estava a utilizar o serviço PostgreSQL ligado com o seguinte payload, ainda é suportado como-é, enquanto são sugeridos para utilizar um novo passa.

**Payload anterior:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
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

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o artigo de conjuntos de dados. Esta secção fornece uma lista de propriedades suportadas por conjunto de dados PostgreSQL.

Para copiar dados de PostgreSQL, defina a propriedade de tipo do conjunto de dados para **RelationalTable**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida: **RelationalTable** | Sim |
| tableName | Nome da tabela na base de dados PostgreSQL. | Não (se for especificada "consulta" na origem de atividade) |

**Exemplo**

```json
{
    "name": "PostgreSQLDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<PostgreSQL linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportadas por origem PostgreSQL.

### <a name="postgresql-as-source"></a>PostgreSQL como origem

Para copiar dados de PostgreSQL, defina o tipo de origem na atividade de cópia para **RelationalSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida: **RelationalSource** | Sim |
| consulta | Utilize a consulta SQL personalizada para ler os dados. Por exemplo: `"query": "SELECT * FROM \"MySchema\".\"MyTable\""`. | Não (se for especificado "tableName" no conjunto de dados) |

> [!NOTE]
> Os nomes de tabela e esquema são maiúsculas e minúsculas. Coloque-os na `""` (aspas) na consulta.

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<PostgreSQL input dataset name>",
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
                "query": "SELECT * FROM \"MySchema\".\"MyTable\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-postgresql"></a>Tipo de dados de mapeamento para PostgreSQL

Quando copiar dados de PostgreSQL, os seguintes mapeamentos são utilizados PostgreSQL tipos de dados para tipos de dados intermédio do Azure Data Factory. Consulte [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md) para saber mais sobre como atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink.

| Tipo de dados PostgreSQL | PostgresSQL aliases | Tipo de dados intermédio de fábrica de dados |
|:--- |:--- |:--- |
| `abstime` |&nbsp; |`String` |
| `bigint` | `int8` | `Int64` |
| `bigserial` | `serial8` | `Int64` |
| `bit [1]` |&nbsp; | `Boolean` |
| `bit [(n)], n>1` |&nbsp; | `Byte[]` |
| `bit varying [(n)]` | `varbit` |`Byte[]` |
| `boolean` | `bool` | `Boolean` |
| `box` |&nbsp; | `String` |
| `bytea` |&nbsp; | `Byte[], String` |
| `character [(n)]` | `char [(n)]` | `String` |
| `character varying [(n)]` | `varchar [(n)]` | `String` |
| `cid` |&nbsp; | `Int32` |
| `cidr` |&nbsp; | `String` |
| `circle` |&nbsp; |` String` |
| `date` |&nbsp; |`Datetime` |
| `daterange` |&nbsp; |`String` |
| `double precision` |`float8` |`Double` |
| `inet` |&nbsp; |`String` |
| `intarray` |&nbsp; |`String` |
| `int4range` |&nbsp; |`String` |
| `int8range` |&nbsp; |`String` |
| `integer` | `int, int4` |`Int32` |
| `interval [fields] [(p)]` | | `String` |
| `json` |&nbsp; | `String` |
| `jsonb` |&nbsp; | `Byte[]` |
| `line` |&nbsp; | `Byte[], String` |
| `lseg` |&nbsp; | `String` |
| `macaddr` |&nbsp; | `String` |
| `money` |&nbsp; | `String` |
| `numeric [(p, s)]`|`decimal [(p, s)]` |`String` |
| `numrange` |&nbsp; |`String` |
| `oid` |&nbsp; |`Int32` |
| `path` |&nbsp; |`String` |
| `pg_lsn` |&nbsp; |`Int64` |
| `point` |&nbsp; |`String` |
| `polygon` |&nbsp; |`String` |
| `real` |`float4` |`Single` |
| `smallint` |`int2` |`Int16` |
| `smallserial` |`serial2` |`Int16` |
| `serial` |`serial4` |`Int32` |
| `text` |&nbsp; |`String` |
| `timewithtimezone` |&nbsp; |`String` |
| `timewithouttimezone` |&nbsp; |`String` |
| `timestampwithtimezone` |&nbsp; |`String` |
| `xid` |&nbsp; |`Int32` |

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia no Azure Data Factory, consulte [arquivos de dados suportados](copy-activity-overview.md##supported-data-stores-and-formats).
