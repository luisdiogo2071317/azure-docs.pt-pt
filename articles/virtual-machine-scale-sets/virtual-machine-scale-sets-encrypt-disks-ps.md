---
title: Encriptar discos do Azure para conjuntos de dimensionamento com o Azure PowerShell | Documentos da Microsoft
description: Saiba como utilizar o Azure PowerShell para encriptar as instâncias de VM e os discos anexados num conjunto de dimensionamento de máquinas virtuais do Windows
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: 8beebfc0bd845fc7dbe8b1f1665aba7820c78767
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54432086"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell-preview"></a>Encriptar o SO e discos de dados anexados num conjunto de dimensionamento com o Azure PowerShell (pré-visualização)

Para proteger e salvaguardar os dados em descanso ao utilizar a tecnologia de encriptação padrão da indústria, os conjuntos de dimensionamento de máquinas virtuais suportam encriptação de disco do Azure (ADE). Encriptação pode ser ativada para máquinas virtuais Windows e Linux conjuntos de dimensionamento. Para obter mais informações, consulte [encriptação de disco do Azure para Windows e Linux](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Azure disk encryption para conjuntos de dimensionamento de máquina virtual está atualmente em pré-visualização pública, disponível em todas as regiões públicas do Azure.

Encriptação de disco do Azure é suportada:
- para dimensionamento conjuntos criados com discos geridos e não suportado para conjuntos de dimensionamento do disco nativo (ou não gerenciado).
- para os volumes de dados e SO de conjuntos de dimensionamento do Windows. Desativar a encriptação é suportada para volumes de dados e SO para conjuntos de dimensionamento do Windows.
- para os volumes de dados de conjuntos de dimensionamento do Linux. Encriptação de disco de SO não é suportada na pré-visualização atual para conjuntos de dimensionamento do Linux.

Dimensionamento VM recriação de imagem e atualização operações de conjunto não são suportadas na pré-visualização atual. Recomenda-se a encriptação de disco do Azure para a pré-visualização de conjuntos de dimensionamento de máquina virtual apenas em ambientes de teste. Na pré-visualização, não ative a encriptação de disco em ambientes de produção em que poderá ter de atualizar uma imagem de sistema operacional num conjunto de dimensionamento encriptados.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o módulo do Azure PowerShell versão 5.7.0 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/azurerm/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="register-for-disk-encryption-preview"></a>Registar-se na pré-visualização da encriptação de disco

A encriptação de disco do Azure para pré-visualização de conjuntos de dimensionamento de máquina virtual requer que Self-registar a sua subscrição com [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature). Apenas terá de efetuar os passos seguintes na primeira vez que utilize a funcionalidade de pré-visualização da encriptação de disco:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Pode demorar até 10 minutos para que o pedido de registo propagar. Pode verificar o estado de registo com [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature). Quando o `RegistrationState` relatórios *registada*, volte a registar o *Microsoft. Compute* fornecedor com [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider):

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Criar um Azure Key Vault ativado para encriptação de disco

O Azure Key Vault pode armazenar chaves, segredos ou palavras-passe que permitem-lhe implementá-los em segurança nas suas aplicações e serviços. As chaves criptográficas são armazenadas no Azure Key Vault com a proteção de software, ou pode importar ou gerar as suas chaves em módulos de segurança de Hardware (HSMs) com certificação FIPS 140-2 de padrões no nível 2. Estas chaves criptográficas são utilizados para encriptar e desencriptar os discos virtuais anexados a sua VM. Manter o controlo destas chaves criptográficas e pode auditar o seu uso.

Criar um cofre de chaves com [novo-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Para permitir que o Cofre de chaves para ser utilizada para encriptação de disco, defina o *EnabledForDiskEncryption* parâmetro. O exemplo seguinte também define as variáveis para o nome do grupo de recursos, o nome do Key Vault e localização. Fornece seu próprio nome exclusivo do Key Vault:

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzureRmResourceGroup -Name $rgName -Location $location
New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>Utilizar um cofre de chaves existente

Este passo só é necessário se tiver um cofre de chaves existentes que deseja usar com a encriptação de disco. Se criou um Key Vault na secção anterior, ignore este passo.

Pode ativar um cofre de chaves existente na mesma subscrição e região como o conjunto para a encriptação de disco com de dimensionamento [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/AzureRM.KeyVault/Set-AzureRmKeyVaultAccessPolicy). Definir o nome do seu Cofre de chaves existente no *$vaultName* variável da seguinte forma:

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

Para encriptar as instâncias de VM num conjunto de dimensionamento, primeiro de obter algumas informações sobre o URI do Cofre de chave e ID de recurso com [Get-AzureRmKeyVault](/powershell/module/AzureRM.KeyVault/Get-AzureRmKeyVault). Estas variáveis são utilizadas para, em seguida, iniciar o processo de encriptação com [Set-AzureRmVmssDiskEncryptionExtension](/powershell/module/AzureRM.Compute/Set-AzureRmVmssDiskEncryptionExtension):

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

Quando lhe for pedido, escreva *y* continuar o processo de encriptação de disco na escala de VM do conjunto de instâncias.

## <a name="check-encryption-progress"></a>Verificar o progresso de criptografia

Para verificar o estado de encriptação de disco, utilize [Get-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Get-AzureRmVmssDiskEncryption):

```azurepowershell-interactive
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

Quando as instâncias de VM são encriptadas, o *EncryptionSummary* relatórios de código *êxito/ProvisioningState* conforme mostrado no seguinte exemplo:

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

Se já não pretender utilizar discos de instâncias VM encriptados, pode desativar a encriptação com [Disable-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Disable-AzureRmVmssDiskEncryption) da seguinte forma:

```azurepowershell-interactive
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, é utilizado o Azure PowerShell para encriptar um conjunto de dimensionamento de máquina virtual. Também pode utilizar o [CLI do Azure](virtual-machine-scale-sets-encrypt-disks-cli.md) ou modelos para [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) ou [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
