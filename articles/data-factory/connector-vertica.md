---
title: Copiar dados de Family utilizando o Azure Data Factory (Beta) | Microsoft Docs
description: Saiba como copiar dados de Family aos arquivos de dados dependente suportados através da utilização de uma atividade de cópia no pipeline Azure Data Factory.
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
ms.date: 02/26/2018
ms.author: jingwang
ms.openlocfilehash: a308de5dea3f97c5b285c1be715969f5ca373e5d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="copy-data-from-vertica-using-azure-data-factory-beta"></a>Copiar dados de Family utilizando o Azure Data Factory (Beta)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de Family. Baseia-se no [copiar descrição geral da atividade](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [atividade de cópia no V1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Este conector está atualmente na versão Beta. Pode experimentar e envie-nos comentários. Não utilizá-lo em ambientes de produção.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de Family para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

O Azure Data Factory fornece um controlador incorporado para ativar a conetividade, pelo que não precisa de instalar manualmente o controlador de utilizar este conector.

## <a name="getting-started"></a>Introdução

Pode criar um pipeline com atividade de cópia com o .NET SDK, o Python SDK, o Azure PowerShell, o REST API ou o modelo Azure Resource Manager. Consulte [tutorial de atividade de cópia](quickstart-create-data-factory-dot-net.md) para obter instruções passo a passo Criar um pipeline com uma atividade de cópia.

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o conector Family.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço de Family ligada:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida: **Family** | Sim |
| connectionString | Uma cadeia de ligação de ODBC para ligar ao Family. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Sim |
| connectVia | O [integração Runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração Self-hosted ou Runtime de integração do Azure (se o arquivo de dados acessível publicamente). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

**Exemplo:**

```json
{
    "name": "VerticaLinkedService",
    "properties": {
        "type": "Vertica",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>"
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

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportadas por Family conjunto de dados.

Para copiar dados de Family, defina a propriedade de tipo do conjunto de dados para **VerticaTable**. Não há nenhuma propriedade de tipo específicas adicional neste tipo de conjunto de dados.

**Exemplo**

```json
{
    "name": "VerticaDataset",
    "properties": {
        "type": "VerticaTable",
        "linkedServiceName": {
            "referenceName": "<Vertica linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportadas por origem Family.

### <a name="vertica-as-source"></a>Family como origem

Para copiar dados de Family, defina o tipo de origem na atividade de cópia para **VerticaSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida: **VerticaSource** | Sim |
| consulta | Utilize a consulta SQL personalizada para ler os dados. Por exemplo: `"SELECT * FROM MyTable"`. | Sim |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromVertica",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Vertica input dataset name>",
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
                "type": "VerticaSource",
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
