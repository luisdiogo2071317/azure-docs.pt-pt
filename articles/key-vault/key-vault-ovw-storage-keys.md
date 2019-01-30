---
ms.assetid: ''
title: O Azure Key Vault geridos a conta de armazenamento - CLI
description: Chaves de conta de armazenamento fornecem uma integração de seemless entre o Azure Key Vault e chave de acesso baseado em à conta de armazenamento do Azure.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: prashanthyv
ms.author: pryerram
manager: mbaldwin
ms.date: 10/03/2018
ms.openlocfilehash: 0392d84efa3a82a6323d6d09db792df7d6c42256
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55210680"
---
# <a name="azure-key-vault-managed-storage-account---cli"></a>O Azure Key Vault geridos a conta de armazenamento - CLI

> [!NOTE]
> [Integração de armazenamento do Azure com o Azure Active Directory (Azure AD) está agora em pré-visualização](https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Recomendamos a utilização do Azure AD para autenticação e autorização, que fornece acesso baseado em tokens do OAuth2 ao armazenamento do Azure, tal como o Azure Key Vault. Isto permite-lhe:
> - Autenticar a sua aplicação de cliente com uma identidade de utilizador ou aplicação, em vez das credenciais da conta de armazenamento. 
> - Utilize um [identidade gerida do Azure AD](/azure/active-directory/managed-identities-azure-resources/) quando em execução no Azure. Gerido identidades remove a necessidade de autenticação de cliente em conjunto e armazenar credenciais no ou com a sua aplicação.
> - Utilize o controlo de acesso baseado em ' (RBAC) da função para gerir a autorização, que também é suportada pelo Key Vault.

- O Azure Key Vault gere chaves de uma conta de armazenamento do Azure (ASA).
    - Internamente, o Azure Key Vault pode listar as chaves de (sincronização) com uma conta de armazenamento do Azure.    
    - O Azure Key Vault gera novamente (roda) as chaves periodicamente.
    - Valores de chaves nunca são retornados em resposta ao chamador.
    - O Azure Key Vault gere chaves de contas de armazenamento e as contas de armazenamento clássico.
    
> [!IMPORTANT]
> Um inquilino do Azure AD fornece cada aplicação registada com um  **[principal de serviço](/azure/active-directory/develop/developer-glossary#service-principal-object)**, que serve como identidade da aplicação. ID da aplicação do principal de serviço é utilizado quando dando a ele autorização para aceder a outros recursos do Azure, através do controlo de acesso baseado em funções (RBAC). Uma vez que o Key Vault é um aplicativo da Microsoft, previamente está registado em todos os inquilinos do Azure AD sob o mesmo ID de aplicação, dentro de cada cloud do Azure:
> - Utilizam o Azure AD inquilinos na cloud do Azure government ID da aplicação `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - ID da aplicação de utilizar do Azure AD inquilinos na cloud pública do Azure e todos os outros `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.

<a name="prerequisites"></a>Pré-requisitos
--------------
1. [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) instalar a CLI do Azure   
2. [Criar uma conta de armazenamento](https://azure.microsoft.com/services/storage/)
    - Siga os passos desta [documento](https://docs.microsoft.com/azure/storage/) para criar uma conta de armazenamento  
    - **Orientações de nomenclatura:** Os nomes das contas do Storage devem ter entre 3 e 24 carateres de comprimento e apenas podem conter números e letras minúsculas.        
      
<a name="step-by-step-instructions-on-how-to-use-key-vault-to-manage-storage-account-keys"></a>Passo a passo instruções sobre como utilizar o Key Vault para gerir chaves de conta de armazenamento
--------------------------------------------------------------------------------
Nas instruções, abaixo, está a atribuir Key Vault como um serviço para ter permissões de operador na sua conta de armazenamento

> [!NOTE]
> . Tenha em atenção que assim que tiver configurado a cópia de segurança gerida do Azure Key Vault conta de armazenamento de chaves que eles deve **não** já é possível alterar, exceto através do Key Vault. Armazenamento de chaves de conta significa que o Key Vault iria gerir a rotação da chave de conta de armazenamento geridas

1. Depois de criar uma conta de armazenamento, execute o seguinte comando para obter o ID de recurso da conta de armazenamento, que pretende gerir

    ```
    az storage account show -n storageaccountname (Copy ID field out of the result of this command)
    ```
    
2. Obter serviço do Azure Key Vault do ID aplicação principal 

    ```
    az ad sp show --id cfa8b339-82a2-471a-a3c9-0fc0be7a4093
    ```
    
3. Atribua a função de operador de chave de armazenamento para o Azure Key Vault Identity

    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ApplicationIdOfKeyVault> --scope <IdOfStorageAccount>
    ```
    
4. Criar um cofre de chaves de conta de armazenamento gerida.     <br /><br />
   Abaixo, iremos estiver a definir um período de regeneração de 90 dias. Após 90 dias, o Cofre de chaves irá regenerar 'chave1' e trocar a chave ativa de 'chave2' para 'chave1'.
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key2 --auto-regenerate-key --regeneration-period P90D --resource-id <Resource-id-of-storage-account>
    ```
    No caso do utilizador não tiver criado a conta de armazenamento e não tem permissões para a conta de armazenamento, os passos abaixo defina as permissões para a sua conta para se certificar de que pode gerir todas as permissões de armazenamento no Key Vault.
 > [!NOTE] 
    No caso de que o utilizador não tem permissões para a conta de armazenamento, vamos primeiro, obtenha o Id de objeto do utilizador

    ```
    az ad user show --upn-or-object-id "developer@contoso.com"

    az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
    ```
    
## <a name="how-to-access-your-storage-account-with-sas-tokens"></a>Como aceder à sua conta de armazenamento com tokens SAS

Nesta seção, abordaremos como pode fazer operações na sua conta de armazenamento, buscando [SAS tokens](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) do Key Vault

Na abaixo da secção, demonstramos como obter a sua chave de conta de armazenamento que é armazenado no Cofre de chaves e usar isso para criar uma definição de SAS (assinatura de acesso partilhado) para a sua conta de armazenamento.

> [!NOTE] 
  Existem 3 formas de autenticar para o Key Vault, como pode ler no [conceitos básicos](key-vault-whatis.md#basic-concepts)
- Utilizar a identidade de serviço gerida (altamente recomendado)
- Com o Principal de serviço e certificado 
- Com o Principal de serviço e a palavra-passe (não recomendado)

```cs
// Once you have a security token from one of the above methods, then create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault. SecretUri is of the format https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Se o token SAS está prestes a expirar, em seguida, poderia obter o token SAS novamente a partir do Key Vault e atualizar o código

```cs
// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```


### <a name="relavant-azure-cli-cmdlets"></a>Cmdlets da CLI do Azure de Relavant
[Cmdlets de armazenamento da CLI do Azure](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

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
