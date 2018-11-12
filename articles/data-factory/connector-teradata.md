---
title: Copiar dados do Teradata com o Azure Data Factory | Documentos da Microsoft
description: Saiba mais sobre Teradata conector do serviço Data Factory que lhe permite copiar dados de base de dados Teradata armazenamentos de dados suportado pelo Data Factory como sinks.
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
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: 37e7281af87a8cfc57aae95411eb2d4cce9eef65
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228067"
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Copiar dados do Teradata com o Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1](v1/data-factory-onprem-teradata-connector.md)
> * [Versão atual](connector-teradata.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de uma base de dados Teradata. Ele se baseia no [copiar descrição geral da atividade](copy-activity-overview.md) artigo apresenta uma visão geral da atividade de cópia.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de base de dados Teradata para qualquer arquivo de dados de sink suportados. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks a atividade de cópia, consulte a [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector de Teradata suporta:

- Teradata **versão 12 e acima**.
- Copiar dados utilizando **básica** ou **Windows** autenticação.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar este conector de Teradata, terá de:

- Configure um Runtime de integração autoalojado. Ver [Integration Runtime autoalojado](create-self-hosted-integration-runtime.md) artigo para obter detalhes.
- Instalar o [fornecedor de dados do .NET para Teradata](https://go.microsoft.com/fwlink/?LinkId=278886) versão 14 ou superior no computador Runtime de integração.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizadas para definir entidades do Data Factory específicas para o conector de Teradata.

## <a name="linked-service-properties"></a>Propriedades do serviço ligado

As seguintes propriedades são suportadas para o serviço de Teradata ligado:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo deve ser definida como: **Teradata** | Sim |
| servidor | Nome do servidor de Teradata. | Sim |
| authenticationType | Tipo de autenticação utilizado para ligar à base de dados Teradata.<br/>Valores permitidos são: **básica**, e **Windows**. | Sim |
| o nome de utilizador | Especifique o nome de utilizador para ligar à base de dados Teradata. | Sim |
| palavra-passe | Especifique a palavra-passe da conta de utilizador que especificou para o nome de utilizador. Marcar esse campo como uma SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Azure Key Vault](store-credentials-in-key-vault.md). | Sim |
| connectVia | O [Integration Runtime](concepts-integration-runtime.md) a ser utilizado para ligar ao arquivo de dados. Um Runtime de integração autoalojado é necessário, conforme mencionado na [pré-requisitos](#prerequisites). |Sim |

**Exemplo:**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "Basic",
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

Para obter uma lista completa das secções e propriedades disponíveis para definir conjuntos de dados, consulte o artigo de conjuntos de dados. Esta seção fornece uma lista de propriedades suportadas pelo conjunto de dados Teradata.

Para copiar dados do Teradata, defina a propriedade de tipo de conjunto de dados para **RelationalTable**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida como: **RelationalTable** | Sim |
| tableName | Nome da tabela na base de dados Teradata. | Não (se for especificada "consulta" na origem de atividade) |

**Exemplo:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para obter uma lista completa das secções e propriedades disponíveis para a definição de atividades, consulte a [Pipelines](concepts-pipelines-activities.md) artigo. Esta seção fornece uma lista de propriedades suportadas pela origem de Teradata.

### <a name="teradata-as-source"></a>Teradata como origem

Para copiar dados do Teradata, defina o tipo de origem na atividade de cópia para **RelationalSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo de origem de atividade de cópia tem de ser definida: **RelationalSource** | Sim |
| consulta | Utilize a consulta SQL personalizada para ler os dados. Por exemplo: `"SELECT * FROM MyTable"`. | Não (se for especificado "tableName" no conjunto de dados) |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromTeradata",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Teradata input dataset name>",
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

## <a name="data-type-mapping-for-teradata"></a>Tipo de dados de mapeamento para Teradata

Ao copiar dados da Teradata, os seguintes mapeamentos são utilizados entre tipos de dados Teradata aos tipos de dados intermediárias do Azure Data Factory. Ver [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md) para saber mais sobre como atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink.

| Tipo de dados Teradata | Tipo de dados intermediárias de fábrica de dados |
|:--- |:--- |
| BigInt |Int64 |
| Blobs |Byte[] |
| Byte |Byte[] |
| ByteInt |Int16 |
| char |Cadeia |
| CLOB |Cadeia |
| Date |DateTime |
| decimal |decimal |
| Valor de duplo |Valor de duplo |
| Gráfico |Cadeia |
| Número inteiro |Int32 |
| Dia do intervalo |Período de tempo |
| Dia de intervalo para a hora |Período de tempo |
| Dia de intervalo de minuto |Período de tempo |
| Dia do intervalo como segundo |Período de tempo |
| Hora do intervalo |Período de tempo |
| Intervalo de hora para minuto |Período de tempo |
| Hora do intervalo como segundo |Período de tempo |
| Minuto do intervalo |Período de tempo |
| Intervalo de minuto para segundo |Período de tempo |
| Mês de intervalo |Cadeia |
| Intervalo de segundo |Período de tempo |
| Ano de intervalo |Cadeia |
| Intervalo de ano para mês |Cadeia |
| Number |Valor de duplo |
| Period(Date) |Cadeia |
| Period(Time) |Cadeia |
| Período (hora com fuso horário) |Cadeia |
| Period(Timestamp) |Cadeia |
| Período (Timestamp com o fuso horário) |Cadeia |
| SmallInt |Int16 |
| Hora |Período de tempo |
| Hora com fuso horário |Cadeia |
| Carimbo de data/hora |DateTime |
| Timestamp com o fuso horário |DateTimeOffset |
| VarByte |Byte[] |
| VarChar |Cadeia |
| VarGraphic |Cadeia |
| Xml |Cadeia |


## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks, a atividade de cópia no Azure Data Factory, veja [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
