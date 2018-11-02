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
ms.date: 10/30/2018
ms.author: cynthn
ms.openlocfilehash: 28bb4c9fd4827f534c5f7bac2bae15451e3ca16c
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50914707"
---
# <a name="how-to-encrypt-virtual-disks-on-a-windows-vm"></a>Como encriptar discos virtuais numa VM do Windows
Para segurança aprimorada de máquina virtual (VM) e de conformidade, os discos virtuais no Azure podem ser encriptados. Os discos são encriptados utilizando chaves criptográficas que são protegidas num Azure Key Vault. Pode controlar estas chaves criptográficas e pode auditar o seu uso. Este artigo detalha como encriptar discos virtuais numa VM do Windows com o Azure PowerShell. Também pode [encriptar uma VM do Linux com a CLI do Azure](../linux/encrypt-disks.md).

## <a name="overview-of-disk-encryption"></a>Descrição geral da encriptação de disco

Discos virtuais em VMs do Windows são encriptados em descanso ao utilizar o Bitlocker. Não existe nenhum custo associado para encriptar discos virtuais no Azure. As chaves criptográficas são armazenadas no Azure Key Vault com a proteção de software, ou pode importar ou gerar as suas chaves em módulos de segurança de Hardware (HSMs) com certificação FIPS 140-2 de padrões no nível 2. Estas chaves criptográficas são utilizados para encriptar e desencriptar os discos virtuais anexados a sua VM. Manter o controlo destas chaves criptográficas e pode auditar o seu uso. 

O processo para encriptar uma VM é o seguinte:

1. Crie uma chave criptográfica num Azure Key Vault.
1. Configure a chave criptográfica para ser utilizada para encriptar discos.
1. Ative a encriptação de disco para os discos virtuais.
1. As chaves criptográficas necessárias são solicitadas no Azure Key Vault.
1. Os discos virtuais são encriptados com a chave criptográfica fornecida.

### <a name="azure-key-vault"></a>Azure Key Vault

Encriptação de disco depende [do Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Key Vault é utilizado para salvaguardar chaves criptográficas e segredos utilizados para o processo de encriptação/desencriptação do disco. 
  * Se existir, pode utilizar um Azure Key Vault existente. Não é preciso dedicar um Key Vault para encriptação de discos.
  * Para separar os limites administrativos e visibilidade de chave, pode criar um cofre de chaves dedicado.


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

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

O Cofre de chaves do Azure que contém as chaves criptográficas e os recursos de computação associado como o armazenamento e a própria VM têm de residir na mesma região. Criar um Azure Key Vault com [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) e ativar o Key Vault para utilização com a encriptação de disco. Especifique um nome exclusivo do Key Vault para *keyVaultName* da seguinte forma:

```azurepowershell-interactive
$keyVaultName = "myKeyVault$(Get-Random)"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

Pode armazenar as chaves criptográficas com o software ou de proteção de modelo de segurança de Hardware (HSM). Utilizar um HSM requer um cofre de chaves de premium. Existe um custo adicional para a criação de uma premium Key Vault, em vez de padrão Key Vault que armazena as chaves protegidas por software. Para criar um cofre de chaves de premium, no passo anterior adicione a *- Sku "Premium"* parâmetros. O exemplo seguinte utiliza as chaves protegidas por software, uma vez que criámos um cofre de chave padrão. 

Para ambos os modelos de proteção, a plataforma do Azure tem de ter acesso para solicitar as chaves criptográficas quando a VM arranca para desencriptar os discos virtuais. Criar uma chave criptográfica no seu Cofre de chaves com [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey). O exemplo seguinte cria uma chave denominada *myKey*:

```azurepowershell-interactive
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```

## <a name="create-virtual-machine"></a>Criar a máquina virtual
Para testar o processo de criptografia, crie uma VM com [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma VM com o nome *myVM* usando um *do Windows Server 2016 Datacenter* imagem. Quando lhe forem pedidas credenciais, introduza o nome de utilizador e palavra-passe a ser utilizado para a sua VM:

```azurepowershell-interactive
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

```azurepowershell-interactive
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName "myVM" `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

Aceite o pedido para continuar com a encriptação de VM. A VM é reiniciada durante o processo. Assim que tiver concluído o processo de encriptação e reiniciou a VM, reveja o estado de encriptação com [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):

```azurepowershell-interactive
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName "myVM"
```

O resultado é semelhante ao seguinte exemplo:

```azurepowershell-interactive
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Passos Seguintes
* Para obter mais informações sobre como gerir o Azure Key Vault, consulte [configurar o Cofre de chaves para máquinas virtuais](key-vault-setup.md).
* Para obter mais informações sobre a encriptação de disco, como preparar uma VM encriptada personalizada para carregar para o Azure, consulte [do Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
