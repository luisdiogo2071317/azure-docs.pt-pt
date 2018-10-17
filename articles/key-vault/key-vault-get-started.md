---
title: Introdução ao Cofre de Chaves do Azure | Microsoft Docs
description: Utilize este tutorial para ajudá-lo com o Cofre de Chaves do Azure para criar um contentor protegido no Azure para armazenar e gerir chaves criptográficas e segredos no Azure.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: barclayn
ms.openlocfilehash: b5e235221d34434b908d605c9077ae2fede2fe94
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365624"
---
# <a name="get-started-with-azure-key-vault"></a>Introdução Cofre de Chaves do Azure
Este artigo ajuda-o a começar a utilizar o Azure Key Vault com o PowerShell e orienta-o ao longo das atividades seguintes:
- Como criar um contentor protegido (cofre) no Azure.
- Como utilizar o Key Vault para armazenar e gerir chaves criptográficas e segredos no Azure.
- Como uma aplicação pode utilizar essa chave ou palavra-passe.

O Cofre de Chaves do Azure chave está disponível na maior parte das regiões. Para obter mais informações, consulte a [página de preços do Cofre de Chaves](https://azure.microsoft.com/pricing/details/key-vault/).

Para obter instruções sobre a Interface de Linha de Comandos de Várias Plataformas, veja [este tutorial equivalente](key-vault-manage-with-cli2.md).

## <a name="requirements"></a>Requisitos
Antes de continuar, confirme que tem:

- **Uma subscrição do Azure**. Se não tiver uma conta, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/free/).
- **Azure PowerShell**, **com a versão mínima 1.1.0**. Para instalar o Azure PowerShell e associá-lo à sua subscrição do Azure, consulte o artigo [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview). Se já tiver instalado o Azure PowerShell e não souber a versão, a partir da consola do Azure PowerShell, introduza `(Get-Module azure -ListAvailable).Version`. Quando tiver instalado as versões 0.9.1 a 0.9.8 do Azure PowerShell, pode continuar a utilizar neste tutorial com algumas pequenas alterações. Por exemplo, tem de utilizar o comando `Switch-AzureMode AzureResourceManager` e alguns dos comandos do Cofre de Chaves do Azure foram alterados. Para obter uma lista dos cmdlets do Cofre de Chaves para versões 0.9.1 a 0.9.8, consulte o artigo [Cmdlets do Cofre de Chaves do Azure](/powershell/module/azurerm.keyvault/#key_vault).
- **Uma aplicação que pode ser configurada para utilizar o Key Vault**. Uma aplicação de exemplo está disponível a partir do [Centro de Transferências da Microsoft](http://www.microsoft.com/download/details.aspx?id=45343). Para obter instruções, veja o ficheiro **Readme**.

>[!NOTE]
Este artigo pressupõe uma compreensão básica do PowerShell e do Azure. Para obter mais informações sobre o PowerShell, veja a [Getting started with Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx) (Introdução ao Windows PowerShell).

Para obter ajuda detalhada para qualquer cmdlet que visualiza neste tutorial, utilize o cmdlet **Get-Help**.

```powershell-interactive
Get-Help <cmdlet-name> -Detailed
```
    
Por exemplo, para obter ajuda para o cmdlet **Connect-AzureRmAccount** introduza:

```PowerShell
Get-Help Connect-AzureRmAccount -Detailed
```

Pode ainda ler os seguintes artigos para se familiarizar com o modelo de implementação Azure Resource Manager no Azure PowerShell:

* [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview)
* [Utilizar o Azure PowerShell com o Resource Manager](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Ligar às suas subscrições
Abra uma sessão no Azure PowerShell e inicie sessão na sua conta do Azure com o seguinte comando:  

```PowerShell
Connect-AzureRmAccount
```

>[!NOTE]
 Se estiver a utilizar uma instância específica do Azure, utilize o parâmetro -Environment. Por exemplo: 
 ```powershell
 Connect-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
 ```

Na janela pop-up do browser, introduza o seu nome de utilizador da conta do Azure e a palavra-passe. O Azure PowerShell obtém todas as subscrições associadas a esta conta e, por defeito, utiliza a primeira.

Se tiver várias subscrições e pretender especificar uma subscrição particular para utilizar com o Cofre de Chaves do Azure, introduza o seguinte para ver as subscrições da sua conta:

```powershell
Get-AzureRmSubscription
```

Em seguida, para especificar a subscrição a utilizar, introduza:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

Para obter mais informações sobre como configurar o Azure PowerShell, veja [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/overview).

## <a id="resource"></a>Criar um novo grupo de recursos
Quando utilizar o Azure Resource Manager, todos os recursos relacionados são criados no interior de um grupo de recursos. Para este tutorial, iremos criar um novo grupo de recursos designado **ContosoResourceGroup** :

```powershell
New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East US'
```

## <a id="vault"></a>Criar um cofre de chaves
Utilize o cmdlet [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) para criar um cofre de chaves. Este cmdlet tem três parâmetros obrigatórios: um **nome do grupo de recursos**, um **nome do cofre de chaves** e a **localização geográfica**.

Por exemplo, se utilizar:
- **ContosoKeyVault** como o nome do cofre.
- **ContosoResourceGroup** como o nome do grupo de recursos.
- **E.U.A. Leste** como a localização.

terá de escrever:

```powershell
New-AzureRmKeyVault -Name 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```
![Resultado após a conclusão do comando creation do Key Vault](./media/key-vault-get-started/output-after-creating-keyvault.png)

O resultado deste cmdlet mostra as propriedades do cofre de chaves que criou. As duas propriedades mais importantes são:

* **Nome do cofre**: **ContosoKeyVault** no exemplo. Irá utilizar este nome para outros cmdlets do Cofre de Chaves.
* **URI do cofre**: https://contosokeyvault.vault.azure.net/ no exemplo. As aplicações que utilizam o cofre através da respetiva API têm de utilizar este URI.

A sua conta do Azure pode agora realizar quaisquer operações neste cofre de chaves. Por enquanto, não foi autorizado mais ninguém.

> [!NOTE]
> Quando tenta criar o cofre de chaves novo, poderá ver o erro **A subscrição não está registada para utilizar o espaço de nomes 'Microsoft.KeyVault'**. Se esta mensagem for apresentada, execute `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"`. Após a conclusão bem-sucedida do registo, pode voltar a executar o comando New-AzureRmKeyVault. Para obter mais informações, veja [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider).
>
>

## <a id="add"></a>Adicionar uma chave ou segredo ao cofre de chaves
Existem duas formas diferentes através das quais pode interagir com o Key Vault e as chaves ou os segredos.

### <a name="azure-key-vault-generates-a-software-protected-key"></a>O Azure Key Vault gera uma chave protegida por software

Se pretender que o Azure Key Vault crie uma chave protegida por software por si, utilize o cmdlet [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) e escreva:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'
```
para ver o URI desta chave, escreva:
```powershell
$key.id
```

Pode referenciar uma chave que criou ou carregou para o Azure Key Vault com o URI da mesma. Para obter a versão atual, pode usar **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** e utilize **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** para obter esta versão específica.  

### <a name="importing-an-existing-pfx-file-into-azure-key-vault"></a>Importar um ficheiro PFX existente para o Azure Key Vault

Se as chaves existentes estão armazenadas num ficheiro pfx que pretende carregar para o Azure Key Vault, os passos são diferentes. Por exemplo:
- Se tiver uma chave protegida por software já existente num ficheiro PFX
- O ficheiro pfx tem o nome softkey.pfx 
- O ficheiro é armazenado na unidade C.

Pode escrever:

```powershell
$securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force  // This stores the password 123 in the variable $securepfxpwd
```

Em seguida, escreva o seguinte para importar a chave do ficheiro .PFX, que protege a chave por software no serviço do Cofre de Chaves:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoImportedPFX' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd
```

Para apresentar o URI para esta chave, escreva:

```powershell
$Key.id
```
Para ver a sua chave, escreva: 

```powershell
Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'
```
Se quiser ver as propriedades do ficheiro PFX no portal, verá algo semelhante à imagem abaixo.

![Aspeto de um certificado no portal](./media/key-vault-get-started/imported-pfx.png)

### <a name="to-add-a-secret-to-azure-key-vault"></a>Para adicionar um segredo ao Azure Key Vault

Para adicionar um segredo ao cofre, que é uma palavra-passe designada SQLPassword e tem o valor Pa$$w0rd, ao Azure Key Vault, comece por converter o valor Pa$$w0rd numa cadeia segura ao escrever:

```powershell    
$secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force
```

Em seguida, escreva:

```powershell
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue
```


Agora, pode referenciar esta palavra-passe que adicionou ao Cofre de Chaves do Azure utilizando o seu respetivo URI. Utilizar **https://ContosoVault.vault.azure.net/secrets/SQLPassword** para obter sempre a versão atual e utilizar **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** para obter esta versão específica.

Para apresentar o URI para este segredo, escreva:

```powershell
$secret.Id
```
Para ver o seu segredo, escreva `Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'` ou, em alternativa, pode ver o segredo no portal.

![segredo](./media/key-vault-get-started/secret-value.png)

Para ver o valor contido no segredo como texto simples:
```powershell
(get-azurekeyvaultsecret -vaultName "Contosokeyvault" -name "SQLPassword").SecretValueText
```
Agora, o Cofre de chaves e chave ou segredo estão prontos para aplicações para utilizar. Agora autoriza as aplicações a utilizá-los.  

## <a id="register"></a>Registar uma aplicação com o Azure Active Directory
Geralmente, este passo deve ser realizado por um programador, num computador à parte. Não é específico do Azure Key Vault. Para obter passos detalhados sobre o registo de aplicações no Azure Active Directory, consulte o artigo intitulado [integrar aplicações com o Azure Active Directory](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) ou [utilize o portal para criar um Azure Active Directory principal de serviço que pode aceder aos recursos e aplicações](../azure-resource-manager/resource-group-create-service-principal-portal.md)

> [!IMPORTANT]
> Para concluir o tutorial, a sua conta, o cofre e a aplicação que irá registar neste passo devem todos estar no mesmo diretório do Azure.


As aplicações que utilizam um cofre de chaves devem ser autenticadas através de um token do Azure Active Directory. O proprietário do aplicativo em primeiro lugar tem de registar a aplicação no seu Azure Active Directory. No final do registo, o proprietário da aplicação obtém os seguintes valores:

- **ID da Aplicação** 
- **Chave de autenticação** (também conhecida como segredo partilhado). 

Para obter um token, a aplicação deve apresentar ambos os valores ao Azure Active Directory. A configuração de aplicativo depende do aplicativo. Na [aplicação de exemplo do Key Vault](https://www.microsoft.com/download/details.aspx?id=45343), o proprietário da aplicação define estes valores no ficheiro app.config.


Registar uma aplicação no Azure Active Directory:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No lado esquerdo, clique em **Registos de aplicações**. Se não vir registos de aplicações, clique em **mais serviços**.  
>[!NOTE]
Tem de selecionar o mesmo diretório que contém a subscrição do Azure com a qual criou o seu cofre de chaves. 
3. Clique em **Novo registo de aplicação**.
4. No painel **Criar**, indique um nome para a aplicação e selecione **APLICAÇÃO WEB E/OU API WEB** (a predefinição) e especifique o **URL DE INÍCIO DE SESSÃO** da sua aplicação Web. Se não tiver estas informações neste momento, pode inventá-las para este passo (por exemplo, pode indicar http://test1.contoso.com). Não interessa se estes sites existem. 

    ![Novo registo de aplicação](./media/key-vault-get-started/new-application-registration.png)
    >[!WARNING]
    Confirme que escolhe **APLICAÇÃO WEB E/OU API WEB**; se não o tiver feito, não verá a opção **chaves** nas definições.

5. Clique no botão **Criar**.
6. Quando o registo de aplicação estiver concluído, verá a lista de aplicações registadas. Localize a aplicação que registou e clique no mesmo.
7. Clique no painel **Aplicação registada** e copie o **ID de Aplicação**
8. Clique em **Todas as definições**
9. No painel **Definições**, clique em **Chaves**
9. Escreva uma descrição na caixa **Descrição da chave**, selecione uma duração e clique em **GUARDAR**. A página é atualizada e mostra agora um valor de chave. 
10. Vai utilizar as informações de **ID da Aplicação** e de **chave** no passo seguinte para definir as permissões do seu cofre.

## <a id="authorize"></a>Autorizar a aplicação a utilizar a chave ou o segredo
Existem duas formas de autorizar a aplicação a aceder à chave ou segredo no cofre.

### <a name="using-powershell"></a>Com o PowerShell
Para utilizar o PowerShell, utilize o cmdlet [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy).

Por exemplo, se o nome do cofre for **ContosoKeyVault** e a aplicação que pretende autorizar tem um ID de cliente de 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, e que pretende autorizar a aplicação a desencriptar e assinar com chaves no seu cofre, execute o seguinte cmdlet:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Se pretender autorizar essa mesma aplicação a ler os segredos no seu cofre, execute o seguinte procedimento:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get
```
### <a name="using-the-azure-portal"></a>Utilizar o portal do Azure
Para alterar a autorização de uma aplicação para utilizar chaves ou segredos:
1. Selecione **Políticas de Acesso** no painel de recursos do Cofre de Chaves
2. Clique no botão [+ Adicionar novo] na parte superior do painel
3. Clique em **Selecionar Principal** para selecionar a aplicação que criou anteriormente
4. No menu pendente **Permissões da chave**, selecione "Desencriptar" e "Assinar" para autorizar a aplicação a desencriptar e assinar com chaves no seu Cofre
5. No menu pendente **Permissões secretas**, selecione "Obter" para permitir que a aplicação leia os segredos no cofre

## <a id="HSM"></a>Trabalhar com um módulo de hardware de segurança (HSM)
Para mais segurança, pode importar ou gerar chaves nos módulos de segurança de hardware (HSMs) que nunca deixam o limite do HSM. Os HSMs têm a certificação FIPS 140-2 de nível 2 validada. Se este requisito não se aplica a si, ignore esta secção e aceda a [Eliminar o cofre de chaves e as chaves e segredos associados](#delete).

Para criar estas chaves protegidas por HSM, tem de utilizar a [camada de serviços do Cofre de Chaves do Azure para suportar chaves protegidas por HSM](https://azure.microsoft.com/pricing/details/key-vault/). Além disso, tenha em atenção que esta funcionalidade não está disponível para o Azure China.

Quando cria o cofre de chaves, adicione o parâmetro **-SKU**:

```powershell
New-AzureRmKeyVault -Name 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US' -SKU 'Premium'
```


Neste cofre de chaves, pode adicionar chaves protegidas por software (conforme mostrado anteriormente) e chaves protegidas por HSM. Para criar uma chave protegida por HSM, defina o parâmetro **-Destino** para 'HSM':

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'
```

Pode utilizar o seguinte comando para importar uma chave a partir de um ficheiro .PFX no seu computador. Este comando importa a chave os HSMs no serviço Cofre de Chaves:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'
```

O comando seguinte importa um pacote "traga a sua própria chave" (BYOK). Este cenário permite-lhe gerar a chave no seu HSM local e transferi-la para HSMs no serviço Cofre de Chaves, sem que a chave saia do limite HSM:

```powershell
$key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'
```

Para obter instruções mais detalhadas sobre como gerar este pacote BYOK, consulte o artigo [Como gerar e transferir as chaves protegidas por HSM para o Cofre de Chaves do Azure](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Eliminar o cofre de chaves e as chaves e segredos associados
Se já não precisar do Cofre de Chaves e da chave e do segredo que contiver, pode eliminar o Cofre de Chaves utilizando cmdlet [Remove-AzureRMKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault):

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'
```

Em alternativa, pode eliminar um grupo de recursos completo do Azure, que inclui o Cofre de Chaves e quaisquer outros recursos que incluiu nesse grupo:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'
```

## <a id="other"></a>Outros Cmdlets do Azure PowerShell
Outros comandos que poderão ser úteis para a gestão do Cofre de Chaves do Azure:

- `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Este comando obtém uma tabela de apresentação de todas as chaves e propriedades selecionadas.
- `$Keys[0]`: Este comando apresenta uma lista completa das propriedades da chave especificada
- `Get-AzureKeyVaultSecret`: Este comando lista uma tabela de apresentação de todos os nomes secretos e propriedades selecionadas.
- `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Exemplo de como remover uma chave específica.
- `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Exemplo de como remover um segredo específico.

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações gerais sobre o Cofre de Chaves do Azure, consulte o artigo [O que é o Cofre de Chaves do Azure?](key-vault-whatis.md)
- Para ver como o seu cofre de chaves está a ser utilizado, consulte o artigo [Registo do Cofre de Chaves do Azure](key-vault-logging.md).
- Para um tutorial de seguimento que utiliza o Cofre de Chaves do Azure numa aplicação Web, consulte o artigo [Utilizar o Cofre de Chaves do Azure a partir de uma Aplicação Web](key-vault-use-from-web-application.md).
- Para as referências de programação, consulte o [Guia para programadores do Cofre de Chaves do Azure](key-vault-developers-guide.md).
- Para obter uma lista dos cmdlets mais recentes do Azure PowerShell para o Cofre de Chaves do Azure, consulte o artigo [Cmdlets do Cofre de Chaves do Azure](/powershell/module/azurerm.keyvault/#key_vault).
