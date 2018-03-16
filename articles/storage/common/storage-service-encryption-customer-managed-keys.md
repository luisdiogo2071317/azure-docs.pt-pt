---
title: "Encriptação do serviço de armazenamento do Azure gerida pelo cliente chaves a utilizar no Cofre de chaves do Azure | Microsoft Docs"
description: "Utilizar a funcionalidade de encriptação do serviço do Storage do Azure para encriptar o armazenamento de Blobs do Azure no lado do serviço ao armazenar os dados e desencriptá-lo ao obter os dados de utilização de chaves gerida pelo cliente."
services: storage
author: lakasa
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 03/07/2018
ms.author: lakasa
ms.openlocfilehash: 1360d8bb0911c424747209c69b830fc1ee461798
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Encriptação do serviço de armazenamento gerida pelo cliente chaves a utilizar no Cofre de chaves do Azure

Microsoft Azure está empenhada em ajudar a proteger e a salvaguardar os seus dados para satisfazer os seus compromissos de conformidade e segurança organizacional. Uma forma que o armazenamento do Azure protege os dados é através de armazenamento serviço encriptação (SSE), que encripta os dados ao escrever-a para o armazenamento e desencripta os dados ao obtê-lo. A encriptação e desencriptação é automática, transparente e utiliza 256 bits [encriptação AES](https://wikipedia.org/wiki/Advanced_Encryption_Standard), um bloco de maior cifras disponíveis.

Pode utilizar as chaves de encriptação gerida pela Microsoft com SSE ou pode utilizar as suas próprias chaves de encriptação. Este artigo descreve como utilizar as suas próprias chaves de encriptação. Para obter mais informações sobre a utilização de chaves gerida pela Microsoft, ou SSE no geral, consulte [encriptação do serviço de armazenamento para dados Inativos](storage-service-encryption.md).

SSE para armazenamento de Blob e o ficheiro está integrado com o Cofre de chaves do Azure, para que possa utilizar um cofre de chaves para gerir as chaves de encriptação. Pode criar as suas próprias chaves de encriptação e armazená-las num cofre de chaves, ou pode utilizar os APIs do Cofre de chaves do Azure para gerar chaves de encriptação. Com o Cofre de chaves do Azure, pode gerir e controlar as chaves e também a utilização da chave de auditoria.

Porquê criar as suas próprias chaves? Chaves personalizadas dão-lhe uma maior flexibilidade, para que possam criar, rodar, desativar e definir os controlos de acesso. Chaves personalizadas também permitem-lhe auditar chaves de encriptação utilizadas para proteger os seus dados.

## <a name="get-started-with-customer-managed-keys"></a>Começar a utilizar chaves gerida pelo cliente

Para utilizar chaves gerida pelo cliente com SSE, pode criar um novo cofre de chaves e chave ou utilizar um cofre de chaves e a chave. A conta de armazenamento e o Cofre de chaves têm de ser na mesma região, mas podem estar em diferentes subscrições. 

### <a name="step-1-create-a-storage-account"></a>Passo 1: Criar uma conta de armazenamento

Em primeiro lugar, crie uma conta de armazenamento se ainda não tiver um. Para obter mais informações, consulte [criar uma nova conta de armazenamento](storage-quickstart-create-account.md).

### <a name="step-2-enable-sse-for-blob-and-file-storage"></a>Passo 2: Ativar SSE para armazenamento de BLOBs e ficheiro

Para ativar a utilização de chaves gerida pelo cliente de SSE, duas funcionalidades de proteção por chave, eliminar de forma recuperável e fazer remover, também devem ser ativadas. Estas definições Certifique-se que as chaves não podem ser de forma acidental ou intencionalmente eliminados. O período de retenção máximo das chaves está definido como 90 dias, os utilizadores relativamente atores maliciosos ou ataques de ransomware a proteger.

Se pretender ativar programaticamente chaves gerida pelo cliente para SSE, pode utilizar o [API de REST do fornecedor de recursos do Azure Storage](https://docs.microsoft.com/rest/api/storagerp), a [biblioteca cliente de fornecedor de recursos do armazenamento para .NET](https://docs.microsoft.com/dotnet/api), [ O Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), ou o [CLI do Azure](https://docs.microsoft.com/azure/storage/storage-azure-cli).

Para utilizar chaves gerida pelo cliente com SSE, tem de atribuir uma identidade de conta de armazenamento para a conta de armazenamento. Pode definir a identidade executando o seguinte comando do PowerShell:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName \$resourceGroup -Name \$accountName -AssignIdentity
```

Pode ativar a eliminar de forma recuperável e não fazer remover executando os seguintes comandos do PowerShell:

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enableSoftDelete -Value 'True'

Set-AzureRmResource -resourceid $resource.ResourceId -Properties
$resource.Properties

($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enablePurgeProtection -Value 'True'

Set-AzureRmResource -resourceid $resource.ResourceId -Properties
$resource.Properties
```

### <a name="step-3-enable-encryption-with-customer-managed-keys"></a>Passo 3: Ativar a encriptação com chaves gerida pelo cliente

Por predefinição, SSE utiliza chaves gerida pela Microsoft. Pode ativar SSE com chaves gerida pelo cliente para a conta de armazenamento utilizando o [portal do Azure](https://portal.azure.com/). No **definições** painel para a conta de armazenamento, clique em **encriptação**. Selecione o **utilizar a sua própria chave** opção, conforme mostrado na figura seguinte.

![Portal captura de ecrã com a opção de encriptação](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

### <a name="step-4-select-your-key"></a>Passo 4: Selecione a sua chave

Pode especificar a chave como um URI ou selecionando a chave de um cofre de chaves.

#### <a name="specify-a-key-as-a-uri"></a>Especifique uma chave como um URI

Para especificar a chave de um URI, siga estes passos:

1. Escolha o **tecla Enter URI** opção.  
2. No **URI chave** campo, especifique o URI.

    ![Captura de ecrã Portal, com encriptação com introduzir uri chave opção](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)


#### <a name="specify-a-key-from-a-key-vault"></a>Especifique uma chave de um cofre de chaves 

Para especificar a chave de um cofre de chaves, siga estes passos:

1. Escolha o **selecione do Cofre de chaves** opção.  
2. Escolha o Cofre de chaves que contém a chave que pretende utilizar.
3. Escolha a chave do Cofre de chaves.

    ![Utilizam o portal encriptações de captura de ecrã com a suas próprias chave opção](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Se a conta de armazenamento não tem acesso ao Cofre de chaves, pode executar o comando do PowerShell do Azure mostrado na imagem seguinte para conceder acesso.

![Captura de ecrã portal com acesso negado para o Cofre de chaves](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Também pode conceder o acesso através do portal do Azure ao navegar para o Cofre de chaves do Azure no portal do Azure e conceder acesso à conta de armazenamento.


Pode associar a chave acima com uma conta de armazenamento existente usando os seguintes comandos do PowerShell:
```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount"
$keyVault = Get-AzureRmKeyVault -VaultName "mykeyvault"
$key = Get-AzureKeyVaultKey -VaultName $keyVault.VaultName -Name "keytoencrypt"
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVault.VaultName -ObjectId $storageAccount.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
Set-AzureRmStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -AccountName $storageAccount.StorageAccountName -EnableEncryptionService "Blob" -KeyvaultEncryption -KeyName $key.Name -KeyVersion $key.Version -KeyVaultUri $keyVault.VaultUri
```


### <a name="step-5-copy-data-to-storage-account"></a>Passo 5: Copiar dados para a conta de armazenamento

Para transferir dados para a sua nova conta de armazenamento, de modo a que está encriptada, consulte o passo 3 de [introdução na encriptação do serviço de armazenamento para dados Inativos](storage-service-encryption.md#step-3-copy-data-to-storage-account).

### <a name="step-6-query-the-status-of-the-encrypted-data"></a>Passo 6: Consultar o estado dos dados encriptados

Para consultar o estado dos dados encriptados, consulte o passo 4 da [introdução na encriptação do serviço de armazenamento para dados Inativos](storage-service-encryption.md#step-4-query-the-status-of-the-encrypted-data).

## <a name="faq-for-sse-with-customer-managed-keys"></a>FAQ acerca de SSE com chaves cliente gerido

**P: Posso estou a utilizar o Premium storage; Pode utilizar as chaves gerida pelo cliente com SSE?**

R: Sim, SSE com chaves gerida pela Microsoft e gerida pelo cliente é suportada no armazenamento Standard e Premium storage.

**P: Posso criar novas contas de armazenamento com SSE com chaves gerida pelo cliente ativadas com o Azure PowerShell e da CLI do Azure?**

R: Sim.

**P: quanto mais armazenamento do Azure custo se utilizar chaves gerida pelo cliente com SSE?**

R: não há um custo associado para utilizar o Cofre de chaves do Azure. Para obter mais detalhes, visite [preços do Cofre de chave](https://azure.microsoft.com/pricing/details/key-vault/). Não há sem custos adicionais de SSE, que está ativado para todas as contas de armazenamento.

**P: posso revogar o acesso às chaves de encriptação?**

R: Sim, pode revogar o acesso em qualquer altura. Existem várias formas para revogar o acesso às suas chaves. Consulte [PowerShell do Cofre de chaves do Azure](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) e [CLI do Azure chave de cofre](https://docs.microsoft.com/cli/azure/keyvault) para obter mais detalhes. Revogar o acesso eficazmente irá bloquear o acesso a todos os blobs na conta de armazenamento como a chave de encriptação da conta não está acessível ao Storage do Azure.

**P: Posso criar uma conta de armazenamento e a chave numa região diferente?**

R: não, a conta de armazenamento e o Cofre de chaves do Azure e a chave tem de estar na mesma região.

**P: posso ativar chaves gerida pelo cliente para SSE ao criar a conta de armazenamento?**

R: um número de Quando cria pela primeira vez que a conta de armazenamento, gerida pela Microsoft chaves só estão disponíveis para SSE. Para usar chaves gerida pelo cliente, tem de atualizar as propriedades da conta de armazenamento. Pode utilizar REST ou uma das bibliotecas de cliente de armazenamento para atualizar programaticamente a sua conta de armazenamento ou atualizar as propriedades da conta de armazenamento no portal do Azure depois de criar a conta.

**P: pode desativar a encriptação durante a utilização de chaves gerida pelo cliente com SSE?**

R: não, não é possível desativar a encriptação. Encriptação está ativada por predefinição para todos os serviços – Blob, ficheiro, tabela e fila de armazenamento. Opcionalmente, pode mudar da utilização de chaves gerida pela Microsoft para utilização de chaves gerida pelo cliente e vice-versa.

**P: é SSE ativada por predefinição, quando criar uma nova conta de armazenamento?**

R: SSE está ativada por predefinição para todas as contas de armazenamento e para todos os serviços – Blob, ficheiro, tabela e fila de armazenamento.

**P: Posso não é possível ativar SSE utilizando chaves gerida pelo cliente na minha conta de armazenamento.**

R: é uma conta do storage do Azure Resource Manager? Contas de armazenamento clássicas não são suportadas com chaves gerida pelo cliente. SSE com chaves gerida pelo cliente só pode ser ativada em contas de armazenamento do Resource Manager.

**P: qual é eliminar de forma recuperável e não fazer remover? É necessário ativar esta definição para utilizar SSE com chaves gerida pelo cliente**

R: soft eliminar e não fazer remover tem de estar ativado para utilizar SSE com chaves gerida pelo cliente. Estas definições Certifique-se de que a chave não forma acidental ou intencionalmente eliminados. O período de retenção máximo das chaves está definido como 90 dias, os utilizadores relativamente atores maliciosos e ataques de ransomware a proteger. Não é possível desativar esta definição.

**P: é SSE com chaves gerida pelo cliente só é permitidas em regiões específicas?**

R: SSE com chaves gerida pelo cliente está disponível em todas as regiões para armazenamento de Blob e o ficheiro.

**P: como posso o contacto certo se posso ter algum problema ou quiser fornecer comentários?**

R: contacto [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) para quaisquer problemas relacionados com a encriptação do serviço de armazenamento.

## <a name="next-steps"></a>Passos seguintes

-   Para mais informações sobre o conjunto completo de segurança capacidades que ajudam os programadores criem aplicações seguras, consulte o [manual de segurança de armazenamento](storage-security-guide.md).

-   Para obter informações gerais sobre o Cofre de chaves do Azure, consulte [que é o Cofre de chaves do Azure](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)?

-   Para começar a trabalhar no Cofre de chaves do Azure, consulte [introdução ao Cofre de chaves do Azure](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).
