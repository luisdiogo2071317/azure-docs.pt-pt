---
title: "Copiar dados de Google BigQuery através da utilização do Azure Data Factory (beta) | Microsoft Docs"
description: "Saiba como copiar dados de Google BigQuery aos arquivos de dados suportados sink utilizando uma atividade de cópia num pipeline de fábrica de dados."
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
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: 03aeb4fd190ec83a61875168116157404c1d730d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="copy-data-from-google-bigquery-by-using-azure-data-factory-beta"></a>Copiar dados de Google BigQuery através da utilização do Azure Data Factory (beta)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de Google BigQuery. Baseia-se no [descrição geral da atividade de cópia](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se utilizar a versão 1 do serviço do Data Factory, o que estiver geralmente disponível, consulte [atividade de cópia na versão 1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Este conector está atualmente na versão beta. Pode experimentar e envie-nos comentários. Não utilizá-lo em ambientes de produção.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de Google BigQuery para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens ou sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

 Fábrica de dados fornece um controlador incorporado para ativar a conetividade. Por conseguinte, não precisa de instalar manualmente um controlador para utilizar este conector.

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o conector Google BigQuery.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o Google BigQuery serviço ligado.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida **GoogleBigQuery**. | Sim |
| projeto | O ID de projeto do projeto de BigQuery predefinido para consulta contra.  | Sim |
| additionalProjects | Uma lista separada por vírgulas de IDs de projeto do público BigQuery projetos para o acesso.  | Não |
| requestGoogleDriveScope | Indica se deve pedir acesso ao Google Drive. Permitir o acesso de Google Drive permite suporte para tabelas federadas que combinam dados BigQuery com dados a partir do Google Drive. O valor predefinido é **falso**.  | Não |
| authenticationType | O mecanismo de autenticação OAuth 2.0 utilizado para autenticação. ServiceAuthentication pode ser utilizado apenas no tempo de execução do Self-hosted integração. <br/>Valores permitidos são **ServiceAuthentication** e **UserAuthentication**. | Sim |
| refreshToken | O token de atualização obtido a partir do Google utilizado para autorizar o acesso ao BigQuery para UserAuthentication. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Não |
| e-mail | O ID de correio eletrónico de conta do serviço é utilizado para ServiceAuthentication. Pode ser utilizado apenas no tempo de execução do Self-hosted integração.  | Não |
| keyFilePath | O caminho completo para o ficheiro de chave. p12 que é utilizado para autenticar o endereço de e-mail da conta de serviço. Pode ser utilizado apenas no tempo de execução do Self-hosted integração.  | Não |
| trustedCertPath | O caminho completo do ficheiro. pem que contém os certificados de AC fidedigna utilizados para verificar o servidor ao estabelecer ligação através de SSL. Esta propriedade pode ser definida apenas quando utilizar o protocolo SSL no tempo de execução do Self-hosted integração. O valor predefinido é o ficheiro de cacerts.pem instalado com o tempo de execução de integração.  | Não |
| useSystemTrustStore | Especifica se deve utilizar um certificado de AC da loja de confiança de sistema ou de um ficheiro. pem especificado. O valor predefinido é **falso**.  | Não |

**Exemplo:**

```json
{
    "name": "GoogleBigQueryLinkedService",
    "properties": {
        "type": "GoogleBigQuery",
        "typeProperties": {
            "project" : "<project ID>",
            "additionalProjects" : "<additional project IDs>",
            "requestGoogleDriveScope" : true,
            "authenticationType" : "UserAuthentication",
            "refreshToken": {
                 "type": "SecureString",
                 "value": "<refresh token>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportado pelo conjunto de dados Google BigQuery.

Para copiar dados a partir do Google BigQuery, defina a propriedade de tipo do conjunto de dados para **GoogleBigQueryObject**. Não há nenhuma propriedade de tipo específicas adicional neste tipo de conjunto de dados.

**Exemplo**

```json
{
    "name": "GoogleBigQueryDataset",
    "properties": {
        "type": "GoogleBigQueryObject",
        "linkedServiceName": {
            "referenceName": "<GoogleBigQuery linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportadas por tipo de origem Google BigQuery.

### <a name="googlebigquerysource-as-a-source-type"></a>GoogleBigQuerySource como um tipo de origem

Para copiar dados a partir do Google BigQuery, defina o tipo de origem na atividade de cópia para **GoogleBigQuerySource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida **GoogleBigQuerySource**. | Sim |
| consulta | Utilize a consulta SQL personalizada para ler os dados. Um exemplo é `"SELECT * FROM MyTable"`. | Sim |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromGoogleBigQuery",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleBigQuery input dataset name>",
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
                "type": "GoogleBigQuerySource",
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
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia numa fábrica de dados, consulte [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
