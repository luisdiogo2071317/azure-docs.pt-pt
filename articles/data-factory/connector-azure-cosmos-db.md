---
title: Copiar dados de/para o Azure Cosmos DB com o Data Factory | Documentos da Microsoft
description: Saiba como copiar dados de arquivos de dados de origem suportada para o Azure Cosmos DB (ou) do Cosmos DB para lojas de sink suportado com o Data Factory.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: jingwang
ms.openlocfilehash: db2f3086da41e990e6f87d8086d6e8defa447532
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543449"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-using-azure-data-factory"></a>Copiar dados de ou para o Azure Cosmos DB com o Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1](v1/data-factory-azure-documentdb-connector.md)
> * [Versão atual](connector-azure-cosmos-db.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de e para o Azure Cosmos DB (SQL API). Ele se baseia no [copiar descrição geral da atividade](copy-activity-overview.md) artigo apresenta uma visão geral da atividade de cópia.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados do Azure Cosmos DB para qualquer arquivo de dados de sink suportados ou copiar dados de qualquer arquivo de dados de origem suportada para o Azure Cosmos DB. Para obter uma lista dos arquivos de dados suportados como origens/sinks a atividade de cópia, consulte a [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector do Azure Cosmos DB suporta:

- Copiar dados de/para o Cosmos DB [API do SQL](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction).
- Escrever no Cosmos DB como INSERT ou UPSERT.
- Importar/exportar documentos JSON como-é ou copiar os dados de/para o conjunto de dados em tabela por exemplo, base de dados SQL, arquivos CSV, etc. Para copiar documentos como-é de/para ficheiros JSON ou de outra coleção do Cosmos DB, veja [documentos JSON de importação/exportação](#importexport-json-documents).

Fábrica de dados se integra [biblioteca de executor do Cosmos DB em massa](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) para proporcionar o melhor desempenho escrever para o Cosmos DB.

>[!TIP]
>Assista [este vídeo](https://youtu.be/5-SRNiC_qOU) que percorre a copiar dados do armazenamento de Blobs do Azure para o Cosmos DB e descreve considerações para a ingestão de dados para o Cosmos DB em geral de ajuste de desempenho.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizadas para definir entidades do Data Factory específicas ao Azure Cosmos DB.

## <a name="linked-service-properties"></a>Propriedades do serviço ligado

As seguintes propriedades são suportadas para o serviço ligado do Azure Cosmos DB:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo deve ser definida como: **CosmosDb**. | Sim |
| connectionString |Especifica as informações necessárias para ligar à base de dados do Azure Cosmos DB. Tenha em atenção de que tem de especificar informações de base de dados na cadeia de ligação conforme abaixo de exemplo. Marcar esse campo como uma SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Azure Key Vault](store-credentials-in-key-vault.md). |Sim |
| connectVia | O [Integration Runtime](concepts-integration-runtime.md) a ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração do Azure ou o Runtime de integração autoalojado (se o seu armazenamento de dados está localizado numa rede privada). Se não for especificado, ele usa o padrão do Runtime de integração do Azure. |Não |

**Exemplo:**

```json
{
    "name": "CosmosDbLinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
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

Para obter uma lista completa das secções e propriedades disponíveis para definir conjuntos de dados, consulte o artigo de conjuntos de dados. Esta seção fornece uma lista de propriedades suportadas pelo conjunto de dados do Azure Cosmos DB.

Para copiar dados de/para o Azure Cosmos DB, defina a propriedade de tipo de conjunto de dados para **DocumentDbCollection**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida como: **DocumentDbCollection** |Sim |
| CollectionName |Nome da coleção de documento do Cosmos DB. |Sim |

**Exemplo:**

```json
{
    "name": "CosmosDbDataset",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Esquema pelo Data Factory

Para arquivos de dados sem esquema, como o Azure Cosmos DB, a atividade de cópia infere o esquema de uma das seguintes formas. Por conseguinte, a menos que queira [importar/exportar documentos JSON como-é](#importexport-json-documents), a prática recomendada é especificar a estrutura de dados no **estrutura** secção.

*. Se especificar a estrutura de dados utilizando o **estrutura** propriedade na definição do conjunto de dados, o serviço Data Factory respeita essa estrutura como o esquema. Neste caso, se uma linha não contém um valor para uma coluna, irá ser fornecido um valor nulo para o mesmo.
*. Se não especificar a estrutura de dados utilizando o **estrutura** propriedade na definição do conjunto de dados, o serviço Data Factory infere o esquema ao utilizar a primeira linha dos dados. Neste caso, se a primeira linha contém o esquema completo, algumas colunas estará em falta no resultado da operação de cópia.

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para obter uma lista completa das secções e propriedades disponíveis para a definição de atividades, consulte a [Pipelines](concepts-pipelines-activities.md) artigo. Esta seção fornece uma lista de propriedades suportadas pelo Azure Cosmos DB origem e sink.

### <a name="azure-cosmos-db-as-source"></a>O Azure Cosmos DB, como origem

Para copiar dados do Azure Cosmos DB, defina o tipo de origem na atividade de cópia para **DocumentDbCollectionSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo de origem de atividade de cópia tem de ser definida: **DocumentDbCollectionSource** |Sim |
| consulta |Especifique a consulta do Cosmos DB para ler os dados.<br/><br/>Exemplo: `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Não <br/><br/>Se não for especificado, a instrução SQL que é executada: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Caráter especial para indicar que o documento está aninhado e como a flattern o conjunto de resultados.<br/><br/>Por exemplo, se uma consulta do Cosmos DB devolve um resultado aninhado `"Name": {"First": "John"}`, atividade de cópia irá identificar o nome da coluna como "Name.First" com o valor "João" quando o nestedSeparator é o ponto. |Não (a predefinição é o ponto `.`) |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Document DB input dataset name>",
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
                "type": "DocumentDbCollectionSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-as-sink"></a>O Azure Cosmos DB, como sink

Para copiar dados para o Azure Cosmos DB, defina o tipo de sink na atividade de cópia para **DocumentDbCollectionSink**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo de sink de atividade de cópia tem de ser definida: **DocumentDbCollectionSink** |Sim |
| WriteBehavior |Descreve como escrever dados para o Cosmos DB. Valores permitidos são: `insert` e `upsert`.<br/>O comportamento das **upsert** é substituir o documento se um documento do mesmo id já existe; caso contrário, inseri-lo. Observe ADF irá gerar automaticamente um id para o documento se não for especificado no documento original ou por mapeamento de colunas), que significa que terá de certificar-se de que o documento tem uma "id", para que upsert funcionar conforme esperado. |Não, o padrão é inserir |
| writeBatchSize | Utilização do Data Factory [biblioteca de executor do Cosmos DB em massa](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) para escrever dados para o Cosmos DB. "writeBatchSize" controla o tamanho de documentos, fornecemos para a biblioteca de cada vez. Pode tentar aumento writeBatchSize para melhorar o desempenho. |Não, a predefinição é 10 000 |
| nestingSeparator |É necessário um caráter especial no nome da coluna de origem para indicar esse documento aninhado. <br/><br/>Por exemplo, `Name.First` do conjunto de dados de saída a estrutura gera a seguinte estrutura JSON no documento do Cosmos DB:`"Name": {"First": "[value maps to this column from source]"}` quando o nestedSeparator é o ponto. |Não (a predefinição é o ponto `.`) |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyToCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DocumentDbCollectionSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

## <a name="importexport-json-documents"></a>Documentos JSON de importação/exportação

Utilizar este conector do Cosmos DB, pode facilmente

* Importe documentos JSON de várias origens para o Cosmos DB, incluindo BLOBs do Azure, Azure Data Lake Store e outros arquivos baseados em ficheiros do Azure Data Factory suportados.
* Exporte documentos JSON do Cosmos DB collecton em vários arquivos baseados em ficheiros.
* Copiar documentos entre duas coleções de Cosmos DB como-é.

Para alcançar essa cópia do esquema desconhecido:

* Ao utilizar a ferramenta de cópia de dados, consulte a **"Exportar como-é ficheiros JSON ou coleção do Cosmos DB"** opção.
* Quando utilizar a atividade de criação, não especifique a seção de "estrutura" (também conhecido como esquema) em conjuntos de dados do Cosmos DB nem propriedade de "nestingSeparator" no Cosmos DB origem/sink na atividade de cópia. Quando importar do / exportar ficheiros JSON, no dataset de arquivo correspondente do ficheiro, especifique o tipo de formato como "JsonFormat" e a configuração "filePattern" corretamente (consulte [formato JSON](supported-file-formats-and-compression-codecs.md#json-format) secção para obter detalhes), em seguida, não especificar a estrutura" "(também conhecido como esquema) secção e ignorar as definições do formato de rest.

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks, a atividade de cópia no Azure Data Factory, veja [arquivos de dados suportados](copy-activity-overview.md##supported-data-stores-and-formats).
