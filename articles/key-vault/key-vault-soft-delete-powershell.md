---
ms.assetid: ''
title: O Azure Key Vault - como utilizar a eliminação de forma recuperável com o PowerShell
description: Utilize os exemplos de casos de eliminação de forma recuperável com recortes de código do PowerShell
services: key-vault
author: bryanla
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bryanla
ms.openlocfilehash: 174a5b41e6a48ea74cd813746b7c070463a8185b
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/14/2018
ms.locfileid: "42054151"
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>Como utilizar a eliminação de forma recuperável do Key Vault com o PowerShell

Funcionalidade de eliminação de forma recuperável do Azure do Key Vault permite que a recuperação de cofres eliminados e os objetos de cofre. Especificamente, eliminação de forma recuperável endereços os seguintes cenários:

- Suporte para eliminação recuperável de um cofre de chaves
- Suporte para eliminação recuperável de objetos de Cofre de chaves; as chaves, segredos e, em certificados

## <a name="prerequisites"></a>Pré-requisitos

- O Azure PowerShell 4.0.0 ou superior - se não tiver já neste programa de configuração, instalar o Azure PowerShell e associá-lo a sua subscrição do Azure, veja [como instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview). 

>[!NOTE]
> Existe uma versão desatualizada do nosso formatação de saída do PowerShell do Cofre de chaves de ficheiro que **poderá** ser carregados para o seu ambiente em vez da versão correta. Nós são prevendo uma versão atualizada do PowerShell para conter a correção necessária para a formatação de saída e atualizaremos este tópico nesse momento. A solução atual, caso encontre esse problema de formatação, é:
> - Utilize a seguinte consulta se notar que não está a ver a eliminação de forma recuperável ativada propriedade descrita neste tópico: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.


