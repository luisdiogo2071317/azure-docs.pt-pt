---
title: Copiar dados de e para o Table storage do Azure utilizando o Data Factory | Microsoft Docs
description: Saiba como copiar dados de arquivos de origem suportada para o Table storage do Azure ou de armazenamento de tabelas em arquivos de receptores suportadas, utilizando o Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: ef43037ff33b693256c82459eec2e4b3beab4d9a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="copy-data-to-and-from-azure-table-storage-by-using-azure-data-factory"></a>Copiar dados de e para o Table storage do Azure utilizando o Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - Disponibilidade geral](v1/data-factory-azure-table-connector.md)
> * [Versão 2 - Pré-visualização](connector-azure-table-storage.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de e para o Table storage do Azure. Baseia-se no [descrição geral da atividade de cópia](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se utilizar a versão 1 do Data Factory, o que é geralmente disponível, consulte [conector de armazenamento de tabela na versão 1](v1/data-factory-azure-table-connector.md).

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados a partir de qualquer arquivo de dados de origem suportada para o Table storage. Também pode copiar dados de armazenamento de tabelas para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens ou sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector suporta da tabela do Azure copiar dados através da utilização de chave de conta e o serviço partilhado autenticações de assinatura de acesso.

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o Table storage.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

### <a name="use-an-account-key"></a>Utilizar uma chave de conta

Pode criar um serviço ligado do Storage do Azure, utilizando a chave de conta. Fornece a fábrica de dados com acesso global para o armazenamento. São suportadas as seguintes propriedades.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida **AzureStorage**. |Sim |
| connectionString | Especifique as informações necessárias para ligar ao armazenamento para a propriedade connectionString. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). |Sim |
| connectVia | O [integração runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração do Azure ou o tempo de execução do Self-hosted integração (se o arquivo de dados está localizado numa rede privada). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

**Exemplo:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="use-service-shared-access-signature-authentication"></a>Utilizar a autenticação de assinatura de acesso partilhado do serviço

Também pode criar um serviço ligado do Storage utilizando uma assinatura de acesso partilhado. Fornece a fábrica de dados com acesso restrito/vínculo de tempo para recursos de todos os/específico no armazenamento.

Uma assinatura de acesso partilhado fornece acesso delegado a recursos na sua conta de armazenamento. Pode utilizá-lo para conceder que um cliente limitada permissões para objetos na sua conta do storage por um período de tempo especificado e com um conjunto especificado de permissões. Não têm de partilhar as chaves de acesso da conta. A assinatura de acesso partilhado é um URI que abrange nos respetivos parâmetros de consulta todas as informações necessárias para acesso autenticado a um recurso de armazenamento. Para aceder a recursos de armazenamento com a assinatura de acesso partilhado, o cliente só tem de passar a assinatura de acesso partilhado para o método ou construtor adequado. Para mais informações sobre assinaturas de acesso partilhado, consulte [assinaturas de acesso partilhado: compreender o modelo de assinatura de acesso partilhado](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!IMPORTANT]
> Fábrica de dados agora suporta apenas assinaturas de acesso partilhado do serviço, mas não assinaturas de acesso de conta partilhada. Para obter mais informações sobre estes dois tipos e de como construi-las, consulte [tipos de assinaturas de acesso partilhado](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures). O acesso partilhado o URL de assinatura gerado a partir do portal do Azure ou no Explorador de armazenamento do Azure é uma assinatura de acesso partilhado de conta, que não é suportada.

> [!TIP]
> Pode executar os seguintes comandos do PowerShell para gerar uma assinatura de acesso partilhado do serviço para a sua conta de armazenamento. Substitua os marcadores de posição e conceder a permissão necessária.
> `$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Para utilizar a autenticação de assinatura de acesso partilhado do serviço, são suportadas as seguintes propriedades.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida **AzureStorage**. |Sim |
| sasUri | Especifique a URI de assinatura de acesso partilhado para os recursos de armazenamento, como BLOBs, contentor ou tabela. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). |Sim |
| connectVia | O [integração runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o tempo de execução de integração do Azure ou o tempo de execução de integração Self-hosted (se o arquivo de dados está localizado numa rede privada). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

**Exemplo:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Quando cria uma assinatura de acesso partilhado URI, considere os seguintes pontos:

- Definir permissões de leitura/escrita adequada em objetos com base na forma como o serviço ligado (leitura, escrita, leitura/escrita) é utilizado na fábrica de dados.
- Definir **hora de expiração** adequadamente. Certifique-se de que o acesso a objetos de armazenamento não expira com o período ativo do pipeline.
- O URI deve ser criado ao nível da tabela da direita em necessário.

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportadas pelo conjunto de dados de tabelas do Azure.

Para copiar dados e de tabelas do Azure, defina a propriedade de tipo do conjunto de dados para **AzureTable**. São suportadas as seguintes propriedades.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida **AzureTable**. |Sim |
| tableName |O nome da tabela na instância de base de dados de armazenamento da tabela que o serviço ligado refere-se a. |Sim |

**Exemplo:**

```json
{
    "name": "AzureTableDataset",
    "properties":
    {
        "type": "AzureTable",
        "linkedServiceName": {
            "referenceName": "<Azure Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Esquema pela fábrica de dados

Para os arquivos de dados sem esquema, tais como tabelas do Azure, o Data Factory infere o esquema de uma das seguintes formas:

* Se especificar a estrutura de dados utilizando o **estrutura** propriedade na definição do conjunto de dados, o Data Factory honra esta estrutura como o esquema. Neste caso, se uma linha não contém um valor para uma coluna, é fornecido um valor nulo para o mesmo.
* Se não especificar a estrutura dos dados ao utilizar o **estrutura** propriedade na definição do conjunto de dados, o Data Factory infere o esquema utilizando a primeira linha dos dados. Neste caso, se a primeira linha não contém o esquema completo, são omitidas algumas colunas nos resultados da operação de cópia.

Para origens de dados sem esquema, a melhor prática é especificar a estrutura de dados utilizando o **estrutura** propriedade.

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportadas pela origem de tabelas do Azure e o sink.

### <a name="azure-table-as-a-source-type"></a>Tabela do Azure como um tipo de origem

Para copiar dados de tabelas do Azure, defina o tipo de origem na atividade de cópia para **AzureTableSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida **AzureTableSource**. |Sim |
| azureTableSourceQuery |Utilize a consulta de armazenamento de tabela personalizada para ler os dados. Veja exemplos na secção seguinte: |Não |
| azureTableSourceIgnoreTableNotFound |Indica se deve permitir a exceção da tabela não existir.<br/>Valores permitidos são **verdadeiro** e **falso** (predefinição). |Não |

### <a name="azuretablesourcequery-examples"></a>Exemplos de azureTableSourceQuery

Se a coluna da tabela do Azure é do tipo datetime:

```json
"azureTableSourceQuery": "LastModifiedTime gt datetime'2017-10-01T00:00:00' and LastModifiedTime le datetime'2017-10-02T00:00:00'"
```

Se a coluna da tabela do Azure é do tipo cadeia:

```json
"azureTableSourceQuery": "LastModifiedTime ge '201710010000_0000' and LastModifiedTime le '201710010000_9999'"
```

Se utilizar o parâmetro de pipeline, converta o valor datetime para o formato correto, de acordo com os exemplos anteriores.

### <a name="azure-table-as-a-sink-type"></a>Tabela do Azure como um tipo de sink

Para copiar dados para tabelas do Azure, defina o tipo de sink na atividade de cópia para **AzureTableSink**. As seguintes propriedades são suportadas na atividade de cópia **sink** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo de sink de atividade de cópia tem de ser definida **AzureTableSink**. |Sim |
| azureTableDefaultPartitionKeyValue |O partição chave valor predefinido que pode ser utilizado pelo sink. |Não |
| azureTablePartitionKeyName |Especifique o nome da coluna cujos valores são utilizados como chaves de partição. Se não for especificado, "AzureTableDefaultPartitionKeyValue" é utilizado como a chave de partição. |Não |
| azureTableRowKeyName |Especifique o nome da coluna cujos valores da coluna são utilizados como a chave de linha. Se não for especificado, utilize um GUID para cada linha. |Não |
| azureTableInsertType |O modo de inserir os dados na tabela do Azure. Esta propriedade controla se as linhas existentes na tabela de saída com correspondentes chaves de partição e a linha tem os respetivos valores substituído ou intercaladas. <br/><br/>Valores permitidos são **intercalação** (predefinição) e **substituir**. <br/><br> Esta definição aplica-se ao nível da linha não é o nível de tabela. Nenhuma opção elimina as linhas na tabela de saída que não existem na entrada. Para saber mais sobre como funcionam as definições de intercalação e substituir, consulte [entidade Insert ou merge](https://msdn.microsoft.com/library/azure/hh452241.aspx) e [inserir ou substituir entidade](https://msdn.microsoft.com/library/azure/hh452242.aspx). |Não |
| writeBatchSize |Inserem dados no Azure Table quando é atingido writeBatchSize ou writeBatchTimeout.<br/>Valores permitidos são número inteiro (número de linhas). |Não (a predefinição é 10 000) |
| writeBatchTimeout |Inserem dados no Azure Table quando é atingido writeBatchSize ou writeBatchTimeout.<br/>Valores permitidos são timespan. Um exemplo é "00: 20:00" (20 minutos). |Não (a predefinição é 90 segundos, o tempo limite predefinido do cliente de armazenamento) |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyToAzureTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Table output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureTableSink",
                "azureTablePartitionKeyName": "<column name>",
                "azureTableRowKeyName": "<column name>"
            }
        }
    }
]
```

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName

Mapear uma coluna de origem para uma coluna de destino utilizando o **"tradutor"** propriedade antes de poder utilizar a coluna de destino como azureTablePartitionKeyName.

No exemplo seguinte, a coluna de origem DivisionID está mapeada para a coluna de destino DivisionID:

```json
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```

"DivisionID" está especificada como a chave de partição.

```json
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID"
}
```

## <a name="data-type-mapping-for-azure-table"></a>Tipo de dados de mapeamento de tabelas do Azure

Quando copiar dados de origem e de tabelas do Azure, os seguintes mapeamentos são utilizados de tipos de dados de tabelas do Azure para tipos de dados intermédio de fábrica de dados. Para saber mais sobre como a atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink, consulte [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md).

Ao mover dados para e da tabela do Azure, o seguinte [mapeamentos definidos pelo Azure Table](https://msdn.microsoft.com/library/azure/dd179338.aspx) são utilizadas de tipos de OData de tabela do Azure para o tipo de .NET e vice-versa.

| Tipo de dados de tabela do Azure | Tipo de dados intermédio de fábrica de dados | Detalhes |
|:--- |:--- |:--- |
| Edm.Binary |byte[] |Uma matriz de bytes até 64 KB. |
| Edm.Boolean |bool |Valor booleano. |
| Edm.DateTime |DateTime |Um valor de 64 bits, expressado como Hora Universal Coordenada (UTC). O intervalo de DateTime suportado começa, 1 de Janeiro de 1601 a.d. (C.E.), UTC. O intervalo de termina a 31 de Dezembro de 9999. |
| Edm.Double |duplo |Um valor de ponto flutuante de 64 bits. |
| Edm.Guid |GUID |Um identificador exclusivo global de 128 bits. |
| Edm.Int32 |Int32 |Um número inteiro de 32 bits. |
| Edm.Int64 |Int64 |Um número inteiro de 64 bits. |
| Edm.String |Cadeia |Um valor com codificação UTF-16. Valores de cadeia podem ter até 64 KB. |

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia numa fábrica de dados, consulte [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
