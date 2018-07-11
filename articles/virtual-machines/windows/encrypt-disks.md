---
title: Encriptar discos numa VM do Windows no Azure | Documentos da Microsoft
description: Como encriptar discos virtuais numa VM do Windows para uma maior segurança com o Azure PowerShell
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/07/2018
ms.author: cynthn
ms.openlocfilehash: 9d8e868eb11e45a01b3992022b729369da6b42e4
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931495"
---
# <a name="how-to-encrypt-virtual-disks-on-a-windows-vm"></a>Como encriptar discos virtuais numa VM do Windows
Para segurança aprimorada de máquina virtual (VM) e de conformidade, os discos virtuais no Azure podem ser encriptados. Os discos são encriptados utilizando chaves criptográficas que são protegidas num Azure Key Vault. Pode controlar estas chaves criptográficas e pode auditar o seu uso. Este artigo detalha como encriptar discos virtuais numa VM do Windows com o Azure PowerShell. Também pode [encriptar uma VM do Linux com a CLI 2.0 do Azure](../linux/encrypt-disks.md).

## <a name="overview-of-disk-encryption"></a>Descrição geral da encriptação de disco
Discos virtuais em VMs do Windows são encriptados em descanso ao utilizar o Bitlocker. Não existe nenhum custo associado para encriptar discos virtuais no Azure. As chaves criptográficas são armazenadas no Azure Key Vault com a proteção de software, ou pode importar ou gerar as suas chaves em módulos de segurança de Hardware (HSMs) com certificação FIPS 140-2 de padrões no nível 2. Estas chaves criptográficas são utilizados para encriptar e desencriptar os discos virtuais anexados a sua VM. Manter o controlo destas chaves criptográficas e pode auditar o seu uso. Um principal de serviço do Azure Active Directory fornece um mecanismo seguro para a emissão estas chaves criptográficas, como as VMs têm a tecnologia e desativar.

O processo para encriptar uma VM é o seguinte:

1. Crie uma chave criptográfica num Azure Key Vault.
2. Configure a chave criptográfica para ser utilizada para encriptar discos.
3. Para ler a chave criptográfica no Azure Key Vault, crie um Azure Active Directory principal de serviço com as permissões adequadas.
4. Emita o comando para encriptar os discos virtuais, especificando o Azure Active Directory service principal e apropriado chave criptográfica para ser utilizado.
5. O principal de serviço do Azure Active Directory solicita a chave criptográfica necessária no Azure Key Vault.
6. Os discos virtuais são encriptados com a chave criptográfica fornecida.

## <a name="encryption-process"></a>Processo de criptografia
Encriptação de disco baseia-se dos seguintes componentes adicionais:

* **O Azure Key Vault** - utilizado para salvaguardar chaves criptográficas e segredos utilizados para o processo de encriptação/desencriptação do disco. 
  * Se existir, pode utilizar um Azure Key Vault existente. Não é preciso dedicar um Key Vault para encriptação de discos.
  * Para separar os limites administrativos e visibilidade de chave, pode criar um cofre de chaves dedicado.
* **O Azure Active Directory** -lida com a troca segura das chaves de criptografia necessárias e autenticação para ações pedidas. 
  * Normalmente, pode utilizar uma instância existente do Azure Active Directory para que hospeda o aplicativo.
  * O principal de serviço fornece um mecanismo seguro para pedir e emitido as chaves criptográficas apropriadas. Não estiver a desenvolver um aplicativo real que se integra com o Azure Active Directory.

## <a name="requirements-and-limitations"></a>Requisitos e limitações
Cenários suportados e os requisitos para encriptação de disco:

* Ativar a encriptação no novo Windows VMs a partir de imagens do Azure Marketplace ou de imagem personalizada do VHD.
* Ativar a encriptação em VMs existentes do Windows no Azure.
* Ativar a encriptação em VMs do Windows que estão configurados com espaços de armazenamento.
* A desativação da encriptação no SO e dados de unidades para as VMs do Windows.
* Todos os recursos (por exemplo, o Key Vault, a conta de armazenamento e a VM) tem de ser na mesma região do Azure e subscrição.
* Escalão Standard VMs, como A, série D, DS, G e GS VMs.

Encriptação de disco não é atualmente suportada nos seguintes cenários:

* VMs de escalão básico.
* VMs criadas com o modelo de implementação clássica.
* A atualizar as chaves criptográficas numa VM já encriptada.
* Integração com o serviço de gestão de chaves no local.

## <a name="create-azure-key-vault-and-keys"></a>Criar Cofre de chaves do Azure e de chaves
Antes de começar, certifique-se de que a versão mais recente do módulo do Azure PowerShell foi instalada. Para obter mais informações, veja [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/overview). Em todo os exemplos de comando, substitua todos os parâmetros de exemplo com seus próprios nomes, localização e valores de chave. Os exemplos seguintes utilizam uma convenção de *myResourceGroup*, *myKeyVault*, *myVM*, etc.

