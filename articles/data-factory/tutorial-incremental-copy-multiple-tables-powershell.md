---
title: "Copiar várias tabelas de forma incremental com o Azure Data Factory | Microsoft Docs"
description: "Neste tutorial, vai criar um pipeline do Azure Data Factory, que copia dados delta de forma incremental de várias tabelas numa base de dados do SQL Server local para uma base de dados SQL do Azure. "
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/01/2017
ms.author: jingwang
ms.openlocfilehash: 5472c41af9b0c524b931ed3e6b149270e17a633a
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2017
---
# <a name="incrementally-load-data-from-multiple-tables-in-sql-server-to-azure-sql-database"></a>Carregar dados de forma incremental a partir de várias tabelas no SQL Server para a Base de Dados SQL do Azure
Neste tutorial, vai criar um pipeline do Azure data factory que carrega dados delta a partir de várias tabelas no SQL server local para uma base de dados SQL do Azure.    

Vai executar os seguintes passos neste tutorial:

> [!div class="checklist"]
> * Prepare os arquivos de dados de origem e de destino.
> * Criar uma fábrica de dados.
> * Crie um integration runtime autoalojado (IR)
> * Instalar o runtime de integração 
> * Criar serviços ligados. 
> * Criar conjuntos de dados de origem, de sink e com limite de tamanho.
> * Criar, executar e monitorizar um pipeline.
> * Resultados da revisão
> * Adicionar ou atualizar os dados nas tabelas de origem
> * Voltar a executar e monitorizar o pipeline
> * Rever os resultados finais 

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço Data Factory, que está disponível em geral (GA), veja a [documentação da versão 1 do Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="overview"></a>Descrição geral
Eis os passos importantes na criação desta solução: 

1. **Selecionar a coluna de limite de tamanho**.
    Selecione uma coluna por cada tabela no arquivo de dados de origem, que pode ser utilizada para identificar os registos novos ou atualizados para cada execução. Normalmente, os dados nesta coluna selecionada (por exemplo, last_modify_time ou ID) continuam a aumentar quando as linhas são criadas ou atualizadas. O valor máximo nesta coluna é utilizado como limite de tamanho.
2. **Preparar um arquivo de dados para armazenar o valor de limite de tamanho**.   
    Neste tutorial, vai armazenar o valor de limite de tamanho numa base de dados SQL do Azure.
3. **Criar um pipeline com as seguintes atividades:** 
    
    1. Criar uma atividade **ForEach** que itera através de uma lista de nomes de tabela de origem que é transmitida como um parâmetro para o pipeline. Para cada tabela de origem, este invoca as seguintes atividades para efetuar o carregamento de diferenças para essa tabela. 
    2. Criar duas atividades de **pesquisa**. Utilize a primeira atividade de pesquisa para obter o último valor de limite de tamanho. Utilize a segunda atividade de pesquisa para obter o novo valor de limite de tamanho. Estes valores de limite de tamanho são transmitidos para a atividade de cópia. 
    3. Criar uma **atividade de cópia** que copia linhas do arquivo de dados de origem com o valor da coluna de limite de tamanho superior ao valor de limite de tamanho antigo e inferior ao novo valor de limite de tamanho. Em seguida, copia os dados delta do arquivo de dados de origem para um armazenamento de blobs como um ficheiro novo. 
    4. Criar uma **atividade de procedimento armazenado** que atualiza o valor de limite de tamanho para o pipeline que vai ser executado da próxima vez. 

        Eis o diagrama de solução de alto nível da solução: 

        ![Carregar dados de forma incremental](media\tutorial-incremental-copy-multiple-tables-powershell\high-level-solution-diagram.png)


Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
* **SQL Server**. Neste tutorial, vai utilizar uma base de dados do SQL Server no local como arquivo de dados de **origem**. 
* **Base de Dados SQL do Azure**. Utilize uma base de dados SQL do Azure como o arquivo de dados de **sink**. Se não tiver uma Base de Dados SQL do Azure, veja o artigo [Criar uma base de dados SQL do Azure](../sql-database/sql-database-get-started-portal.md) para obter os passos para criar uma. 

### <a name="create-source-tables-in-your-sql-server-database"></a>Criar tabelas de origem na base de dados do SQL Server

1. Iniciar **SQL Server Management Studio** e ligar ao SQL Server local. 
2. No **Explorador de Servidores**, clique com botão direito do rato na base de dados e escolha **Nova Consulta**.
3. Execute o seguinte comando SQL na base de dados para criar tabelas com o nome `customer_table` e `project_table`.

    ```sql
    create table customer_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );
    
    create table project_table
    (
        Project varchar(255),
        Creationtime datetime
    );
        
    INSERT INTO customer_table
    (PersonID, Name, LastModifytime)
    VALUES
    (1, 'John','9/1/2017 12:56:00 AM'),
    (2, 'Mike','9/2/2017 5:23:00 AM'),
    (3, 'Alice','9/3/2017 2:36:00 AM'),
    (4, 'Andy','9/4/2017 3:21:00 AM'),
    (5, 'Anny','9/5/2017 8:06:00 AM');
    
    INSERT INTO project_table
    (Project, Creationtime)
    VALUES
    ('project1','1/1/2015 0:00:00 AM'),
    ('project2','2/2/2016 1:23:00 AM'),
    ('project3','3/4/2017 5:16:00 AM');
    
    ```

### <a name="create-destination-tables-in-your-azure-sql--database"></a>Criar tabelas de destino na base de dados SQL do Azure
1. Iniciar **SQL Server Management Studio** e ligar ao seu SQL Server do Azure. 
2. No **Explorador de Servidores**, clique com botão direito do rato na **base de dados** e escolha **Nova Consulta**.
3. Execute o seguinte comando SQL na base de dados SQL do Azure para criar tabelas com o nome `customer_table` e `project_table`.  
    
    ```sql
    create table customer_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );
    
    create table project_table
    (
        Project varchar(255),
        Creationtime datetime
    );

    ```

### <a name="create-another-table-in-azure-sql-database-to-store-the-high-watermark-value"></a>Criar outra tabela na base de dados SQL do Azure para armazenar o valor de limite superior de tamanho
1. Execute o seguinte comando SQL na base de dados SQL do Azure para criar uma tabela com o nome `watermarktable` para armazenar o valor de limite de tamanho.  
    
    ```sql
    create table watermarktable
    (
    
        TableName varchar(255),
        WatermarkValue datetime,
    );
    ```
3. Inserir valores de marca d'água iniciais para ambas as tabelas de origem na tabela de marca d'água.

    ```sql

    INSERT INTO watermarktable
    VALUES 
    ('customer_table','1/1/2010 12:00:00 AM'),
    ('project_table','1/1/2010 12:00:00 AM');
    
    ```

### <a name="create-a-stored-procedure-in-azure-sql-database"></a>Criar um procedimento armazenado na base de dados SQL do Azure 

Execute o seguinte comando para criar um procedimento armazenado na base de dados SQL do Azure. Este procedimento armazenado atualiza o valor da marca d'água após cada execução de pipeline. 

```sql
CREATE PROCEDURE sp_write_watermark @LastModifiedtime datetime, @TableName varchar(50)
AS

BEGIN

    UPDATE watermarktable
    SET [WatermarkValue] = @LastModifiedtime 
WHERE [TableName] = @TableName

END

```

### <a name="create-data-types-and-additional-stored-procedures"></a>Criar tipos de dados e procedimentos armazenados adicionais
Crie dois procedimentos armazenados e dois tipos de dados na base de dados SQL do Azure executando a seguinte consulta: estes são utilizados para unir dados de tabelas de origem em tabelas de destino.

```sql
CREATE TYPE DataTypeforCustomerTable AS TABLE(
    PersonID int,
    Name varchar(255),
    LastModifytime datetime
);

GO

CREATE PROCEDURE sp_upsert_customer_table @customer_table DataTypeforCustomerTable READONLY
AS

BEGIN
  MERGE customer_table AS target
  USING @customer_table AS source
  ON (target.PersonID = source.PersonID)
  WHEN MATCHED THEN
      UPDATE SET Name = source.Name,LastModifytime = source.LastModifytime
  WHEN NOT MATCHED THEN
      INSERT (PersonID, Name, LastModifytime)
      VALUES (source.PersonID, source.Name, source.LastModifytime);
END

GO

CREATE TYPE DataTypeforProjectTable AS TABLE(
    Project varchar(255),
    Creationtime datetime
);

GO

CREATE PROCEDURE sp_upsert_project_table @project_table DataTypeforProjectTable READONLY
AS

BEGIN
  MERGE project_table AS target
  USING @project_table AS source
  ON (target.Project = source.Project)
  WHEN MATCHED THEN
      UPDATE SET Creationtime = source.Creationtime
  WHEN NOT MATCHED THEN
      INSERT (Project, Creationtime)
      VALUES (source.Project, source.Creationtime);
END

```

[!INCLUDE [data-factory-quickstart-prerequisites-2](../../includes/data-factory-quickstart-prerequisites-2.md)]

## <a name="create-a-data-factory"></a>Criar uma fábrica de dados
1. Defina uma variável para o nome do grupo de recursos que vai utilizar nos comandos do PowerShell mais tarde. Copie o texto do comando seguinte para o PowerShell, especifique um nome para o [Grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) com aspas duplas e execute o comando. Por exemplo: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Se o grupo de recursos já existir, pode não substituí-lo. Atribua outro valor à variável `$resourceGroupName` e execute novamente o comando
2. Defina uma variável para a localização da fábrica de dados: 

    ```powershell
    $location = "East US"
    ```
3. Para criar o grupo de recursos do Azure, execute o comando abaixo: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 
    Se o grupo de recursos já existir, pode não substituí-lo. Atribua outro valor à variável `$resourceGroupName` e execute novamente o comando. 
3. Defina uma variável para o nome da fábrica de dados. 

    > [!IMPORTANT]
    >  Atualize o nome da fábrica de dados para que seja globalmente exclusivo. Por exemplo, ADFIncMultiCopyTutorialFactorySP1127. 

    ```powershell
    $dataFactoryName = "ADFIncMultiCopyTutorialFactory";
    ```
5. Para criar a fábrica de dados, execute o cmdlet **Set-AzureRmDataFactoryV2**: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location $location -Name $dataFactoryName 
    ```

Tenha em atenção os seguintes pontos:

* O nome do Azure Data Factory deve ser globalmente exclusivo. Se receber o erro seguinte, altere o nome e tente novamente.

    ```
    The specified Data Factory name 'ADFIncMultiCopyTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Para criar instâncias do Data Factory, a conta de utilizador que utiliza para iniciar sessão no Azure tem de ser um membro das funções **contribuidor** ou **proprietário**, ou um **administrador** da subscrição do Azure.
* Atualmente, a versão 2 do Data Factory só permite criar fábricas de dados nas regiões E.U.A. Leste, E.U.A. Leste 2 e Europa Ocidental. Os arquivos de dados (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) e as computações (HDInsight, etc.) utilizados pela fábrica de dados podem estar noutras regiões.

[!INCLUDE [data-factory-create-install-integration-runtime](../../includes/data-factory-create-install-integration-runtime.md)]



## <a name="create-linked-services"></a>Criar serviços ligados
Os serviços ligados são criados numa fábrica de dados para ligar os seus arquivos de dados e serviços de computação a essa fábrica de dados. Nesta secção, vai criar serviços ligados à base de dados SQL do Azure local e à base de dados SQL do Azure. 

### <a name="create-sql-server-linked-service"></a>Crie o serviço ligado do SQL Server.
Neste passo, vai ligar o seu SQL Server no local à fábrica de dados.

1. Crie um ficheiro JSON com o nome **SqlServerLinkedService.json** na pasta **C:\ADFTutorials\IncCopyMultiTableTutorial** com o conteúdo seguinte: selecione a secção certa com base na **autenticação** que utiliza para se ligar ao SQL Server. Crie as pastas locais, caso ainda não existam. 

    > [!IMPORTANT]
    > Selecione a secção certa com base na **autenticação** que utiliza para se ligar ao SQL Server.

    **Se estiver a utilizar a autenticação do SQL (sa), copie a seguinte definição JSON:**

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```    
    **Se estiver a utilizar a autenticação do Windows, copie a seguinte definição JSON:**

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<server>;Database=<database>;Integrated Security=True"
                },
                "userName": "<user> or <domain>\\<user>",
                "password": {
                    "type": "SecureString",
                    "value": "<password>"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }    
    ```
    > [!IMPORTANT]
    > - Selecione a secção certa com base na **autenticação** que utiliza para se ligar ao SQL Server.
    > - Substitua **&lt;integration** **runtime** **name>** pelo nome do seu integration runtime.
    > - Substitua **&lt;servername>**, **&lt;databasename>**, **&lt;username>** e **&lt;password>** pelos valores do seu SQL Server antes de guardar o ficheiro.
    > - Se precisar de utilizar um caráter de barra invertida (`\`) no nome da sua conta de utilizador ou no nome do seu servidor, utilize o caráter de escape (`\`). Por exemplo, `mydomain\\myuser`.

2. No **Azure PowerShell**, mude para a pasta **C:\ADFTutorials\IncCopyMultiTableTutorial**.
3. Execute o cmdlet **Set-AzureRmDataFactoryV2LinkedService** para criar o serviço ligado: **AzureStorageLinkedService**. No exemplo seguinte, vai transmitir os valores para os parâmetros **ResourceGroupName** e **DataFactoryName**. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerLinkedService" -File ".\SqlServerLinkedService.json"
    ```

    Segue-se o resultado do exemplo:

    ```json
    LinkedServiceName : SqlServerLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFIncMultiCopyTutorialFactory1201
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerLinkedService
    ```

### <a name="create-azure-sql-database-linked-service"></a>Criar o serviço ligado da Base de Dados SQL do Azure.
1. Crie um ficheiro JSON com o nome **AzureSQLDatabaseLinkedService.json** na pasta **C:\ADFTutorials\IncCopyMultiTableTutorial** com o seguinte conteúdo: (crie a pasta ADF, caso ainda não exista.). Substitua **&lt;server&gt;, &lt;database name&gt;, &lt;user id&gt; e &lt;password&gt;** pelo nome do seu Azure SQL server, base de dados, ID de utilizador e palavra-passe antes de guardar o ficheiro. 

    ```json
    {
        "name": "AzureSQLDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "value": "Server = tcp:<server>.database.windows.net,1433;Initial Catalog=<database name>; Persist Security Info=False; User ID=<user name>; Password=<password>; MultipleActiveResultSets = False; Encrypt = True; TrustServerCertificate = False; Connection Timeout = 30;",
                    "type": "SecureString"
                }
            }
        }
    }
    ```
2. No **Azure PowerShell**, execute o cmdlet **Set-AzureRmDataFactoryV2LinkedService** para criar o serviço ligado: **AzureSQLDatabaseLinkedService**. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureSQLDatabaseLinkedService" -File ".\AzureSQLDatabaseLinkedService.json"
    ```

    Segue-se o resultado do exemplo:

    ```json
    LinkedServiceName : AzureSQLDatabaseLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFIncMultiCopyTutorialFactory1201
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlDatabaseLinkedService
    ```

## <a name="create-datasets"></a>Criar conjuntos de dados
Neste passo, vai criar conjuntos de dados para representar a origem de dados e o destino dos dados. e o local para armazenar a marca d'água.

### <a name="create-a-source-dataset"></a>Criar um conjunto de dados de origem

1. Crie um ficheiro JSON com o nome **SourceDataset.json** na mesma pasta com o seguinte conteúdo: 

    ```json
    {
        "name": "SourceDataset",
        "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dummyName"
            },
            "linkedServiceName": {
                "referenceName": "SqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }
   
    ```

    o nome da tabela é um nome fictício. A atividade de cópia no pipeline utiliza uma consulta SQL para carregar os dados em vez de carregar a tabela inteira. 
1.  Execute o cmdlet Set-AzureRmDataFactoryV2Dataset para criar o conjunto de dados: SourceDataset
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SourceDataset" -File ".\SourceDataset.json"
    ```

    Eis a saída de exemplo do cmdlet:
    
    ```json
    DatasetName       : SourceDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFIncMultiCopyTutorialFactory1201
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-sink-dataset"></a>Criar um conjunto de dados de sink

1. Crie um ficheiro JSON com o nome **SinkDataset.json** na mesma pasta com o seguinte conteúdo: o tableName é definido pelo pipeline dinamicamente durante a execução. A atividade ForEach no pipeline itera através de uma lista de nomes de tabela e transmite o nome da tabela para este conjunto de dados em cada iteração. 

    ```json
    {
        "name": "SinkDataset",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": {
                    "value": "@{dataset().SinkTableName}",
                    "type": "Expression"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            },
            "parameters": {
                "SinkTableName": {
                    "type": "String"
                }
            }
        }
    }
    ```

2.  Execute o cmdlet Set-AzureRmDataFactoryV2Dataset para criar o conjunto de dados: SinkDataset
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SinkDataset" -File ".\SinkDataset.json"
    ```

    Eis a saída de exemplo do cmdlet:
    
    ```json
    DatasetName       : SinkDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFIncMultiCopyTutorialFactory1201
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset
    ```

### <a name="create-a-dataset-for-watermark"></a>Criar um conjunto de dados para o limite de tamanho
Neste passo, vai criar um conjunto de dados para armazenar um valor de limite superior de tamanho. 

1. Crie um ficheiro JSON com o nome WatermarkDataset.json na mesma pasta com o seguinte conteúdo: 

    ```json
    {
        "name": " WatermarkDataset ",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": "watermarktable"
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }    
    ```
2.  Execute o cmdlet Set-AzureRmDataFactoryV2Dataset para criar o conjunto de dados: WatermarkDataset
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "WatermarkDataset" -File ".\WatermarkDataset.json"
    ```

    Eis a saída de exemplo do cmdlet:
    
    ```json
    DatasetName       : WatermarkDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : <data factory name>
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset    
    ```

## <a name="create-a-pipeline"></a>Criar um pipeline
O pipeline aceita uma lista de nomes de tabela como parâmetro. A **atividade ForEach** itera através da lista de nomes de tabela e efetua as seguintes operações: 

1. utiliza a **atividade de pesquisa** para obter o valor de marca d'água antigo (valor inicial ou que foi utilizado a última iteração).
2. utiliza a **atividade de pesquisa** para obter o novo valor de marca d'água (valor máximo da coluna de marca d'água na tabela de origem).
3. utiliza a **atividade de cópia** para copiar dados entre estes dois valores de marca d'água a partir da base de dados de origem para a base de dados de destino. 
4. utiliza a **atividade de procedimentos armazenados** para atualizar o valor de marca d'água antigo a ser utilizado no primeiro passo da iteração seguinte. 

### <a name="create-the-pipeline"></a>Criar o pipeline
1. Crie um ficheiro JSON: IncrementalCopyPipeline.json na mesma pasta com o seguinte conteúdo: 

    ```json
    {
        "name": "IncrementalCopyPipeline",
        "properties": {
            "activities": [{
    
                "name": "IterateSQLTables",
                "type": "ForEach",
                "typeProperties": {
                    "isSequential": "false",
                    "items": {
                        "value": "@pipeline().parameters.tableList",
                        "type": "Expression"
                    },
    
                    "activities": [
                        {
                            "name": "LookupOldWaterMarkActivity",
                            "type": "Lookup",
                            "typeProperties": {
                                "source": {
                                    "type": "SqlSource",
                                    "sqlReaderQuery": "select * from watermarktable where TableName  =  '@{item().TABLE_NAME}'"
                                },
    
                                "dataset": {
                                    "referenceName": "WatermarkDataset",
                                    "type": "DatasetReference"
                                }
                            }
                        },
                        {
                            "name": "LookupNewWaterMarkActivity",
                            "type": "Lookup",
                            "typeProperties": {
                                "source": {
                                    "type": "SqlSource",
                                    "sqlReaderQuery": "select MAX(@{item().WaterMark_Column}) as NewWatermarkvalue from @{item().TABLE_NAME}"
                                },
    
                                "dataset": {
                                    "referenceName": "SourceDataset",
                                    "type": "DatasetReference"
                                }
                            }
                        },
    
                        {
                            "name": "IncrementalCopyActivity",
                            "type": "Copy",
                            "typeProperties": {
                                "source": {
                                    "type": "SqlSource",
                                    "sqlReaderQuery": "select * from @{item().TABLE_NAME} where @{item().WaterMark_Column} > '@{activity('LookupOldWaterMarkActivity').output.firstRow.WatermarkValue}' and @{item().WaterMark_Column} <= '@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}'"
                                },
                                "sink": {
                                    "type": "SqlSink",
                                    "SqlWriterTableType": "@{item().TableType}",
                                    "SqlWriterStoredProcedureName": "@{item().StoredProcedureNameForMergeOperation}"
                                }
                            },
                            "dependsOn": [{
                                    "activity": "LookupNewWaterMarkActivity",
                                    "dependencyConditions": [
                                        "Succeeded"
                                    ]
                                },
                                {
                                    "activity": "LookupOldWaterMarkActivity",
                                    "dependencyConditions": [
                                        "Succeeded"
                                    ]
                                }
                            ],
    
                            "inputs": [{
                                "referenceName": "SourceDataset",
                                "type": "DatasetReference"
                            }],
                            "outputs": [{
                                "referenceName": "SinkDataset",
                                "type": "DatasetReference",
                                "parameters": {
                                    "SinkTableName": "@{item().TABLE_NAME}"
                                }
                            }]
                        },
    
                        {
                            "name": "StoredProceduretoWriteWatermarkActivity",
                            "type": "SqlServerStoredProcedure",
                            "typeProperties": {
    
                                "storedProcedureName": "sp_write_watermark",
                                "storedProcedureParameters": {
                                    "LastModifiedtime": {
                                        "value": "@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}",
                                        "type": "datetime"
                                    },
                                    "TableName": {
                                        "value": "@{activity('LookupOldWaterMarkActivity').output.firstRow.TableName}",
                                        "type": "String"
                                    }
                                }
                            },
    
                            "linkedServiceName": {
                                "referenceName": "AzureSQLDatabaseLinkedService",
                                "type": "LinkedServiceReference"
                            },
    
                            "dependsOn": [{
                                "activity": "IncrementalCopyActivity",
                                "dependencyConditions": [
                                    "Succeeded"
                                ]
                            }]
                        }
    
                    ]
    
                }
            }],
    
            "parameters": {
                "tableList": {
                    "type": "Object"
                }
            }
        }
    }
    ```
2. Execute o cmdlet Set-AzureRmDataFactoryV2Pipeline para criar o pipeline: IncrementalCopyPipeline.
    
   ```powershell
   Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "IncrementalCopyPipeline" -File ".\IncrementalCopyPipeline.json"
   ``` 

   Segue-se o resultado do exemplo: 

   ```json
    PipelineName      : IncrementalCopyPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : ADFIncMultiCopyTutorialFactory1201
    Activities        : {IterateSQLTables}
    Parameters        : {[tableList, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
   ```
 
## <a name="run-the-pipeline"></a>Executar o pipeline

1. Crie um ficheiro de parâmetro: **Parameters.json** na mesma pasta com o seguinte conteúdo:

    ```json
    {
        "tableList": 
        [
            {
                "TABLE_NAME": "customer_table",
                "WaterMark_Column": "LastModifytime",
                "TableType": "DataTypeforCustomerTable",
                "StoredProcedureNameForMergeOperation": "sp_upsert_customer_table"
            },
            {
                "TABLE_NAME": "project_table",
                "WaterMark_Column": "Creationtime",
                "TableType": "DataTypeforProjectTable",
                "StoredProcedureNameForMergeOperation": "sp_upsert_project_table"
            }
        ]
    }
    ```
2. Execute o pipeline: **IncrementalCopyPipeline** através do cmdlet **Invoke-AzureRmDataFactoryV2Pipeline**. Substitua os marcadores de posição pelos seus nomes do grupo de recursos e da fábrica de dados.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup $resourceGroupName -dataFactoryName $dataFactoryName -ParameterFile ".\Parameters.json"        
    ``` 

## <a name="monitor-the-pipeline"></a>Monitorizar o pipeline

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Clique em **Mais serviços**, pesquise com a palavra-chave `data factories` e selecione **Fábricas de dados**. 

    ![Menu de fábricas de dados](media\tutorial-incremental-copy-multiple-tables-powershell\monitor-data-factories-menu-1.png)
3. Pesquise pela **sua fábrica de dados** na lista de fábricas de dados e selecione-a para iniciar a página da Fábrica de dados. 

    ![Pesquisar pela sua fábrica de dados](media\tutorial-incremental-copy-multiple-tables-powershell\monitor-search-data-factory-2.png)
4. Na página Fábrica de dados, clique no mosaico **Monitorizar e Gerir**. 

    ![Mosaico Monitorizar e Gerir](media\tutorial-incremental-copy-multiple-tables-powershell\monitor-monitor-manage-tile-3.png)    
5. A **Aplicação de Integração de Dados**  abre-se numa aba separada. Pode ver todas as **execuções de pipelines** e os respetivos estados. Note que no seguinte exemplo, o estado da execução do pipeline é **Com Êxito**. Pode verificar os parâmetros transmitidos para o pipeline ao clicar na ligação na coluna **Parâmetros**. Se tiver ocorrido um erro, pode ver uma ligação na coluna **Erro**. Clique na ligação na coluna **Ações**. 

    ![Execuções de pipeline](media\tutorial-incremental-copy-multiple-tables-powershell\monitor-pipeline-runs-4.png)    
6. Ao clicar na ligação na coluna **Ações**, verá a seguinte página que mostra todas as **execuções de atividade** no pipeline. 

    ![Execuções de atividade](media\tutorial-incremental-copy-multiple-tables-powershell\monitor-activity-runs-5.png)
7. Para mudar de volta para a visualização **Execuções do pipeline**, clique em **Pipelines** conforme mostrado na imagem. 

## <a name="review-the-results"></a>Rever os resultados
No SQL Server Management Studio, execute as seguintes consultas na base de dados SQL do Azure de destino para verificar que os dados foram copiados a partir de tabelas de origem para tabelas de destino. 

**Consulta** 
```sql
select * from customer_table
```

**Saída**
```
===========================================
PersonID    Name    LastModifytime
===========================================
1           John    2017-09-01 00:56:00.000
2           Mike    2017-09-02 05:23:00.000
3           Alice   2017-09-03 02:36:00.000
4           Andy    2017-09-04 03:21:00.000
5           Anny    2017-09-05 08:06:00.000
```

**Consulta:**

```sql
select * from project_table
```

**Saída**

```
===================================
Project     Creationtime
===================================
project1    2015-01-01 00:00:00.000
project2    2016-02-02 01:23:00.000
project3    2017-03-04 05:16:00.000
```

**Consulta**

```sql
select * from watermarktable
```

**Saída**

```
======================================
TableName       WatermarkValue
======================================
customer_table  2017-09-05 08:06:00.000
project_table   2017-03-04 05:16:00.000
```

Tenha em atenção que os valores da marca d'água para ambas as tabelas foram atualizados. 

## <a name="add-more-data-to-the-source-tables"></a>Adicione mais dados às tabelas de origem

Execute a seguinte consulta na base de dados do SQL Server de origem para atualizar uma linha existente na customer_table e insera uma nova linha na project_table. 

```sql
UPDATE customer_table
SET [LastModifytime] = '2017-09-08T00:00:00Z', [name]='NewName' where [PersonID] = 3

INSERT INTO project_table
(Project, Creationtime)
VALUES
('NewProject','10/1/2017 0:00:00 AM');
``` 

## <a name="rerun-the-pipeline"></a>Volte a executar o pipeline

1. Agora, execute novamente o pipeline executando o seguinte comando do PowerShell:

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup $resourceGroupname -dataFactoryName $dataFactoryName -ParameterFile ".\Parameters.json"
    ```
2. monitorizar as execuções do pipeline seguindo as instruções na secção [Monitorizar o pipeline](#monitor-the-pipeline). Dado que o estado do pipeline se encontra **em curso**, verá outra ligação de ação em **Ações** para cancelar a execução do pipeline. 

    ![Execuções de pipeline](media\tutorial-incremental-copy-multiple-tables-powershell\monitor-pipeline-runs-6.png)    
3. Clique em **Atualizar** para atualizar a lista até a execução do pipeline ter êxito. 

    ![Execuções de pipeline](media\tutorial-incremental-copy-multiple-tables-powershell\monitor-pipeline-runs-succeded-7.png)
4. (opcional) clique na ligação (ícone) **Ver Execuções de Atividade** em Ações para ver todas a execuções de atividade associadas a esta execução de pipeline. 

## <a name="review-final-results"></a>Rever os resultados finais
No SQL Server Management Studio, execute as seguintes consultas na base de dados de destino para verificar que os dados atualizados/novos foram copiados a partir de tabelas de origem para tabelas de destino. 

**Consulta** 
```sql
select * from customer_table
```

**Saída**
```
===========================================
PersonID    Name    LastModifytime
===========================================
1           John    2017-09-01 00:56:00.000
2           Mike    2017-09-02 05:23:00.000
3           NewName 2017-09-08 00:00:00.000
4           Andy    2017-09-04 03:21:00.000
5           Anny    2017-09-05 08:06:00.000
```

Tenha em atenção aos novos valores de Nome e LastModifytime para o PersonID: 3. 

**Consulta:**

```sql
select * from project_table
```

**Saída**

```
===================================
Project     Creationtime
===================================
project1    2015-01-01 00:00:00.000
project2    2016-02-02 01:23:00.000
project3    2017-03-04 05:16:00.000
NewProject  2017-10-01 00:00:00.000
```

Tenha em atenção que a entrada NewProject foi adicionada à project_table. 

**Consulta**

```sql
select * from watermarktable
```

**Saída**

```
======================================
TableName       WatermarkValue
======================================
customer_table  2017-09-08 00:00:00.000
project_table   2017-10-01 00:00:00.000
```

Tenha em atenção que os valores da marca d'água para ambas as tabelas foram atualizados.
     
## <a name="next-steps"></a>Passos seguintes
Neste tutorial, executou os passos seguintes: 

> [!div class="checklist"]
> * Prepare os arquivos de dados de origem e de destino.
> * Criar uma fábrica de dados.
> * Crie um integration runtime autoalojado (IR)
> * Instalar o runtime de integração 
> * Criar serviços ligados. 
> * Criar conjuntos de dados de origem, de sink e com limite de tamanho.
> * Criar, executar e monitorizar um pipeline.
> * Resultados da revisão
> * Adicionar ou atualizar os dados nas tabelas de origem
> * Voltar a executar e monitorizar o pipeline
> * Rever os resultados finais 

Avance para o tutorial seguinte para saber como transformar dados através de um cluster do Spark no Azure:

> [!div class="nextstepaction"]
>[Carregar dados de forma incremental da Base de Dados SQL do Azure para o Armazenamento de Blobs do Azure com tecnologia de Controlo de Alterações](tutorial-incremental-copy-multiple-tables-powershell.md)


