---
title: "Copiar dados do serviço Web do Amazon Marketplace utilizando o Azure Data Factory (Beta) | Microsoft Docs"
description: "Saiba como copiar dados de serviço Web do Amazon Marketplace aos arquivos de dados dependente suportados através da utilização de uma atividade de cópia no pipeline Azure Data Factory."
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
ms.date: 11/30/2017
ms.author: jingwang
ms.openlocfilehash: 736d286bb7b97e842e73afd362ba70a42d1a9d9d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-amazon-marketplace-web-service-using-azure-data-factory-beta"></a>Copiar dados de serviço Web do Amazon Marketplace utilizando o Azure Data Factory (Beta)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados do serviço Web do Amazon Marketplace. Baseia-se no [copiar descrição geral da atividade](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [atividade de cópia no V1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Este conector está atualmente na versão Beta. Pode experimentar e envie-nos comentários. Não utilizá-lo em ambientes de produção.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados do serviço Web do Amazon Marketplace para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

O Azure Data Factory fornece um controlador incorporado para ativar a conetividade, pelo que não precisa de instalar manualmente o controlador de utilizar este conector.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o conector do serviço de Web de Marketplace Amazon.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço Amazon Marketplace Web serviço ligado:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida: **AmazonMWS** | Sim |
| endpoint | O ponto final do servidor Amazon MWS, (ou seja, mws.amazonservices.com)  | Sim |
| marketplaceID | O ID de Marketplace Amazon que pretende obter dados a partir de. Para obter dados de vários IDs de Marketplace, separe-os com uma vírgula (`,`). (ou seja, A2EUQ1WTGCTBG2)  | Sim |
| sellerID | O ID de vendedor Amazon.  | Sim |
| mwsAuthToken | O token de autenticação Amazon MWS. Pode escolher marcar este campo como um SecureString armazena de forma segura pelo serviço Data Factory ou armazenar a palavra-passe no Cofre de chaves do Azure e permitir que o acitivity cópia solicitar a partir daí quando efetuar a cópia de dados - Saiba mais de [armazenar credenciais no Cofre de chaves ](store-credentials-in-key-vault.md). | Sim |
| accessKeyId | O acesso da chave ID utilizado para aceder a dados.  | Sim |
| secretKey | A chave secreta utilizada para aceder a dados. Pode escolher marcar este campo como um SecureString armazena de forma segura na ADF ou armazenar a palavra-passe no Cofre de chaves do Azure e permitir que o acitivity cópia solicitar a partir daí quando efetuar a cópia de dados - Saiba mais de [armazenar credenciais no Cofre de chaves](store-credentials-in-key-vault.md). | Sim |
| useEncryptedEndpoints | Especifica se os pontos finais de origem de dados são encriptados através de HTTPS. O valor predefinido é verdadeiro.  | Não |
| useHostVerification | Especifica se o nome de anfitrião no certificado do servidor para fazer corresponder o nome de anfitrião do servidor ao ligar através de SSL. O valor predefinido é verdadeiro.  | Não |
| usePeerVerification | Especifica se pretende verificar a identidade do servidor ao ligar através de SSL. O valor predefinido é verdadeiro.  | Não |

**Exemplo:**

```json
{
    "name": "AmazonMWSLinkedService",
    "properties": {
        "type": "AmazonMWS",
        "typeProperties": {
            "endpoint" : "mws.amazonservices.com",
            "marketplaceID" : "A2EUQ1WTGCTBG2",
            "sellerID" : "<sellerID>",
            "mwsAuthToken": {
                 "type": "SecureString",
                 "value": "<mwsAuthToken>"
            },
            "accessKeyId" : "<accessKeyId>",
            "secretKey": {
                 "type": "SecureString",
                 "value": "<secretKey>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportadas por conjunto de dados do serviço de Web de Marketplace Amazon.

Para copiar dados de serviço Web do Amazon Marketplace, defina a propriedade de tipo do conjunto de dados para **AmazonMWSObject**. Não há nenhuma propriedade de tipo específicas adicional neste tipo de conjunto de dados.

**Exemplo**

```json
{
    "name": "AmazonMWSDataset",
    "properties": {
        "type": "AmazonMWSObject",
        "linkedServiceName": {
            "referenceName": "<AmazonMWS linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}

```

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportado pela origem de serviço de Web de Marketplace Amazon.

### <a name="amazonmwssource-as-source"></a>AmazonMWSSource como origem

Para copiar dados de serviço Web do Amazon Marketplace, defina o tipo de origem na atividade de cópia para **AmazonMWSSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida: **AmazonMWSSource** | Sim |
| consulta | Utilize a consulta SQL personalizada para ler os dados. Por exemplo: `"SELECT * FROM Orders where  Amazon_Order_Id = 'xx'"`. | Sim |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromAmazonMWS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<AmazonMWS input dataset name>",
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
                "type": "AmazonMWSSource",
                "query": "SELECT * FROM Orders where Amazon_Order_Id = 'xx'"
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
