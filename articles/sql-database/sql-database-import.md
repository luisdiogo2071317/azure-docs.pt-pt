---
title: Importar um ficheiro BACPAC para criar uma base de dados SQL do Azure | Documentos da Microsoft
description: Crie uma base de dados do SQL newAzure ao importar um ficheiro BACPAC.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/11/2019
ms.openlocfilehash: 7ea1de4719d171605a49727d6924cb4b617b04bc
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56097868"
---
# <a name="quickstart-import-a-bacpac-file-to-a-new-azure-sql-database"></a>Início rápido: Importar um ficheiro BACPAC para uma nova base de dados do Azure SQL

Pode migrar uma base de dados do SQL Server para uma base de dados SQL do Azure com um [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) ficheiro (um arquivo zip com um `.bacpac` extensão tem metadados e dados de uma base de dados). Pode importar um ficheiro BACPAC do armazenamento de Blobs do Azure (apenas no armazenamento standard) ou do armazenamento local numa localização no local. Para maximizar velocidade de importação, pode especificar um escalão de serviço mais elevado e tamanho (como P6) de computação. Em seguida, pode reduzir verticalmente quando a importação for concluída com êxito. Nível de compatibilidade da base de dados importados baseia-se no nível de compatibilidade da base de dados origem.

> [!IMPORTANT]
> Depois de importar a base de dados, pode optar por operar a base de dados em seu nível de compatibilidade atual (nível 100 para a base de dados AdventureWorks2008R2) ou num nível superior. Para obter mais informações sobre as implicações e as opções para o funcionamento de uma base de dados a um nível de compatibilidade específico, veja [ALTERAR Nível de Compatibilidade de BASE DE DADOS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Veja também [ALTERAR CONFIGURAÇÃO DO ÂMBITO DA BASE DE DADOS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) para obter informações sobre as definições adicionais do nível da base de dados relacionadas com os níveis de compatibilidade.

## <a name="import-from-a-bacpac-file-in-the-azure-portal"></a>Importar a partir de um ficheiro BACPAC no portal do Azure

Esta secção mostra como, na [portal do Azure](https://portal.azure.com), para criar uma base de dados SQL do Azure a partir de um ficheiro BACPAC armazenado no armazenamento de Blobs do Azure. O portal *apenas* suporta a importação de um ficheiro BACPAC do armazenamento de Blobs do Azure.

> [!NOTE]
> [O Azure SQL Database Managed Instance](sql-database-managed-instance.md) suporta a importação a partir de um ficheiro BACPAC com os outros métodos neste artigo, mas não suporta atualmente a migrar no portal do Azure.

Para importar uma base de dados no portal do Azure, abra a página para o servidor de base de dados SQL que irá alojar a importação e, na barra de ferramentas, selecione **importar base de dados**.  

   ![importação de base de dados](./media/sql-database-import/import.png)

Selecione a conta de armazenamento, o contentor e o ficheiro BACPAC que pretende importar. Especifique o novo tamanho de base de dados (normalmente, a mesma como origem) e fornecer credenciais do SQL Server de destino. Para obter uma lista de valores possíveis para uma nova base de dados SQL do Azure, consulte [Create Database](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current).

### <a name="monitor-imports-progress"></a>Monitorizar o progresso da importação

Para monitorizar o progresso de uma importação, abra a página de servidor da base de dados importados e, em **configurações**, selecione **importar/exportar histórico**. Quando for bem-sucedido, a importação tem um **concluído** estado.

Para verificar a base de dados está em direto no servidor, selecione **bases de dados SQL** e certifique-se de que a nova base de dados é **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Importar a partir de um ficheiro BACPAC com SqlPackage

Para importar uma base de dados SQL com o [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) utilitário de linha de comandos, veja [importar parâmetros e as propriedades](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). SqlPackage tem a versão mais recente [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) e [SQL Server Data Tools para Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). Também pode transferir a versão mais recente [SqlPackage](https://www.microsoft.com/download/details.aspx?id=53876) do Centro de download da Microsoft.

Para dimensionamento e desempenho, recomendamos que utilize SqlPackage na maioria dos ambientes de produção. Para saber mais sobre a migração com ficheiros BACPAC num blogue da Equipa de Aconselhamento ao Cliente do SQL Server, consulte [Migrating from SQL Server to Azure SQL Database using BACPAC Files (Migrar a partir do SQL Server para a Base de Dados SQL do Azure com Ficheiros BACPAC)](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

O seguinte comando SqlPackage importa os **AdventureWorks2008R2** base de dados do armazenamento local para um servidor de base de dados do Azure SQL chamado **mynewserver20170403**. Ele cria uma nova base de dados chamado **myMigratedDatabase** com um **Premium** escalão de serviço e um **P6** objetivo de serviço. Altere estes valores conforme adequado para o seu ambiente.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=<your_server_admin_account_user_id>;Password=<your_server_admin_account_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Um servidor de base de dados SQL escuta na porta 1433. Para ligar a um servidor de base de dados SQL através de uma firewall empresarial, o firewall tem de ter esta porta abrir.
>

Este exemplo mostra como importar uma base de dados com SqlPackage com o Active Directory Universal Authentication.

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>Importar a partir de um ficheiro BACPAC, com o PowerShell

Utilize o [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet para submeter um pedido de base de dados de importação para o serviço de base de dados do Azure SQL. Dependendo do tamanho da base de dados, a importação pode demorar algum tempo a concluir.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport 
    -ResourceGroupName "<your_resource_group>" `
    -ServerName "<your_server>" `
    -DatabaseName "<your_database>" `
    -DatabaseMaxSizeBytes "<database_size_in_bytes>" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "<your_resource_group>" -StorageAccountName "<your_storage_account").Value[0] `
    -StorageUri "https://myStorageAccount.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "<your_server_admin_account_user_id>" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "<your_server_admin_account_password>" -AsPlainText -Force)

 ```

 Pode utilizar o [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet para verificar o progresso da importação. Executar o cmdlet imediatamente após a solicitação normalmente devolve **Estado: InProgress**. A importação estiver concluída, quando vir **Estado: Foi efetuada com êxito**.

```powershell
$importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
[Console]::Write("Importing")
while ($importStatus.Status -eq "InProgress")
{
    $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
    [Console]::Write(".")
    Start-Sleep -s 10
}
[Console]::WriteLine("")
$importStatus
```

> [!TIP]
Para obter outro exemplo de script, consulte [importar uma base de dados de um ficheiro BACPAC](scripts/sql-database-import-from-bacpac-powershell.md).

## <a name="limitations"></a>Limitações

Importar uma base de dados no conjunto elástico não é suportada. Pode importar dados para uma base de dados e, em seguida, mover a base de dados para um conjunto elástico.

## <a name="import-using-wizards"></a>Importar com assistentes

Também pode utilizar estes assistentes.

- [Importar Assistente de aplicação de camada de dados no SQL Server Management Studio](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [Importação do SQL Server e o Assistente de exportação](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Passos Seguintes

- Para saber como ligar e consultar uma base de dados importados do SQL, veja [início rápido: Base de Dados SQL do Azure: Utilizar o SQL Server Management Studio para ligar e consultar dados](sql-database-connect-query-ssms.md).
- Para saber mais sobre a migração com ficheiros BACPAC num blogue da Equipa de Aconselhamento ao Cliente do SQL Server, consulte [Migrating from SQL Server to Azure SQL Database using BACPAC Files (Migrar a partir do SQL Server para a Base de Dados SQL do Azure com Ficheiros BACPAC)](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
- Para uma discussão sobre o inteiro do SQL Server da base de dados do processo de migração, incluindo recomendações de desempenho, consulte [migração de base de dados do SQL Server para a base de dados do Azure SQL](sql-database-single-database-migrate.md).
- Para saber como gerir e partilhar as chaves de armazenamento e acesso partilhado assinaturas de forma segura, consulte [guia de segurança de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