A primeira etapa é criar um Azure Key Vault para armazenar as chaves criptográficas. O Azure Key Vault pode armazenar chaves, segredos ou palavras-passe que permitem-lhe implementá-los em segurança nas suas aplicações e serviços. Para a encriptação de disco virtual, crie um Key Vault para armazenar uma chave criptográfica utilizada para encriptar ou desencriptar os discos virtuais. 

Ativar o fornecedor do Azure Key Vault com a sua subscrição do Azure com [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), em seguida, crie um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um nome de grupo de recursos *myResourceGroup* no *E.U.A. Leste* localização:

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

O Cofre de chaves do Azure que contém as chaves criptográficas e os recursos de computação associado como o armazenamento e a própria VM têm de residir na mesma região. Criar um Azure Key Vault com [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) e ativar o Key Vault para utilização com a encriptação de disco. Especifique um nome exclusivo do Key Vault para *keyVaultName* da seguinte forma:

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

Pode armazenar as chaves criptográficas com o software ou de proteção de modelo de segurança de Hardware (HSM). Utilizar um HSM requer um cofre de chaves de premium. Existe um custo adicional para a criação de uma premium Key Vault, em vez de padrão Key Vault que armazena as chaves protegidas por software. Para criar um cofre de chaves de premium, no passo anterior adicione a *- Sku "Premium"* parâmetros. O exemplo seguinte utiliza as chaves protegidas por software, uma vez que criámos um cofre de chave padrão. 

Para ambos os modelos de proteção, a plataforma do Azure tem de ter acesso para solicitar as chaves criptográficas quando a VM arranca para desencriptar os discos virtuais. Criar uma chave criptográfica no seu Cofre de chaves com [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey). O exemplo seguinte cria uma chave denominada *myKey*:

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-the-azure-active-directory-service-principal"></a>Criar principal de serviço de Azure Active Directory
Quando os discos virtuais são encriptados ou desencriptados, especificar uma conta para lidar com a autenticação e a troca de chaves criptográficas do Key Vault. Esta conta, um principal de serviço do Azure Active Directory, permite que a plataforma do Azure pedir as chaves criptográficas apropriadas em nome da VM. Uma instância do Azure Active Directory padrão está disponível na sua subscrição, embora muitas organizações têm dedicado diretórios do Azure Active Directory.

Criar um principal de serviço no Azure Active Directory com [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal). Para especificar uma palavra-passe segura, siga os [políticas de palavra-passe e restrições no Azure Active Directory](../../active-directory/authentication/concept-sspr-policy.md):

```powershell
$appName = "My App"
$securePassword = ConvertTo-SecureString -String "P@ssw0rd!" -AsPlainText -Force
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

Com êxito criptografar ou descriptografar discos virtuais, as permissões na chave de criptografia armazenadas no Key Vault tem de ser definidas para permitir o principal de serviço do Azure Active Directory para ler as chaves. Definir as permissões no Cofre de chaves com [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a>Criar a máquina virtual
Para testar o processo de criptografia, crie uma VM com [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma VM com o nome *myVM* usando um *do Windows Server 2016 Datacenter* imagem. Quando lhe forem pedidas credenciais, introduza o nome de utilizador e palavra-passe a ser utilizado para a sua VM:

```powershell
$cred = Get-Credential

New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```


## <a name="encrypt-virtual-machine"></a>Encriptar a máquina virtual
Para encriptar os discos virtuais, reunir todos os componentes anteriores:

1. Especifica o principal de serviço do Azure Active Directory e a palavra-passe.
2. Especifique o Key Vault para armazenar os metadados para os discos encriptados.
3. Especifica as chaves criptográficas para utilizar para a encriptação real e a desencriptação.
4. Especifique se pretende encriptar o disco do SO, os discos de dados ou todos.

Encriptar VM com [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) usando a chave de Cofre de chaves do Azure e as credenciais do principal de serviço do Azure Active Directory. O exemplo seguinte obtém todas as informações da chave, em seguida, criptografa a VM com o nome *myVM*:

```powershell
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName "myVM" `
    -AadClientID $app.ApplicationId `
    -AadClientSecret (New-Object PSCredential "user",$securePassword).GetNetworkCredential().Password `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

Aceite o pedido para continuar com a encriptação de VM. A VM é reiniciada durante o processo. Assim que tiver concluído o processo de encriptação e reiniciou a VM, reveja o estado de encriptação com [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName "myVM"
```

O resultado é semelhante ao seguinte exemplo:

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Passos Seguintes
* Para obter mais informações sobre como gerir o Azure Key Vault, consulte [configurar o Cofre de chaves para máquinas virtuais](key-vault-setup.md).
* Para obter mais informações sobre a encriptação de disco, como preparar uma VM encriptada personalizada para carregar para o Azure, consulte [do Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
