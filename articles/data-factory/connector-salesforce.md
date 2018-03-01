---
title: "Copiar dados de origem e de Salesforce através da utilização do Azure Data Factory | Microsoft Docs"
description: "Saiba como copiar dados do Salesforce aos arquivos de dados de receptores suportado ou a partir de arquivos de dados de origem suportada para o Salesforce ao utilizar uma atividade de cópia num pipeline de fábrica de dados."
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
ms.date: 02/26/2018
ms.author: jingwang
ms.openlocfilehash: 3d48f1f3df7b626ec33b07b6275581821453f626
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="copy-data-from-and-to-salesforce-by-using-azure-data-factory"></a>Copiar os dados de origem e de Salesforce através da utilização do Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - Disponibilidade geral](v1/data-factory-salesforce-connector.md)
> * [Versão 2 - Pré-visualização](connector-salesforce.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de origem e de Salesforce. Baseia-se no [descrição geral da atividade de cópia](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se utilizar a versão 1 do Data Factory, o que é geralmente disponível, consulte [conector do Salesforce na versão 1](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados do Salesforce para qualquer arquivo de dados suportados sink. Também pode copiar dados de qualquer arquivo de dados de origem suportada para Salesforce. Para obter uma lista dos arquivos de dados que são suportados como origens ou sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector do Salesforce suporta:

- Edições de programador do Salesforce, Professional, Enterprise ou ilimitada.
- Copiar dados de origem e de produção, o sandbox e o domínio personalizado de Salesforce.

## <a name="prerequisites"></a>Pré-requisitos

Permissão de API tem de estar ativada no Salesforce. Para obter mais informações, consulte [acesso ativar API no Salesforce pelo conjunto de permissões](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Limites de pedido do Salesforce

O Salesforce tem limites para o total de pedidos de API e pedidos de API em simultâneo. Tenha em atenção os seguintes pontos:

- Se o número de pedidos simultâneos excede o limite, limitação ocorre e vir falhas aleatórias.
- Se o número total de pedidos excede o limite, a conta do Salesforce é bloqueada durante 24 horas.

Poderá também receber a mensagem de erro "REQUEST_LIMIT_EXCEEDED" em ambos os cenários. Para obter mais informações, consulte a secção "Limites de pedidos de API" [limites de programador do Salesforce](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf).

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o conector do Salesforce.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço ligado do Salesforce.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo |A propriedade de tipo tem de ser definida **Salesforce**. |Sim |
| environmentUrl | Especifique o URL da instância do Salesforce. <br> -Predefinição é `"https://login.salesforce.com"`. <br> -Para copiar dados a partir de sandbox, especifique `"https://test.salesforce.com"`. <br> -Para copiar dados de domínio personalizado, especificar, por exemplo, `"https://[domain].my.salesforce.com"`. |Não |
| o nome de utilizador |Especifique um nome de utilizador para a conta de utilizador. |Sim |
| palavra-passe |Especifique uma palavra-passe da conta de utilizador.<br/><br/>Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). |Sim |
| securityToken |Especifique um token de segurança para a conta de utilizador. Para obter instruções sobre como repor e obter um token de segurança, consulte [obter um token de segurança](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm). Para saber mais sobre os tokens de segurança em geral, consulte [segurança e a API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).<br/><br/>Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). |Sim |
| connectVia | O [integração runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. | Não origem Sim para sink se ligado a origem de serviço não tem tempo de execução de integração |

>[!IMPORTANT]
>Quando copiar dados para o Salesforce, a predefinição de Runtime de integração do Azure não pode ser utilizada para executar a cópia. Por outras palavras, se ligado a origem de serviço não tem um tempo de execução de integração especificado explicitamente [criar um tempo de execução de integração do Azure](create-azure-integration-runtime.md#create-azure-ir) com uma localização perto a instância do Salesforce. Associe o serviço ligado do Salesforce como no exemplo seguinte.

**Exemplo: Armazenar credenciais no Factory de dados**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "securityToken": {
                "type": "SecureString",
                "value": "<security token>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemplo: Guarde as credenciais no Cofre de chaves**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of password in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            },
            "securityToken": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of security token in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
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

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportadas pelo conjunto de dados do Salesforce.

Para copiar dados de origem e de Salesforce, defina a propriedade de tipo do conjunto de dados para **SalesforceObject**. São suportadas as seguintes propriedades.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida **SalesforceObject**.  | Sim |
| objectApiName | O nome de objeto do Salesforce ao obter dados de. | Não para a origem, Sim para sink |

> [!IMPORTANT]
> A parte "__c" **nome API** é necessário para qualquer objeto personalizado.

![Nome da API de ligação de Salesforce fábrica de dados](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Exemplo:**

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "objectApiName": "MyTable__c"
        }
    }
}
```

>[!NOTE]
>Por questões de compatibilidade: Quando copia dados do Salesforce, se utilizar o conjunto de dados de tipo de "RelationalTable" anterior, que mantém a funcionar enquanto vir uma sugestão para mudar para o novo tipo de "SalesforceObject".

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida **RelationalTable**. | Sim |
| tableName | Nome da tabela no Salesforce. | Não (se for especificada "consulta" na origem de atividade) |

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportadas por Salesforce origem e dependente.

### <a name="salesforce-as-a-source-type"></a>Salesforce como um tipo de origem

Para copiar dados do Salesforce, defina o tipo de origem na atividade de cópia para **SalesforceSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida **SalesforceSource**. | Sim |
| consulta |Utilize a consulta personalizada para ler os dados. Pode utilizar uma consulta de SQL 92 ou [idioma de consulta de objeto do Salesforce (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) consulta. Um exemplo é `select * from MyTable__c`. | Não (se for especificado "tableName" no conjunto de dados) |
| readBehavior | Indica se deve consultar os registos existentes ou consultar todos os registos, incluindo aqueles eliminado. Se não for especificado, o comportamento predefinido é a primeira. <br>Valores permitidos: **consulta** (predefinição), **queryAll**.  | Não |

> [!IMPORTANT]
> A parte "__c" **nome API** é necessário para qualquer objeto personalizado.

![Lista de nomes de API de ligação de Salesforce de fábrica de dados](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
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
                "type": "SalesforceSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

>[!NOTE]
>Por questões de compatibilidade: Quando copia dados do Salesforce, se utilizar a cópia de tipo "RelationalSource" anterior, a origem mantém funcionar enquanto vir uma sugestão para mudar para o novo tipo de "SalesforceSource".

### <a name="salesforce-as-a-sink-type"></a>Salesforce como um tipo de sink

Para copiar dados para o Salesforce, defina o tipo de sink na atividade de cópia para **SalesforceSink**. As seguintes propriedades são suportadas na atividade de cópia **sink** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo de sink de atividade de cópia tem de ser definida **SalesforceSink**. | Sim |
| WriteBehavior | O comportamento de escrita para a operação.<br/>Valores permitidos são **inserir** e **Upsert**. | Não (a predefinição é Insert) |
| externalIdFieldName | O nome do campo ID externo para a operação de upsert. O campo especificado tem de ser definido como "Campo Id externo" no objeto do Salesforce. Não pode ter valores nulos dos dados de entrada correspondente. | Sim para "Upsert" |
| writeBatchSize | O número de linhas de dados escritos na Salesforce em cada lote. | Não (a predefinição é 5 000) |
| ignoreNullValues | Indica se deve ignorar valores nulos de dados de entrada durante uma operação de escrita.<br/>Valores permitidos são **verdadeiro** e **falso**.<br>- **Verdadeiro**: mantenha os dados no objeto de destino inalterados quando fizer uma operação upsert ou atualização. Inserir um valor predefinido definido quando efetuar uma operação de inserção.<br/>- **FALSO**: atualizar os dados no objeto de destino como NULL quando fizer uma operação upsert ou atualização. Inserir um valor nulo quando efetuar uma operação de inserção. | Não (a predefinição é false) |

**Exemplo: Sink do Salesforce numa atividade de cópia**

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceSink",
                "writeBehavior": "Upsert",
                "externalIdFieldName": "CustomerId__c",
                "writeBatchSize": 10000,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="query-tips"></a>Sugestões de consulta

### <a name="retrieve-data-from-a-salesforce-report"></a>Obter dados a partir de um relatório do Salesforce

Pode obter dados de relatórios do Salesforce ao especificar uma consulta como `{call "<report name>"}`. Um exemplo é `"query": "{call \"TestReport\"}"`.

### <a name="retrieve-deleted-records-from-the-salesforce-recycle-bin"></a>Obter registos eliminados do Salesforce Reciclagem

Para consultar os registos eliminados recuperável do Salesforce Reciclagem, pode especificar **"IsDeleted = 1"** na sua consulta. Por exemplo:

* Para consultar apenas os registos eliminados, especifique "selecionar * de MyTable__c **onde IsDeleted = 1**."
* Para consultar todos os registos, incluindo o existente e o eliminado, especifique "selecionar * de MyTable__c **onde IsDeleted = 0 ou IsDeleted = 1**."

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>Obter dados utilizando um onde cláusula na coluna DateTime

Quando especificar a consulta SOQL ou SQL Server, preste atenção a diferença de formato DateTime. Por exemplo:

* **Exemplo SOQL**: `SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **Exemplo SQL**: `SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}"`

## <a name="data-type-mapping-for-salesforce"></a>Tipo de dados de mapeamento para o Salesforce

Quando copia dados do Salesforce, os seguintes mapeamentos são utilizados Salesforce tipos de dados para tipos de dados intermédio do Data Factory. Para saber mais sobre como a atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink, consulte [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md).

| Tipo de dados do Salesforce | Tipo de dados intermédio de fábrica de dados |
|:--- |:--- |
| Número de automática |Cadeia |
| Caixa de verificação |Booleano |
| Moeda |Duplo |
| Data |DateTime |
| Data/Hora |DateTime |
| E-mail |Cadeia |
| Id |Cadeia |
| Relação de referência |Cadeia |
| Selecionar vários Picklist |Cadeia |
| Número |Duplo |
| Percentagem |Duplo |
| Telefone |Cadeia |
| Picklist |Cadeia |
| Texto |Cadeia |
| Área de texto |Cadeia |
| Área de texto (longa) |Cadeia |
| Área de texto (avançada) |Cadeia |
| Texto (encriptado) |Cadeia |
| do IdP |Cadeia |

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia numa fábrica de dados, consulte [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).