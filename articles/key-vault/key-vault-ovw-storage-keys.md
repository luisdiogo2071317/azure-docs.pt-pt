---
ms.assetid: 
title: Chaves de conta de armazenamento do Cofre de chaves do Azure
description: "Chaves de conta de armazenamento fornecem uma integração seemless entre o Cofre de chaves do Azure e a chave de acesso com base para a conta de armazenamento do Azure."
ms.topic: article
services: key-vault
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 10/12/2017
ms.openlocfilehash: a3f8d540c7e4c8a86b151540980724777fd150fd
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="azure-key-vault-storage-account-keys"></a>Chaves de conta de armazenamento do Cofre de chaves do Azure

Antes de chaves de conta do Azure chave de Cofre de armazenamento, os programadores tinham de gerir as suas próprias chaves de conta de armazenamento do Azure (ASA) e rodá-las manualmente ou através de uma automatização externa. Agora, a chave de Cofre de chaves de conta de armazenamento são implementadas como [segredos do Cofre de chaves](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) para autenticar com uma conta de armazenamento do Azure.

A funcionalidade de chave de conta de armazenamento do Azure (ASA) gere rotação secreta para si. Também remove a necessidade do contacto direto com uma chave ASA por oferta de acesso partilhado assinaturas (SAS) como um método.

