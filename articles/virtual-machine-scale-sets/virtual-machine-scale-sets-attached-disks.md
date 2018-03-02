---
title: "Discos de Dados Anexados a Conjuntos de Dimensionamento de Máquinas Virtuais do Azure | Microsoft Docs"
description: "Saiba como utilizar discos de dados anexados com conjuntos de dimensionamento de máquinas virtuais"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/25/2017
ms.author: negat
ms.openlocfilehash: 52ea7e35b941d5b1e45f39203757e4a3644cc9a5
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="azure-virtual-machine-scale-sets-and-attached-data-disks"></a>Conjunto de dimensionamento de máquinas virtuais do Azure e discos de dados anexados
Agora, os [conjuntos de dimensionamento de máquinas virtuais](/azure/virtual-machine-scale-sets/) do Azure suportam máquinas virtuais com discos de dados anexados. Pode definir os discos de dados no perfil de armazenamento dos conjuntos de dimensionamento que foram criados com os Managed Disks do Azure. Anteriormente, as únicas opções de armazenamento diretamente anexadas disponíveis com as VMs em conjuntos de dimensionamento eram a unidade de SO e as unidades temporárias.

> [!NOTE]
>  Quando cria um conjunto de dimensionamento com discos de dados anexados definidos, continua a ter de montar e formatar os discos a partir de uma VM para poder utilizá-los (tal como acontece com VMs do Azure autónomas). Uma forma conveniente de completar este processo é utilizar uma extensão de script personalizado que chama um script padrão para particionar e formatar todos os discos de dados numa VM.

## <a name="create-a-scale-set-with-attached-data-disks"></a>Criar um conjunto de dimensionamento com discos de dados anexados
Uma forma simples de criar um conjunto de dimensionamento com discos anexados é utilizar o comando [az vmss create](/cli/azure/vmss#az_vmss_create). O exemplo seguinte cria um grupo de recursos do Azure e um conjunto de dimensionamento de máquina virtual de 10 VMs Ubuntu, cada uma com dois discos de dados anexados, de 50 GB e 100 GB, respetivamente.

```bash
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

O comando [az vmss create](/cli/azure/vmss#az_vmss_create) repõe as predefinições de certos valores de configuração, caso não os especifique. Para ver as opções disponíveis que pode ignorar, tente:

```bash
az vmss create --help
```

Outra forma de criar um conjunto de dimensionamento com discos de dados anexados será definir um conjunto de dimensionamento num modelo do Azure Resource Manager, incluir uma secção _dataDisks_ no _storageProfile_ e implementar o modelo. O disco de 50 GB e 100 GB do exemplo anterior seriam definidos conforme mostrado no seguinte exemplo de modelo:

```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    }
]
```

Pode ver um exemplo de modelo de conjunto de dimensionamento com um disco anexado completo e pronto a utilizar definido aqui: [https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data](https://github.com/chagarw/MDPP/tree/master/101-vmss-os-data).

## <a name="create-a-service-fabric-cluster-with-attached-data-disks"></a>Criar um cluster do Service Fabric com discos de dados anexados
Cada [tipo de nó](../service-fabric/service-fabric-cluster-nodetypes.md) num cluster do [Service Fabric](/azure/service-fabric) em execução no Azure é suportado por um conjunto de dimensionamento de máquinas virtuais.  Ao utilizar um modelo do Azure Resource Manager, pode anexar discos de dados aos conjuntos de dimensionamento que compõem o cluster do Service Fabric. Pode utilizar um [modelo existente](https://github.com/Azure-Samples/service-fabric-cluster-templates) como um ponto de partida. No modelo, inclua uma secção _dataDisks_ no _storageProfile_ do(s) recursos _Microsoft.Compute/virtualMachineScaleSets_ e implemente o modelo. O exemplo seguinte anexa um disco de dados de 128 GB:

```json
"dataDisks": [
    {
    "diskSizeGB": 128,
    "lun": 0,
    "createOption": "Empty"
    }
]
```

Pode automaticamente criar partições, formatar e montar os discos de dados quando o cluster é implementado.  Adicione uma extensão de script personalizado ao _extensionProfile_ do _virtualMachineProfile_ do(s) conjunto(s) de dimensionamento.

Para preparar automaticamente o(s) disco(s) de dados num cluster do Windows, adicione o seguinte:

```json
{
    "name": "customScript",    
    "properties": {    
        "publisher": "Microsoft.Compute",    
        "type": "CustomScriptExtension",    
        "typeHandlerVersion": "1.8",    
        "autoUpgradeMinorVersion": true,    
        "settings": {    
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1"
        ],
        "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"
        }
    }
}
```
Para preparar automaticamente o(s) disco(s) de dados num cluster do Linux, adicione o seguinte:
```json
{
    "name": "lapextension",
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"
        ],
        "commandToExecute": "bash prepare_vm_disks.sh"
        }
    }
}
```

## <a name="adding-a-data-disk-to-an-existing-scale-set"></a>Adicionar um disco de dados a um conjunto de dimensionamento existente
> [!NOTE]
>  Apenas pode anexar discos de dados a um conjunto de dimensionamento que foi criado com os [Azure Managed Disks](./virtual-machine-scale-sets-managed-disks.md).

Pode adicionar um disco de dados a um conjunto de dimensionamento de máquina virtual o comando _az vmss disk attach_ da CLI do Azure. Certifique-se de que especifica um LUN que não esteja já em utilização. O seguinte exemplo da CLI adiciona uma unidade de 50 GB ao LUN 3:

```bash
az vmss disk attach -g dsktest -n dskvmss --size-gb 50 --lun 3
```

O seguinte exemplo do PowerShell adiciona uma unidade de 50 GB ao LUN 3:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myvmssrg -VMScaleSetName myvmss
$vmss = Add-AzureRmVmssDataDisk -VirtualMachineScaleSet $vmss -Lun 3 -Caching 'ReadWrite' -CreateOption Empty -DiskSizeGB 50 -StorageAccountType StandardLRS
Update-AzureRmVmss -ResourceGroupName myvmssrg -Name myvmss -VirtualMachineScaleSet $vmss
```

