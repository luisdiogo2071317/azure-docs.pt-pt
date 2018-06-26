---
title: Ativar a encriptação de disco para clusters do Windows de recursos de infraestrutura de serviço | Microsoft Docs
description: Este artigo descreve como ativar a encriptação de disco para nós de cluster do Service Fabric no Azure utilizando o Azure Resource Manager e o Cofre de chaves do Azure.
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
ms.openlocfilehash: 58f9481fa27f44de30fd80fe52e9b6d06d341b41
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750898"
---
# <a name="enable-disk-encryption-for-service-fabric-windows-cluster-nodes"></a>Ativar a encriptação de disco para nós de cluster do Windows de recursos de infraestrutura de serviço 
> [!div class="op_single_selector"]
> * [Encriptação de disco para o Windows](service-fabric-enable-azure-disk-encryption-windows.md)
> * [Encriptação de disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md)
>
>

Utilize os seguintes passos para ativar a encriptação de disco em nós de cluster do Windows de recursos de infraestrutura de serviço do Azure. Terá de efetuar estes para cada um dos tipos de nó ou conjuntos de dimensionamento de máquina virtual. Para encriptar os nós, irá utilizar a capacidade de Azure Disk Encryption em conjuntos de dimensionamento de máquina virtual.

O guia abrange os procedimentos seguintes:

* Conceitos chave para ativar a encriptação de disco em conjuntos de dimensionamento de máquina virtual para os clusters do Windows de recursos de infraestrutura de serviço.
* Passos de pré-requisitos a seguir antes de ativar a encriptação de disco na máquina virtual Dimensionar conjuntos para clusters do Windows de recursos de infraestrutura de serviço.
* Passos para ativar e desativar a encriptação em conjuntos de dimensionamento de máquina virtual para os clusters do Windows de recursos de infraestrutura do serviço de disco.


## <a name="prerequisites"></a>Pré-requisitos
1. Self-registe a sua subscrição introduzindo o seguinte comando:

   ```PowerShell
   Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
   ```
   
   Aguarde cerca de 10 minutos até que o estado é `Registered`. Pode verificar o estado, executando os seguintes comandos:

   ```PowerShell
   Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
   ```

2. Crie um cofre de chaves na mesma subscrição e região, como o conjunto de dimensionamento. Definir a política de acesso `EnabledForDiskEncryption` no Cofre de chaves utilizando o cmdlet do PowerShell. Também pode definir a política utilizando a IU de Cofre de chave do Azure no portal do Azure.

   ```PowerShell
   Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -EnabledForDiskEncryption
   ```

3. Instalar [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest), que tem os comandos de encriptação mais recentes.

4. Instale a versão mais recente do [SDK do Azure a partir do Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Utilize os seguintes cmdlets para ativar ([definir](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension?view=azurermps-4.4.1)) encriptação, obter ([obter](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption?view=azurermps-4.4.1)) Estado de encriptação e remover ([desativar](https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption?view=azurermps-4.4.1)) encriptação numa escala definir instância:

| Comando | Versão |  Origem  |
| ------------- |-------------| ------------|
| Get-AzureRmVmssDiskEncryptionStatus   | 3.4.0 ou posterior | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryptionStatus   | 3.4.0 ou posterior | AzureRM.Compute |
| Desativar AzureRmVmssDiskEncryption   | 3.4.0 ou posterior | AzureRM.Compute |
| Get-AzureRmVmssDiskEncryption   | 3.4.0 ou posterior | AzureRM.Compute |
| Get-AzureRmVmssVMDiskEncryption   | 3.4.0 ou posterior | AzureRM.Compute |
| Conjunto AzureRmVmssDiskEncryptionExtension   | 3.4.0 ou posterior | AzureRM.Compute |


## <a name="supported-scenarios-for-disk-encryption"></a>Cenários suportados para a encriptação de disco
* Encriptação de conjunto de dimensionamento de máquina virtual só é suportada para conjuntos de dimensionamento criados com discos geridos. Não é suportada para conjuntos de dimensionamento de disco nativo (ou não geridos).
* Encriptação de conjunto de dimensionamento de máquina virtual é suportada para volumes de dados e SO para conjuntos de dimensionamento de máquina virtual do Windows. Pode ativar e desativar a encriptação.
* Recriação de imagem de VM de conjunto de dimensionamento de máquina virtual e as operações de atualização não são suportadas na pré-visualização atual.


## <a name="create-a-windows-cluster"></a>Criar um cluster do Windows

Utilize os seguintes comandos para criar um cluster e ativar a encriptação de disco, utilizando um modelo Azure Resource Manager e um certificado autoassinado.

### <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

```PowerShell

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId <guid>

```

```CLI

azure login
az account set --subscription $subscriptionId

```

### <a name="use-a-custom-template"></a>Utilizar um modelo personalizado 

Se precisar de criar um modelo personalizado para se adequarem às suas necessidades, recomendamos que comece com uma do [amostras de modelo do Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master).

Se já tiver um modelo personalizado, certifique-se de que todos os três relacionadas com o certificado parâmetros no modelo e o ficheiro de parâmetros são com o nome da seguinte forma. Certifique-se também que os valores são nulos.

```JSON
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


```PowerShell


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

### <a name="deploy-an-application-to-the-windows-service-fabric-cluster"></a>Implementar uma aplicação para o cluster do Windows Service Fabric
Siga os passos e a orientação para [implementar uma aplicação no seu cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-deploy-remove-applications).


## <a name="enable-disk-encryption-for-a-virtual-machine-scale-set"></a>Ativar a encriptação de disco para um conjunto de dimensionamento de máquina virtual
Ative a encriptação de disco para o conjunto de dimensionamento de máquina virtual que criou anteriormente para o cluster do Windows de recursos de infraestrutura de serviço.
 
```PowerShell

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


## <a name="validate-that-disk-encryption-is-enabled-for-a-virtual-machine-scale-set"></a>Validar esse disco encriptação está ativada para um conjunto de dimensionamento de máquina virtual
Utilize os seguintes comandos para obter o estado de um conjunto de dimensionamento de toda a máquina virtual ou qualquer instância VM num conjunto de dimensionamento. Também pode iniciar sessão a uma VM no conjunto de dimensionamento e certifique-se de que as unidades estejam encriptadas.

```PowerShell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Get-AzureRmVmssDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName

Get-AzureRmVmssVMDiskEncryption -ResourceGroupName $resourceGroupName -VMScaleSetName $VmssName -InstanceId "0"

```

```CLI

az vmss encryption show -g <resourceGroupName> -n <VMSS name>

```


## <a name="disable-disk-encryption-for-a-virtual-machine-scale-set"></a>Desativar a encriptação de disco para um conjunto de dimensionamento de máquina virtual 
Se tiver de desativar a encriptação de disco para o conjunto para um cluster do Windows de recursos de infraestrutura do serviço de dimensionamento de máquina virtual, utilize os seguintes comandos. A desativação da encriptação de disco aplica-se para o conjunto de dimensionamento de toda a máquina virtual e não por instância. 


```PowerShell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Passos Seguintes
Neste momento, tiver um cluster seguro e saber como ativar e desativar a encriptação de disco para um cluster do Windows de recursos de infraestrutura de serviço. Em seguida, saiba mais sobre [encriptação de disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md). 

