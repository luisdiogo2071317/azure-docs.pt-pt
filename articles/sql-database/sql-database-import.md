---
title: Importar um ficheiro BACPAC para criar uma base de dados SQL do Azure | Documentos da Microsoft
description: Crie uma base de dados do SQL newAzure ao importar um ficheiro BACPAC.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: 913e374e51b968650b14d3f6563f64c217b76ad0
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913816"
---
# <a name="quickstart-import-a-bacpac-file-to-a-new-azure-sql-database"></a>Início rápido: Importar um ficheiro BACPAC para uma nova base de dados do Azure SQL

Quando precisa importar uma base de dados de um arquivo ou ao migrar de outra plataforma, pode importar o esquema de base de dados e os dados a partir de um [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) ficheiro. Um ficheiro BACPAC é um arquivo ZIP com uma extensão de BACPAC que contém os metadados e dados a partir de uma base de dados do SQL Server. Um ficheiro BACPAC pode ser importado a partir do armazenamento de Blobs do Azure (apenas no armazenamento standard) ou do armazenamento local numa localização no local. Para maximizar a velocidade de importação, é recomendável especificar um escalão de serviço mais elevado e tamanho, por exemplo, um P6, computação e, em seguida, dimensionar para diminuir conforme apropriado quando a importação for concluída com êxito. Além disso, o nível de compatibilidade de base de dados após a importação baseia-se no nível de compatibilidade da base de dados de origem.

