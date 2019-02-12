---
title: 'PowerShell e CLI: Ativar a TDE SQL - com o Azure Key Vault – traga a sua própria chave - base de dados SQL do Azure | Documentos da Microsoft'
description: Saiba como configurar uma base de dados do Azure SQL e o armazém de dados para começar a utilizar a encriptação de dados transparente (TDE) para encriptação em repouso com o PowerShell ou CLI.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: b6abfa24d790554de239171ffef1899cdc135d26
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/11/2019
ms.locfileid: "55996258"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-with-customer-managed-key-from-azure-key-vault"></a>PowerShell e CLI: Ativar a encriptação de dados transparente com a chave gerida pelo cliente do Azure Key Vault

Este artigo explica como utilizar uma chave de Cofre de chaves do Azure para dados de encriptação transparente (TDE) numa base de dados SQL ou armazém de dados. Para saber mais sobre o TDE com a integração do Azure Key Vault - suporte de Bring Your Own Key (BYOK), visite [TDE com chaves geridas pelo cliente no Azure Key Vault](transparent-data-encryption-byok-azure-sql.md). 

## <a name="prerequisites-for-powershell"></a>Pré-requisitos para o PowerShell

- Tem de ter uma subscrição do Azure e de ser um administrador da subscrição.
- [Recomendável, mas opcional] Ter um módulo de segurança de hardware (HSM) ou chave local armazenar para a criação de uma cópia local do material de chave de Protetor de TDE.
- Tem de ter o Azure PowerShell versão 4.2.0 ou mais recente instalado e em execução. 
- Crie um cofre de chaves do Azure e a chave a utilizar para TDE.
   - [Instruções do PowerShell do Key Vault](../key-vault/key-vault-get-started.md)
   - [Instruções para utilizar um módulo de segurança de hardware (HSM) e o Key Vault](../key-vault/key-vault-get-started.md#HSM)
 - O Cofre de chaves tem de ter a seguinte propriedade a ser utilizado para TDE:
   - [soft-delete](../key-vault/key-vault-ovw-soft-delete.md)
   - [Como utilizar a eliminação de forma recuperável do Key Vault com o PowerShell](../key-vault/key-vault-soft-delete-powershell.md) 
- A chave tem de ter os seguintes atributos a ser utilizado para TDE:
   - Sem data de expiração
   - Não foi desativada
   - Conseguir realizar *Obtenha*, *moldar chave*, *anular a moldagem de chave* operações

## <a name="step-1-assign-an-azure-ad-identity-to-your-server"></a>Passo 1. Atribuir uma identidade do Azure AD ao seu servidor 

Se tiver um servidor existente, utilize o seguinte para adicionar uma identidade do Azure AD ao seu servidor:

   ```powershell
   $server = Set-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -AssignIdentity
   ```

Se estiver a criar um servidor, utilize o [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) cmdlet com a marca-identidade para adicionar uma identidade do Azure AD durante a criação do servidor:

   ```powershell
   $server = New-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -Location <RegionName> `
   -ServerName <LogicalServerName> `
   -ServerVersion "12.0" `
   -SqlAdministratorCredentials <PSCredential> `
   -AssignIdentity 
   ```

## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Passo 2. Conceder permissões de Key Vault para o seu servidor

Utilize o [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet para conceder o acesso de servidor para a chave do cofre antes de utilizar uma chave do mesmo para TDE.

   ```powershell
   Set-AzureRmKeyVaultAccessPolicy  `
   -VaultName <KeyVaultName> `
   -ObjectId $server.Identity.PrincipalId `
   -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Passo 3. Adicione a chave do Key Vault para o servidor e defina o Protetor de TDE

- Utilize o [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) cmdlet para adicionar a chave a partir do Key Vault para o servidor.
- Utilize o [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlet para definir a chave como o protetor de TDE para todos os recursos de servidor.
- Utilize o [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) cmdlet para confirmar que o protetor de TDE foi configurado conforme pretendido.

> [!Note]
> O comprimento combinado para o nome de Cofre de chaves e o nome da chave não pode exceder 94 carateres.
> 

>[!Tip]
>Um exemplo KeyId do Key Vault: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>

   ```powershell
   <# Add the key from Key Vault to the server #>
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> 

   <# To confirm that the TDE protector was configured as intended: #>
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> 
   ```

## <a name="step-4-turn-on-tde"></a>Passo 4. Ativar o TDE 

Utilize o [Set-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) cmdlet para ativar a TDE.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `
   -State "Enabled"
   ```

Agora o armazém de dados ou base de dados tem o TDE ativado com uma chave de encriptação no Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Passo 5. Verifique o estado de encriptação e a atividade de encriptação

Utilize o [Get-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) para obter o estado de encriptação e o [Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) para verificar o progresso de criptografia para um armazém de dados ou base de dados.

   ```powershell
   # Get the encryption state
   Get-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `

   <# Check the encryption progress for a database or data warehouse #>
   Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName>  
   ```

## <a name="other-useful-powershell-cmdlets"></a>Outros cmdlets do PowerShell útil

- Utilize o [Set-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) cmdlet para desativar a TDE.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -DatabaseName <DatabaseName> `
   -State "Disabled”
   ```
 
- Utilize o [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) cmdlet devolver a lista de chaves do Cofre de chaves adicionadas ao servidor.

   ```powershell
   <# KeyId is an optional parameter, to return a specific key version #>
   Get-AzureRmSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```
 
- Utilize o [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) para remover uma chave de Cofre de chaves do servidor.

   ```powershell
   <# The key set as the TDE Protector cannot be removed. #>
   Remove-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>   
   ```
 
## <a name="troubleshooting"></a>Resolução de problemas

Verifique o seguinte se ocorrer um problema:
- Se não for encontrado o Cofre de chaves, certificar-se de que está na subscrição correta com o [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription) cmdlet.

   ```powershell
   Get-AzureRmSubscription `
   -SubscriptionId <SubscriptionId>
   ```

- Se a nova chave não é possível adicionar o servidor ou a nova chave não pode ser atualizada como o Protetor de TDE, verifique o seguinte:
   - A chave não deve ter uma data de expiração
   - A chave tem de ter o *Obtenha*, *moldar chave*, e *anular a moldagem de chave* operações ativadas.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como rodar o Protetor de TDE de um servidor para atender aos requisitos de segurança: [Rodar para a encriptação de dados transparente protetor através do PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- Em caso de um risco de segurança, saiba como remover um Protetor de TDE potencialmente comprometidos: [Remover uma chave potencialmente comprometida](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md). 

## <a name="prerequisites-for-cli"></a>Pré-requisitos para a CLI

- Tem de ter uma subscrição do Azure e de ser um administrador da subscrição.
- [Recomendável, mas opcional] Ter um módulo de segurança de hardware (HSM) ou chave local armazenar para a criação de uma cópia local do material de chave de Protetor de TDE.
- Interface de linha de comandos versão 2.0 ou posterior. Para instalar a versão mais recente e ligar à sua subscrição do Azure, veja [instalar e configurar a Interface de linha de comandos de várias plataformas do Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 
- Crie um cofre de chaves do Azure e a chave a utilizar para TDE.
   - [Gerir o Cofre de chaves com a CLI 2.0](../key-vault/key-vault-manage-with-cli2.md)
   - [Instruções para utilizar um módulo de segurança de hardware (HSM) e o Key Vault](../key-vault/key-vault-get-started.md#HSM)
 - O Cofre de chaves tem de ter a seguinte propriedade a ser utilizado para TDE:
   - [soft-delete](../key-vault/key-vault-ovw-soft-delete.md)
   - [Como utilizar a eliminação de forma recuperável do Key Vault com a CLI](../key-vault/key-vault-soft-delete-cli.md) 
- A chave tem de ter os seguintes atributos a ser utilizado para TDE:
   - Sem data de expiração
   - Não foi desativada
   - Conseguir realizar *Obtenha*, *moldar chave*, *anular a moldagem de chave* operações
   
## <a name="step-1-create-a-server-and-assign-an-azure-ad-identity-to-your-server"></a>Passo 1. Criar um servidor e atribuir uma identidade do Azure AD ao seu servidor
      cli
      # create server (with identity) and database
      az sql server create -n "ServerName" -g "ResourceGroupName" -l "westus" -u "cloudsa" -p "YourFavoritePassWord99@34" -i 
      az sql db create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 
      

 
## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Passo 2. Conceder permissões de Key Vault para o seu servidor
      cli
      # create key vault, key and grant permission
      az keyvault create -n "VaultName" -g "ResourceGroupName" 
      az keyvault key create -n myKey -p software --vault-name "VaultName" 
      az keyvault set-policy -n "VaultName" --object-id "ServerIdentityObjectId" -g "ResourceGroupName" --key-permissions wrapKey unwrapKey get list 
      

 
## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Passo 3. Adicione a chave do Key Vault para o servidor e defina o Protetor de TDE
  
     cli
     # add server key and update encryption protector
      az sql server key create -g "ResourceGroupName" -s "ServerName" -t "AzureKeyVault" -u "FullVersionedKeyUri 
      az sql server tde-key update -g "ResourceGroupName" -s "ServerName" -t AzureKeyVault -u "FullVersionedKeyUri" 
      
  
  > [!Note]
> O comprimento combinado para o nome de Cofre de chaves e o nome da chave não pode exceder 94 carateres.
> 

>[!Tip]
>Um exemplo KeyId do Key Vault: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>
  
## <a name="step-4-turn-on-tde"></a>Passo 4. Ativar o TDE 
      cli
      # enable encryption
      az sql db tde create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" --status Enabled 
      

Agora o armazém de dados ou base de dados tem o TDE ativado com uma chave de encriptação no Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Passo 5. Verifique o estado de encriptação e a atividade de encriptação

     cli
      # get encryption scan progress
      az sql db tde show-activity -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

      # get whether encryption is on or off
      az sql db tde show-configuration -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

## <a name="sql-cli-references"></a>Referências de CLI de SQL

https://docs.microsoft.com/cli/azure/sql?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/tde-key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/db/tde?view=azure-cli-latest 

