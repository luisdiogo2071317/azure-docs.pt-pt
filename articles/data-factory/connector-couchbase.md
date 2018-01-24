---
title: Copiar dados de Couchbase utilizando o Azure Data Factory (Beta) | Microsoft Docs
description: "Saiba como copiar dados de Couchbase aos arquivos de dados dependente suportados através da utilização de uma atividade de cópia no pipeline Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 
ms.author: jingwang
ms.openlocfilehash: edef6987ce85127aff5c44ba526cbb5a3879fd90
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-couchbase-using-azure-data-factory-beta"></a>Copiar dados de Couchbase utilizando o Azure Data Factory (Beta)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de Couchbase. Baseia-se no [copiar descrição geral da atividade](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [atividade de cópia no V1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Este conector está atualmente na versão Beta. Pode experimentar e envie-nos comentários. Não utilizá-lo em ambientes de produção.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de Couchbase para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

O Azure Data Factory fornece um controlador incorporado para ativar a conetividade, pelo que não precisa de instalar manualmente o controlador de utilizar este conector.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir o conector Couchbase entidades do Data Factory específicas.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço de Couchbase ligada:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida: **Couchbase** | Sim |
| connectionString | Uma cadeia de ligação de ODBC para ligar ao Couchbase. Pode escolher marcar este campo como um SecureString armazena de forma segura na ADF ou armazenar a palavra-passe no Cofre de chaves do Azure e permitir que a atividade de cópia solicitar a partir daí quando efetuar a cópia de dados - Saiba mais de [armazenar credenciais no Cofre de chaves](store-credentials-in-key-vault.md). | Sim |
| connectVia | O [integração Runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração Self-hosted ou Runtime de integração do Azure (se o arquivo de dados acessível publicamente). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

**Exemplo:**

```json
{
    "name": "CouchbaseLinkedService",
    "properties": {
        "type": "Couchbase",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>; Port=<port>;AuthMech=1;CredString=[{\"user\": \"JSmith\", \"pass\":\"access123\"}, {\"user\": \"Admin\", \"pass\":\"simba123\"}];"
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

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportadas por Couchbase conjunto de dados.

Para copiar dados de Couchbase, defina a propriedade de tipo do conjunto de dados para **CouchbaseTable**. Não há nenhuma propriedade de tipo específicas adicional neste tipo de conjunto de dados.

**Exemplo**

```json
{
    "name": "CouchbaseDataset",
    "properties": {
        "type": "CouchbaseTable",
        "linkedServiceName": {
            "referenceName": "<Couchbase linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportadas por Couchbase origem.

### <a name="couchbasesource-as-source"></a>CouchbaseSource como origem

Para copiar dados de Couchbase, defina o tipo de origem na atividade de cópia para **CouchbaseSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida: **CouchbaseSource** | Sim |
| consulta | Utilize a consulta SQL personalizada para ler os dados. Por exemplo: `"SELECT * FROM MyTable"`. | Sim |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromCouchbase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Couchbase input dataset name>",
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
                "type": "CouchbaseSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia no Azure Data Factory, consulte [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
