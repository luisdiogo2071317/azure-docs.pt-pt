---
title: "Copiar dados de ECC SAP através do Azure Data Factory | Microsoft Docs"
description: "Saiba como copiar dados de SAP ECC aos arquivos de dados dependente suportados através da utilização de uma atividade de cópia no pipeline Azure Data Factory."
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
ms.date: 02/27/2018
ms.author: jingwang
ms.openlocfilehash: efca2c129a1c7b8aca6b879d6d1311c6be157be1
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="copy-data-from-sap-ecc-using-azure-data-factory"></a>Copiar dados de ECC SAP através do Azure Data Factory

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de SAP ECC (SAP Enterprise Central componente). Baseia-se no [copiar descrição geral da atividade](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [atividade de cópia no V1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de SAP ECC para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector SAP ECC suporta:

- Copiar dados a partir do SAP ECC no SAP NetWeaver versão 7.0 e posterior. 
- Copiar dados de todos os objetos expostos pelos serviços OData do SAP ECC (por exemplo, SAP/vistas de tabelas, BAPI, Extractors de dados, etc.) ou os dados/IDOCs enviados para SAP PI que pode ser recebida como OData através de adaptadores relativos.
- A copiar os dados utilizando a autenticação básica.

## <a name="prerequisites"></a>Pré-requisitos

Geralmente, SAP ECC expõe entidades através de serviços de OData através do SAP Gateway. Para utilizar este conector SAP ECC, tem de:

- **Configurar o SAP Gateway**. Para servidores com a versão do SAP NetWeaver superior ao 7.4, o Gateway SAP já está instalado. Caso contrário, terá de instalar imbedded Gateway ou Gateway hub antes de exposição de dados SAP ECC através dos serviços de OData. Saiba como configurar o Gateway de SAP da [guia de instalação](https://help.sap.com/saphelp_gateway20sp12/helpdata/en/c3/424a2657aa4cf58df949578a56ba80/frameset.htm).

- **Ativar e configurar o serviço OData do SAP**. Pode ativar os serviços de OData através de TCODE SICF em segundos. Também pode configurar que objetos tem de ser expostos. Eis um exemplo [orientações passo a passo](https://blogs.sap.com/2012/10/26/step-by-step-guide-to-build-an-odata-service-based-on-rfcs-part-1/).

## <a name="getting-started"></a>Introdução

Pode criar um pipeline com atividade de cópia com o .NET SDK, o Python SDK, o Azure PowerShell, o REST API ou o modelo Azure Resource Manager. Consulte [tutorial de atividade de cópia](quickstart-create-data-factory-dot-net.md) para obter instruções passo a passo Criar um pipeline com uma atividade de cópia.

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o conector do SAP ECC.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço de ECC SAP ligada:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida: **SapEcc** | Sim |
| url | O url do serviço OData do SAP ECC. | Sim |
| o nome de utilizador | O nome de utilizador utilizada para ligar para o SAP ECC. | Não |
| palavra-passe | A palavra-passe de texto simples utilizada para estabelecer ligação com o SAP ECC. | Não |
| connectVia | O [integração Runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração Self-hosted ou Runtime de integração do Azure (se o arquivo de dados acessível publicamente). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

**Exemplo:**

```json
{
    "name": "SapECCLinkedService",
    "properties": {
        "type": "SapEcc",
        "typeProperties": {
            "url": "<SAP ECC OData url e.g. http://eccsvrname:8000/sap/opu/odata/sap/zgw100_dd02l_so_srv/>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportadas por conjunto de dados SAP ECC.

Para copiar dados de SAP ECC, defina a propriedade de tipo do conjunto de dados para **SapEccResource**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| caminho | Caminho da entidade SAP ECC OData. | Sim |

**Exemplo**

```json
{
    "name": "SapEccDataset",
    "properties": {
        "type": "SapEccResource",
        "typePoperties": {
            "path": "<entity path e.g. dd04tentitySet>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP ECC linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportado pela origem de SAP ECC.

### <a name="sap-ecc-as-source"></a>SAP ECC como origem

Para copiar dados de SAP ECC, defina o tipo de origem na atividade de cópia para **SapEccSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida: **SapEccSource** | Sim |
| consulta | Opções de consulta de OData para filtrar dados. Exemplo: "$select = o nome, descrição & $top = 10".<br/><br/>Conector SAP ECC copia dados a partir do URL combinado: (o url especificado no serviço ligado) / (caminho especificado no conjunto de dados)? (consulta especificada na origem de atividade de cópia). Consulte [componentes de URL de OData](http://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Sim |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromSAPECC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP ECC input dataset name>",
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
                "type": "SapEccSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-ecc"></a>Mapeamento de tipo de dados para SAP ECC

Quando copiar dados a partir do SAP ECC, são utilizados os seguintes mapeamentos de OData tipos de dados para dados de SAP ECC para tipos de dados intermédio do Azure Data Factory. Consulte [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md) para saber mais sobre como atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink.

| Tipo de dados de OData | Tipo de dados intermédio de fábrica de dados |
|:--- |:--- |:--- |
| Edm.Binary | Cadeia |
| Edm.Boolean | Booleano |
| Edm.Byte | Cadeia |
| Edm.DateTime | DateTime |
| Edm.Decimal | Decimal |
| Edm.Double | Duplo |
| Edm.Single | Solteiro |
| Edm.Guid | Cadeia |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Cadeia |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> Agora, não são suportados tipos de dados complexas.

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia no Azure Data Factory, consulte [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
