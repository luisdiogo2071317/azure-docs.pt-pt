---
title: Encriptar discos para conjuntos de dimensionamento do Azure com o Azure PowerShell | Microsoft Docs
description: Saiba como utilizar o Azure PowerShell para encriptar as instâncias VM e os discos ligados num conjunto de dimensionamento de máquina virtual do Windows
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: iainfou
ms.openlocfilehash: 91138ffad0fd906061e0b0ce5cdb251f402d3341
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell-preview"></a>Encriptar o SO e discos de dados anexados num conjunto com o Azure PowerShell (pré-visualização) de dimensionamento de máquina virtual

Para proteger e proteger dados em descanso ao utilizar tecnologias de encriptação padrão da indústria, os conjuntos de dimensionamento de máquina virtual suportam encriptação de disco do Azure (ADE). Encriptação pode ser ativada para a máquina virtual Windows e Linux conjuntos de dimensionamento. Para obter mais informações, consulte [encriptação de disco do Azure para Windows e Linux](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Encriptação de disco do Azure para conjuntos de dimensionamento de máquina virtual está atualmente em pré-visualização pública, disponível em todas as regiões públicas do Azure.

A encriptação de disco do Azure é suportada:
- para o dimensionamento conjuntos criado com discos geridos e não suportadas para conjuntos de dimensionamento de disco nativo (ou não geridos).
- para volumes de dados e SO em conjuntos de dimensionamento do Windows. Desativar a encriptação é suportada para volumes de dados e SO para conjuntos de dimensionamento do Windows.
- volumes de dados em conjuntos de dimensionamento do Linux. Encriptação de disco de SO não é suportada na pré-visualização atual para conjuntos de dimensionamento do Linux.

Conjunto VM recriação de imagem e atualizar as operações de dimensionamento não são suportadas na pré-visualização atual. A encriptação de disco do Azure para a pré-visualização de conjuntos de dimensionamento de máquina virtual é recomendada apenas em ambientes de teste. Na pré-visualização, não ative a encriptação de disco em ambientes de produção, onde poderá ter de atualizar a imagem do SO num conjunto de dimensionamento encriptados.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o Azure PowerShell versão do módulo 5.7.0 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="register-for-disk-encryption-preview"></a>Registar-se na pré-visualização de encriptação de disco

A encriptação de disco do Azure para a pré-visualização de conjuntos de dimensionamento de máquina virtual requer a Self-registar a sua subscrição com [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature). Só precisa de efetuar os passos seguintes na primeira vez que utilize a funcionalidade de pré-visualização de encriptação de disco:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Pode demorar até 10 minutos para que o pedido de registo se propagarem. Pode verificar o estado de registo com [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Quando o `RegistrationState` relatórios *registada*, volte a registar o *Mirosoft.Compute* fornecedor com [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Criar um cofre de chaves do Azure ativados para encriptação de disco

O Cofre de chaves do Azure pode armazenar as chaves, os segredos ou palavras-passe que permitem-lhe implementá-los em segurança nos seus serviços e aplicações. As chaves criptográficas são armazenadas no Cofre de chaves do Azure utilizando a proteção de software, ou pode importar ou gerar as suas chaves nos módulos de segurança de Hardware (HSMs) com certificação FIPS 140-2 normas de nível 2. Estas chaves criptográficas são utilizados para encriptar e desencriptar discos virtuais anexados à VM. Manter o controlo destas chaves criptográficas e pode auditar a sua utilização.

Criar um cofre de chaves com [novo-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Para permitir que o Cofre de chaves ser utilizada para encriptação de disco, defina o *EnabledForDiskEncryption* parâmetro. O exemplo seguinte também define variáveis de nome do grupo de recursos, nome do Cofre de chave e localização. Forneça o seu próprio nome exclusivo do Cofre de chaves:

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzureRmResourceGroup -Name $rgName -Location $location
New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>Utilize um cofre de chaves existente

Este passo só é necessário se tiver um cofre de chaves existente que pretende utilizar com a encriptação de disco. Ignore este passo se criou um cofre de chaves na secção anterior.

É possível ativar um cofre de chaves existentes na mesma subscrição e região como a escala definido para a encriptação de disco com [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/AzureRM.KeyVault/Set-AzureRmKeyVaultAccessPolicy). Definir o nome do seu Cofre de chaves existente no *$vaultName* variável da seguinte forma:

```azurepowershell-interactive
$vaultName="myexistingkeyvault"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```

## <a name="create-a-scale-set"></a>Criar um conjunto de dimensionamento

Primeiro, defina um nome de utilizador e uma palavra-passe para as instâncias da VM com [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Agora, crie um conjunto de dimensionamento de máquinas virtuais com [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss). Para distribuir o tráfego para instâncias de VM individuais, é também criado um balanceador de carga. O balanceador de carga inclui regras para distribuir o tráfego na porta TCP 80, bem como para permitir o tráfego de ambiente de trabalho remoto na porta TCP 3389 e a comunicação remota do PowerShell na porta TCP 5985:

```azurepowershell-interactive
$vmssName="myScaleSet"

New-AzureRmVmss `
    -ResourceGroupName $rgName `
    -VMScaleSetName $vmssName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -PublicIpAddressName "myPublicIPAddress" `
    -LoadBalancerName "myLoadBalancer" `
    -UpgradePolicy "Automatic" `
    -Credential $cred
```

## <a name="enable-encryption"></a>Ativar encriptação

Para encriptar as instâncias de VM num conjunto de dimensionamento, obtenha primeiro algumas informações sobre o ID de recurso e o URI do Cofre de chave com [Get-AzureRmKeyVault](/powershell/module/AzureRM.KeyVault/Get-AzureRmKeyVault). Estas variáveis são utilizadas para, em seguida, iniciar o processo de encriptação com [conjunto AzureRmVmssDiskEncryptionExtension](/powershell/module/AzureRM.Compute/Set-AzureRmVmssDiskEncryptionExtension):

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

Quando lhe for pedido, escreva *y* continuar o processo de encriptação de disco na escala definir VM instâncias.

## <a name="check-encryption-progress"></a>Verificar progresso de encriptação

Para verificar o estado de encriptação de disco, utilize [Get-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Get-AzureRmVmssDiskEncryption):

```azurepowershell-interactive
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

Quando são encriptadas instâncias de VM, o *EncryptionSummary* code relatórios *êxito/ProvisioningState* conforme mostrado no seguinte exemplo de saída:

```powershell
ResourceGroupName            : myResourceGroup
VmScaleSetName               : myScaleSet
EncryptionSettings           :
  KeyVaultURL                : https://myuniquekeyvault.vault.azure.net/
  KeyEncryptionKeyURL        :
  KeyVaultResourceId         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myuniquekeyvault
  KekVaultResourceId         :
  KeyEncryptionAlgorithm     :
  VolumeType                 : All
  EncryptionOperation        : EnableEncryption
EncryptionSummary[0]         :
  Code                       : ProvisioningState/succeeded
  Count                      : 2
EncryptionEnabled            : True
EncryptionExtensionInstalled : True
```

## <a name="disable-encryption"></a>Desativar a encriptação

Se já não pretender utilizar discos de instâncias VM encriptados, pode desativar a encriptação com [desativar AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Disable-AzureRmVmssDiskEncryption) da seguinte forma:

```azurepowershell-interactive
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, utilizado o Azure PowerShell para encriptar um conjunto de dimensionamento de máquina virtual. Também pode utilizar o [Azure CLI 2.0](virtual-machine-scale-sets-encrypt-disks-cli.md) ou modelos para [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) ou [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
