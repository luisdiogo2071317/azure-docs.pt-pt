---
title: Modificar um conjunto de dimensionamento da máquina virtual do Azure | Microsoft Docs
description: Saiba como modificar e atualize um conjunto de REST APIs, o Azure PowerShell e o Azure CLI 2.0 de dimensionamento de máquina virtual do Azure
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: negat
ms.openlocfilehash: 91eccd2b4587d7cb926ca506ae2f2e5b91ea1f3e
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Modificar um conjunto de dimensionamento de máquina virtual
Ao longo do ciclo de vida das suas aplicações, poderá ter de modificar ou atualizar o conjunto de dimensionamento de máquina virtual. Estas atualizações podem incluir como atualizar a configuração do conjunto de dimensionamento, ou alterar a configuração da aplicação. Este artigo descreve como modificar um conjunto com as REST APIs, o Azure PowerShell ou o Azure CLI 2.0 de dimensionamento existente.

## <a name="fundamental-concepts"></a>conceitos fundamentais

### <a name="the-scale-set-model"></a>O modelo de conjunto de dimensionamento
Um conjunto de dimensionamento tem um "escala conjunto modelo" que capture o *pretendido* estado da escala definido como um todo. Para consultar o modelo para um conjunto de dimensionamento, pode utilizar o 

- API de REST com [computação/virtualmachinescalesets/obter](/rest/api/compute/virtualmachinescalesets/get) da seguinte forma:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- O Azure Powershell com [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss):

    ```powershell
    Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
    ```

