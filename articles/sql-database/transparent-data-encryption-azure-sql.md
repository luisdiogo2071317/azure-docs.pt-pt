---
title: Encriptação de dados transparente para o Azure SQL Database e o armazém de dados | Documentos da Microsoft
description: Uma descrição geral da criptografia de dados transparente para a base de dados SQL e o armazém de dados. O documento abrange suas vantagens e as opções de configuração, que inclui encriptação de dados transparente gerida pelo serviço e traga a sua própria chave.
services: sql-database
author: becczhang
manager: craigg
ms.prod: ''
ms.reviewer: vanto
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: aliceku
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 8c1db822c1c0e7b91843687fdc1e6f6808cf60c1
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45544167"
---
# <a name="transparent-data-encryption-for-sql-database-and-data-warehouse"></a>Encriptação de dados transparente para a base de dados SQL e o armazém de dados

Encriptação de dados transparente (TDE) ajuda a proteger a base de dados do Azure SQL e o armazém de dados do Azure contra ameaças de atividades maliciosas. Ele executa criptografia em tempo real e a descriptografia da base de dados, cópias de segurança associadas e ficheiros de registo de transação em repouso sem a necessidade de alterações à aplicação. Por predefinição, a TDE está ativada para todos os recentemente implementados bases de dados do SQL do Azure. TDE não pode ser utilizado para encriptar lógico **mestre** base de dados na base de dados SQL.  O **mestre** base de dados contém objetos que são necessárias para executar as operações de TDE nas bases de dados do utilizador.

TDE tem de ser habilitadas manualmente para o Azure SQL Data Warehouse ou bases de dados mais antigos.  

Encriptação de dados transparente criptografa o armazenamento de uma base de dados completa com uma chave simétrica denominada a chave de encriptação da base de dados. Esta chave de encriptação da base de dados está protegido pelo protetor de encriptação de dados transparente. O componente de proteção é a um serviço gerido certificado (encriptação de dados transparente gerida pelo serviço) ou uma chave assimétrica armazenados no Azure Key Vault (traga a sua própria chave). Definir o protetor de encriptação de dados transparente ao nível do servidor. 

Na inicialização do banco de dados, a chave de encriptação da base de dados encriptado é descriptografada e, em seguida, utilizada para desencriptação e nova criptografia de arquivos de banco de dados no processo de motor de base de dados do SQL Server. Encriptação de dados transparente executa em tempo real e/s criptografia e descriptografia de dados no nível da página. Cada página é descriptografada quando é lido na memória e, em seguida, são encriptados antes de serem escritos no disco. Para obter uma descrição geral da encriptação transparente de dados, consulte [encriptação de dados transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).

SQL Server em execução numa máquina virtual do Azure também pode utilizar uma chave assimétrica do Key Vault. Os passos de configuração são diferentes de utilizar uma chave assimétrica na base de dados SQL. Para obter mais informações, consulte [gestão de chaves extensível com o Azure Key Vault (SQL Server)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).

## <a name="service-managed-transparent-data-encryption"></a>Encriptação de dados transparente gerida pelo serviço

No Azure, a definição de encriptação de dados transparente predefinida é que a chave de encriptação da base de dados está protegida por um certificado de servidor interno. O certificado de servidor interno é exclusivo para cada servidor. Se uma base de dados está numa relação de replicação geográfica, a base de dados primária e geo-secundária estão protegidas por chave de servidor da base de dados primário principal. Se duas bases de dados estiverem ligados ao mesmo servidor, eles compartilham o mesmo certificado incorporado. Microsoft gira automaticamente estes certificados, pelo menos a cada 90 dias.

A Microsoft diretamente move e gerencia as chaves conforme necessário para georreplicação e restaura. 

> [!IMPORTANT]
> Todas as recém-criadas bases de dados do SQL são criptografadas por padrão, usando a criptografia de dados transparente gerida pelo serviço. Bases de dados antes de Maio de 2017 e bases de dados criadas através de restauro, georreplicação e cópia de base de dados existentes não são criptografadas por padrão.
>

## <a name="bring-your-own-key"></a>Traga a sua própria chave

Com o suporte de traga a sua própria chave, pode tirar o controle sobre as chaves de encriptação de dados transparente e controlar quem pode aceder aos mesmos e quando. Cofre de chaves, que é o sistema de gestão do Azure com base na cloud externo principais, é o primeiro serviço de gestão de chaves integrou-se a encriptação de dados transparente com suporte de traga a sua própria chave. Com o suporte de traga a sua própria chave, a chave de encriptação da base de dados está protegida por uma chave assimétrica armazenada no Key Vault. A chave assimétrica nunca deixa o Key Vault. Depois do servidor tem permissões para um cofre de chaves, o servidor envia pedidos de operação básica de chave ao mesmo através do Key Vault. Definir a chave assimétrica ao nível do servidor e todas as bases de dados nesse servidor herdá-la.

