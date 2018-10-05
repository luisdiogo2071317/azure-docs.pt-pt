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
ms.openlocfilehash: 38717fed9f3877dfd0aa9819571ef0f32befc117
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785516"
---
# <a name="azure-key-vault-storage-account-keys"></a>Chaves de conta de armazenamento do Azure Key Vault

Antes de chaves de conta de armazenamento do Azure Key Vault, os desenvolvedores tinham de gerenciar suas próprias chaves de conta de armazenamento do Azure (ASA) e girá-las manualmente ou através de uma automatização externa. Agora, a chave de Cofre de chaves de conta de armazenamento são implementadas como [segredos do Key Vault](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) para autenticar com uma conta de armazenamento do Azure.

A funcionalidade de chave de conta de armazenamento do Azure (ASA) gere rotação secreta para. Esta operação também remove a necessidade de contacto direto com uma chave ASA, oferecendo partilhado assinaturas de acesso (SAS) como um método.

Para obter mais informações sobre as contas de armazenamento do Azure, consulte [sobre as contas de armazenamento](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Interfaces de suporte

Encontrará uma lista completa e links para nosso interfaces de programação e scripts no [Guia do programador do Cofre de chave](key-vault-developers-guide.md#coding-with-key-vault).


## <a name="what-key-vault-manages"></a>O que gere o Key Vault

Key Vault realiza várias funções de gestão interna em seu nome quando utilizar chaves de conta de armazenamento gerido.

- O Azure Key Vault gere chaves de uma conta de armazenamento do Azure (ASA).
    - Internamente, o Azure Key Vault pode listar as chaves de (sincronização) com uma conta de armazenamento do Azure.
    - O Azure Key Vault gera novamente (roda) as chaves periodicamente.
    - Valores de chaves nunca são retornados em resposta ao chamador.
    - O Azure Key Vault gere chaves de contas de armazenamento e as contas de armazenamento clássico.
- O Azure Key Vault permite-lhe, o proprietário do cofre/objeto, para criar definições de SAS (assinatura de acesso partilhado, conta ou o serviço SAS).
    - O valor SAS, criado com a definição de SAS, é retornado como um segredo via o caminho do URI de REST. Para obter mais informações, consulte as operações de definição de SAS no [referência de API de REST do Cofre de chaves do Azure](/rest/api/keyvault).

## <a name="naming-guidance"></a>Orientações de nomenclatura

- Os nomes das contas do Storage devem ter entre 3 e 24 carateres de comprimento e apenas podem conter números e letras minúsculas.
- Um nome de definição do SAS tem de ser 1 102 carateres de comprimento e conter apenas 0-9, a-z, A-Z.

## <a name="developer-experience"></a>Experiência do desenvolvedor

### <a name="before-azure-key-vault-storage-keys"></a>Antes das chaves de armazenamento do Azure Key Vault

Desenvolvedores utilizados precisará fazer as seguintes práticas com uma chave de conta de armazenamento para obter acesso ao armazenamento do Azure.
1. Store cadeia de ligação ou SAS token em definições da aplicação de serviço de aplicações do Azure ou outro armazenamento.
1. Na inicialização do aplicativo, obter a cadeia de ligação ou SAS token.
1. Crie [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) para interagir com o armazenamento.

```cs
// The Connection string is being fetched from App Service application settings
var connectionStringOrSasToken = CloudConfigurationManager.GetSetting("StorageConnectionString");
var storageAccount = CloudStorageAccount.Parse(connectionStringOrSasToken);
var blobClient = storageAccount.CreateCloudBlobClient();
 ```

### <a name="after-azure-key-vault-storage-keys"></a>Depois das chaves de armazenamento do Azure Key Vault

Os programadores criar uma [KeyVaultClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient) e aproveitá-lo para obter o token SAS para seu armazenamento. Depois disso, eles criam [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) com esse token.

```cs
// Create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();

// Use the blobClientWithSas
...

// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

 ### <a name="developer-guidance"></a>Orientação para programadores

- Permitir apenas o Key Vault para gerir as chaves de ASA. Não tente geri-los por conta própria, irá interferir com os processos de Key Vault.
- Não permitir que as chaves do ASA para serem geridas por mais de um objeto do Cofre de chaves.
- Se precisar de voltar a gerar as chaves ASA manualmente, é recomendável que regenere-los através do Key Vault.

## <a name="authorize-key-vault-to-access-to-your-storage-account"></a>Autorizar o Key Vault para aceder à sua conta de armazenamento

Antes de Key Vault pode aceder e gerir as chaves de conta de armazenamento, tem de autorizar o acesso da sua conta de armazenamento.  Como muitos aplicativos, o Key Vault se integra com o Azure AD para os serviços de gestão de identidades e acessos. 

Uma vez que o Key Vault é um aplicativo da Microsoft, previamente está registado em todos os inquilinos do Azure AD com o ID de aplicação `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`. E como todos os aplicativos registrados com o Azure AD, uma [principal de serviço](/azure/active-directory/develop/app-objects-and-service-principals) objeto fornece propriedades de identidade da aplicação. O principal de serviço, em seguida, pode ser atribuído da autorização para aceder a outro recurso, por meio do controle de acesso baseado em funções (RBAC).  

A aplicação do Azure Key Vault requer permissões para *lista* e *voltar a gerar* chaves para a sua conta de armazenamento. Estas permissões são habilitadas pelo incorporada [serviço de operador de chave de conta de armazenamento](/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role) função RBAC. O principal de serviço de Key Vault é atribuir a esta função através dos seguintes passos:

```powershell
# Get the resource ID of the Azure Storage Account you want Key Vault to manage
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Assign Storage Key Operator role to Azure Key Vault Identity
New-AzureRmRoleAssignment -ApplicationId “cfa8b339-82a2-471a-a3c9-0fc0be7a4093” -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storage.Id
```

> [!NOTE]
> Para um tipo de conta clássica, defina o parâmetro de função como *"Clássico conta chave operador função do serviço armazenamento."*

Após a atribuição de função efetuada com êxito, deverá ver um resultado semelhante ao seguinte

```console
RoleAssignmentId   : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgSandbox/providers/Microsoft.Storage/storageAccounts/sabltest/providers/Microsoft.Authorization/roleAssignments/189cblll-12fb-406e-8699-4eef8b2b9ecz
Scope              : /subscriptions/03f0blll-ce69-483a-a092-d06ea46dfb8z/resourceGroups/rgSandbox/providers/Microsoft.Storage/storageAccounts/sabltest
DisplayName        : Azure Key Vault
SignInName         :
RoleDefinitionName : Storage Account Key Operator Service Role
RoleDefinitionId   : 81a9blll-bebf-436f-a333-f67b29880f1z
ObjectId           : c730c8da-blll-4032-8ad5-945e9dc8262z
ObjectType         : ServicePrincipal
CanDelegate        : False
```

## <a name="working-example"></a>Exemplo de trabalho

O exemplo seguinte demonstra a criação de um cofre de chaves geridas conta de armazenamento do Azure e as definições de SAS associadas.

### <a name="prerequisite"></a>Pré-requisito

Antes de começar, certifique-se de que [autorizar o Key Vault para aceder à sua conta de armazenamento](#authorize-key-vault-to-access-to-your-storage-account).

### <a name="setup"></a>Configurar

```powershell
# This is the name of our Key Vault
$keyVaultName = "mykeyVault"

# Fetching all the storage account object, of the ASA we want to manage with KeyVault
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure KeyVault Identity service principal
$servicePrincipalId = $(Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

Em seguida, defina as permissões para **sua conta** para se certificar de que pode gerir todas as permissões de armazenamento no Key Vault. No exemplo a seguir, é a nossa conta do Azure _developer@contoso.com_.

```powershell
# Searching our Azure Active Directory for our account's ObjectId
$userPrincipalId = $(Get-AzureRmADUser -SearchString "developer@contoso.com").Id

# We use the ObjectId we found to setting permissions on the vault
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $userPrincipalId -PermissionsToStorage all
```

### <a name="create-a-key-vault-managed-storage-account"></a>Criar um cofre de chaves de conta de armazenamento gerida

Agora, crie uma conta de armazenamento gerido no Azure Key Vault e utilizar uma chave de acesso da conta de armazenamento para criar os tokens de SAS.
- `-ActiveKeyName` utiliza 'chave2' para gerar os tokens de SAS.
- `-AccountName` é utilizado para identificar a sua conta de armazenamento gerido. Abaixo, estamos a utilizar o nome da conta de armazenamento para manter tudo simples, mas pode ser qualquer nome.
- `-DisableAutoRegenerateKey` Especifica não voltar a gerar as chaves de conta de armazenamento.

```powershell
# Adds your storage account to be managed by Key Vault and will use the access key, key2
Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storage.StorageAccountName -AccountResourceId $storage.Id -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="key-regeneration"></a>Regeneração da chave

Se pretender que o Cofre de chaves para voltar a gerar chaves de acesso do seu armazenamento periodicamente, pode definir um período de regeneração. Abaixo, iremos estiver a definir um período de regeneração de 3 dias. Após 3 dias, o Cofre de chaves irá regenerar 'chave1' e trocar a chave ativa de 'chave2' para 'chave1'.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
$accountName = $storage.StorageAccountName

Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $accountName -AccountResourceId $storage.Id -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Definições de SAS de conjunto

A conta SAS fornece acesso ao serviço de Blobs com permissões diferentes.
Defina as definições de SAS no Key Vault para a sua conta de armazenamento gerido.
- `-AccountName` é o nome da conta de armazenamento gerido no Cofre de chaves.
- `-Name` é o identificador para o token SAS no seu armazenamento.
- `-ValidityPeriod` Define a data de expiração para o token SAS gerado.

```powershell
$validityPeriod = [System.Timespan]::FromDays(1)
$readSasName = "readBlobSas"
$writeSasName = "writeBlobSas"

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName $keyVaultName -AccountName $accountName -Name $readSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName $keyVaultName -AccountName $accountName -Name $writeSasName -Protocol HttpsOnly -ValidityPeriod $validityPeriod -Permission Read,List,Write
```

### <a name="get-sas-tokens"></a>Obter os tokens SAS

Obter os tokens de SAS correspondentes e fazer chamadas para o armazenamento. `-SecretName` é construído de acordo com a entrada do `AccountName` e `Name` parâmetros quando tiver executado [conjunto AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition).

```powershell
$readSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$readSasName").SecretValueText
$writeSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$writeSasName").SecretValueText
```

### <a name="create-storage"></a>Criar armazenamento

Tenha em atenção que tentar aceder com *$readSasToken* falhar, mas que somos capazes de acessar com *$writeSasToken*.

```powershell
$context1 = New-AzureStorageContext -SasToken $readSasToken -StorageAccountName $storage.StorageAccountName
$context2 = New-AzureStorageContext -SasToken $writeSasToken -StorageAccountName $storage.StorageAccountName

# Ensure the txt file in command exists in local path mentioned
Set-AzureStorageBlobContent -Container containertest1 -File "./abc.txt" -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "./file.txt" -Context $context2
```

É capaz de aceder o conteúdo com o token SAS que tem acesso de escrita de blob de armazenamento.

### <a name="relevant-powershell-cmdlets"></a>Cmdlets do Powershell relevantes

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="storage-account-onboarding"></a>Integração de conta de armazenamento

Exemplo: como um proprietário do objeto do Cofre de chaves que adicionar um objeto de conta de armazenamento para o Azure Key Vault para integrar uma conta de armazenamento.

Durante a integração, Cofre de chaves tem de verificar que a identidade da conta de integração tem permissões para *lista* e, a *voltar a gerar* chaves de armazenamento. Para verificar estas permissões, o Key Vault obtém um OBO (no nome de) token do serviço de autenticação, o público-alvo definido para o Azure Resource Manager e faz uma *lista* chave chamada para o serviço de armazenamento do Azure. Se o *lista* chamada falhar, o Key Vault Falha ao criar o objeto com um código de estado HTTP *proibido*. As chaves listadas dessa forma são colocadas em cache com o armazenamento de entidade do Cofre de chaves.

Cofre de chaves tem de verificar que a identidade tem *regenerar* permissões antes de ele pode assumir a propriedade de regeneração das chaves. Para verificar que a identidade, através do token OBO, bem como a identidade de terceiros primeiro do Key Vault tem estas permissões:

- Key Vault apresenta uma lista de permissões de RBAC do recurso de conta de armazenamento.
- Key Vault valida a resposta por meio de correspondência de expressões regulares de ações e não ações.

Encontrar alguns exemplos de suporte em [Key Vault, exemplos de chaves de conta de armazenamento gerido](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=).

Se não tiver a identidade *regenerar* permissões ou se não tiver a identidade de terceiros primeiro Cofre de chaves *lista* ou *regenerar* permissão, em seguida, a integração pedido falha retornando um código de erro apropriada e a mensagem.

O token OBO só funcionará quando utiliza o original, aplicativos de cliente nativo do PowerShell ou CLI.

## <a name="other-applications"></a>Outras aplicações

- Tokens SAS, construído de acordo com as chaves de conta de armazenamento do Cofre de chaves, fornecem acesso controlado ainda mais a uma conta de armazenamento do Azure. Para obter mais informações, consulte [assinaturas de acesso partilhado do Using](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Consulte também

- [Sobre chaves, segredos e certificados](https://docs.microsoft.com/rest/api/keyvault/)
- [Blog da Equipe do Cofre de chaves](https://blogs.technet.microsoft.com/kv/)