> [!NOTE]
> Diferentes tamanhos de VM têm limites diferentes relativamente ao número de unidades anexadas que suportam. Verifique as [características do tamanho da máquina virtual](../virtual-machines/windows/sizes.md) antes de adicionar um novo disco.

Também pode adicionar um disco ao criar uma nova entrada na propriedade _dataDisks_ no _storageProfile_ de uma definição de conjunto de dimensionamento e ao aplicar essa alteração. Para testar isto, localize a definição de um conjunto de dimensionamento existente no [Explorador de Recursos do Azure](https://resources.azure.com/). Selecione _Editar_ e adicione um novo disco à lista de discos de dados, como mostrado no exemplo seguinte:

```json
"dataDisks": [
    {
    "lun": 1,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 50
    },
    {
    "lun": 2,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 100
    },
    {
    "lun": 3,
    "createOption": "Empty",
    "caching": "ReadOnly",
    "diskSizeGB": 20
    }          
]
```

Em seguida, selecione _PUT_ para aplicar as alterações ao seu conjunto de dimensionamento. Este exemplo funciona desde que utilize um tamanho de VM que suporte mais do que dois discos de dados anexados.

> [!NOTE]
> Quando altera uma definição do conjunto de dimensionamento, como adicionar ou remover um disco de dados, a alteração é aplicada a todas as VMs criadas recentemente, mas apenas afeta as VMs existentes se a propriedade _upgradePolicy_ estiver definida como "Automático". Se estiver definida como "Manual", tem de aplicar manualmente o novo modelo às VMs existentes. Pode fazer isto no portal, através do comando _Update-AzureRmVmssInstance_ do PowerShell ou do comando _az vmss update-instances_ da CLI.

## <a name="adding-pre-populated-data-disks-to-an-existent-scale-set"></a>Adicionar discos de dados pré-preenchidos a um conjunto de dimensionamento existente 
> Quando adicionar discos a um modelo de conjunto de dimensionamento existente, por predefinição, o disco é sempre criado vazio. Este cenário também inclui novas instâncias criadas pelo conjunto de dimensionamento. Este comportamento acontece porque a definição do conjunto de dimensionamento tem um disco de dados vazio. Para criar unidades de dados pré-preenchidas para um modelo de conjunto de dimensionamento existente, pode escolher entre duas opções:

* Copie dados da VM 0 da instância para o(s) disco(s) de dados nas outras VMs, executando um script personalizado.
* Crie uma imagem gerida com o disco do SO e o disco de dados (com os dados necessários) e crie um novo conjunto de dimensionamento com a imagem. Desta forma, cada nova VM criada tem um disco de dados que é fornecido na definição do conjunto de dimensionamento. Uma vez que esta definição se refere a uma imagem com um disco de dados que tem dados personalizados, cada máquina virtual no conjunto de dimensionamento tem estas alterações.

> A forma de criar uma imagem personalizada pode ser encontrada aqui: [Criar uma imagem gerida de uma VM generalizada no Azure](/azure/virtual-machines/windows/capture-image-resource/) 

> O utilizador tem de capturar a VM 0 da instância que tem os dados requeridos e, em seguida, utilizar o vhd dessa definição de imagem.

## <a name="removing-a-data-disk-from-a-scale-set"></a>Remover um disco de dados de um conjunto de dimensionamento
Pode remover um disco de dados de um conjunto de dimensionamento de máquina virtual com o comando _az vmss disk detach_ da CLI do Azure. Por exemplo, o seguinte comando remove o disco definido no LUN 2:
```bash
az vmss disk detach -g dsktest -n dskvmss --lun 2
```  
Da mesma forma, para remover um disco de um conjunto de dimensionamento, pode também remover uma entrada da propriedade _dataDisks_ no _storageProfile_ e aplicar a alteração. 

## <a name="additional-notes"></a>Notas adicionais
O suporte para o Azure Managed Disks e para discos de dados anexados a conjuntos de dimensionamento está disponível na versão da API [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2016-04-30-preview/swagger/compute.json) ou posterior da API Microsoft.Compute.

Na implementação inicial do suporte para discos anexados para conjuntos de dimensionamento, não pode anexar nem desanexar discos de dados a/de VMs individuais num conjunto de dimensionamento.

O suporte do portal do Azure para discos de dados anexados em conjuntos de dimensionamento é limitado inicialmente. Dependendo das suas necessidades, pode utilizar modelos do Azure, a CLI, o PowerShell, SDKs e a API REST para gerir discos anexados.