Com o suporte de traga a sua própria chave, agora pode controlar tarefas de gestão de chaves, como rotações de chave e permissões do Cofre de chaves. Também pode eliminar chaves e ativar a auditoria/relatórios sobre todas as chaves de encriptação. Key Vault fornece gestão de chaves central e utiliza os módulos de segurança de hardware rigidamente monitorizados. Key Vault promove a separação da gestão de chaves e os dados para ajudar a cumprir a conformidade a normas. Para saber mais sobre o Key Vault, consulte a [página de documentação do Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

Para saber mais sobre a encriptação de dados transparente com suporte de traga a sua própria chave para a base de dados SQL e o armazém de dados, veja [encriptação de dados transparente com suporte de traga a sua própria chave](transparent-data-encryption-byok-azure-sql.md).

Para começar a utilizar a encriptação de dados transparente com suporte de traga a sua própria chave, consulte o guia de procedimentos [ativar a criptografia transparente de dados utilizando a sua própria chave do Key Vault com o PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Mover uma base de protegido por encriptação de dados transparente

Não precisa de desencriptar as bases de dados para operações no Azure. As definições de encriptação de dados transparente na base de dados de origem ou base de dados primária transparente são herdadas no destino. Envolvem a operações que estão incluídas:
- Restauro geográfico.
- Restauro de ponto no tempo de self-service.
- Restauro de bases de dados eliminadas.
- Georreplicação ativa.
- Criação de uma cópia da base de dados.

Ao exportar uma base de protegido por encriptação de dados transparente, o conteúdo exportado da base de dados não está encriptado. Este conteúdo exportado é armazenado nos arquivos BACPAC não encriptados. Certifique-se de que proteger os ficheiros BACPAC adequadamente e ativar a encriptação de dados transparente, depois de concluída a importação da nova base de dados.

Por exemplo, se o ficheiro BACPAC é exportado a partir de uma instância do SQL Server no local, o conteúdo importado da nova base de dados não encriptado automaticamente. Da mesma forma, se o ficheiro BACPAC é exportado para uma instância do SQL Server no local, a nova base de dados também não é automaticamente encriptado.

A única exceção é quando exportar de e para uma base de dados SQL. Encriptação de dados transparente está ativada na base de dados nova, mas o próprio ficheiro BACPAC ainda não está encriptado.

## <a name="manage-transparent-data-encryption-in-the-azure-portal"></a>Gerir a encriptação transparente de dados no portal do Azure

Para configurar a encriptação de dados transparente através do portal do Azure, tem de estar ligado como o Azure proprietário, Contribuidor ou Gestor de segurança SQL. 

Encriptação de dados transparente é definir o nível de base de dados. Para ativar a encriptação de dados transparente numa base de dados, vá para o [portal do Azure](https://portal.azure.com) e inicie sessão com a sua conta de administrador do Azure ou de contribuinte. Localize as definições de encriptação de dados transparente em sua base de dados do utilizador. Por predefinição, é utilizada a encriptação de dados transparente gerida pelo serviço. Um certificado de encriptação de dados transparente é gerado automaticamente para o servidor que contém a base de dados. 

![Encriptação de dados transparente gerida pelo serviço](./media/transparent-data-encryption-azure-sql/service-managed-tde.png)  

Defina a chave mestra de encriptação transparente de dados, também conhecido como o protetor de encriptação transparente de dados no nível do servidor. Para utilizar a encriptação de dados transparente com suporte de traga a sua própria chave e proteger seus bancos de dados com uma chave do Key Vault, consulte as definições de encriptação de dados transparente no seu servidor. 

![Encriptação de dados transparente com suporte de traga a sua própria chave](./media/transparent-data-encryption-azure-sql/tde-byok-support.png) 

## <a name="manage-transparent-data-encryption-by-using-powershell"></a>Gerir a encriptação de dados transparente com o PowerShell

Para configurar a encriptação de dados transparente através do PowerShell, tem de estar ligado como o Azure proprietário, Contribuidor ou Gestor de segurança SQL. 

| Cmdlet | Descrição |
| --- | --- |
| [Conjunto AzureRmSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) |Ativa ou desativa a encriptação de dados transparente para uma base de dados|
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) |Obtém o estado de encriptação de dados transparente para uma base de dados |
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption-Activity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) |Verifica o progresso de criptografia para uma base de dados |
| [AzureRmSqlServerKeyVaultKey adicionar](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) |Adiciona uma chave de Cofre de chaves para uma instância do SQL Server |
| [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) |Obtém as chaves do Cofre de chaves para uma instância do SQL server  |
| [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) |Define o protetor de encriptação de dados transparente para uma instância do SQL Server |
| [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) |Obtém o protetor de encriptação de dados transparente |
| [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) |Remove uma chave de Cofre de chaves a partir de uma instância do SQL Server |
|  | |

## <a name="manage-transparent-data-encryption-by-using-transact-sql"></a>Gerir a encriptação de dados transparente com o Transact-SQL

Ligar à base de dados com um início de sessão que seja administrador ou membro de uma a **dbmanager** função na base de dados mestra.

| Comando | Descrição |
| --- | --- |
| [Alterar base de dados (banco de dados SQL do Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) | Definir ativa/inativa de encriptação encripta ou desencripta uma base de dados |
| [dm database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Devolve informações sobre o estado de encriptação de uma base de dados e a respetiva base de dados associado chaves de encriptação |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Devolve informações sobre o estado de encriptação de cada dados do armazém de nó e as chaves de encriptação de base de dados associada | 
|  | |

Não é possível mudar o protetor de encriptação de dados transparente para uma chave do Key Vault com o Transact-SQL. Utilize o PowerShell ou o portal do Azure.

## <a name="manage-transparent-data-encryption-by-using-the-rest-api"></a>Gerir a encriptação de dados transparente com a API de REST
 
Para configurar a encriptação de dados transparente através da API REST, tem de estar ligado como o Azure proprietário, Contribuidor ou Gestor de segurança SQL. 

| Comando | Descrição |
| --- | --- |
|[Criar ou atualizar o servidor](/rest/api/sql/servers/createorupdate)|Adiciona uma identidade do Azure Active Directory para uma instância do SQL Server (utilizada para conceder acesso ao Cofre de chaves)|
|[Criar ou atualizar a chave do servidor](/rest/api/sql/serverkeys/createorupdate)|Adiciona uma chave de Cofre de chaves para uma instância do SQL Server|
|[Eliminar chave de servidor](/rest/api/sql/serverkeys/delete)|Remove uma chave de Cofre de chaves a partir de uma instância do SQL Server|
|[Obter chaves de servidor](/rest/api/sql/serverkeys/get)|Obtém uma chave específica do Key Vault a partir de uma instância do SQL Server|
|[Listar chaves de servidor por servidor](/rest/api/sql/serverkeys/listbyserver)|Obtém as chaves do Cofre de chaves para uma instância do SQL Server |
|[Criar ou atualizar o Protetor de encriptação](/rest/api/sql/encryptionprotectors/createorupdate)|Define o protetor de encriptação de dados transparente para uma instância do SQL Server|
|[Obter o Protetor de encriptação](/rest/api/sql/encryptionprotectors/get)|Obtém o protetor de encriptação de dados transparente para uma instância do SQL Server|
|[Protetores de encriptação de lista pelo servidor](/rest/api/sql/encryptionprotectors/listbyserver)|Obtém os protetores de encriptação de dados transparente para uma instância do SQL Server |
|[Criar ou atualizar a configuração da encriptação transparente de dados](/rest/api/sql/transparentdataencryptions/createorupdate)|Ativa ou desativa a encriptação de dados transparente para uma base de dados|
|[Obter configuração de encriptação de dados transparente](/rest/api/sql/transparentdataencryptions/get)|Obtém a configuração de encriptação de dados transparente para uma base de dados|
|[Resultados de configuração de encriptação de dados transparente de lista](/rest/api/sql/transparentdataencryptionactivities/ListByConfiguration)|Obtém o resultado de encriptação para uma base de dados|

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral da encriptação transparente de dados, consulte [encriptação de dados transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).
- Para saber mais sobre a encriptação de dados transparente com suporte de traga a sua própria chave para a base de dados SQL e o armazém de dados, veja [encriptação de dados transparente com suporte de traga a sua própria chave](transparent-data-encryption-byok-azure-sql.md).
- Para começar a utilizar a encriptação de dados transparente com suporte de traga a sua própria chave, consulte o guia de procedimentos [ativar a criptografia transparente de dados utilizando a sua própria chave do Key Vault com o PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).
- Para obter mais informações sobre o Key Vault, consulte a [página de documentação do Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