Para informações de refernece específico do Cofre de chaves para o PowerShell, consulte [referência do PowerShell do Azure Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

## <a name="required-permissions"></a>Permissões obrigatórias

Operações do Cofre de chave separadamente geridas através de permissões de controlo (RBAC) de acesso baseado em função da seguinte forma:

| Operação | Descrição | Permissão de utilizador |
|:--|:--|:--|
|Lista|Excluídas listas de cofres de chaves.|Microsoft.KeyVault/deletedVaults/read|
|Recuperar|Restaura um cofre de chaves eliminado.|Microsoft.KeyVault/vaults/write|
|Remover|Remove permanentemente a um cofre de chaves eliminado e todos os respetivos conteúdos.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Para obter mais informações sobre o controlo de acesso e permissões, consulte [proteger o seu Cofre de chave](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Ativar a eliminação de forma recuperável

Para poder recuperar um cofre de chaves eliminado ou objetos armazenados num cofre de chaves, primeiro tem de ativar eliminação de forma recuperável para esse cofre de chaves.

### <a name="existing-key-vault"></a>Cofre de chaves existente

Para um cofre de chaves existente com o nome ContosoVault, ative a eliminação de forma recuperável da seguinte forma. 

>[!NOTE]
>Atualmente tem de utilizar a manipulação de recursos do Azure Resource Manager para escrever diretamente a *enableSoftDelete* propriedade para o recurso do Key Vault.

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Novo cofre de chaves

Ativar a eliminação de forma recuperável para um novo cofre de chaves é feita no momento da criação ao adicionar o sinalizador de ativação de eliminação de forma recuperável para o comando de criação.

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Certifique-se a ativação da eliminação de forma recuperável

Para verificar que um cofre de chaves tenha a eliminação de forma recuperável ativada, execute o *obter* de comandos e procure o "Soft eliminar Enabled?" atributo e sua definição, verdadeira ou falsa.

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>A eliminar um cofre de chaves protegido por eliminação de forma recuperável

O comando para eliminar (ou remover) um cofre de chaves permanece o mesmo, mas seu comportamento muda dependendo de ter habilitado a eliminação de forma recuperável ou não.

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
>Se executar o comando anterior para um cofre de chaves que não tenha a eliminação de forma recuperável ativada, irá eliminar permanentemente este Cofre de chaves e todo o seu conteúdo sem qualquer opções para recuperação.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Como a eliminação de forma recuperável protege seus cofres de chaves

Com a eliminação de forma recuperável ativada:

- Quando é eliminado um cofre de chaves, é removido do seu grupo de recursos e colocado num espaço de nomes reservado que só está associado à localização onde foi criado. 
- Objetos numa chave eliminado cofre, como chaves, segredos e certificados, estão inacessíveis e continuam assim, enquanto o seu Cofre de chaves que contêm está no Estado eliminado. 
- O nome DNS para um cofre de chaves num Estado eliminado está ainda reservado, portanto, não é possível criar um novo cofre de chaves com o mesmo nome.  

Pode visualizar cofres de chaves de estado de eliminado, associados à subscrição, utilizando o seguinte comando:

```powershell
PS C:\> Get-AzureRmKeyVault -InRemovedStateVault 

Name           : ContosoVault
Location             : westus
Id                   : /subscriptions/xxx/providers/Microsoft.KeyVault/locations/westus/deletedVaults/ContosoVault
Resource ID          : /subscriptions/xxx/resourceGroups/ContosoVault/providers/Microsoft.KeyVault/vaults/ContosoVault
Deletion Date        : 5/9/2017 12:14:14 AM
Scheduled Purge Date : 8/7/2017 12:14:14 AM
Tags                 :
```

O *ID de recurso* na saída refere-se para o ID de recurso original deste cofre. Uma vez que este Cofre de chaves está agora num Estado eliminado, nenhum recurso existe com esse ID de recurso. O *Id* campo pode ser utilizado para identificar o recurso quando recuperar ou remoção. O *data de remoção agendada* campo indica quando o Cofre será permanentemente eliminado (removidas) se foi efetuada nenhuma ação para este cofre eliminado. O período de retenção padrão, utilizado para calcular a *data de remoção agendada*, é de 90 dias.

## <a name="recovering-a-key-vault"></a>Recuperar um cofre de chaves

Para recuperar um cofre de chaves, terá de especificar o nome do Cofre de chaves, o grupo de recursos e a localização. Tenha em atenção a localização e o grupo de recursos do Cofre de chaves eliminado conforme vai precisar de ambos para um processo de recuperação do Cofre de chaves.

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Quando um cofre de chaves é recuperado, o resultado é um novo recurso com original ID de recurso. o Cofre de chaves Se tiver sido removido do grupo de recursos em que o Cofre de chaves existente, um novo grupo de recursos com o mesmo nome deve ser criado antes do Cofre de chaves que pode ser recuperado.

## <a name="key-vault-objects-and-soft-delete"></a>Objetos de Cofre de chaves e eliminação de forma recuperável

Para uma chave, "ContosoFirstKey", num cofre de chaves com o nome "ContosoVault' com a eliminação de forma recuperável ativada, aqui como iria eliminar essa chave.

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

Com o seu Cofre de chaves ativado para eliminação de forma recuperável, uma chave eliminada ainda é apresentado como este é eliminado, exceto, quando listar ou obter chaves eliminadas explicitamente. A maioria das operações numa chave no Estado eliminado irá falhar, exceto para a listagem de uma chave de eliminado, recuperá-lo ou removê-lo. 

Por exemplo, para pedir às chaves de lista eliminada num cofre de chaves, utilize o seguinte comando:

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Estado de transição 

Quando elimina uma chave no Cofre de chaves com eliminação de forma recuperável ativada, poderá demorar alguns segundos para a transição para concluir. Durante este estado de transição, que pode parecer que a chave não está no Estado ativo ou estado eliminado. Este comando irá listar todas as chaves eliminadas no seu Cofre de chaves com o nome 'ContosoVault'.

```powershell
  Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
  Vault Name           : ContosoVault
  Name                 : ContosoFirstKey
  Id                   : https://ContosoVault.vault.azure.net:443/keys/ContosoFirstKey
  Deleted Date         : 2/14/2017 8:20:52 PM
  Scheduled Purge Date : 5/15/2017 8:20:52 PM
  Enabled              : True
  Expires              :
  Not Before           :
  Created              : 2/14/2017 8:16:07 PM
  Updated              : 2/14/2017 8:16:07 PM
  Tags                 :
```

### <a name="using-soft-delete-with-key-vault-objects"></a>Utilizar a eliminação de forma recuperável com objetos de Cofre de chaves

Tais como cofres de chaves, uma chave de eliminado, segredo ou certificado irá permanecer no estado de eliminado durante 90 dias, a menos que recuperá-la ou removê-lo. 

#### <a name="keys"></a>Chaves

Para recuperar uma chave eliminada:

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

Eliminar permanentemente uma chave:

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
>Remover uma chave eliminará permanentemente, que significa que não serão recuperável.

O **recuperar** e **remover** ações têm suas próprias permissões associadas a uma política de acesso do Cofre de chaves. Para um utilizador ou principal de serviço poder executar uma **recuperar** ou **remover** ação têm de ter a respetiva permissão para esse objeto (chave ou segredo) na política de acesso do Cofre de chaves. Por predefinição, o **remover** permissão não é adicionada à política de acesso de um cofre de chaves, quando o atalho "todos" é usado para conceder todas as permissões a um utilizador. Tem de conceder explicitamente **remover** permissão. Por exemplo, o seguinte comando concede user@contoso.com permissão para executar diversas operações de chaves numa *ContosoVault* incluindo **remover**.

#### <a name="set-a-key-vault-access-policy"></a>Definir uma política de acesso do Cofre de chaves

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Se tiver um cofre de chaves que apenas tenha tido a eliminação de forma recuperável ativada, pode não ter **recuperar** e **remover** permissões.

#### <a name="secrets"></a>Segredos

Como chaves, segredos num cofre de chaves são utilizados com seus próprios comandos. A seguir, são os comandos para a eliminar, listar, recuperar e remover os segredos.

- Elimine um segredo designado SQLPassword: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Liste eliminados todos os segredos num cofre de chaves: 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Recupere um segredo no Estado eliminado: 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Remover um segredo no estado de eliminado: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
>Remover um segredo eliminará permanentemente, que significa que não serão recuperável.

## <a name="purging-and-key-vaults"></a>Cofres de chaves e limpeza

### <a name="key-vault-objects"></a>Objetos de Cofre de chaves

Remover uma chave, segredo ou certificado vai eliminar permanentemente, que significa que não serão recuperável. O Cofre de chaves que continha o objeto excluído entretanto permanecerão intacto assim como todos os outros objetos no Cofre de chaves. 

### <a name="key-vaults-as-containers"></a>Cofres de chaves como contentores
Quando um cofre de chaves é removido, todos os respetivos conteúdos, incluindo chaves, segredos e certificados, são eliminados permanentemente. Para remover um cofre de chaves, utilize o `Remove-AzureRmKeyVault` comando com a opção `-InRemovedState` e ao especificar a localização do Cofre de chaves eliminado com o `-Location location` argumento. Pode encontrar a localização de um cofre eliminado com o comando `Get-AzureRmKeyVault -InRemovedState`.

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
>Remover um cofre de chaves vai eliminar permanentemente, que significa que não serão recuperável.

### <a name="purge-permissions-required"></a>Remover as permissões necessárias
- Para remover um cofre de chaves eliminado de forma a que o Cofre e todos os conteúdos são removidos permanentemente, o utilizador precisa de permissão RBAC para efetuar uma *Microsoft.KeyVault/locations/deletedVaults/purge/action* operação. 
- Para listar a chave de eliminado, o Cofre de um utilizador tem permissão de RBAC para efetuar *Microsoft.KeyVault/deletedVaults/read* permissão. 
- Por predefinição, apenas um administrador de subscrição tem estas permissões. 

### <a name="scheduled-purge"></a>Remoção agendada

A listagem de seus objetos de Cofre de chaves eliminado mostra quando eles estiverem schedled remoção com regularidade pelo Key Vault. O *data de remoção agendada* campo indica quando um objeto do Cofre de chaves serão permanentemente eliminado, se foi efetuada nenhuma ação. Por predefinição, o período de retenção para um objeto do Cofre de chaves eliminado é de 90 dias.

>[!NOTE]
>Um objeto de cofre eliminados, acionado por seus *data de remoção agendada* campo, é eliminado permanentemente. Não é recuperável.

## <a name="other-resources"></a>Outros recursos

- Para uma descrição geral da funcionalidade de eliminação de forma recuperável do Key Vault, consulte [descrição geral da eliminação de forma recuperável do Azure Key Vault](key-vault-ovw-soft-delete.md).
- Para obter uma visão geral da utilização do Azure Key Vault, consulte [introdução ao Azure Key Vault](key-vault-get-started.md).