Para obter mais informações sobre contas do Storage do Azure, consulte [contas do storage do Azure sobre](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

## <a name="supporting-interfaces"></a>Interfaces de suporte

Irá encontrar uma lista completa e ligações para as nossas interfaces de programação e scripts no [Guia do programador do Cofre de chave](key-vault-developers-guide.md#coding-with-key-vault).


## <a name="what-key-vault-manages"></a>O que gere o Cofre de chaves

O Cofre de chaves efetua várias funções de gestão interno em seu nome quando utilizar chaves de conta de armazenamento gerido.

- O Cofre de chaves do Azure gere as chaves de uma conta de armazenamento do Azure (ASA).
    - Internamente, o Cofre de chaves do Azure pode listar as chaves de (sincronização) com uma conta de armazenamento do Azure.
    - Gera de novo cofre de chaves do Azure (roda) periodicamente as chaves.
    - Valores de chave nunca são devolvidos em resposta ao autor da chamada.
    - O Cofre de chaves do Azure gere as chaves de contas de armazenamento e as contas de armazenamento clássico.
- O Cofre de chaves do Azure permite-lhe, o proprietário do cofre/objeto, para criar definições de SAS (conta ou o serviço SAS).
    - O valor SAS, criado com a definição de SAS, é devolvido como um segredo através do caminho URI de REST. Para obter mais informações, consulte [operações de conta de armazenamento do Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations).

## <a name="naming-guidance"></a>Orientações de nomenclatura

- Os nomes das contas do Storage devem ter entre 3 e 24 carateres de comprimento e apenas podem conter números e letras minúsculas.
- Um nome de definição de SAS tem de ser 1 102 carateres de comprimento apenas 0-9, a-z, A-Z.

## <a name="developer-experience"></a>Experiência de programação

### <a name="before-azure-key-vault-storage-keys"></a>Antes das chaves de armazenamento do Cofre de chaves do Azure

Programadores utilizados para precisa de fazer as seguintes práticas com uma chave de conta do storage para aceder ao armazenamento do Azure.
1. Armazenar a cadeia de ligação ou SAS token no outro armazenamento ou de definições da aplicação de serviço de aplicações do Azure.
1. No arranque de aplicação, obter a cadeia de ligação ou o SAS token.
1. Criar [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) para interagir com o armazenamento.

```cs
// The Connection string is being fetched from App Service application settings
var connectionStringOrSasToken = CloudConfigurationManager.GetSetting("StorageConnectionString");
var storageAccount = CloudStorageAccount.Parse(connectionStringOrSasToken);
var blobClient = storageAccount.CreateCloudBlobClient();
 ```

### <a name="after-azure-key-vault-storage-keys"></a>Depois das chaves de armazenamento do Cofre de chaves do Azure

Os programadores criar uma [KeyVaultClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient) e tirar partido do que ao obter o token SAS para o seu armazenamento. Em seguida, criar [CloudStorageAccount](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount) com esse token.

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

 ### <a name="developer-guidance"></a>Orientações para programadores

- Permitir apenas o Cofre de chaves para gerir as chaves do ASA. Não tente geri-los por si, irá interferir com processos de Cofre de chaves.
- Não permitir que as chaves do ASA para serem geridos pelo mais do que um objeto do Cofre de chaves.
- Se precisar de voltar a gerar as chaves do ASA manualmente, recomendamos que voltar a gerá-los através do Cofre de chaves.

## <a name="getting-started"></a>Introdução

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Configuração de permissões de controlo (RBAC) de acesso baseado em funções

A identidade de aplicação do Cofre de chaves do Azure necessita de permissões para *lista* e *regenerar* chaves para uma conta de armazenamento. Configure estas permissões utilizando os seguintes passos:

```powershell
# Get the resource ID of the Azure Storage Account you want to manage.
# Below, we are fetching a storage account using Azure Resource Manager
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure Key Vault Identity
$servicePrincipal = Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093

# Assign Storage Key Operator role to Azure Key Vault Identity
New-AzureRmRoleAssignment -ObjectId $servicePrincipal.Id -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope $storage.Id
```

    >[!NOTE]
    > For a classic account type, set the role parameter to *"Classic Storage Account Key Operator Service Role."*

## <a name="working-example"></a>Exemplo de trabalho

O exemplo seguinte demonstra a criar um cofre de chaves geridas conta do Storage do Azure e as definições de assinatura de acesso partilhado (SAS) associados.

### <a name="prerequisite"></a>Pré-requisito

Certifique-se de que concluiu [configurar permissões de controlo (RBAC) de acesso baseado em funções](#setup-for-role-based-access-control-rbac-permissions).

### <a name="setup"></a>Configurar

```powershell
# This is the name of our Key Vault
$keyVaultName = "mykeyVault"

# Fetching all the storage account object, of the ASA we want to manage with KeyVault
$storage = Get-AzureRmStorageAccount -ResourceGroupName "mystorageResourceGroup" -StorageAccountName "mystorage"

# Get ObjectId of Azure KeyVault Identity service principal
$servicePrincipalId = $(Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093).Id
```

Em seguida, defina as permissões **sua conta** para se certificar de que pode gerir todas as permissões de armazenamento no Cofre de chaves. No exemplo abaixo, nossa conta do Azure é  _developer@contoso.com_ .

```powershell
# Searching our Azure Active Directory for our account's ObjectId
$userPrincipalId = $(Get-AzureRmADUser -SearchString "developer@contoso.com").Id

# We use the ObjectId we found to setting permissions on the vault
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ObjectId $userPrincipalId -PermissionsToStorage all
```

### <a name="create-a-key-vault-managed-storage-account"></a>Criar um cofre de chaves gerido a conta de armazenamento

Agora, crie uma conta de armazenamento gerida no Cofre de chaves do Azure e utilizar uma chave de acesso da sua conta de armazenamento para criar os tokens SAS.
- `-ActiveKeyName` utiliza 'key2' para gerar os tokens SAS.
- `-AccountName` é utilizado para identificar a sua conta de armazenamento gerido. Abaixo, estamos a utilizar o nome da conta de armazenamento para manter simples, mas pode ser qualquer nome.
- `-DisableAutoRegenerateKey` Especifica não voltar a gerar as chaves de conta de armazenamento.

```powershell
# Adds your storage account to be managed by Key Vault and will use the access key, key2
Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $storage.StorageAccountName -AccountResourceId $storage.Id -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="key-regeneration"></a>Regeneração da chave

Se pretender que o Cofre de chaves para voltar a gerar chaves de acesso do seu armazenamento periodicamente, pode definir um período de regeneração uma vez. Abaixo, iremos estiver a definir um período de regeneração uma vez de 3 dias. Depois de 3 dias, Cofre de chaves será regenerar 'chave1' e a chave ativa 'key2' para 'chave1' de comutação.

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
$accountName = $storage.StorageAccountName

Add-AzureKeyVaultManagedStorageAccount -VaultName $keyVaultName -AccountName $accountName -AccountResourceId $storage.Id -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Definições de SAS do conjunto

A conta SAS fornece acesso ao serviço blob com permissões diferentes.
Defina as definições de SAS no Cofre de chaves para a sua conta de armazenamento gerido.
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

Obter os tokens SAS correspondentes e efetuar chamadas para o armazenamento. `-SecretName` é construído de acordo com a entrada do `AccountName` e `Name` parâmetros quando é executada [conjunto AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/en-us/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition).

```powershell
$readSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$readSasName").SecretValueText
$writeSasToken = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -SecretName "$accountName-$writeSasName").SecretValueText
```

### <a name="create-storage"></a>Criar armazenamento

Repare que tentar aceder com *$readSasToken* falhar, mas que estamos a conseguir aceder com *$writeSasToken*.

```powershell
$context1 = New-AzureStorageContext -SasToken $readSasToken -StorageAccountName $storage.StorageAccountName
$context2 = New-AzureStorageContext -SasToken $writeSasToken -StorageAccountName $storage.StorageAccountName

Set-AzureStorageBlobContent -Container containertest1 -File "abc.txt" -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "file.txt" -Context $context2
```

É capaz de acesso a conteúdo com o token SAS que tem acesso de escrita de BLOBs de armazenamento.

### <a name="relevant-powershell-cmdlets"></a>Cmdlets do Powershell relevantes

- [Get-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition)

## <a name="storage-account-onboarding"></a>Integração de conta de armazenamento

Exemplo: como proprietário do objeto um cofre de chaves que adiciona um objeto de conta de armazenamento para o seu Cofre de chaves do Azure para carregar uma conta de armazenamento.

Durante a integração, Cofre de chaves tem de verificar que a identidade da conta de integração tem permissões para *lista* e *regenerar* chaves de armazenamento. Para verificar a estas permissões, o Cofre de chaves obtém um OBO (no nome de) token do serviço de autenticação, o público-alvo definido para o Azure Resource Manager e faz com que um *lista* chave chamada para o serviço de armazenamento do Azure. Se o *lista* chamada falhar, o Cofre de chave de criação do objecto falha com um código de estado HTTP de *proibido*. As chaves listadas neste modo, são colocadas em cache com o armazenamento de entidade do Cofre de chaves.

O Cofre de chaves tem de verificar que a identidade tem *regenerar* permissões antes-pode assumir a propriedade de regenerar as chaves. Para verificar que a identidade, através do token OBO, bem como a identidade de terceiros primeiro do Cofre de chaves com estas permissões:

- O Cofre de chaves apresenta uma lista de permissões de RBAC no recurso de conta de armazenamento.
- O Cofre de chaves valida a resposta através de expressão regular de correspondência de ações e não as ações.

Localizar alguns exemplos de suporte em [Cofre de chaves - geridos amostras de chaves de conta de armazenamento](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Se não tiver a identidade *regenerar* permissões ou se a identidade de terceiros primeiro Cofre de chaves não tiver *lista* ou *regenerar* falha devolver um código de erro apropriada e a mensagem de pedido de permissão, em seguida, a integração.

O token OBO funcionará apenas quando utilizar originais, aplicações de cliente nativo do PowerShell ou a CLI.

## <a name="other-applications"></a>Outras aplicações

- Tokens SAS, construído de acordo com as chaves de conta de armazenamento do Cofre de chaves, fornecem acesso controlado ainda mais a uma conta de armazenamento do Azure. Para obter mais informações, consulte [utilizar assinaturas de acesso partilhado](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Consulte também

- [Sobre chaves, segredos e certificados](https://docs.microsoft.com/rest/api/keyvault/)
- [Blogue da equipa do Cofre de chaves](https://blogs.technet.microsoft.com/kv/)
