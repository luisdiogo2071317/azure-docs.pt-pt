---
title: Copiar dados de e para Oracle através da utilização do Azure Data Factory | Microsoft Docs
description: Saiba como copiar dados de arquivos de origem suportada para uma base de dados Oracle ou de Oracle para lojas de dependente suportados por utilizar o Data Factory.
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
ms.date: 06/14/2018
ms.author: jingwang
ms.openlocfilehash: 6a232787793f9f4992a4dece821ae0bcc9059afc
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37058991"
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Copiar os dados de origem e de Oracle através da utilização do Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1](v1/data-factory-onprem-oracle-connector.md)
> * [Versão atual](connector-oracle.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de origem e de uma base de dados Oracle. Baseia-se no [descrição geral da atividade de cópia](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de uma base de dados Oracle para qualquer arquivo de dados suportados sink. Também pode copiar dados de qualquer arquivo de dados de origem suportada para uma base de dados Oracle. Para obter uma lista dos arquivos de dados que são suportados como origens ou sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector Oracle suporta as seguintes versões de uma base de dados Oracle. Também suporta autenticações Basic ou OID:

- R1 Oracle 12c (12.1)
- Oracle 11g R1, R2 (11.1, 11.2)
- Oracle 10g R1, R2 (10.1, 10.2)
- Oracle 9i R1, R2 (9.0.1, 9.2)
- Oracle 8i R3 (8.1.7)

> [!Note]
> Não é suportado o servidor de proxy do Oracle.

## <a name="prerequisites"></a>Pré-requisitos

Para copiar dados de origem e de uma base de dados Oracle que não se encontra acessível publicamente, terá de configurar um tempo de execução de integração Self-hosted. Para mais informações sobre o tempo de execução de integração, consulte [Self-hosted integração Runtime](create-self-hosted-integration-runtime.md). O tempo de execução de integração fornece um controlador de Oracle incorporado. Por conseguinte, não precisa de instalar manualmente um controlador ao copiar dados de origem e de Oracle.

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o conector do Oracle.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço ligado do Oracle.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida **Oracle**. | Sim |
| connectionString | Especifica as informações necessárias para estabelecer ligação à instância de base de dados Oracle. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md).<br><br>**Suportada de tipo de ligação**: pode utilizar **Oracle SID** ou **nome do serviço Oracle** para identificar a sua base de dados:<br>-Se utilizar SID: `Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>-Se utilizar o nome do serviço: `Host=<host>;Port=<port>;ServiceName=<servicename>;User Id=<username>;Password=<password>;` | Sim |
| connectVia | O [integração runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração Self-hosted ou Runtime de integração do Azure (se o arquivo de dados acessível publicamente). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

**Exemplo:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
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

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportado pelo conjunto de dados Oracle.

Para copiar dados de origem e de Oracle, defina a propriedade de tipo do conjunto de dados para **OracleTable**. São suportadas as seguintes propriedades.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida **OracleTable**. | Sim |
| tableName |O nome da tabela na base de dados Oracle que referencia o serviço ligado. | Sim |

**Exemplo:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportado pela origem de Oracle e sink.

### <a name="oracle-as-a-source-type"></a>Oracle como um tipo de origem

Para copiar dados da Oracle, defina o tipo de origem na atividade de cópia para **OracleSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida **OracleSource**. | Sim |
| oracleReaderQuery | Utilize a consulta SQL personalizada para ler os dados. Um exemplo é `"SELECT * FROM MyTable"`. | Não |

Se não especificar "oracleReaderQuery", as colunas definidas na secção "estrutura" do conjunto de dados são utilizadas para construir uma consulta (`select column1, column2 from mytable`) para executar a base de dados Oracle. Se a definição do conjunto de dados não tiver "estrutura", todas as colunas são selecionadas da tabela.

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
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
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-a-sink-type"></a>Oracle como um tipo de sink

Para copiar dados para Oracle, defina o tipo de sink na atividade de cópia para **OracleSink**. As seguintes propriedades são suportadas na atividade de cópia **sink** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo de sink de atividade de cópia tem de ser definida **OracleSink**. | Sim |
| writeBatchSize | Insere dados para a tabela SQL quando o tamanho da memória intermédia atinge writeBatchSize.<br/>Valores permitidos são número inteiro (número de linhas). |Não (a predefinição é 10 000) |
| writeBatchTimeout | De tempo de espera para a operação de inserção de lote seja concluída antes de atingir o tempo limite.<br/>Valores permitidos são Timespan. Um exemplo é 00:30:00 (30 minutos). | Não |
| preCopyScript | Especifique uma consulta SQL para a atividade de cópia ser executado antes de escrever dados para Oracle em cada execução. Pode utilizar esta propriedade para limpar os dados pré-carregada. | Não |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-oracle"></a>Tipo de dados de mapeamento para Oracle

Quando copiar dados de origem e de Oracle, os seguintes mapeamentos são utilizados Oracle tipos de dados para tipos de dados intermédio do Data Factory. Para saber mais sobre como a atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink, consulte [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md).

| Tipo de dados Oracle | Tipo de dados intermédio de fábrica de dados |
|:--- |:--- |
| BFILE |Byte[] |
| BLOB |Byte[]<br/>(apenas suportada em Oracle 10g e superior) |
| CHAR |Cadeia |
| CLOB |Cadeia |
| DATA |DateTime |
| NÚMERO DE VÍRGULA FLUTUANTE |Decimal, cadeia (se precisão > 28) |
| NÚMERO INTEIRO |Decimal, cadeia (se precisão > 28) |
| LONGA |Cadeia |
| PERÍODO DE TEMPO EM BRUTO |Byte[] |
| NCHAR |Cadeia |
| NCLOB |Cadeia |
| NÚMERO |Decimal, cadeia (se precisão > 28) |
| NVARCHAR2 |Cadeia |
| NÃO PROCESSADO |Byte[] |
| ROWID |Cadeia |
| TIMESTAMP |DateTime |
| TIMESTAMP COM O FUSO HORÁRIO LOCAL |Cadeia |
| TIMESTAMP COM O FUSO HORÁRIO |Cadeia |
| NÚMERO INTEIRO NÃO ASSINADO |Number |
| VARCHAR2 |Cadeia |
| XML |Cadeia |

> [!NOTE]
> Os tipos de dados de intervalo para mês e intervalo dia para segundo não são suportados.


## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia numa fábrica de dados, consulte [arquivos de dados suportados](copy-activity-overview.md##supported-data-stores-and-formats).