> [!IMPORTANT]
> Depois de migrar a base de dados para a base de dados do Azure SQL, pode optar por operar a base de dados em seu nível de compatibilidade atual (nível 100 para a base de dados AdventureWorks2008R2) ou num nível superior. Para obter mais informações sobre as implicações e as opções para o funcionamento de uma base de dados a um nível de compatibilidade específico, veja [ALTERAR Nível de Compatibilidade de BASE DE DADOS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Veja também [ALTERAR CONFIGURAÇÃO DO ÂMBITO DA BASE DE DADOS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) para obter informações sobre as definições adicionais do nível da base de dados relacionadas com os níveis de compatibilidade.

## <a name="import-from-a-bacpac-file-using-azure-portal"></a>Importar a partir de um ficheiro BACPAC com o portal do Azure

Este artigo fornece orientações para criar uma base de dados SQL do Azure a partir de um ficheiro BACPAC armazenado no armazenamento de Blobs do Azure com o [portal do Azure](https://portal.azure.com). Importar com o portal do Azure só suporta a importação de um ficheiro BACPAC do armazenamento de Blobs do Azure.

Para importar uma base de dados com o portal do Azure, abra a página do servidor (não a página da base de dados) para associar a base de dados e, em seguida, clique em **importar** na barra de ferramentas. Especifique a conta de armazenamento e o contentor e selecione o ficheiro BACPAC que pretende importar. Selecione o tamanho da nova base de dados (normalmente, a mesma como origem) e fornecer credenciais do SQL Server de destino.  

   ![importação de base de dados](./media/sql-database-import/import.png)

Para monitorizar o progresso da operação de importação, abra a página para o servidor lógico que contém a base de dados a ser importado. Desloque para baixo até **Operations** e, em seguida, clique em **importar/exportar** histórico.

> [!NOTE]
> [O Azure SQL Database Managed Instance](sql-database-managed-instance.md) suportado importar a partir de um BACPAC ficheiro com os outros métodos neste artigo, mas faz atualmente não suporta migração usando o portal do Azure.

### <a name="monitor-the-progress-of-an-import-operation"></a>Monitorizar o progresso de uma operação de importação

Para monitorizar o progresso da operação de importação, abra a página para o servidor lógico no qual a base de dados está a ser importado. Desloque para baixo até **Operations** e, em seguida, clique em **importar/exportar** histórico.

   ![importar](./media/sql-database-import/import-history.png) ![Importar estado](./media/sql-database-import/import-status.png)

Para verificar a base de dados está em direto no servidor, clique em **bases de dados SQL** e certifique-se de que a nova base de dados é **Online**.

## <a name="import-from-a-bacpac-file-using-sqlpackage"></a>Importar a partir de um ficheiro BACPAC com SQLPackage

Para importar uma base de dados SQL com o [SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) utilitário de linha de comandos, veja [importar parâmetros e as propriedades](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties). O utilitário SQLPackage exe é fornecido com as versões mais recentes [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) e [SQL Server Data Tools para Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), ou pode baixar a versão mais recente do [SqlPackage ](https://www.microsoft.com/download/details.aspx?id=53876) diretamente a partir do Centro de transferências da Microsoft.

Recomendamos a utilização do utilitário SQLPackage exe para dimensionamento e desempenho na maioria dos ambientes de produção. Para saber mais sobre a migração com ficheiros BACPAC num blogue da Equipa de Aconselhamento ao Cliente do SQL Server, consulte [Migrating from SQL Server to Azure SQL Database using BACPAC Files (Migrar a partir do SQL Server para a Base de Dados SQL do Azure com Ficheiros BACPAC)](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Consulte o seguinte comando SQLPackage para obter um exemplo de script para saber como importar os **AdventureWorks2008R2** base de dados do armazenamento local para um servidor lógico de base de dados do Azure SQL, chamado **mynewserver20170403** Neste exemplo. Este script mostra a criação de uma nova base de dados chamado **myMigratedDatabase**, com uma camada de serviços de **Premium**e um objetivo de serviço de **P6**. Altere estes valores conforme adequado ao seu ambiente.

```cmd
SqlPackage.exe /a:import /tcs:"Data Source=mynewserver20170403.database.windows.net;Initial Catalog=myMigratedDatabase;User Id=ServerAdmin;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Um servidor lógico da Base de Dados SQL do Azure ouve na porta 1433. Se está a tentar ligar a um servidor lógico da Base de Dados SQL do Azure a partir de uma firewall empresarial, esta porta tem de estar aberta na firewall empresarial para se ligar com êxito.
>

Este exemplo mostra como importar uma base de dados com o Active Directory Universal Authentication SqlPackage.exe:

```cmd
SqlPackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="import-from-a-bacpac-file-using-powershell"></a>Importar a partir de um ficheiro BACPAC, com o PowerShell

Utilize o [New-AzureRmSqlDatabaseImport](/powershell/module/azurerm.sql/new-azurermsqldatabaseimport) cmdlet para submeter um pedido de base de dados de importação para o serviço de base de dados do Azure SQL. Dependendo do tamanho da base de dados, a operação de importação pode demorar algum tempo a concluir.

 ```powershell
 $importRequest = New-AzureRmSqlDatabaseImport -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "MyImportSample" `
    -DatabaseMaxSizeBytes "262144000" `
    -StorageKeyType "StorageAccessKey" `
    -StorageKey $(Get-AzureRmStorageAccountKey -ResourceGroupName "myResourceGroup" -StorageAccountName $storageaccountname).Value[0] `
    -StorageUri "http://$storageaccountname.blob.core.windows.net/importsample/sample.bacpac" `
    -Edition "Standard" `
    -ServiceObjectiveName "P6" `
    -AdministratorLogin "ServerAdmin" `
    -AdministratorLoginPassword $(ConvertTo-SecureString -String "ASecureP@assw0rd" -AsPlainText -Force)

 ```

Para verificar o estado do pedido de importação, utilize o [Get-AzureRmSqlDatabaseImportExportStatus](/powershell/module/azurerm.sql/get-azurermsqldatabaseimportexportstatus) cmdlet. Executar isso imediatamente após a solicitação normalmente devolve **Estado: em curso**. Quando vir **Estado: concluída com êxito** a importação estiver concluída.

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

- Importar uma base de dados no conjunto elástico não é suportada. Pode importar dados para uma base de dados e, em seguida, mover a base de dados para um conjunto.

## <a name="import-using-other-methods"></a>Importar com outros métodos

Também pode utilizar estes assistentes:

- [Importar Assistente de aplicação de camada de dados no SQL Server Management Studio](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database#using-the-import-data-tier-application-wizard).
- [Importação do SQL Server e o Assistente de exportação](https://docs.microsoft.com/sql/integration-services/import-export-data/start-the-sql-server-import-and-export-wizard).

## <a name="next-steps"></a>Passos Seguintes

- Para saber como ligar e consultar uma base de dados importados do SQL, veja [ligar à base de dados SQL com o SQL Server Management Studio e executar uma consulta de T-SQL de exemplo](sql-database-connect-query-ssms.md).
- Para saber mais sobre a migração com ficheiros BACPAC num blogue da Equipa de Aconselhamento ao Cliente do SQL Server, consulte [Migrating from SQL Server to Azure SQL Database using BACPAC Files (Migrar a partir do SQL Server para a Base de Dados SQL do Azure com Ficheiros BACPAC)](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
- Para uma discussão sobre o inteiro do SQL Server da base de dados do processo de migração, incluindo recomendações de desempenho, consulte [migrar uma base de dados do SQL Server para a base de dados do Azure SQL](sql-database-cloud-migrate.md).
- Para saber como gerir e partilhar as chaves de armazenamento e acesso partilhado assinaturas de forma segura, consulte [guia de segurança de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-security-guide).
