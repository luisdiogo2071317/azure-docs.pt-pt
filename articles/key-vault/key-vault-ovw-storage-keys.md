---
ms.assetid: ''
title: Chaves de conta de armazenamento do Azure Key Vault
description: Chaves de conta de armazenamento fornecem uma integração de seemless entre o Azure Key Vault e chave de acesso baseado em à conta de armazenamento do Azure.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 10/03/2018
ms.openlocfilehash: adc8b84f0f22e85de88c4bd80c10a2a35d7b490a
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114605"
---
# <a name="azure-key-vault-storage-account-keys"></a>Chaves de conta de armazenamento do Azure Key Vault

> [!NOTE]
> [O armazenamento do Azure suporta agora a autorização do AAD](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Recomendamos que utilize o Azure Active Directory para autenticação e autorização para o armazenamento de que os utilizadores não tenham de se preocupar sobre rotação das suas chaves de conta de armazenamento.

- O Azure Key Vault gere chaves de uma conta de armazenamento do Azure (ASA).
    - Internamente, o Azure Key Vault pode listar as chaves de (sincronização) com uma conta de armazenamento do Azure.    
    - O Azure Key Vault gera novamente (roda) as chaves periodicamente.
    - Valores de chaves nunca são retornados em resposta ao chamador.
    - O Azure Key Vault gere chaves de contas de armazenamento e as contas de armazenamento clássico.

<a name="prerequisites"></a>Pré-requisitos
--------------
1. [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) instalar a CLI do Azure   
2. [Criar uma conta de armazenamento](https://azure.microsoft.com/services/storage/)
    - Siga os passos desta [documento](https://docs.microsoft.com/azure/storage/) para criar uma conta de armazenamento  
    - **Orientações de nomenclatura:** nomes de conta de armazenamento tem de ter entre 3 e 24 carateres de comprimento e pode conter apenas letras minúsculas e números.        
      
<a name="step-by-step-instructions"></a>Passo a passo-instruções
-------------------------

1. Obtenha o ID de recurso da conta do Storage do Azure que pretende gerir.
    a. Depois de criar uma conta de armazenamento, execute o seguinte comando para obter o ID de recurso da conta de armazenamento que pretende gerir
    ```
    az storage account show -n storageaccountname (Copy ID out of the result of this command)
    ```
2. Obter serviço do Azure Key Vault do ID aplicação principal 
    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
3. Atribua a função de operador de chave de armazenamento para o Azure Key Vault Identity
    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id hhjkh --scope idofthestorageaccount
    ```
4. Criar um cofre de chaves de conta de armazenamento gerida.     <br /><br />
   Comando abaixo solicita o Key Vault para voltar a gerar chaves de acesso do seu armazenamento periodicamente, com um ponto de nova geração. Abaixo, iremos estiver a definir um período de regeneração de 90 dias. Após 90 dias, o Cofre de chaves irá regenerar 'chave1' e trocar a chave ativa de 'chave2' para 'chave1'.
   ### <a name="key-regeneration"></a>Regeneração da chave
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-generate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    No caso do utilizador não tiver criado a conta de armazenamento e não tem permissões para a conta de armazenamento, os passos abaixo defina as permissões para a sua conta para se certificar de que pode gerir todas as permissões de armazenamento no Key Vault.
    [!NOTE] No caso de que o usuário executa não permissões para a conta de armazenamento, primeiro, obtenha o id de objeto do utilizador

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover purge restore set setsas update
    ```

### <a name="relevant-powershell-cmdlets"></a>Cmdlets do Powershell relevantes

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="see-also"></a>Consulte também

- [Sobre chaves, segredos e certificados](https://docs.microsoft.com/rest/api/keyvault/)
- [Blog da Equipe do Cofre de chaves](https://blogs.technet.microsoft.com/kv/)
