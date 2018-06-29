---
title: Copiar dados de ou para SQL Database do Azure utilizando o Data Factory | Microsoft Docs
description: Saiba como copiar dados de arquivos de dados de origem suportada para a SQL Database do Azure ou de base de dados SQL para os arquivos de dados dependente suportados por utilizar o Data Factory.
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
ms.date: 05/05/2018
ms.author: jingwang
ms.openlocfilehash: 5287a1d1f09a7057590b455c14aa7f70128ad7fa
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37053647"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Copiar os dados de ou para SQL Database do Azure utilizando o Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you use:"]
> * [Versão 1](v1/data-factory-azure-sql-connector.md)
> * [Versão atual](connector-azure-sql-database.md)

Este artigo explica como utilizar a atividade de cópia no Azure Data Factory para copiar dados de ou para a SQL Database do Azure. Baseia-se no [descrição geral da atividade de cópia](copy-activity-overview.md) artigo, que apresenta uma descrição geral da atividade de cópia.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de ou para a SQL Database do Azure para qualquer arquivo de dados suportados sink. E pode copiar dados a partir de qualquer arquivo de dados de origem suportada para SQL Database do Azure. Para obter uma lista dos arquivos de dados que são suportados como origens ou sinks pela atividade de cópia, consulte o [arquivos de dados e formatos suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector da SQL Database do Azure suporta estas funções:

- Copie os dados utilizando autenticação do SQL Server e a autenticação de token de aplicação do Azure Active Directory (Azure AD) com um principal de serviço ou uma identidade de serviço geridas (MSI).
- Obter os dados utilizando uma consulta de SQL Server ou o procedimento armazenado como uma origem.
- Como um sink acrescentar dados a uma tabela de destino ou invocar um procedimento armazenado com lógica personalizada durante a cópia.

> [!IMPORTANT]
> Se copiar dados utilizando o tempo de execução do Azure Data Factory integração, configure um [firewall do servidor SQL do Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) para que os serviços do Azure podem aceder ao servidor.
> Se copiar os dados através da utilização de um tempo de execução automática alojada integração, configure a firewall do servidor SQL do Azure para permitir que o intervalo IP adequado. Este intervalo inclui IP do computador que é utilizada para ligar à SQL Database do Azure.

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para um conector do SQL Database do Azure.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

Estas propriedades são suportadas para um serviço ligado de SQL Database do Azure:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | O **tipo** propriedade tem de ser definida como **AzureSqlDatabase**. | Sim |
| connectionString | Especificar as informações necessárias para ligar à instância do SQL Database do Azure para o **connectionString** propriedade. Marcar este campo como um **SecureString** armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Sim |
| servicePrincipalId | Especifique o ID de cliente. da aplicação | Sim, quando utiliza a autenticação do Azure AD com um principal de serviço. |
| servicePrincipalKey | Especifique a chave da aplicação. Marcar este campo como um **SecureString** armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Sim, quando utiliza a autenticação do Azure AD com um principal de serviço. |
| inquilino | Especifique as informações de inquilino (nome ou o inquilino ID de domínio) em que reside a aplicação. Obtê-lo por posicionado o rato no canto superior direito do portal do Azure. | Sim, quando utiliza a autenticação do Azure AD com um principal de serviço. |
| connectVia | O [integração runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração do Azure ou de um tempo de execução de integração personalizada alojada se o arquivo de dados estiver localizado numa rede privada. Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. | Não |

Para tipos de autenticação diferentes, consulte as secções seguintes na pré-requisitos e exemplos JSON, respetivamente:

- [Autenticação do SQL Server](#sql-authentication)
- [Autenticação de token do Azure AD aplicação: principal de serviço](#service-principal-authentication)
- [Autenticação de token do Azure AD aplicação: identidade de serviço geridas](#managed-service-identity-authentication)

### <a name="sql-authentication"></a>Autenticação do SQL

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Exemplo de serviço ligado que utiliza a autenticação do SQL Server

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Autenticação do principal de serviço

Para utilizar uma autenticação de token de aplicação de serviço com base em principal do Azure AD, siga estes passos:

1. **[Criar uma aplicação do Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application)**  do portal do Azure. Anote o nome da aplicação e os valores seguintes que definem o serviço ligado:

    - ID da aplicação
    - Chave da aplicação
    - ID do inquilino

2. **[Aprovisionar um administrador do Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  para o servidor de SQL do Azure no portal do Azure se ainda não o tiver feito. O administrador do Azure AD tem de ser um utilizador do Azure AD ou o grupo do Azure AD, mas não pode ser um principal de serviço. Este passo é realizado para que, no próximo passo, pode utilizar uma identidade do Azure AD para criar um utilizador de base de dados contida para o serviço principal.

3. **[Criar utilizadores de base de dados contida](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  para o principal de serviço. Ligar à base de dados de ou para o qual pretende copiar os dados utilizando ferramentas como o SSMS, com uma identidade do Azure AD que tenha, pelo menos, permissões ALTER qualquer utilizador. Execute o seguinte T-SQL: 
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Conceder as permissões necessárias do principal de serviço** como faz normalmente para os utilizadores do SQL Server ou outras pessoas. Execute o seguinte código:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. **Configurar um serviço ligado de SQL Database do Azure** no Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Exemplo de serviço ligado que utiliza a autenticação principal de serviço

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-service-identity-authentication"></a>Autenticação de identidade de serviço gerida

Uma fábrica de dados pode ser associada um [identidade de serviço geridas](data-factory-service-identity.md) que representa a fábrica de dados específicos. Pode utilizar esta identidade de serviço para a autenticação de SQL Database do Azure. A fábrica designada pode aceder e copiar dados de ou para a base de dados ao utilizar esta identidade.

Para utilizar a autenticação de token de aplicação baseada em MSI do Azure AD, siga estes passos:

1. **Crie um grupo no Azure AD.** Torne a fábrica do MSI um membro do grupo.

    a. Determinar a identidade de serviço da fábrica de dados do portal do Azure. Vá para a fábrica de dados **propriedades**. Copie o ID de identidade de serviço.

    b. Instalar o [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) módulo. Inicie sessão utilizando o `Connect-AzureAD` comando. Execute os seguintes comandos para criar um grupo e adicionar a fábrica de dados MSI como membro.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

2. **[Aprovisionar um administrador do Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  para o servidor de SQL do Azure no portal do Azure se ainda não o tiver feito. O administrador do Azure AD pode ser um utilizador do Azure AD ou o grupo do Azure AD. Se conceder o grupo com MSI uma função de administrador, ignore os passos 3 e 4. O administrador terá acesso total à base de dados.

3. **[Criar utilizadores de base de dados contida](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  para o grupo do Azure AD. Ligar à base de dados de ou para o qual pretende copiar os dados utilizando ferramentas como o SSMS, com uma identidade do Azure AD que tenha, pelo menos, permissões ALTER qualquer utilizador. Execute o seguinte T-SQL: 
    
    ```sql
    CREATE USER [your AAD group name] FROM EXTERNAL PROVIDER;
    ```

4. **Conceder as permissões necessárias do grupo do Azure AD** como faz normalmente para os utilizadores do SQL e outros. Por exemplo, execute o seguinte código:

    ```sql
    EXEC sp_addrolemember [role name], [your AAD group name];
    ```

5. **Configurar um serviço ligado de SQL Database do Azure** no Azure Data Factory.

#### <a name="linked-service-example-that-uses-msi-authentication"></a>Exemplo de serviço ligado que utiliza a autenticação de MSI

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
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

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](https://docs.microsoft.com/en-us/azure/data-factory/concepts-datasets-linked-services) artigo. Esta secção fornece uma lista de propriedades suportado pelo conjunto de dados SQL Database do Azure.

Para copiar dados de ou para a SQL Database do Azure, defina o **tipo** propriedade do conjunto de dados para **AzureSqlTable**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | O **tipo** propriedade do conjunto de dados tem de ser definida como **AzureSqlTable**. | Sim |
| tableName | O nome da tabela ou vista na instância SQL Database do Azure que referencia o serviço ligado. | Sim |

#### <a name="dataset-properties-example"></a>Exemplo de propriedades do conjunto de dados

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportadas pela origem de SQL Database do Azure e o sink.

### <a name="azure-sql-database-as-the-source"></a>Base de dados Azure SQL Server como origem

Para copiar dados de SQL Database do Azure, defina o **tipo** propriedade da origem de atividade de cópia para **SqlSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | O **tipo** propriedade da origem de atividade de cópia tem de ser definida como **SqlSource**. | Sim |
| sqlReaderQuery | Utilize a consulta SQL personalizada para ler os dados. Exemplo: `select * from MyTable`. | Não |
| sqlReaderStoredProcedureName | O nome do procedimento armazenado que lê dados a partir da tabela de origem. A última instrução de SQL tem de ser uma instrução SELECT no procedimento armazenado. | Não |
| storedProcedureParameters | Parâmetros para o procedimento armazenado.<br/>Valores permitidos são pares de nome ou valor. Nomes e maiúsculas e minúsculas de parâmetros têm de corresponder os nomes e a maiúsculas e minúsculas dos parâmetros de procedimento armazenado. | Não |

### <a name="points-to-note"></a>Pontos a ter em atenção

- Se o **sqlReaderQuery** especificado para o **SqlSource**, atividade de cópia executa esta consulta contra a origem de SQL Database do Azure para obter os dados. Em alternativa, pode especificar um procedimento armazenado. Especifique **sqlReaderStoredProcedureName** e **storedProcedureParameters** se o procedimento armazenado aceita parâmetros.
- Se não especificar um **sqlReaderQuery** ou **sqlReaderStoredProcedureName**, as colunas definidas no **estrutura** secção do conjunto de dados JSON são utilizados para construir uma consulta. `select column1, column2 from mytable` é executada relativamente a SQL Database do Azure. Se a definição do conjunto de dados não tem o **estrutura**, todas as colunas são selecionadas da tabela.
- Quando utiliza **sqlReaderStoredProcedureName**, terá de especificar um fictício **tableName** propriedade no conjunto de dados JSON.

#### <a name="sql-query-example"></a>Exemplo de consulta SQL

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="stored-procedure-example"></a>Exemplo de procedimento armazenado

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="stored-procedure-definition"></a>Definição de procedimento armazenado

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-as-the-sink"></a>Base de dados de SQL do Azure como o sink

Para copiar dados para a SQL Database do Azure, defina o **tipo** sink propriedade na atividade de cópia para **SqlSink**. As seguintes propriedades são suportadas na atividade de cópia **sink** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | O **tipo** propriedade o sink de atividade de cópia tem de ser definida **SqlSink**. | Sim |
| writeBatchSize | Insere dados na tabela SQL quando atinge o tamanho da memória intermédia **writeBatchSize**.<br/> O valor permitido é **número inteiro** (número de linhas). | Não. A predefinição é 10000. |
| writeBatchTimeout | O tempo de espera para o lote de inserção concluir antes de atingir o tempo limite da operação.<br/> O valor permitido é **timespan**. Exemplo: "00: 30:00" (30 minutos). | Não |
| preCopyScript | Especifique uma consulta SQL para a atividade de cópia ser executada antes de escrever dados na base de dados do Azure SQL. Só é invocado uma vez por cópia executar. Utilize esta propriedade para limpar os dados pré-carregada. | Não |
| sqlWriterStoredProcedureName | O nome do procedimento armazenado que define como aplicar dados de origem para uma tabela de destino. Um exemplo é não upserts ou transformar por utilizar a sua própria lógica de negócio. <br/><br/>Este procedimento armazenado é **invocado por lote**. Para operações que apenas são executados uma vez e não com dados de origem, utilize o `preCopyScript` propriedade. As operações de exemplo são delete e truncar. | Não |
| storedProcedureParameters |Parâmetros para o procedimento armazenado.<br/>Valores permitidos são pares nome / valor. Nomes e maiúsculas e minúsculas de parâmetros têm de corresponder os nomes e a maiúsculas e minúsculas dos parâmetros de procedimento armazenado. | Não |
| sqlWriterTableType | Especifique um nome de tipo de tabela a ser utilizado no procedimento armazenado. Atividade de cópia faz com que os dados a ser movidos disponível numa tabela temporária com este tipo de tabela. Código do procedimento armazenado, em seguida, pode intercalar os dados que está a ser copiados com dados existentes. | Não |

> [!TIP]
> Quando copiar dados para a SQL Database do Azure, atividade de cópia acrescenta dados para a tabela do sink por predefinição. Para fazer uma upsert ou lógica de negócio adicionais, utilize o procedimento armazenado no **SqlSink**. Saiba mais detalhes de [invocar um procedimento armazenado de SQL Sink](#invoking-stored-procedure-for-sql-sink).

#### <a name="append-data-example"></a>Acrescentar o exemplo de dados

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

#### <a name="invoke-a-stored-procedure-during-copy-for-upsert-example"></a>Invocar um procedimento armazenado durante a cópia para upsert exemplo

Saiba mais detalhes de [invocar um procedimento armazenado de SQL Sink](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="identity-columns-in-the-target-database"></a>Colunas de identidade na base de dados de destino

Esta secção mostra como copiar dados a partir de uma tabela de origem sem uma coluna de identidade para uma tabela de destino com uma coluna de identidade.

#### <a name="source-table"></a>Tabela de origem

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

#### <a name="destination-table"></a>Tabela de destino

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

> [!NOTE]
> A tabela de destino tem uma coluna de identidade.

#### <a name="source-dataset-json-definition"></a>Definição de JSON do conjunto de dados de origem

```json
{
    "name": "SampleSource",
    "properties": {
        "type": " AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTbl"
        }
    }
}
```

#### <a name="destination-dataset-json-definition"></a>Definição de JSON do conjunto de dados de destino

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "TestIdentitySQL",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "TargetTbl"
        }
    }
}
```

> [!NOTE]
> A tabela de origem e destino têm esquemas diferentes. 

O destino tem uma coluna adicional com uma identidade. Neste cenário, tem de especificar o **estrutura** propriedade na definição de conjunto de dados de destino, que não inclui a coluna de identidade.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Invocar um procedimento armazenado do sink do SQL Server

Quando copia dados na base de dados do Azure SQL, também pode configurar e invocar um procedimento armazenado especificado de um utilizador com os parâmetros adicionais.

Pode utilizar um procedimento armazenado quando mecanismos incorporados de cópia não servem o objetivo. São utilizados, normalmente, quando um upsert, a inserir e a atualização ou processamento adicional tem de ser efetuado antes da final inserção de dados de origem na tabela de destino. Alguns exemplos de processamento adicional são colunas de intercalação, procura de valores adicionais e a inserção em mais de uma tabela.

O exemplo seguinte mostra como utilizar um procedimento armazenado para efetuar um upsert numa tabela numa SQL Database do Azure. Partem do princípio de que dados de entrada e o sink **Marketing** tabela cada ter três colunas: **ProfileID**, **estado**, e **categoria**. Efetue a upsert com base no **ProfileID** coluna e apenas aplicá-la para uma categoria específica.

#### <a name="output-dataset"></a>Conjunto de dados de saída

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Definir o **SqlSink** secção na atividade de cópia:

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

Na base de dados, definir o procedimento armazenado com o mesmo nome que o **SqlWriterStoredProcedureName**. Processa dados de entrada da sua origem especificado e intercala na tabela de saída. O nome do parâmetro do procedimento armazenado deve ser o mesmo que o **tableName** definida no conjunto de dados.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category)
END
```

Na base de dados, definir o tipo de tabela com o mesmo nome que o **sqlWriterTableType**. O esquema do tipo de tabela deve ser igual ao esquema devolvido pelos seus dados de entrada.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL,
)
```

A funcionalidade de procedimento armazenado tira partido da [Table-Valued parâmetros](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database"></a>Mapeamento de tipo de dados para a SQL Database do Azure

Quando copiar dados de ou para a SQL Database do Azure, os seguintes mapeamentos são utilizados de tipos de dados de SQL Database do Azure para tipos de dados intermédio do Azure Data Factory. Consulte [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md) para saber como atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink.

| Tipo de dados de base de dados SQL do Azure | Tipo de dados intermédio de fábrica de dados |
|:--- |:--- |
| bigint |Int64 |
| Binário |Byte[] |
| bits |Booleano |
| char |Cadeia, Char [] |
| data |DateTime |
| Datetime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| Atributo FILESTREAM (varbinary(max)) |Byte[] |
| Flutuante |Valor de duplo |
| image |Byte[] |
| Int |Int32 |
| dinheiro |Decimal |
| nchar |Cadeia, Char [] |
| ntext |Cadeia, Char [] |
| um valor numérico |Decimal |
| nvarchar |Cadeia, Char [] |
| real |Único |
| ROWVERSION |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| em smallmoney |Decimal |
| sql_variant |Objeto * |
| texto |Cadeia, Char [] |
| hora |TimeSpan |
| carimbo de data/hora |Byte[] |
| tinyint |Byte |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Cadeia, Char [] |
| xml |Xml |

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia no Azure Data Factory, consulte [arquivos de dados e formatos suportados](copy-activity-overview.md##supported-data-stores-and-formats).
