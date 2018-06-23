---
title: Copiar dados de e para o Azure SQL Data Warehouse, utilizando o Azure Data Factory | Microsoft Docs
description: Saiba como copiar dados de arquivos de origem suportada para o Azure SQL Data Warehouse ou a partir do armazém de dados do SQL Server para lojas de dependente suportados por utilizar o Data Factory.
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
ms.date: 05/28/2018
ms.author: jingwang
ms.openlocfilehash: 5a7ee7862e102093efa2c203eac2497b025af4e5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36337822"
---
#  <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Copiar os dados de ou para o Azure SQL Data Warehouse, utilizando o Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you're using:"]
> * [Versão 1: GA](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Versão 2: pré-visualização](connector-azure-sql-data-warehouse.md)

Este artigo explica como utilizar a atividade de cópia no Azure Data Factory para copiar dados de ou para o Azure SQL Data Warehouse. Baseia-se no [descrição geral da atividade de cópia](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se para a versão 2 do Data Factory, atualmente em pré-visualização. Se utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [conector do Azure SQL Data Warehouse no V1](v1/data-factory-azure-sql-data-warehouse-connector.md).

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados do Azure SQL Data Warehouse para qualquer arquivo de dados suportados sink. E pode copiar dados a partir de qualquer arquivo de dados de origem suportada para o Azure SQL Data Warehouse. Para obter uma lista dos arquivos de dados que são suportados como origens ou sinks pela atividade de cópia, consulte o [arquivos de dados e formatos suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector de armazém de dados SQL do Azure suporta estas funções:

- Copie os dados utilizando autenticação do SQL Server e a autenticação de token de aplicação do Azure Active Directory (Azure AD) com um principal de serviço ou uma identidade de serviço geridas (MSI).
- Obter os dados utilizando uma consulta de SQL Server ou o procedimento armazenado como uma origem.
- Como um sink, inserir dados de carga, utilizando o PolyBase ou um volume. Recomendamos o PolyBase para melhorar o desempenho de cópia.

> [!IMPORTANT]
> Tenha em atenção que o PolyBase suporta apenas a autenticação do SQL Server, mas a autenticação não do Azure AD.

> [!IMPORTANT]
> Se copiar dados utilizando o tempo de execução do Azure Data Factory integração, configure um [firewall do servidor SQL do Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) para que os serviços do Azure podem aceder ao servidor.
> Se copiar os dados através da utilização de um tempo de execução automática alojada integração, configure a firewall do servidor SQL do Azure para permitir que o intervalo IP adequado. Este intervalo inclui IP do computador que é utilizada para ligar à SQL Database do Azure.

## <a name="get-started"></a>Introdução

> [!TIP]
> Para alcançar o melhor desempenho, utilize o PolyBase para carregar dados para o Azure SQL Data Warehouse. O [PolyBase de utilização para carregar dados para o Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) secção tenha detalhes. Para obter instruções com um caso de utilização, consulte [1 TB no Azure SQL Data Warehouse em 15 minutos de carga com o Azure Data Factory](load-azure-sql-data-warehouse.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que definem as entidades do Data Factory específicas para um conector do Azure SQL Data Warehouse.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para um serviço ligado do Azure SQL Data Warehouse:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida **AzureSqlDW**. | Sim |
| connectionString | Especifique as informações necessárias para ligar à instância do armazém de dados SQL do Azure para o **connectionString** propriedade. Marcar este campo como um **SecureString** armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Sim |
| servicePrincipalId | Especifique o ID de cliente. da aplicação | Sim, quando utiliza a autenticação do Azure AD com um principal de serviço. |
| servicePrincipalKey | Especifique a chave da aplicação. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Sim, quando utiliza a autenticação do Azure AD com um principal de serviço. |
| inquilino | Especifique as informações de inquilino (nome ou o inquilino ID de domínio) em que reside a aplicação. Pode obtê-lo por posicionado o rato no canto superior direito do portal do Azure. | Sim, quando utiliza a autenticação do Azure AD com um principal de serviço. |
| connectVia | O [integração runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração do Azure ou de um tempo de execução automática alojada integração (se o arquivo de dados está localizado numa rede privada). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. | Não |

Para tipos de autenticação diferentes, consulte as secções seguintes na pré-requisitos e exemplos JSON, respetivamente:

- [Autenticação do SQL Server](#sql-authentication)
- Autenticação de token do Azure AD aplicação: [principal de serviço](#service-principal-authentication)
- Autenticação de token do Azure AD aplicação: [identidade de serviço geridas](#managed-service-identity-authentication)

### <a name="sql-authentication"></a>Autenticação do SQL

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Exemplo de serviço ligado que utiliza a autenticação do SQL Server

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

Para utilizar a autenticação de token de aplicação de serviço com base em principal do Azure AD, siga estes passos:

1. **[Criar uma aplicação do Azure Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application)**  do portal do Azure. Anote o nome da aplicação e os valores seguintes que definem o serviço ligado:

    - ID da aplicação
    - Chave da aplicação
    - ID do inquilino

2. **[Aprovisionar um administrador do Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  para o servidor de SQL do Azure no portal do Azure se ainda não o tiver feito. O administrador do Azure AD pode ser um utilizador do Azure AD ou o grupo do Azure AD. Se conceder o grupo com MSI uma função de administrador, ignore os passos 3 e 4. O administrador terá acesso total à base de dados.

3. **[Criar utilizadores de base de dados contida](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  para o principal de serviço. Ligar ao armazém de dados de ou para o qual pretende copiar os dados utilizando ferramentas como o SSMS, com uma identidade do Azure AD que tenha, pelo menos, permissões ALTER qualquer utilizador. Execute o seguinte T-SQL:
    
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Conceder as permissões necessárias do principal de serviço** como faz normalmente para os utilizadores do SQL Server ou outras pessoas. Execute o seguinte código:

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. **Configurar um serviço ligado do Azure SQL Data Warehouse** no Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Exemplo de serviço ligado que utiliza a autenticação principal de serviço

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

Uma fábrica de dados pode ser associada um [identidade de serviço geridas](data-factory-service-identity.md) que representa a fábrica específica. Pode utilizar esta identidade de serviço para a autenticação do Azure SQL Data Warehouse. A fábrica designada pode aceder e do armazém de dados de cópia de ou para os dados ao utilizar esta identidade.

> [!IMPORTANT]
> Tenha em atenção que o PolyBase atualmente não é suportado para a autenticação de MSI.

Para utilizar a autenticação de token de aplicação baseada em MSI do Azure AD, siga estes passos:

1. **Crie um grupo no Azure AD.** Torne a fábrica do MSI um membro do grupo.

    a. Determinar a identidade de serviço da fábrica de dados do portal do Azure. Vá para a fábrica de dados **propriedades**. Copie o ID de identidade de serviço.

    b. Instalar o [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) módulo. Inicie sessão utilizando o `Connect-AzureAD` comando. Execute os seguintes comandos para criar um grupo e adicionar a fábrica de dados MSI como membro.
    ```powershell
    $Group = New-AzureADGroup -DisplayName "<your group name>" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId "<your data factory service identity ID>"
    ```

2. **[Aprovisionar um administrador do Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  para o servidor de SQL do Azure no portal do Azure se ainda não o tiver feito.

3. **[Criar utilizadores de base de dados contida](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  para o grupo do Azure AD. Ligar ao armazém de dados de ou para o qual pretende copiar os dados utilizando ferramentas como o SSMS, com uma identidade do Azure AD que tenha, pelo menos, permissões ALTER qualquer utilizador. Execute o seguinte T-SQL. 
    
    ```sql
    CREATE USER [your Azure AD group name] FROM EXTERNAL PROVIDER;
    ```

4. **Conceder as permissões necessárias do grupo do Azure AD** como faz normalmente para os utilizadores do SQL e outros. Por exemplo, execute o seguinte código.

    ```sql
    EXEC sp_addrolemember [role name], [your Azure AD group name];
    ```

5. **Configurar um serviço ligado do Azure SQL Data Warehouse** no Azure Data Factory.

#### <a name="linked-service-example-that-uses-msi-authentication"></a>Exemplo de serviço ligado que utiliza a autenticação de MSI

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](https://docs.microsoft.com/en-us/azure/data-factory/concepts-datasets-linked-services) artigo. Esta secção fornece uma lista de propriedades suportado pelo conjunto de dados Azure SQL Data Warehouse.

Para copiar dados de ou para o Azure SQL Data Warehouse, defina o **tipo** propriedade do conjunto de dados para **AzureSqlDWTable**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | O **tipo** propriedade do conjunto de dados tem de ser definida como **AzureSqlDWTable**. | Sim |
| tableName | O nome da tabela ou vista na instância do Azure SQL Data Warehouse que referencia o serviço ligado. | Sim |

#### <a name="dataset-properties-example"></a>Exemplo de propriedades do conjunto de dados

```json
{
    "name": "AzureSQLDWDataset",
    "properties":
    {
        "type": "AzureSqlDWTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Data Warehouse linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportado pelo Azure SQL Data Warehouse origem e o sink.

### <a name="azure-sql-data-warehouse-as-the-source"></a>O Azure SQL Data Warehouse como origem

Para copiar dados do Azure SQL Data Warehouse, defina o **tipo** propriedade da origem de atividade de cópia para **SqlDWSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | O **tipo** propriedade da origem de atividade de cópia tem de ser definida como **SqlDWSource**. | Sim |
| sqlReaderQuery | Utilize a consulta SQL personalizada para ler os dados. Exemplo: `select * from MyTable`. | Não |
| sqlReaderStoredProcedureName | O nome do procedimento armazenado que lê dados a partir da tabela de origem. A última instrução de SQL tem de ser uma instrução SELECT no procedimento armazenado. | Não |
| storedProcedureParameters | Parâmetros para o procedimento armazenado.<br/>Valores permitidos são pares de nome ou valor. Nomes e maiúsculas e minúsculas de parâmetros têm de corresponder os nomes e a maiúsculas e minúsculas dos parâmetros de procedimento armazenado. | Não |

### <a name="points-to-note"></a>Pontos a ter em atenção

- Se o **sqlReaderQuery** especificado para o **SqlSource**, a atividade de cópia executa esta consulta contra a origem do armazém de dados SQL do Azure para obter os dados. Em alternativa, pode especificar um procedimento armazenado. Especifique o **sqlReaderStoredProcedureName** e **storedProcedureParameters** se o procedimento armazenado aceita parâmetros.
- Se não especificar um **sqlReaderQuery** ou **sqlReaderStoredProcedureName**, as colunas definidas no **estrutura** secção do conjunto de dados JSON são utilizados para construir uma consulta. `select column1, column2 from mytable` é executada relativamente a Azure SQL Data Warehouse. Se a definição do conjunto de dados não tem o **estrutura**, todas as colunas são selecionadas da tabela.
- Quando utiliza **sqlReaderStoredProcedureName**, terá de especificar um fictício **tableName** propriedade no conjunto de dados JSON.

#### <a name="sql-query-example"></a>Exemplo de consulta SQL

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
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
                "type": "SqlDWSource",
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
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
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
                "type": "SqlDWSource",
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

### <a name="azure-sql-data-warehouse-as-sink"></a> O Azure SQL Data Warehouse como sink

Para copiar dados para o Azure SQL Data Warehouse, defina o tipo de sink na atividade de cópia para **SqlDWSink**. As seguintes propriedades são suportadas na atividade de cópia **sink** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | O **tipo** propriedade o sink de atividade de cópia tem de ser definida **SqlDWSink**. | Sim |
| allowPolyBase | Indica se deve utilizar o PolyBase, quando aplicável, em vez do mecanismo BULKINSERT. <br/><br/> Recomendamos que carregar dados para o SQL Data Warehouse, utilizando o PolyBase. Consulte o [PolyBase de utilização para carregar dados para o Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) restrições e os detalhes na secção.<br/><br/>Valores permitidos são **verdadeiro** e **falso** (predefinição).  | Não |
| polyBaseSettings | Um grupo de propriedades que podem ser especificados quando o **allowPolybase** propriedade está definida como **verdadeiro**. | Não |
| rejectValue | Especifica o número ou a percentagem de linhas que pode ser rejeitada antes da consulta falha.<br/><br/>Saiba mais sobre as opções de rejeitar do PolyBase na secção argumentos [criar tabela externa (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx). <br/><br/>Valores permitidos são 0 (predefinição), 1, 2, etc. |Não |
| rejectType | Especifica se o **rejectValue** opção é um valor literal ou uma percentagem.<br/><br/>Valores permitidos são **valor** (predefinição) e **percentagem**. | Não |
| rejectSampleValue | Determina o número de linhas a obter antes do PolyBase recalcula a percentagem de linhas rejeitadas.<br/><br/>Valores permitidos são 1, 2, etc. | Sim, se o **rejectType** é **percentagem**. |
| useTypeDefault | Especifica como processar os valores em falta nos ficheiros de texto delimitado quando PolyBase obtém dados a partir do ficheiro de texto.<br/><br/>Saiba mais sobre esta propriedade da secção de argumentos no [criar formato de ficheiro externo (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).<br/><br/>Valores permitidos são **verdadeiro** e **falso** (predefinição). | Não |
| writeBatchSize | Insere dados na tabela SQL quando atinge o tamanho da memória intermédia **writeBatchSize**. Aplica-se apenas quando não é utilizado o PolyBase.<br/><br/>O valor permitido é **número inteiro** (número de linhas). | Não. A predefinição é 10000. |
| writeBatchTimeout | De tempo de espera para a operação de inserção de lote concluir antes de atingir o tempo limite. Aplica-se apenas quando não é utilizado o PolyBase.<br/><br/>O valor permitido é **timespan**. Exemplo: "00: 30:00" (30 minutos). | Não |
| preCopyScript | Especifique uma consulta de SQL Server executar antes de escrever dados no Azure SQL Data Warehouse em cada execução de atividade de cópia. Utilize esta propriedade para limpar os dados pré-carregada. | Não | (#repeatability durante-cópia). | Uma instrução de consulta. | Não |

#### <a name="sql-data-warehouse-sink-example"></a>Exemplo de sink do SQL Data Warehouse

```json
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

Saiba mais sobre como utilizar o PolyBase para carregar eficiente do armazém de dados do SQL Server na secção seguinte.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Utilize o PolyBase para carregar dados para o Azure SQL Data Warehouse

Utilizar [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) é uma forma eficiente para carregar uma grande quantidade de dados para o Azure SQL Data Warehouse com débito elevado. Verá um grandes ganhos de débito da utilizando o PolyBase em vez do mecanismo BULKINSERT predefinido. Consulte [referência de desempenho](copy-activity-performance.md#performance-reference) para uma comparação detalhada. Para obter instruções com um caso de utilização, consulte [carregar 1 TB para o Azure SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-load-sql-data-warehouse).

* Se a origem de dados no Blob storage do Azure ou do Azure Data Lake Store, e o formato é compatível com o PolyBase, cópia direta ao Azure SQL Data Warehouse, utilizando o PolyBase. Para obter mais informações, consulte  **[direcionar cópia utilizando o PolyBase](#direct-copy-by-using-polybase)**.
* Se o arquivo de dados de origem e o formato originalmente não é suportada pelo PolyBase, utilize o **[testado cópia utilizando o PolyBase](#staged-copy-by-using-polybase)** em vez disso, a funcionalidade. A funcionalidade de cópia pré-configurada também fornece-lhe melhorar o débito. Converte automaticamente os dados num ficheiro em formato compatível com o PolyBase. E armazena os dados no Blob storage do Azure. Em seguida, carrega os dados para o SQL Data Warehouse.

> [!IMPORTANT]
> Tenha em atenção que o PolyBase atualmente não é suportado para a autenticação de token de aplicação baseada em MSI Azure AD.

### <a name="direct-copy-by-using-polybase"></a>Cópia direta, utilizando o PolyBase

O PolyBase do armazém de dados SQL suporta diretamente Blob do Azure e o Azure Data Lake Store. Utiliza o principal de serviço como uma origem e tem os requisitos de formato de ficheiro específico. Se os dados de origem satisfaz os critérios descritos nesta secção, utilize o PolyBase para copiar direta do arquivo de dados de origem para o Azure SQL Data Warehouse. Caso contrário, utilize [testado cópia utilizando o PolyBase](#staged-copy-by-using-polybase).

> [!TIP]
> Para copiar dados eficiente de Data Lake Store ao SQL Data Warehouse, saiba mais de [do Azure Data Factory torna mais fácil e conveniente desvendar informações a partir dos dados ao utilizar o Data Lake Store com o SQL Data Warehouse mesmo](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).

Se não são cumpridos os requisitos, o Azure Data Factory verifica as definições e automaticamente retrocede para o mecanismo BULKINSERT para o movimento de dados.

1. O **serviço ligado de origem** é do tipo **AzureStorage** ou **AzureDataLakeStore** com a autenticação principal de serviço.
2. O **conjunto de dados de entrada** é do tipo **AzureBlob** ou **AzureDataLakeStoreFile**. O tipo de formato em `type` propriedades **OrcFormat**, **ParquetFormat**, ou **TextFormat**, com as seguintes configurações:

   1. `rowDelimiter` tem de ser **\n**.
   2. `nullValue` está definido como **uma cadeia vazia** (""), ou `treatEmptyAsNull` está definido como **verdadeiro**.
   3. `encodingName` está definido como **utf-8**, que é o valor predefinido.
   4. `escapeChar`, `quoteChar`, `firstRowAsHeader`, e `skipLineCount` não estão especificados.
   5. `compression` pode ser **sem compressão**, **GZip**, ou **Deflate**.

    ```json
    "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",
           "nullValue": "",
           "encodingName": "utf-8"
       },
       "compression": {
           "type": "GZip",
           "level": "Optimal"
       }
    },
    ```

3. Não existe nenhum `skipHeaderLineCount` em **BlobSource** ou **AzureDataLakeStore** para a atividade de cópia no pipeline.
4. Não existe nenhum `sliceIdentifierColumnName` em **SqlDWSink** para a atividade de cópia no pipeline. O PolyBase garante que todos os dados são atualizados ou nada é atualizado numa execução única. Para alcançar **repetibilidade**, utilize `sqlWriterCleanupScript`.

```json
"activities":[
    {
        "name": "CopyFromAzureBlobToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "BlobDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            }
        }
    }
]
```

### <a name="staged-copy-by-using-polybase"></a>Cópia testada utilizando o PolyBase

Quando os dados de origem não cumprem os critérios na secção anterior, ative dados copiar através de uma instância de armazenamento de Blobs do Azure transição intermédio. Não pode ser Premium Storage do Azure. Neste caso, o Azure Data Factory executa automaticamente a transformações nos dados para cumprir os requisitos de formato de dados do PolyBase. Em seguida, utiliza o PolyBase para carregar dados para o SQL Data Warehouse. Por fim,-limpa os seus dados temporários o armazenamento de Blobs. Consulte [testado cópia](copy-activity-performance.md#staged-copy) para obter detalhes sobre a cópia dos dados através de uma instância de armazenamento de Blobs do Azure transição.

Para utilizar esta funcionalidade, crie um [serviço ligado do Storage do Azure](connector-azure-blob-storage.md#linked-service-properties) que refere-se à conta do storage do Azure com o armazenamento de BLOBs intermédio. Em seguida, especifique o `enableStaging` e `stagingSettings` propriedades para a atividade de cópia, conforme mostrado no seguinte código:

```json
"activities":[
    {
        "name": "CopyFromSQLServerToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "SQLServerDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
]
```

## <a name="best-practices-for-using-polybase"></a>Melhores práticas para utilizar o PolyBase

As secções seguintes fornecem procedimentos recomendados para além dos mencionados [melhores práticas para o Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).

### <a name="required-database-permission"></a>Permissão de base de dados necessária

Para utilizar o PolyBase, o utilizador que carrega dados para o SQL Data Warehouse tem de ter [permissão "Controlo"](https://msdn.microsoft.com/library/ms191291.aspx) na base de dados de destino. Uma forma de alcançar que está a adicionar o utilizador como um membro do **db_owner** função. Saber como fazê-lo no [descrição geral do SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

### <a name="row-size-and-data-type-limits"></a>Os limites de tipo de dados e de tamanho de linha

O PolyBase cargas estão limitadas a linhas mais pequenas do que 1 MB. Estes não é possível carregar a VARCHR(MAX), nvarchar (Max) ou varbinary (Max). Para obter mais informações, consulte [limites de capacidade de serviço do SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Quando os dados de origem tem linhas superiores a 1 MB, pode querer verticalmente dividir as tabelas de origem em várias relações de pequenas. Certifique-se de que o tamanho maior de cada linha não excede o limite. As tabelas de menores, em seguida, podem ser carregadas utilizando o PolyBase e intercaladas em conjunto no Azure SQL Data Warehouse.

### <a name="sql-data-warehouse-resource-class"></a>Classe de recursos do SQL Data Warehouse

Para alcançar o débito de possíveis melhor, atribua uma classe de recurso superior ao utilizador que carrega dados para o SQL Data Warehouse através do PolyBase.

### <a name="tablename-in-azure-sql-data-warehouse"></a>**tableName** no Azure SQL Data Warehouse

A tabela seguinte fornece exemplos de como especificar o **tableName** propriedade no conjunto de dados JSON. Mostra várias combinações de esquema e a tabela de nomes.

| Esquema da BD | Nome da tabela | **tableName** propriedade JSON |
| --- | --- | --- |
| dbo | MyTable | MyTable ou dbo. MyTable ou [dbo]. [MyTable] |
| dbo1 | MyTable | dbo1. MyTable ou [dbo1]. [MyTable] |
| dbo | My.Table | [My.Table] ou [dbo]. [My.Table] |
| dbo1 | My.Table | [dbo1].[My.Table] |

Se vir o seguinte erro, o problema poderá ser o valor especificado para o **tableName** propriedade. Consulte a tabela anterior para a forma correta especificar valores para o **tableName** propriedade JSON.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a>Colunas com valores predefinidos

Atualmente, a funcionalidade PolyBase na fábrica de dados aceita apenas o mesmo número de colunas como tabela de destino. Um exemplo é uma tabela com quatro colunas onde uma delas está definida com um valor predefinido. Os dados de entrada ainda tem de ter quatro colunas. Um conjunto de dados de entrada de coluna de três gera um erro semelhante a mensagem seguinte:

```
All columns of the table must be specified in the INSERT BULK statement.
```

O valor NULL é um formulário especial do valor predefinido. Se a coluna pode ser nula, os dados de entrada no blob para essa coluna podem estar vazios. Mas não pode estar em falta no conjunto de dados de entrada. O PolyBase insere um valor nulo para os valores em falta no Azure SQL Data Warehouse.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>Mapeamento de tipo de dados para o Azure SQL Data Warehouse

Quando copiar dados de ou para o Azure SQL Data Warehouse, os seguintes mapeamentos são utilizados Azure SQL Data Warehouse tipos de dados para tipos de dados intermédio do Azure Data Factory. Consulte [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md) para saber como atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink.

| Tipo de dados do armazém de dados SQL do Azure | Tipo de dados intermédio de fábrica de dados |
|:--- |:--- |
| bigint | Int64 |
| Binário | Byte[] |
| bits | Booleano |
| char | Cadeia, Char [] |
| data | DateTime |
| Datetime | DateTime |
| datetime2 | DateTime |
| Datetimeoffset | DateTimeOffset |
| Decimal | Decimal |
| Atributo FILESTREAM (varbinary(max)) | Byte[] |
| Flutuante | Valor de duplo |
| image | Byte[] |
| Int | Int32 |
| dinheiro | Decimal |
| nchar | Cadeia, Char [] |
| ntext | Cadeia, Char [] |
| um valor numérico | Decimal |
| nvarchar | Cadeia, Char [] |
| real | Único |
| ROWVERSION | Byte[] |
| smalldatetime | DateTime |
| smallint | Int16 |
| em smallmoney | Decimal |
| sql_variant | Objeto * |
| texto | Cadeia, Char [] |
| hora | TimeSpan |
| carimbo de data/hora | Byte[] |
| tinyint | Byte |
| uniqueidentifier | GUID |
| varbinary | Byte[] |
| varchar | Cadeia, Char [] |
| xml | Xml |

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia no Azure Data Factory, consulte [arquivos de dados e formatos suportados](copy-activity-overview.md##supported-data-stores-and-formats).
