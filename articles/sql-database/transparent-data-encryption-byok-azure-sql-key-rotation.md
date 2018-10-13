---
title: SQL Database do Azure PowerShell - protetor de TDE rodar - | Documentos da Microsoft
description: Saiba como rodar o protetor de encriptação de dados transparente (TDE) para um servidor SQL do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: jhubbard
ms.date: 08/07/2017
ms.openlocfilehash: fc8660b6fa6d9778a4eac9ce939181aa24d5097d
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309643"
---
# <a name="rotate-the-transparent-data-encryption-tde-protector-using-powershell"></a>Rodar o protetor de encriptação de dados transparente (TDE) com o PowerShell 

Este artigo descreve a rotação de chaves para um servidor SQL do Azure com um protetor de TDE no Azure Key Vault. Girando meios de protetor de TDE de um Azure SQL server mudar para uma nova chave assimétrica que protege as bases de dados no servidor. Rotação de chaves é uma operação online e deverá demorar apenas alguns segundos para concluir, uma vez que isso só descriptografa e encripta novamente a chave de encriptação de dados da base de dados, não a base de dados.

Este guia aborda duas opções para girar o protetor de TDE no servidor.

> [!NOTE]
> Tem de ser retomado um SQL Data Warehouse em pausa antes de rotações de chave.
>

> [!IMPORTANT]
> **Fazer não eliminar** versões anteriores da chave depois de um rollover.  Quando as chaves estão a ser transferidas, alguns dados ainda são encriptados com as chaves anteriores, como cópias de segurança de base de dados mais antigas. 
>

## <a name="prerequisites"></a>Pré-requisitos

- Este guia de procedimentos pressupõe que já estiver a utilizar uma chave de Cofre de chaves do Azure como o protetor de TDE para uma base de dados do Azure SQL ou armazém de dados. Ver [encriptação de dados transparente com suporte BYOK](transparent-data-encryption-byok-azure-sql.md).
- Tem de ter o Azure PowerShell versão 3.7.0 ou mais recente instalado e em execução. 
- [Recomendável, mas opcional] Criar o material de chave para o protetor de TDE num módulo de segurança de hardware (HSM) ou armazenar a primeira chave local e importar o material de chave para o Azure Key Vault. Siga os [instruções para utilizar um módulo de segurança de hardware (HSM) e o Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) para saber mais.

## <a name="option-1-auto-rotation"></a>Opção 1: Rotação automática

Gere uma nova versão da chave de protetor de TDE existente no Cofre de chaves, sob o mesmo nome de chave e o Cofre de chaves. O serviço do SQL do Azure começa a utilizar esta nova versão dentro de 24 horas. 

Para criar uma nova versão da TDE protetor a utilizar o [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) cmdlet:

   ```powershell
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>
   ```

## <a name="option-2-manual-rotation"></a>Opção 2: Rotação de Manual

A opção utiliza a [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey), [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey), e [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlets para adicionar uma chave completamente nova, que poderia ser num novo nome de chave ou até mesmo outro Cofre de chaves. 

>[!NOTE]
>O comprimento combinado para o nome de Cofre de chaves e o nome da chave não pode exceder 94 carateres.
>

   ```powershell
   # Add a new key to Key Vault
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>   
  
   <# Set the key as the TDE protector for all resources 
   under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
  
## <a name="other-useful-powershell-cmdlets"></a>Outros cmdlets do PowerShell útil

- Para mudar o protetor de TDE de gerida pela Microsoft para o modo BYOK, utilize o [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlet.

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- Para mudar o protetor de TDE do modo BYOK para gerida pela Microsoft, utilize o [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlet.

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type ServiceManaged `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName> 
   ``` 

## <a name="next-steps"></a>Passos Seguintes

- Em caso de um risco de segurança, saiba como remover um protetor de TDE potencialmente comprometido: [remover uma chave potencialmente comprometida](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md) 

- Introdução ao traga a sua própria chave de suporte para TDE: [ativar TDE com sua própria chave do Key Vault com o PowerShell](transparent-data-encryption-byok-azure-sql-configure.md)