- Azure CLI 2.0 com [mostrar de vmss az](/cli/azure/vmss#az_vmss_show):

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet
    ```

- Também pode utilizar [resources.azure.com](https://resources.azure.com) a específicas do idioma ou [Azure SDKs](https://azure.microsoft.com/downloads/).

A apresentação de saída exata depende as opções de que fornecer ao comando. O exemplo seguinte mostra a saída de exemplo condensed do 2.0 de CLI do Azure:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
{
  "location": "westus",
  "overprovision": true,
  "plan": null,
  "singlePlacementGroup": true,
  "sku": {
    "additionalProperties": {},
    "capacity": 1,
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
}
```

Estas propriedades aplicam-se à escala definido como um todo.


### <a name="the-scale-set-instance-view"></a>Vista de instância de conjunto de escala
Um conjunto também de dimensionamento tem uma "escala conjunto instância ver" que capture atual *runtime* estado da escala definido como um todo. Para consultar a vista de instância de um conjunto de dimensionamento, pode utilizar:

- API de REST com [virtualmachinescalesets/computação/getinstanceview](/rest/api/compute/virtualmachinescalesets/getinstanceview) da seguinte forma:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/instanceView?api-version={apiVersion}
    ```

- O Azure Powershell com [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss):

    ```powershell
    Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceView
    ```

- Azure CLI 2.0 com [az vmss get--vista de instância](/cli/azure/vmss#az_vmss_get_instance_view):

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet
    ```

- Também pode utilizar [resources.azure.com](https://resources.azure.com) a específicas do idioma ou [SDKs do Azure](https://azure.microsoft.com/downloads/)

A apresentação de saída exata depende as opções de que fornecer ao comando. O exemplo seguinte mostra a saída de exemplo condensed do 2.0 de CLI do Azure:

```azurecli
$ az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet
{
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": null,
      "time": "{time}"
    }
  ],
  "virtualMachine": {
    "additionalProperties": {},
    "statusesSummary": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "count": 1
      }
    ]
  }
}
```

Estas propriedades fornecem um resumo do Estado de runtime atual das VMs no conjunto de dimensionamento, tais como o estado das extensões aplicadas ao conjunto de dimensionamento.


### <a name="the-scale-set-vm-model-view"></a>Vista de modelo VM de conjunto de escala
Semelhante à forma como um conjunto de dimensionamento tem uma vista de modelo, cada VM no conjunto de dimensionamento tem a suas próprias vista do modelo. Para consultar a vista de modelo para um conjunto de dimensionamento, pode utilizar:

- API de REST com [computação/virtualmachinescalesetvms/obter](/rest/api/compute/virtualmachinescalesetvms/get) da seguinte forma:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId?api-version={apiVersion}
    ```

- O Azure Powershell com [Get-AzureRmVmssVm](/powershell/module/azurerm.compute/get-azurermvmssvm):

    ```powershell
    Get-AzureRmVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- Azure CLI 2.0 com [mostrar de vmss az](/cli/azure/vmss#az_vmss_show):

    ```azurecli
    az vmss show --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- Também pode utilizar [resources.azure.com](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/).

A apresentação de saída exata depende as opções de que fornecer ao comando. O exemplo seguinte mostra a saída de exemplo condensed do 2.0 de CLI do Azure:

```azurecli
$ az vmss show --resource-group myResourceGroup --name myScaleSet
{
  "location": "westus",
  "name": "{name}",
  "sku": {
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
}
```

Estas propriedades descrevem a configuração da VM e não a configuração da escala definido como um todo. Por exemplo, o modelo de conjunto de dimensionamento tem `overprovision` como uma propriedade, enquanto o modelo para uma VM num conjunto de dimensionamento não. Esta diferença é porque provocam um aprovisionamento é uma propriedade para o conjunto como um todo, não individuais VMs no conjunto de dimensionamento de dimensionamento (para obter mais informações sobre provocam um aprovisionamento, consulte [considerações de Design para conjuntos de dimensionamento](virtual-machine-scale-sets-design-overview.md#overprovisioning)).


### <a name="the-scale-set-vm-instance-view"></a>Vista de instância VM de conjunto de escala
Semelhante à forma como um conjunto de dimensionamento tem uma vista de instância, cada VM no conjunto de dimensionamento tem a suas próprias vista de instância. Para consultar a vista de instância de um conjunto de dimensionamento, pode utilizar:

- API de REST com [virtualmachinescalesetvms/computação/getinstanceview](/rest/api/compute/virtualmachinescalesetvms/getinstanceview) da seguinte forma:

    ```rest
    GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualmachines/instanceId/instanceView?api-version={apiVersion}
    ```

- O Azure Powershell com [Get-AzureRmVmssVm](/powershell/module/azurerm.compute/get-azurermvmssvm):

    ```powershell
    Get-AzureRmVmssVm -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -InstanceView
    ```

- Azure CLI 2.0 com [az vmss get--vista de instância](/cli/azure/vmss#az_vmss_get_instance_view)

    ```azurecli
    az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- Também pode utilizar [resources.azure.com](https://resources.azure.com) ou [SDKs do Azure](https://azure.microsoft.com/downloads/)

A apresentação de saída exata depende as opções de que fornecer ao comando. O exemplo seguinte mostra a saída de exemplo condensed do 2.0 de CLI do Azure:

```azurecli
$ az vmss get-instance-view --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
{
  "additionalProperties": {
    "osName": "ubuntu",
    "osVersion": "16.04"
  },
  "disks": [
    {
      "name": "{name}",
      "statuses": [
        {
          "additionalProperties": {},
          "code": "ProvisioningState/succeeded",
          "displayStatus": "Provisioning succeeded",
          "time": "{time}"
        }
      ]
    }
  ],
  "statuses": [
    {
      "additionalProperties": {},
      "code": "ProvisioningState/succeeded",
      "displayStatus": "Provisioning succeeded",
      "time": "{time}"
    },
    {
      "additionalProperties": {},
      "code": "PowerState/running",
      "displayStatus": "VM running"
    }
  ],
  "vmAgent": {
    "statuses": [
      {
        "additionalProperties": {},
        "code": "ProvisioningState/succeeded",
        "displayStatus": "Ready",
        "level": "Info",
        "message": "Guest Agent is running",
        "time": "{time}"
      }
    ],
    "vmAgentVersion": "{version}"
  },
}
```

Estas propriedades descrevem o estado atual do tempo de execução da VM, que inclui quaisquer extensões aplicadas ao conjunto de dimensionamento.


## <a name="how-to-update-global-scale-set-properties"></a>Como atualizar escala global definir propriedades
Para atualizar uma propriedade de conjunto de dimensionamento global, tem de atualizar a propriedade no modelo de conjunto de dimensionamento. Pode utilizar para esta atualização através de:

- API de REST com [virtualmachinescalesets/computação/createorupdate](/rest/api/compute/virtualmachinescalesets/createorupdate) da seguinte forma:

    ```rest
    PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version={apiVersion}
    ```

- Pode implementar um modelo do Resource Manager com as propriedades a partir da API de REST ao atualizar as propriedades do conjunto de dimensionamento global.

- O Azure Powershell com [atualização AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss):

    ```powershell
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -VirtualMachineScaleSet {scaleSetConfigPowershellObject}
    ```

- Azure CLI 2.0 com [atualização az vmss](/cli/azure/vmss#az_vmss_update):
    - Para modificar uma propriedade:

        ```azurecli
        az vmss update --set {propertyPath}={value}
        ```

    - Para adicionar um objeto para uma propriedade de lista num conjunto de dimensionamento: 

        ```azurecli
        az vmss update --add {propertyPath} {JSONObjectToAdd}
        ```

    - Para remover um objeto de uma propriedade de lista num conjunto de dimensionamento: 

        ```azurecli
        az vmss update --remove {propertyPath} {indexToRemove}
        ```

    - Se implementou anteriormente a escala definida com o `az vmss create` comando, pode executar o `az vmss create` comando novamente para atualizar o conjunto de dimensionamento. Certifique-se de que todas as propriedades de `az vmss create` comando são os mesmos como anteriormente, exceto as propriedades que pretende modificar.

- Também pode utilizar [resources.azure.com](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/).

Assim que o modelo de conjunto de dimensionamento é atualizado, a nova configuração aplica-se para VMs novas criadas no conjunto de dimensionamento. No entanto, os modelos para as VMs existentes no conjunto de dimensionamento tem ainda ser colocados atualizado com o mais recentes geral escala conjunto modelo. O modelo para cada VM é uma propriedade booleana designada `latestModelApplied` que indica se a VM está atualizada com o mais recentes geral escala conjunto modelo (`true` significa que a VM está atualizada com o modelo mais recente).


## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>Como colocar as VMs com o modelo de conjunto de dimensionamento mais recente
Conjuntos de dimensionamento de tem uma "política de atualização de" que determine como as VMs estão colocadas atualizados com o modelo de conjunto de dimensionamento mais recente. Os três modos para a política de atualização são:

- **Automática** -neste modo, o conjunto de dimensionamento torna sem garantias sobre a ordem das VMs que está a ser colocada para baixo. O conjunto de dimensionamento pode demorar baixo todas as VMs em simultâneo. 
- **A anular** -neste modo, o conjunto de dimensionamento lança a atualização em lotes com um período de tempo de interrupção opcionais entre lotes.
- **Manual** - neste modo, quando atualizar o modelo de conjunto de dimensionamento, nada acontece para VMs existentes.
 
Para atualizar as VMs existentes, tem de efetuar uma "atualização manual" de cada VM existente. Pode utilizar para esta atualização manual com:

- API de REST com [virtualmachinescalesets/computação/updateinstances](/rest/api/compute/virtualmachinescalesets/updateinstances) da seguinte forma:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/manualupgrade?api-version={apiVersion}
    ```

- O Azure Powershell com [atualização AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance):
    
    ```powershell
    Update-AzureRmVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId
    ```

- Azure CLI 2.0 com [az vmss update-instâncias](/cli/azure/vmss#az_vmss_update_instances)

    ```azurecli
    az vmss update-instances --resource-group myResourceGroup --name myScaleSet --instance-ids {instanceIds}
    ```

- Também pode utilizar a específicas do idioma [Azure SDKs](https://azure.microsoft.com/downloads/).

>[!NOTE]
> Só podem utilizar o Service Fabric clusters *automática* modo, mas a atualização é processada de forma. Para obter mais informações, consulte [ as atualizações de aplicações de Service Fabric](../service-fabric/service-fabric-application-upgrade.md).

Não há um tipo de modificação para propriedades do conjunto de dimensionamento global que segue a política de atualização. Mudar para o conjunto de dimensionamento do perfil do SO (por exemplo, admin nome de utilizador e palavra-passe) só pode ser alterado na versão de API *2017-12-01* ou posterior. Estas alterações só se aplicam a VMs criados após a alteração na escala de modelo. Para colocar as VMs existentes atualizadas, tem de fazer uma "recriação de imagem" de cada VM existente. Pode efetuar esta recriação de imagem através de:

- API de REST com [computação/virtualmachinescalesets/recriação de imagem](/rest/api/compute/virtualmachinescalesets/reimage) da seguinte forma:

    ```rest
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/reimage?api-version={apiVersion}
    ```

- O Azure Powershell com [conjunto AzureRmVmssVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm):

    ```powershell
    Set-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId instanceId -Reimage
    ```

- Azure CLI 2.0 com [recriação de imagem do az vmss](https://docs.microsoft.com/cli/azure/vmss#az_vmss_reimage):

    ```azurecli
    az vmss reimage --resource-group myResourceGroup --name myScaleSet --instance-id instanceId
    ```

- Também pode utilizar a específicas do idioma [Azure SDKs](https://azure.microsoft.com/downloads/).


## <a name="properties-with-restrictions-on-modification"></a>Propriedades com restrições de modificação

### <a name="create-time-properties"></a>Propriedades do tempo criar
Algumas propriedades só podem ser definidas quando criar o conjunto de dimensionamento. Estas propriedades incluem:

- Zonas de Disponibilidade
- publicador de referência da imagem
- oferta de referência da imagem

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Propriedades que só podem ser alteradas com base no valor atual
Algumas propriedades podem ser alteradas com exceções dependendo do valor atual. Estas propriedades incluem:

- **singlePlacementGroup** - se singlePlacementGroup for VERDADEIRO, poderão ser alterado como false. No entanto, se for FALSO, singlePlacementGroup- **não pode** ser modificado como true.
- **sub-rede** - a sub-rede de um conjunto de dimensionamento, desde que a sub-rede original pode ser modificada e nova sub-rede estão na mesma rede virtual.

### <a name="properties-that-require-deallocation-to-change"></a>Propriedades que necessitam de Desalocação para alterar
Algumas propriedades só podem ser alteradas para determinados valores se as VMs no conjunto de dimensionamento são desalocadas. Estas propriedades incluem:

- **Nome do SKU**- se o novo SKU de VM não é suportado no hardware do conjunto de dimensionamento é atualmente, tem de anular a atribuição de VMs no dimensionamento definido antes de modificar o nome do SKU. Para obter mais informações, consulte [como redimensionar uma VM do Azure](../virtual-machines/windows/resize-vm.md).


## <a name="vm-specific-updates"></a>Atualizações específicas da VM
Determinados modificações podem ser aplicadas às VMs específicas em vez das propriedades do conjunto de dimensionamento global. Atualmente, a única VM específicos a atualização que é suportada é ao anexar/desanexar os discos de dados da VMs no conjunto de dimensionamento. Esta funcionalidade está em pré-visualização. Para obter mais informações, consulte o [pré-visualizar documentação](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).


## <a name="scenarios"></a>Cenários

### <a name="application-updates"></a>Atualizações da aplicação
Se uma aplicação for implementada para uma escala definido através de extensões, uma atualização para a configuração da extensão faz com que a aplicação de atualização de acordo com a política de atualização. Por exemplo, se tiver uma nova versão de um script para executar uma extensão de Script personalizado, foi possível atualizar o *fileUris* propriedade para apontar para o script de novo. Em alguns casos, poderá pretender forçar uma atualização, apesar da configuração de extensão é alterada (por exemplo, atualizar o script sem uma alteração para o URI do script). Nestes casos, pode modificar o *forceUpdateTag* para forçar uma atualização. A plataforma do Azure não interpretar esta propriedade. Se alterar o valor, não há sem qualquer efeito nas como a extensão é executado. Uma alteração simplesmente força a extensão para voltar a executar. Para mais informações sobre o *forceUpdateTag*, consulte o [documentação da REST API para extensões](/rest/api/compute/virtualmachineextensions/createorupdate).

É também comum as aplicações implementadas através de uma imagem personalizada. Este cenário é descrito na secção seguinte.

### <a name="os-updates"></a>Atualizações do SO
Se utilizar imagens da plataforma do Azure, pode atualizar a imagem ao modificar o *imageReference* (mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

>[!NOTE]
> Com imagens da plataforma, é comum para especificar "mais recente" para a versão de referência da imagem. Quando cria, aumentar horizontalmente e recriação de imagem, as VMs são criadas com a versão mais recente disponível. No entanto, este **não** significa que a imagem do SO é atualizada automaticamente ao longo do tempo, são lançadas novas versões de imagem. Uma funcionalidade individual está atualmente em pré-visualização que fornece atualizações automáticas do SO. Para obter mais informações, consulte o [documentação de atualizações automáticas de SO](virtual-machine-scale-sets-automatic-upgrade.md).

Se utilizar imagens personalizadas, pode atualizar a imagem, atualizando o *imageReference* ID (mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

## <a name="examples"></a>Exemplos

### <a name="update-the-os-image-for-your-scale-set"></a>Atualizar a imagem do SO para o conjunto de dimensionamento
Pode ter um conjunto de dimensionamento que executa uma versão antiga do Ubuntu LTS 16.04. Pretende atualizar para uma versão mais recente do Ubuntu LTS 16.04, tais como versão *16.04.201801090*. A propriedade de versão de referência de imagem não faz parte de uma lista, pelo que pode modificar diretamente estas propriedades com um dos seguintes comandos:

- O Azure Powershell com [atualização AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) da seguinte forma:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -ImageReferenceVersion 16.04.201801090
    ```

- Azure CLI 2.0 com [atualização az vmss](/cli/azure/vmss#az_vmss_update_instances):

    ```azurecli
    az vmss update --resource-group myResourceGroup --name myScaleSet --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090
    ```


### <a name="update-the-load-balancer-for-your-scale-set"></a>Atualizar o Balanceador de carga para o conjunto de dimensionamento
Imaginemos que tem uma escala definida com um balanceador de carga do Azure e que pretende substituir o Balanceador de carga do Azure com um Gateway de aplicação do Azure. O Balanceador de carga e propriedades de Gateway de aplicação para um conjunto de dimensionamento fazem parte de uma lista, pelo que pode utilizar os comandos para remover ou adicionar elementos de lista em vez de modificar as propriedades diretamente:

- O Azure Powershell:

    ```powershell
    # Get the current model of the scale set and store it in a local powershell object named $vmss
    $vmss=Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet"
    
    # Create a local powershell object for the new desired IP configuration, which includes the referencerence to the application gateway
    $ipconf = New-AzureRmVmssIPConfig "myNic" -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name
    
    # Replace the existing IP configuration in the local powershell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
    $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf
    
    # Update the model of the scale set with the new configuration in the local powershell object
    Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -virtualMachineScaleSet $vmss
    ```

- CLI 2.0 do Azure:

    ```azurecli
    # Remove the load balancer backend pool from the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0
    
    # Remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
    az vmss update --resource-group myResourceGroup --name myScaleSet --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0
    
    # Add the application gateway backend pool to the scale set model
    az vmss update --resource-group myResourceGroup --name myScaleSet --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/myResourceGroup/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}'
    ```

>[!NOTE]
> Estes comandos pressupõem que haja apenas um balanceador de configuração e a carga IP num conjunto de dimensionamento. Se existirem vários, poderá ter de utilizar um índice de lista diferente de *0*.


## <a name="next-steps"></a>Passos Seguintes
Também pode efetuar tarefas de gestão comuns nos conjuntos de dimensionamento com o [Azure CLI 2.0](virtual-machine-scale-sets-manage-cli.md) ou [Azure PowerShell](virtual-machine-scale-sets-manage-powershell.md).