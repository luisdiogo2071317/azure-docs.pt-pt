---
title: Ativar a encriptação de disco para clusters de recursos de infraestrutura do serviço Windows | Microsoft Docs
description: Este artigo descreve como ativar a encriptação de disco para nós de cluster do Service Fabric no Azure utilizando o Azure Resource Manager, o Cofre de chaves do Azure.
services: service-fabric
documentationcenter: .net
author: v-viban
manager: navya
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/23/2018
ms.author: v-viban
ms.openlocfilehash: 0b84d270cc50888822b8463df2b95aedaa34ee9a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655747"
---
# <a name="enable-disk-encryption-for-service-fabric-windows-cluster-nodes"></a>Ativar a encriptação de disco para nós de cluster do service fabric Windows 
> [!div class="op_single_selector"]
> * [Encriptação de disco para o Windows](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Encriptação de disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Siga os passos abaixo para ativar a encriptação de disco em nós de Cluster do Service Fabric Windows. Terá de efetuar estes para cada um dos conjuntos de dimensionamento de máquina virtual/tipos do nó. Para encriptar os nós, iremos tirar partido da capacidade de Azure Disk Encryption em conjuntos de dimensionamento de máquina virtual.

O guia abrange os procedimentos seguintes:

* Conceitos chave que precisa de ter em consideração desativar ativar a encriptação de disco em dimensionamento da máquina virtual de Cluster do Service Fabric Windows definido.
* Passos de pré-requisitos para ser seguidas antes de ativar a encriptação de disco no conjunto de dimensionamento de máquina virtual de Cluster do Windows do Service Fabric.
* Conjunto de passos ser seguidas para ativar a encriptação de disco em dimensionamento da máquina virtual de Cluster do Windows do Service Fabric.


## <a name="prerequisites"></a>Pré-requisitos
1. **Registo Self** - para utilizar, necessita de pré-visualização do virtual machine escala conjunto disco encriptação registo automática
2. Self-pode registar a sua subscrição, executando os seguintes passos: 
```Powershell
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```
3. Aguarde cerca de 10 minutos até o estado como "Registada". Pode verificar o estado, executando o seguinte comando: 
```Powershell
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```
4. **O Cofre de chaves do Azure** -criar um KeyVault na mesma subscrição e região, como a escala definido e define a política de acesso 'EnabledForDiskEncryption' no KeyVault utilizando o cmdlet PS. Também pode definir a política utilizando a IU do KeyVault no portal do Azure: 
```Powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
```
5. Instalar a versão mais recente [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) , que tem os novos comandos de encriptação.
6. Instale a versão mais recente do [SDK do Azure a partir do Azure PowerShell](https://github.com/Azure/azure-powershell/releases) de versão. Seguem-se do dimensionamento da máquina virtual definir ADE cmdlets para permitir ([definir](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) encriptação, obter ([obter](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) Estado de encriptação e remover ([desativar](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) encriptação no conjunto de dimensionamento instância.

| Comando | Versão |  Origem  |
| ------------- |-------------| ------------|
| Get-AzureRmVmssDiskEncryptionStatus   | 3.4.0 ou superior | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryptionStatus   | 3.4.0 ou superior | AzureRM.Compute |
| Desativar AzureRmVmssDiskEncryption   | 3.4.0 ou superior | AzureRM.Compute |
| Get-AzureRmVmssDiskEncryption   | 3.4.0 ou superior | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryption   | 3.4.0 ou superior | AzureRM.Compute |
| Conjunto AzureRmVmssDiskEncryptionExtension   | 3.4.0 ou superior | AzureRM.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Cenários suportados para a encriptação de disco
* Encriptação de conjunto de dimensionamento de máquina virtual só é suportada para conjuntos de dimensionamento criado com discos geridos e não suportadas para conjuntos de dimensionamento de disco nativo (ou não geridos).
* Encriptação de conjunto de dimensionamento de máquina virtual é suportada para volumes de dados e SO para VMSS do Windows. Desativar a encriptação é suportada para volumes de dados e SO de conjunto de dimensionamento do Windows.
* Conjunto de dimensionamento de máquina virtual de VM recriar a imagem e as operações de atualização não são suportadas na pré-visualização atual.


### <a name="create-new-cluster-and-enable-disk-encryption"></a>Criar novo cluster e ativar a encriptação de disco

Utilize os seguintes comandos para criar o cluster e ativar a encriptação de disco utilizando o modelo Azure Resource Manager & Self-assinou o certificado.

### <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

```Powershell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

#### <a name="use-the-custom-template-that-you-already-have"></a>Utilizar o modelo personalizado que já tenha 

Se precisar de criar um modelo personalizado para se adequarem às suas necessidades, recomenda-se vivamente que comece com um dos modelos que estão disponíveis no [amostras de modelo de recursos de infraestrutura de serviço do azure](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master). Siga as orientações e explicações [personalizar o modelo de cluster] [personalizar-your-cluster-modelo] secção abaixo.

Se já tiver um modelo personalizado, certifique-se de que a verificação de double, que todos os três relacionadas com o certificado parâmetros no modelo e o ficheiro de parâmetros são denominados de forma e os valores são nulas da seguinte forma.

```Json
   "certificateThumbprint": {
      "value": ""
    },
    "sourceVaultValue": {
      "value": ""
    },
    "certificateUrlValue": {
      "value": ""
    },
```


```Powershell


$resourceGroupLocation="westus"
$resourceGroupName="mycluster"
$CertSubjectName="mycluster.westus.cloudapp.azure.com"
$certPassword="Password!1" | ConvertTo-SecureString -AsPlainText -Force 
$certOutputFolder="c:\certificates"

$parameterFilePath="c:\templates\templateparam.json"
$templateFilePath="c:\templates\template.json"


New-AzureRmServiceFabricCluster -ResourceGroupName $resourceGroupName -CertificateOutputFolder $certOutputFolder -CertificatePassword $certpassword -CertificateSubjectName $CertSubjectName -TemplateFile $templateFilePath -ParameterFile $parameterFilePath 

```


```CLI

declare certPassword=""
declare resourceGroupLocation="westus"
declare resourceGroupName="mylinux"
declare certSubjectName="mylinuxsecure.westus.cloudapp.azure.com"
declare parameterFilePath="c:\mytemplates\linuxtemplateparm.json"
declare templateFilePath="c:\mytemplates\linuxtemplate.json"
declare certOutputFolder="c:\certificates"


az sf cluster create --resource-group $resourceGroupName --location $resourceGroupLocation  \
    --certificate-output-folder $certOutputFolder --certificate-password $certPassword  \
    --certificate-subject-name $certSubjectName \
    --template-file $templateFilePath --parameter-file $parametersFilePath

```

#### <a name="deploy-application-to-windows-service-fabric-cluster"></a>Implementar a aplicação em cluster do Windows Service Fabric
Siga os passos e a orientação para [implementar a aplicação no cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications)


#### <a name="enable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set-created-above"></a>Ativar a encriptação de disco para o conjunto de dimensionamento de máquina virtual de Cluster do Service Fabric criado acima
 
```Powershell

$VmssName = "nt1vm"
$vaultName = "mykeyvault"
$resourceGroupName = "mycluster"
$KeyVault = Get-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName
$DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
$KeyVaultResourceId = $KeyVault.ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -VolumeType All

```

```CLI

az vmss encryption enable -g <resourceGroupName> -n <VMSS name> --disk-encryption-keyvault <KeyVaultResourceId>

```


#### <a name="validate-if-disk-encryption-enabled-for-windows-virtual-machine-scale-set"></a>Valide se definir a encriptação de disco ativada para o dimensionamento de máquina virtual do Windows.
Obter o estado de um conjunto de dimensionamento de toda a máquina virtual ou qualquer instância num conjunto de dimensionamento. Consulte os comandos abaixo.
Além do utilizador pode iniciar sessão na VM num conjunto de dimensionamento e certifique-se de unidades são encriptadas

```Powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```CLI

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


#### <a name="disable-disk-encryption-for-service-fabric-cluster-virtual-machine-scale-set"></a>Desativar a encriptação de disco de conjunto de dimensionamento de máquina virtual de Cluster do Service Fabric 
Desativar a encriptação de disco aplica-se ao conjunto de dimensionamento de toda a máquina virtual e não por instância 

```Powershell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Passos Seguintes
Neste momento, tiver um cluster seguro com como ativar/desativar a encriptação de disco de conjunto de dimensionamento de máquina virtual de Cluster do Service Fabric. Em seguida, [disco encriptação para Linux](service-fabric-enable-azure-disk-encryption-linux.md) 

