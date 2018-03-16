---
title: Copiar dados de origem e de Dynamics CRM ou Dynamics 365 utilizando o Azure Data Factory | Microsoft Docs
description: "Saiba como copiar dados do Microsoft Dynamics CRM ou Microsoft Dynamics 365 suportado sink arquivos de dados ou a partir de suportada arquivos de dados de origem para o Dynamics CRM ou Dynamics 365, utilizando uma atividade de cópia num pipeline de fábrica de dados."
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
ms.openlocfilehash: dc0b01e23ebb2695fd0365f054b3cacd2573f3c6
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="copy-data-from-and-to-dynamics-365-or-dynamics-crm-by-using-azure-data-factory"></a>Copiar dados de origem e de Dynamics 365 ou do Dynamics CRM através da utilização do Azure Data Factory

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de origem e de Microsoft Dynamics 365 ou do Microsoft Dynamics CRM. Baseia-se no [descrição geral da atividade de cópia](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se utilizar a versão 1 do Data Factory, o que é geralmente disponível, consulte [atividade de cópia na versão 1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de Dynamics 365 ou do Dynamics CRM para qualquer arquivo de dados suportados sink. Também pode copiar dados de qualquer arquivo de dados de origem suportada para o Dynamics 365 ou do Dynamics CRM. Para obter uma lista dos arquivos de dados suportados como origens ou sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Este conector Dynamics suporta as seguintes versões de Dynamics e tipos de autenticação. (IFD é curto para implementação de acesso à internet.)

| Versões de Dynamics | Tipos de autenticação | Exemplos de serviço ligado |
|:--- |:--- |:--- |
| Dynamics 365 online <br> Dynamics CRM Online | Office365 | [Dynamics online + autenticação do Office 365](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 no local com IFD <br> Dynamics CRM 2016 no local com IFD <br> Dynamics CRM 2015 no local com IFD | IFD | [Dynamics no local com IFD + IFD auth](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Especificamente, para o Dynamics 365 são suportados os seguintes tipos de aplicação:

- Dynamics 365 de vendas
- Dynamics 365 para o serviço de cliente
- Dynamics 365 para o serviço do campo
- Dynamics 365 para a automatização de serviço do projeto
- Dynamics 365 por razões de Marketing

Tipos de outra aplicação, por exemplo, operações e financeiros, Talent, etc. não são suportadas.

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir o Dynamics entidades do Data Factory específicas.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço ligado do Dynamics.

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 e Dynamics CRM Online

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida **Dynamics**. | Sim |
| deploymentType | O tipo de implementação da instância do Dynamics. Tem de ser **"Online"** para Dynamics online. | Sim |
| organizationName | O nome da organização da instância do Dynamics. | Não, deve especificar quando existem várias instâncias de Dynamics associadas ao utilizador |
| authenticationType | O tipo de autenticação para ligar a um servidor de Dynamics. Especifique **"Office 365"** para Dynamics online. | Sim |
| o nome de utilizador | Especifique o nome de utilizador para ligar ao Dynamics. | Sim |
| palavra-passe | Especifique a palavra-passe da conta de utilizador especificado para nome de utilizador. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Sim |
| connectVia | O [integração runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. | Não origem Sim para sink se ligado a origem de serviço não tem um tempo de execução de integração |

>[!IMPORTANT]
>Quando copiar dados para o Dynamics, a predefinição de Runtime de integração do Azure não pode ser utilizada para executar a cópia. Por outras palavras, se ligado a origem de serviço não tem um tempo de execução de integração especificado explicitamente [criar um tempo de execução de integração do Azure](create-azure-integration-runtime.md#create-azure-ir) com uma localização perto a instância de Dynamics. Associe-o no serviço ligado do Dynamics, como no exemplo seguinte.

**Exemplo: Dynamics online através da autenticação do Office 365**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics online linked service using Office365 authentication",
        "typeProperties": {
            "deploymentType": "Online",
            "organizationName": "orga02d9c75",
            "authenticationType": "Office365",
            "username": "test@contoso.onmicrosoft.com",
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

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>Dynamics 365 e Dynamics CRM no local com IFD

*Propriedades adicionais que comparam a Dynamics online são "nome do anfitrião" e "porta".*

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida **Dynamics**. | Sim |
| deploymentType | O tipo de implementação da instância do Dynamics. Tem de ser **"OnPremisesWithIfd"** para Dynamics no local com IFD.| Sim |
| hostName | O nome de anfitrião do servidor de Dynamics no local. | Sim |
| porta | A porta do servidor de Dynamics no local. | Não, a predefinição é 443 |
| organizationName | O nome da organização da instância do Dynamics. | Sim |
| authenticationType | O tipo de autenticação para ligar ao servidor de Dynamics. Especifique **"Ifd"** para Dynamics no local com IFD. | Sim |
| o nome de utilizador | Especifique o nome de utilizador para ligar ao Dynamics. | Sim |
| palavra-passe | Especifique a palavra-passe da conta de utilizador especificado para nome de utilizador. Pode escolher marcar este campo como um SecureString armazena de forma segura na ADF ou armazenar a palavra-passe no Cofre de chaves do Azure e permitir que a atividade de cópia solicitar a partir daí quando efetuar a cópia de dados - Saiba mais de [armazenar credenciais no Cofre de chaves](store-credentials-in-key-vault.md). | Sim |
| connectVia | O [integração runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. | Não para a origem, Sim para sink |

>[!IMPORTANT]
>Para copiar dados para Dynamics, explicitamente [criar um tempo de execução de integração do Azure](create-azure-integration-runtime.md#create-azure-ir) com a localização junto da sua instância de Dynamics. Associe-o no serviço ligado do exemplo seguinte.

**Exemplo: Dynamics no local com IFD através da autenticação IFD**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics on-premises with IFD linked service using IFD authentication",
        "typeProperties": {
            "deploymentType": "OnPremisesWithIFD",
            "hostName": "contosodynamicsserver.contoso.com",
            "port": 443,
            "organizationName": "admsDynamicsTest",
            "authenticationType": "Ifd",
            "username": "test@contoso.onmicrosoft.com",
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

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportadas por Dynamics conjunto de dados.

Para copiar dados de origem e de Dynamics, defina a propriedade de tipo do conjunto de dados para **DynamicsEntity**. São suportadas as seguintes propriedades.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida **DynamicsEntity**. |Sim |
| entityName | O nome lógico da entidade a obter. | Não para a origem (se não for especificada "consulta" na origem de atividade), Sim para sink |

> [!IMPORTANT]
>- Quando copiar dados de Dynamics, a secção "estrutura" é necessária no Dynamics conjunto de dados. Define o tipo de dados e o nome de coluna para dados de Dynamics que pretende copiar através de. Para obter mais informações, consulte [estrutura do conjunto de dados](concepts-datasets-linked-services.md#dataset-structure) e [mapeamento de tipo de dados para o Dynamics](#data-type-mapping-for-dynamics).
>- Quando copiar dados para o Dynamics, a secção "estrutura" é opcional no Dynamics conjunto de dados. As colunas para copiar para é determinado pelo esquema de origem de dados. Se a origem é um ficheiro CSV sem cabeçalho, no conjunto de dados de entrada, especifique "estrutura" com o tipo de dados e o nome de coluna. Mapeiam para os campos no ficheiro CSV de um de cada por ordem.

**Exemplo:**

```json
{
    "name": "DynamicsDataset",
    "properties": {
        "type": "DynamicsEntity",
        "structure": [
            {
                "name": "accountid",
                "type": "Guid"
            },
            {
                "name": "name",
                "type": "String"
            },
            {
                "name": "marketingonly",
                "type": "Boolean"
            },
            {
                "name": "modifiedon",
                "type": "Datetime"
            }
        ],
        "typeProperties": {
            "entityName": "account"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics linked service name>",
            "type": "linkedservicereference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportadas por tipos de origem e dependente do Dynamics.

### <a name="dynamics-as-a-source-type"></a>Dynamics como um tipo de origem

Para copiar dados de Dynamics, defina o tipo de origem na atividade de cópia para **DynamicsSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida **DynamicsSource**. | Sim |
| consulta | FetchXML é uma linguagem de consulta proprietárias, que é utilizada no Dynamics (online e no local). Veja o seguinte exemplo. Para obter mais informações, consulte [construir consultas com FeachXML](https://msdn.microsoft.com/en-us/library/gg328332.aspx). | Não (se for especificado "entityName" no conjunto de dados) |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DynamicsSource",
                "query": "<FetchXML Query>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sample-fetchxml-query"></a>Consulta de FetchXML de exemplo

```xml
<fetch>
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="marketingonly" />
    <attribute name="modifiedon" />
    <order attribute="modifiedon" descending="false" />
    <filter type="and">
      <condition attribute ="modifiedon" operator="between">
        <value>2017-03-10 18:40:00z</value>
        <value>2017-03-12 20:40:00z</value>
      </condition>
    </filter>
  </entity>
</fetch>
```

### <a name="dynamics-as-a-sink-type"></a>Dynamics como um tipo de sink

Para copiar dados para o Dynamics, defina o tipo de sink na atividade de cópia para **DynamicsSink**. As seguintes propriedades são suportadas na atividade de cópia **sink** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo de sink de atividade de cópia tem de ser definida **DynamicsSink**. | Sim |
| WriteBehavior | O comportamento da operação de escrita.<br/>Permitido é de valor **"Upsert"**. | Sim |
| writeBatchSize | O número de linhas de dados escritos na Dynamics em cada lote. | Não (a predefinição é 10) |
| ignoreNullValues | Indica se deve ignorar valores nulos de dados de entrada (exceto os campos de chave) durante uma operação de escrita.<br/>Valores permitidos são **verdadeiro** e **falso**.<br>- **Verdadeiro**: mantenha os dados no objeto de destino inalterados quando fizer uma operação upsert/atualização. Inserir um valor predefinido definido quando efetuar uma operação de inserção.<br/>- **FALSO**: atualizar os dados no objeto de destino como NULL quando fizer uma operação upsert/atualização. Inserir um valor nulo quando efetuar uma operação de inserção. | Não (a predefinição é false) |

>[!NOTE]
>O valor predefinido de writeBatchSize o sink e a atividade de cópia [parallelCopies](copy-activity-performance.md#parallel-copy) para o sink de Dynamics são ambos os 10. Por conseguinte, 100 registos são submetidos Dynamics em simultâneo.

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyToDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Dynamics output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DynamicsSink",
                "writeBehavior": "Upsert",
                "writeBatchSize": 10,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="data-type-mapping-for-dynamics"></a>Tipo de dados de mapeamento para o Dynamics

Quando copiar dados de Dynamics, os seguintes mapeamentos são utilizados Dynamics tipos de dados para tipos de dados intermédio do Data Factory. Para saber como a atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink, consulte [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md).

Configure o tipo de dados do Data Factory correspondente numa estrutura de conjunto de dados com base na sua origem de tipo de dados de Dynamics utilizando a seguinte tabela de mapeamento.

| Tipo de dados de Dynamics | Tipo de dados intermédio de fábrica de dados | Suportada como origem | Suportado como sink |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode.BigInt | Longo | ✓ | ✓ |
| AttributeTypeCode.Boolean | Booleano | ✓ | ✓ |
| AttributeType.DateTime | Datetime | ✓ | ✓ |
| AttributeType.Decimal | Decimal | ✓ | ✓ |
| AttributeType.Double | Duplo | ✓ | ✓ |
| AttributeType.EntityName | Cadeia | ✓ | ✓ |
| AttributeType.Integer | Int32 | ✓ | ✓ |
| AttributeType.Lookup | GUID | ✓ | |
| AttributeType.ManagedProperty | Booleano | ✓ | |
| AttributeType.Memo | Cadeia | ✓ | ✓ |
| AttributeType.Money | Decimal | ✓ | ✓ |
| AttributeType.Owner | GUID | ✓ | |
| AttributeType.Picklist | Int32 | ✓ | ✓ |
| AttributeType.Uniqueidentifier | GUID | ✓ | ✓ |
| AttributeType.String | Cadeia | ✓ | ✓ |
| AttributeType.State | Int32 | ✓ | ✓ |
| AttributeType.Status | Int32 | ✓ | ✓ |


> [!NOTE]
> Os tipos de dados de Dynamics AttributeType.CalendarRules e AttributeType.PartyList não são suportados.

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia numa fábrica de dados, consulte [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
