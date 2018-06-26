---
title: Ativar a encriptação de disco para clusters do Linux de recursos de infraestrutura de serviço | Microsoft Docs
description: Este artigo descreve como ativar a encriptação de disco para o Service Fabric conjuntos de dimensionamento de cluster no Azure utilizando o Azure Resource Manager e o Cofre de chaves do Azure.
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
ms.date: 05/24/2018
ms.author: v-viban
ms.openlocfilehash: e5caa3a787ceb1c8828b4a52648a3c74546c217b
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750463"
---
# <a name="enable-disk-encryption-for-service-fabric-linux-cluster-nodes"></a>Ativar a encriptação de disco para nós de cluster do Service Fabric Linux 
> [!div class="op_single_selector"]
> * [Encriptação de disco para Linux](service-fabric-enable-azure-disk-encryption-linux.md)
> * [Encriptação de disco para o Windows](service-fabric-enable-azure-disk-encryption-windows.md)
>
>

Utilize os seguintes passos para ativar a encriptação de disco em nós de cluster do Linux do Azure Service Fabric. Terá de efetuar estes para cada um dos tipos de nó ou conjuntos de dimensionamento de máquina virtual. Para encriptar os nós, irá utilizar a capacidade de Azure Disk Encryption em conjuntos de dimensionamento de máquina virtual.

O guia abrange os procedimentos seguintes:

* Conceitos chave para ativar a encriptação de disco em conjuntos de dimensionamento de máquina virtual para os clusters do Linux de recursos de infraestrutura de serviço.
* Passos de pré-requisitos a seguir antes de ativar a encriptação de disco na máquina virtual Dimensionar conjuntos para clusters do Linux de recursos de infraestrutura de serviço.
* Passos para ativar e desativar a encriptação em conjuntos de dimensionamento de máquina virtual para os clusters do Linux de recursos de infraestrutura do serviço de disco.


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
* Encriptação de conjunto de dimensionamento de máquina virtual é suportada para volumes de dados para conjuntos de dimensionamento de máquina virtual do Linux. Encriptação de disco de SO não é suportada na pré-visualização atual para Linux.
* Recriação de imagem de VM de conjunto de dimensionamento de máquina virtual e as operações de atualização não são suportadas na pré-visualização atual.


## <a name="create-a-linux-cluster"></a>Criar um cluster do Linux

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

Se precisar de criar um modelo personalizado para se adequarem às suas necessidades, recomendamos que comece com uma do [amostras de modelo do Azure Service Fabric](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master) para clusters do Linux. 

Se já tiver um modelo personalizado, certifique-se de que todos os parâmetros relacionados com o certificado no modelo e o ficheiro de parâmetros três são com o nome da seguinte forma. Certifique-se também que os valores são nulos.

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

Para conjuntos de dimensionamento de máquina virtual do Linux, a encriptação de disco de dados só é suportada. Por isso, terá de adicionar um disco de dados utilizando um modelo Azure Resource Manager. Atualize o modelo de dados de disco aprovisionamento da seguinte forma:

```JSON
   
   "storageProfile": { 
            "imageReference": { 
              "publisher": "[parameters('vmImagePublisher')]", 
              "offer": "[parameters('vmImageOffer')]", 
              "sku": "[parameters('vmImageSku')]", 
              "version": "[parameters('vmImageVersion')]" 
            }, 
            "osDisk": { 
              "caching": "ReadOnly", 
              "createOption": "FromImage", 
              "managedDisk": { 
                "storageAccountType": "[parameters('storageAccountType')]" 
              } 
           }, 
                "dataDisks": [ 
                { 
                    "diskSizeGB": 1023, 
                    "lun": 0, 
                    "createOption": "Empty" 
   
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

Seguem-se os comandos equivalentes do CLI do Azure para atualizar o modelo. Altere os valores nas instruções ' declare para os valores adequados. CLI do Azure suporta todos os parâmetros que suportam os comandos do PowerShell anteriores.

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

### <a name="confirm-that-the-linux-data-disk-is-mounted"></a>Confirme que o disco de dados de Linux está montado
Antes de continuar com a encriptação no conjunto de dimensionamento de máquina virtual Linux, certifique-se de que o disco de dados foi adicionada corretamente está montado. Inicie sessão para o cluster de Linux VM e execute o comando LSBLK. 

O resultado deve mostrar o disco de dados foi adicionada uma coluna de ponto de montagem.


### <a name="deploy-an-application-to-the-linux-service-fabric-cluster"></a>Implementar uma aplicação para o cluster do Linux Service Fabric
Siga os passos e a orientação para [implementar a aplicação no cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-quickstart-containers-linux).


## <a name="enable-disk-encryption-for-a-virtual-machine-scale-set"></a>Ativar a encriptação de disco para um conjunto de dimensionamento de máquina virtual
Ative a encriptação de disco para o conjunto de dimensionamento de máquina virtual que criou anteriormente para o cluster do Service Fabric Linux.
 
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
Utilize os seguintes comandos para obter o estado de um conjunto de dimensionamento de toda a máquina virtual ou qualquer instância VM num conjunto de dimensionamento. Também pode iniciar sessão para o cluster de Linux VM e execute o comando LSBLK. O resultado deve mostrar o disco de dados adicionados na coluna de ponto de montagem e `Type` coluna como `Crypt`.

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
Se tiver de desativar a encriptação de disco para o conjunto para um cluster do Service Fabric Linux de dimensionamento de máquina virtual, utilize os seguintes comandos. A desativação da encriptação de disco aplica-se para o conjunto de dimensionamento de toda a máquina virtual e não por instância. 

```PowerShell

$VmssName = "nt1vm"
$resourceGroupName = "mycluster"
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $VmssName

```

```CLI

az vmss encryption disable -g <resourceGroupName> -n <VMSS name>

```


## <a name="next-steps"></a>Passos Seguintes
Neste momento, tiver um cluster seguro e saber como ativar e desativar a encriptação de disco para um cluster do Service Fabric Linux. Em seguida, saiba mais sobre [encriptação de disco para o Windows](service-fabric-enable-azure-disk-encryption-windows.md). 

