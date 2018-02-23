---
title: "Conjuntos de dimensionamento de Máquina Virtual do Azure encriptar discos | Microsoft Docs"
description: "Saiba como encriptar discos ligados em conjuntos de dimensionamento de máquina virtual."
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: iainfou
ms.openlocfilehash: dddcece9f7566961b256369330661e5dbd5d4665
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set"></a>Encriptar o SO e discos de dados anexados num conjunto de dimensionamento de máquina virtual
Azure [conjuntos de dimensionamento de máquina virtual](/azure/virtual-machine-scale-sets/) suporta a encriptação de disco do Azure (ADE).  Pode ser ativada a encriptação de disco do Azure para Windows e Linux em conjuntos de dimensionamento de máquina virtual para proteger e a salvaguardar a escala define dados em descanso ao utilizar tecnologias de encriptação padrão da indústria. Para obter mais informações, leia as máquinas virtuais de encriptação de disco do Azure para Windows e Linux.

> [!NOTE]
>  Encriptação de disco do Azure para conjuntos de dimensionamento de máquina virtual está atualmente em pré-visualização pública, disponível em todas as regiões públicas do Azure.

A encriptação de disco do Azure é suportada:
- para o dimensionamento conjuntos criado com discos geridos e não suportadas para conjuntos de dimensionamento de disco nativo (ou não geridos).
- para volumes de dados e SO em conjuntos de dimensionamento do Windows. Desativar a encriptação é suportada para volumes de dados e SO para conjuntos de dimensionamento do Windows.
- volumes de dados em conjuntos de dimensionamento do Linux. Encriptação de disco de SO não é suportada na pré-visualização atual para conjuntos de dimensionamento do Linux.

Conjunto VM recriação de imagem e atualizar as operações de dimensionamento não são suportadas na pré-visualização atual. A encriptação de disco do Azure para a pré-visualização de conjuntos de dimensionamento de máquina virtual é recomendada apenas em ambientes de teste. Na pré-visualização, não ative a encriptação de disco em ambientes de produção, onde poderá ter de atualizar a imagem do SO num conjunto de dimensionamento encriptados.

## <a name="prerequisites"></a>Pré-requisitos
Instalar as versões mais recentes do [Azure Powershell](https://github.com/Azure/azure-powershell/releases), que contém os comandos de encriptação.

A encriptação de disco do Azure para a pré-visualização de conjuntos de dimensionamento de máquina virtual requer que automática registar a sua subscrição através dos seguintes comandos do PowerShell: 

```powershell
Login-AzureRmAccount
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

Aguarde cerca de 10 minutos, até que o estado de 'Registada' é devolvido pelo comando seguinte: 

```powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Criar um cofre de chaves do Azure ativado para encriptação de disco
Crie um novo cofre de chaves na mesma subscrição e região, como a escala definidas e definir a política de acesso 'EnabledForDiskEncryption'.

```powershell
$rgname="windatadiskencryptiontest"
$VaultName="encryptionvault321"

New-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $rgName -Location southcentralus -EnabledForDiskEncryption
``` 

Em alternativa, ative um cofre de chaves existente na mesma subscrição e região como a escala definido para a encriptação de disco.

```powershell
$VaultName="encryptionvault321"
Set-AzureRmKeyVaultAccessPolicy -VaultName $VaultName -EnabledForDiskEncryption
```

## <a name="enable-encryption"></a>Ativar encriptação
Os seguintes comandos encriptam um disco de dados na escala em execução definido utilizando um cofre de chaves no mesmo grupo de recursos. Também pode utilizar modelos para encriptar os discos em execução [conjunto de dimensionamento de Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) ou [conjunto de dimensionamento de Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
$DiskEncryptionKeyVaultUrl="https://encryptionvault321.vault.azure.net"
$KeyVaultResourceId="/subscriptions/0754ecc2-d80d-426a-902c-b83f4cfbdc95/resourceGroups/windatadiskencryptiontest/providers/Microsoft.KeyVault/vaults/encryptionvault321"

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName `
    -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId –VolumeType Data
```

## <a name="check-encryption-progress"></a>Verificar progresso de encriptação
Utilize os seguintes comandos para mostrar o estado de encriptação de conjunto de dimensionamento.

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName -InstanceId "4"
```

## <a name="disable-encryption"></a>Desativar a encriptação
Desative a encriptação numa escala de máquina virtual em execução definida utilizando os seguintes comandos. Também pode utilizar modelos para desativar a encriptação em execução [conjunto de dimensionamento de Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows) ou [conjunto de dimensionamento de Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux).

```powershell
$rgname="windatadiskencryptiontest"
$VmssName="nt1vm"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName
```
