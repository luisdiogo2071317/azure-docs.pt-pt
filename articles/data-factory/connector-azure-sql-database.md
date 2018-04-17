---
title: Copiar dados da base de dados do SQL do Azure utilizando o Data Factory | Microsoft Docs
description: Saiba como copiar dados de arquivos de dados de origem suportada para a SQL Database do Azure (ou) da base de dados do SQL Server aos arquivos de dados dependente suportados por utilizar o Data Factory.
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
ms.date: 04/13/2018
ms.author: jingwang
ms.openlocfilehash: c4f27f59412fbfc72e193f916895c3e67091f5f6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Copiar os dados de ou para SQL Database do Azure utilizando o Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - GA](v1/data-factory-azure-sql-connector.md)
> * [Versão 2 - Pré-visualização](connector-azure-sql-database.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de origem e de uma base de dados do SQL do Azure. Baseia-se no [copiar descrição geral da atividade](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [conector de SQL Database do Azure no V1](v1/data-factory-azure-sql-connector.md).

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de/para SQL Database do Azure para qualquer arquivo de dados suportados sink ou copiar dados a partir de qualquer arquivo de dados de origem suportada para SQL Database do Azure. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector de SQL Database do Azure suporta:

- Copiar dados utilizando **autenticação SQL**, e **autenticação de token de aplicação do Azure Active Directory** com o Principal de serviço ou a identidade de serviço geridas (MSI).
- Como origem, a obtenção de dados através de consulta de SQL Server ou o procedimento armazenado.
- Como sink, acrescentar dados para a tabela de destino ou invocar um procedimento armazenado com lógica personalizada durante a cópia.

> [!IMPORTANT]
> Se copiar dados através de Runtime de integração do Azure, configure [Firewall de servidor de SQL do Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) para [permitir que os serviços do Azure aceder ao servidor](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Se copiar os dados utilizando o tempo de execução do Self-hosted integração, configure a firewall do servidor SQL do Azure para permitir que o intervalo IP adequado, incluindo o IP do computador que é utilizada para ligar à SQL Database do Azure.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o conector do SQL Database do Azure.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço ligado de SQL Database do Azure:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida: **AzureSqlDatabase** | Sim |
| connectionString |Especificar as informações necessárias para ligar à instância do SQL Database do Azure para a propriedade connectionString. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). |Sim |
| servicePrincipalId | Especifique o ID de cliente. da aplicação | Sim quando utilizar a autenticação do AAD com o Principal de serviço. |
| servicePrincipalKey | Especifique a chave da aplicação. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Sim quando utilizar a autenticação do AAD com o Principal de serviço. |
| inquilino | Especifique as informações de inquilino (nome ou o inquilino ID de domínio) em que reside a aplicação. Pode obtê-lo por posicionado o rato no canto superior direito do portal do Azure. | Sim quando utilizar a autenticação do AAD com o Principal de serviço. |
| connectVia | O [integração Runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração do Azure ou o tempo de execução do Self-hosted integração (se o arquivo de dados esteja localizado numa rede privada). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

Para tipos de autenticação diferentes, consulte as secções seguintes na pré-requisitos e exemplos JSON respetivamente:

- [Utilizando a autenticação SQL](#using-sql-authentication)
- [Utilizar a autenticação de token de aplicação AAD - principal de serviço](#using-service-principal-authentication)
- [Utilizando a autenticação de token do AAD aplicação - identidade de serviço geridas](#using-managed-service-identity-authentication)

### <a name="using-sql-authentication"></a>Utilizando a autenticação SQL

**Exemplo de serviço ligado através da autenticação do SQL Server:**

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

### <a name="using-service-principal-authentication"></a>Utilizar a autenticação principal de serviço

Para utilizar o serviço principal autenticação com base em AAD aplicação token, siga estes passos:

1. **[Criar uma aplicação do Azure Active Directory a partir do portal do Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application).**  Anote o nome da aplicação e os valores seguintes que utilizar para definir o serviço ligado:

    - ID da Aplicação
    - Chave da aplicação
    - ID do Inquilino

2. **[Aprovisionar um administrador do Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#create-an-azure-ad-administrator-for-azure-sql-server)**  para o servidor de SQL do Azure no portal do Azure se ainda não o feito. O administrador AAD tem de ser um utilizador do AAD ou grupo AAD, mas não pode ser um principal de serviço. Este passo é realizado para que, num passo subsequente, pode utilizar uma identidade do AAD para criar um utilizador de base de dados contida para o serviço principal.

3. **Criar um utilizador de base de dados contida para o principal de serviço**, estabelecendo ligação à base de dados de/para a qual pretende copiar os dados utilizando ferramentas como o SSMS, com um AAD identidade ter, pelo menos, ALTER qualquer permissão de utilizador e a executar o seguinte T-SQL. Saiba mais no utilizador de uma base de dados contida [aqui](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities).
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Conceder as permissões necessárias do principal de serviço** forma que normalmente para os utilizadores do SQL Server, por exemplo, executando abaixo:

    ```sql
    EXEC sp_addrolemember '[your application name]', 'readonlyuser';
    ```

5. ADF, configure um serviço ligado de SQL Database do Azure.


**Exemplo de serviço ligado através da autenticação principal de serviço:**

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

### <a name="using-managed-service-identity-authentication"></a>Utilizando a autenticação de identidade de serviço geridas

Uma fábrica de dados pode ser associada com um [identidade do serviço (MSI) gerido](data-factory-service-identity.md), que representa esta fábrica de dados específicos. Pode utilizar esta identidade de serviço para autenticação de SQL Database do Azure, o que permite que este designado fábrica de dados de acesso e de cópia de/para a base de dados.

Para utilizar MSI baseado em autenticação de token do AAD aplicação, siga estes passos:

1. **Crie um grupo no Azure AD e tornar a fábrica do MSI num membro do grupo**.

    a. Determinar a identidade de serviço do data factory no portal do Azure. Aceda à fábrica de dados -> propriedades -> cópia o **ID do serviço de identidade**.

    b. Instalar o [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) módulo, início de sessão com `Connect-AzureAD` de comandos e execute os seguintes comandos para criar um grupo e adicionar a fábrica de dados MSI como membro.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

2. **[Aprovisionar um administrador do Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#create-an-azure-ad-administrator-for-azure-sql-server)**  para o servidor de SQL do Azure no portal do Azure se ainda não o feito. O administrador AAD pode ser um utilizador do AAD ou grupo AAD. Se conceder o grupo com MSI uma função de administrador, ignore o passo 3 e 4 abaixo, o administrador iria têm acesso total à BD.

3. **Criar um utilizador de base de dados contida para o grupo AAD**, estabelecendo ligação à base de dados de/para a qual pretende copiar os dados utilizando ferramentas como o SSMS, com um AAD identidade ter, pelo menos, ALTER qualquer permissão de utilizador e a executar o seguinte T-SQL. Saiba mais no utilizador de uma base de dados contida [aqui](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities).
    
    ```sql
    CREATE USER [your AAD group name] FROM EXTERNAL PROVIDER;
    ```

4. **Conceder as permissões necessárias do grupo AAD** forma que normalmente para os utilizadores do SQL Server, por exemplo, executando abaixo:

    ```sql
    EXEC sp_addrolemember '[your AAD group name]', 'readonlyuser';
    ```

5. ADF, configure um serviço ligado de SQL Database do Azure.

**Exemplo de serviço ligado através da autenticação de MSI:**

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

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o artigo de conjuntos de dados. Esta secção fornece uma lista de propriedades suportadas por conjunto de dados do SQL Database do Azure.

Para copiar dados de/para a SQL Database do Azure, defina a propriedade de tipo do conjunto de dados para **AzureSqlTable**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida: **AzureSqlTable** | Sim |
| tableName |Nome da tabela ou vista na instância SQL Database do Azure que o serviço ligado refere-se. | Sim |

**Exemplo:**

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

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportadas por origem de SQL Database do Azure e o sink.

### <a name="azure-sql-database-as-source"></a>Base de dados Azure SQL Server como origem

Para copiar dados de SQL Database do Azure, defina o tipo de origem na atividade de cópia para **SqlSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida: **SqlSource** | Sim |
| sqlReaderQuery |Utilize a consulta SQL personalizada para ler os dados. Exemplo: `select * from MyTable`. |Não |
| sqlReaderStoredProcedureName |Nome do procedimento armazenado que lê dados a partir da tabela de origem. A última instrução de SQL tem de ser uma instrução SELECT no procedimento armazenado. |Não |
| storedProcedureParameters |Parâmetros para o procedimento armazenado.<br/>Valores permitidos são: pares nome/valor. Nomes e maiúsculas e minúsculas de parâmetros têm de corresponder os nomes e a maiúsculas e minúsculas dos parâmetros de procedimento armazenado. |Não |

**Pontos a ter em atenção:**

- Se o **sqlReaderQuery** está especificado para o SqlSource, a atividade de cópia executa esta consulta contra a origem de SQL Database do Azure para obter os dados. Em alternativa, pode especificar um procedimento armazenado, especificando o **sqlReaderStoredProcedureName** e **storedProcedureParameters** (se o procedimento armazenado recebe parâmetros).
- Se não especificar "sqlReaderQuery" ou "sqlReaderStoredProcedureName", as colunas definidas na secção "estrutura" do conjunto de dados JSON são utilizadas para construir uma consulta (`select column1, column2 from mytable`) para executar a base de dados do SQL do Azure. Se a definição do conjunto de dados não tiver a estrutura"", todas as colunas são selecionadas da tabela.
- Quando utiliza **sqlReaderStoredProcedureName**, terá de especificar um fictício **tableName** propriedade no conjunto de dados JSON.

**Exemplo: utilizar a consulta SQL**

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

**Exemplo: utilizar o procedimento armazenado**

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

**A definição de procedimento armazenado:**

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

### <a name="azure-sql-database-as-sink"></a>Base de dados de SQL do Azure como sink

Para copiar dados para a SQL Database do Azure, defina o tipo de sink na atividade de cópia para **SqlSink**. As seguintes propriedades são suportadas na atividade de cópia **sink** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo de sink de atividade de cópia tem de ser definida: **SqlSink** | Sim |
| writeBatchSize |Insere dados para a tabela SQL quando o tamanho da memória intermédia atinge writeBatchSize.<br/>Valores permitidos são: número inteiro (número de linhas). |Não (a predefinição é 10000) |
| writeBatchTimeout |De tempo de espera para a operação de inserção de lote seja concluída antes de atingir o tempo limite.<br/>Valores permitidos são: timespan. Exemplo: "00: 30:00" (30 minutos). |Não |
| preCopyScript |Especifique uma consulta SQL para a atividade de cópia executar antes de escrever dados na base de dados do Azure SQL. Este será apenas ser invocado uma vez por cópia executar. Pode utilizar esta propriedade para limpar os dados previamente carregados. |Não |
| sqlWriterStoredProcedureName |Nome do procedimento armazenado que define como aplicar dados de origem na tabela de destino, por exemplo, efetue upserts ou utilizando a sua própria lógica de negócio de transformação. <br/><br/>Tenha em atenção de que este procedimento armazenado será **invocado por lote**. Se pretender efetuar a operação que apenas é executada uma vez e não tem nada a fazer com dados de origem, por exemplo, eliminar/truncar, utilize `preCopyScript` propriedade. |Não |
| storedProcedureParameters |Parâmetros para o procedimento armazenado.<br/>Valores permitidos são: pares nome/valor. Nomes e maiúsculas e minúsculas de parâmetros têm de corresponder os nomes e a maiúsculas e minúsculas dos parâmetros de procedimento armazenado. |Não |
| sqlWriterTableType |Especifique um nome de tipo de tabela a ser utilizado no procedimento armazenado. Atividade de cópia faz com que os dados a ser movidos disponível numa tabela temporária com este tipo de tabela. Código do procedimento armazenado, em seguida, pode intercalar os dados que está a ser copiados com dados existentes. |Não |

> [!TIP]
> Quando copiar dados para a SQL Database do Azure, a atividade de cópia acrescenta dados para a tabela do sink por predefinição. Para executar um UPSERT ou lógica de negócio adicionais, utilize o procedimento armazenado no SqlSink. Saiba mais detalhes de [invocar o procedimento armazenado para Sink do SQL Server](#invoking-stored-procedure-for-sql-sink).

**Exemplo 1: acrescentar dados**

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

**Exemplo 2: a invocar um procedimento armazenado durante a cópia para upsert**

Saiba mais detalhes de [invocar o procedimento armazenado para Sink do SQL Server](#invoking-stored-procedure-for-sql-sink).

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

Esta secção fornece um exemplo para copiar dados a partir de uma tabela de origem sem uma coluna de identidade para uma tabela de destino com uma coluna de identidade.

**Tabela de origem:**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```

**Tabela de destino:**

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

Tenha em atenção que a tabela de destino tem uma coluna de identidade.

**Definição de JSON do conjunto de dados de origem**

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

**Definição de JSON do conjunto de dados de destino**

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

Tenha em atenção que, como a tabela de origem e destino têm esquemas diferentes (o destino tem uma coluna adicional com a identidade). Neste cenário, tem de especificar **estrutura** propriedade na definição de conjunto de dados de destino, que não inclui a coluna de identidade.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Invocar um procedimento armazenado do sink do SQL Server

Ao copiar dados para o SQL Database do Azure, um utilizador especificado armazenados os procedimento foi configurado e invocado com parâmetros adicionais.

Um procedimento armazenado pode ser utilizado quando mecanismos incorporados de cópia não servem o objetivo. É normalmente utilizado quando upsert (insert + update) ou processamento adicional (intercalação colunas, procurar valores adicionais, inserir na várias tabelas, etc.) tiver de ser efetuada antes da final inserção de dados de origem na tabela de destino.

O exemplo seguinte mostra como utilizar um procedimento armazenado para efetuar um upsert numa tabela numa SQL Database do Azure. Partindo do princípio de dados de entrada e a tabela do sink "Marketing" tem três colunas: ProfileID, estado e categoria. Efetuar upsert com base na coluna "ProfileID" e aplicam-se apenas para uma categoria específica.

**Conjunto de dados de saída**

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

Defina a secção "SqlSink" na atividade de cópia da seguinte forma.

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

A base de dados, defina o procedimento armazenado com o mesmo nome "SqlWriterStoredProcedureName". Processa a entrada de dados de origem especificada e intercalação na tabela de saída. Tenha em atenção que o nome do parâmetro do procedimento armazenado deve ser igual a "tableName" definida no conjunto de dados.

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

A base de dados, defina o tipo de tabela com o mesmo nome como sqlWriterTableType. Tenha em atenção que o esquema do tipo de tabela deve ser igual ao esquema devolvido pelos seus dados de entrada.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL，
)
```

A funcionalidade de procedimento armazenado tira partido da [Table-Valued parâmetros](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database"></a>Mapeamento de tipo de dados para a SQL Database do Azure

Quando copiar dados de/para SQL Database do Azure, os seguintes mapeamentos são utilizados de tipos de dados de SQL Database do Azure para tipos de dados intermédio do Azure Data Factory. Consulte [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md) para saber mais sobre como atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink.

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
| Número de vírgula flutuante |Duplo |
| Imagem |Byte[] |
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
| Texto |Cadeia, Char [] |
| tempo |TimeSpan |
| carimbo de data/hora |Byte[] |
| tinyint |Bytes |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Cadeia, Char [] |
| xml |Xml |

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia no Azure Data Factory, consulte [arquivos de dados suportados](copy-activity-overview.md##supported-data-stores-and-formats).