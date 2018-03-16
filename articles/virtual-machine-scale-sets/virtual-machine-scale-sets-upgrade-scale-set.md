---
title: "Modificar um conjunto de dimensionamento da máquina virtual do Azure | Microsoft Docs"
description: "Modificar um conjunto de dimensionamento da máquina virtual do Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: negat
ms.openlocfilehash: fcca912a8120a51d2f0a454ef0a6341cd5882015
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Modificar um conjunto de dimensionamento de máquina virtual
Este artigo descreve como modificar um conjunto de dimensionamento de máquina virtual existente. As tarefas incluem como alterar a configuração da escala definido, como alterar a configuração das aplicações em execução em escala definido, como gerir a disponibilidade e muito mais.

## <a name="fundamental-concepts"></a>conceitos fundamentais

### <a name="scale-set-model"></a>Modelo de conjunto de dimensionamento

Um conjunto de dimensionamento tem um modelo que capture o *pretendido* estado da escala definido como um todo. Para consultar o modelo para um conjunto de dimensionamento, pode utilizar:

* API DE REST: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` 
   
  Para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get).

* PowerShell:

  `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}`
   
  Para obter mais informações, consulte o [PowerShell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).

* CLI do Azure: 

  `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` 
   
  Para obter mais informações, consulte o [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).

Também pode utilizar [Explorador de recursos do Azure (pré-visualização)](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) para consultar o modelo para um conjunto de dimensionamento.

A apresentação de saída exata depende as opções que fornecem ao comando. Segue-se a saída de exemplo da CLI do Azure:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
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
  .
  .
  .
}
```

Como pode ver, estas propriedades aplicam-se à escala definido como um todo.



### <a name="scale-set-instance-view"></a>Vista de instância de conjunto de dimensionamento

Um conjunto também de dimensionamento tem uma vista de instância que capture atual *runtime* estado da escala definido como um todo. Para consultar a vista de instância de um conjunto de dimensionamento, pode utilizar:

* API DE REST: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` 
   
  Para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview).

* PowerShell: 

  `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` 
  
  Para obter mais informações, consulte o [PowerShell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss).

* CLI do Azure: 

  `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` 
   
  Para obter mais informações, consulte o [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).

Também pode utilizar [Explorador de recursos do Azure (pré-visualização)](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) para consultar a vista de instância de um conjunto de dimensionamento.

A apresentação de saída exata depende as opções que fornecem ao comando. Segue-se a saída de exemplo da CLI do Azure:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {virtualMachineScaleSetName}
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
  .
  .
  .
}
```

Como pode ver, estas propriedades fornecem um resumo do Estado de runtime atual das VMs no conjunto de dimensionamento. O resumo inclui o estado das extensões aplicada à escala definida (omitida de uma forma abreviada).



### <a name="scale-set-vm-model-view"></a>Vista de modelo VM de conjunto de dimensionamento

Semelhante à forma como um conjunto de dimensionamento tem uma vista de modelo, cada VM no conjunto de dimensionamento tem a suas próprias vista do modelo. Para consultar a vista de modelo para um conjunto de dimensionamento, pode utilizar:

* API DE REST: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` 
  
  Para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get).

* PowerShell: 

  `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` 
  
  Para obter mais informações, consulte o [PowerShell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).

* CLI do Azure: 

  `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` 
  
  Para obter mais informações, consulte o [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show).

Também pode utilizar [Explorador de recursos do Azure (pré-visualização)](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) para consultar o modelo para uma VM num conjunto de dimensionamento.

A apresentação de saída exata depende as opções que fornecem ao comando. Segue-se a saída de exemplo da CLI do Azure:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
{
  "location": "westus",
  "name": "{name}",
  "sku": {
    "name": "Standard_D2_v2",
    "tier": "Standard"
  },
  .
  .
  .
}
```

Como pode ver, estas propriedades descrevem a configuração da VM e não a configuração da escala definido como um todo. Por exemplo, o modelo de conjunto de dimensionamento tem `overprovision` como uma propriedade, enquanto que o modelo para uma VM com uma escala definido não. O motivo para esta diferença é que provocam um aprovisionamento é uma propriedade para o conjunto como um todo, não individuais VMs no conjunto de dimensionamento de dimensionamento. (Para obter mais informações sobre provocam um aprovisionamento, consulte [considerações de Design para conjuntos de dimensionamento](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning).)



### <a name="scale-set-vm-instance-view"></a>Vista de instância VM de conjunto de dimensionamento

Semelhante à forma como um conjunto de dimensionamento tem uma vista de instância, cada VM no conjunto de dimensionamento tem a suas próprias vista de instância. Para consultar a vista de instância de um conjunto de dimensionamento, pode utilizar:

* API DE REST: 

  `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` 
 
  Para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview).

* PowerShell: 

  `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` 
  
  Para obter mais informações, consulte o [PowerShell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm).

* CLI do Azure: 

  `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` 
  
  Para obter mais informações, consulte o [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view).

Também pode utilizar [Explorador de recursos do Azure (pré-visualização)](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) para consultar a vista de instância para uma VM num conjunto de dimensionamento.

A apresentação de saída exata depende as opções que fornecem ao comando. Segue-se a saída de exemplo da CLI do Azure:

```
$ az vmss get-instance-view -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}
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
  .
  .
  .
}
```

Como pode ver, estas propriedades descrevem o estado atual de tempo de execução da própria VM. O estado inclui quaisquer extensões aplicadas à escala definida (omitida de uma forma abreviada).




## <a name="techniques-for-updating-global-scale-set-properties"></a>Propriedades do conjunto de técnicas para atualizar a escala global

Para atualizar uma propriedade de conjunto de dimensionamento global, tem de atualizar a propriedade no modelo de conjunto de dimensionamento. Pode utilizar para esta atualização através de:

* API DE REST: 

  `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` 
  
  Para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate).

  Em alternativa, pode implementar um modelo Azure Resource Manager utilizando as propriedades da REST API para atualizar as propriedades do conjunto de dimensionamento global.

* PowerShell: 

  `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` 
  
  Para obter mais informações, consulte o [PowerShell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss).

* CLI do Azure:

  * Para modificar uma propriedade: `az vmss update --set {propertyPath}={value}` 
  
  * Para adicionar um objeto para uma propriedade de lista num conjunto de dimensionamento: `az vmss update --add {propertyPath} {JSONObjectToAdd}` 
  
  * Para remover um objeto de uma propriedade de lista num conjunto de dimensionamento: `az vmss update --remove {propertyPath} {indexToRemove}` 
  
  Para obter mais informações, consulte o [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update). 
  
  Em alternativa, se tiver implementado anteriormente a escala definida utilizando o `az vmss create` comando, pode executar o `az vmss create` comando novamente para atualizar o conjunto de dimensionamento. Para tal, certifique-se de que todas as propriedades de `az vmss create` comando são os mesmos como anteriormente, exceto as propriedades que pretende modificar.



Também pode utilizar [Explorador de recursos do Azure (pré-visualização)](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) atualizar a escala de modelo definido.

Depois do modelo de conjunto de dimensionamento é atualizado, a nova configuração aplica-se para VMs novas criadas no conjunto de dimensionamento. No entanto, os modelos para as VMs existentes no conjunto de dimensionamento tem ainda ser colocados atualizados com o mais recentes geral escala conjunto modelo. O modelo para cada VM, uma propriedade booleana designado `latestModelApplied` indica se a VM está atualizada com o mais recentes geral escala conjunto modelo. (Um valor de `true` significa que a VM está atualizada com o modelo mais recente.)




## <a name="techniques-for-bringing-vms-up-to-date-with-the-latest-scale-set-model"></a>Técnicas para colocar as VMs atualizadas com o modelo de conjunto de dimensionamento mais recente

Conjuntos de dimensionamento de tem um *atualizar política* que determina como VMs são colocadas atualizados com o modelo de conjunto de dimensionamento mais recente. Os três modos para a política de atualização são:

- **Automática**: neste modo, o conjunto de dimensionamento torna sem garantias sobre a ordem das VMs que são colocados em baixo. O conjunto de dimensionamento pode demorar baixo todas as VMs em simultâneo. 
- **A anular**: neste modo, o conjunto de dimensionamento lança a atualização em lotes, com um período de tempo entre lotes pausa opcional.
- **Manual**: neste modo, quando atualizar o modelo de conjunto de dimensionamento, nada acontece para VMs existentes. Para atualizar as VMs existentes, tem de atualizar manualmente cada um deles. Pode utilizar para esta atualização manual através de:

  - API DE REST: 
  
    `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` 
    
    Para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances).

  - PowerShell: 
  
    `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` 
    
    Para obter mais informações, consulte o [PowerShell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance).

  - CLI do Azure: 
  
    `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` 
    
    Para obter mais informações, consulte o [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances).

  Também pode utilizar o [Azure SDKs](https://azure.microsoft.com/downloads/) para atualizar manualmente uma VM num conjunto de dimensionamento.

>[!NOTE]
> Clusters de Service Fabric do Azure podem utilizar apenas o modo automático, mas a atualização é processada de forma. Para mais informações sobre atualizações de recursos de infraestrutura de serviço, consulte o [documentação do Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

Um tipo de modificação para propriedades do conjunto de dimensionamento global não segue a política de atualização: perfil de SO de conjunto de alterações à escala. (Os exemplos são admin nome de utilizador e palavra-passe.) Estas propriedades podem ser alteradas apenas na versão de API de 2017-12-01 ou posterior. Estas alterações são aplicadas apenas a VMs criados após a alteração na escala de modelo. Para colocar as VMs existentes atualizadas, tem de recriar cada VM existente. Criar nova imagem de uma VM através de:

* API DE REST: 

  `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` 
  
  Para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage).

* PowerShell: 

  `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` 
  
  Para obter mais informações, consulte o [PowerShell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm).

* CLI do Azure: 

  `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` 
  
  Para obter mais informações, consulte o [documentação da CLI do Azure](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage).

Também pode utilizar o [Azure SDKs](https://azure.microsoft.com/downloads/) para recriar uma VM num conjunto de dimensionamento.




## <a name="properties-with-restrictions-on-modification"></a>Propriedades com restrições de modificação

### <a name="create-time-properties"></a>Propriedades do tempo criar

Algumas propriedades podem ser definidas apenas quando estiver a criar o conjunto de dimensionamento inicialmente. Estas propriedades incluem:

- Zonas
- publicador de referência da imagem
- oferta de referência da imagem

### <a name="properties-that-can-be-changed-based-on-the-current-value-only"></a>Propriedades que podem ser alteradas com base no valor atual apenas

Algumas propriedades podem ser alteradas, com exceções, dependendo do valor atual. Estas propriedades incluem:

- `singlePlacementGroup`: Se `singlePlacementGroup` for VERDADEIRO, o que pode ser modificado para false. No entanto, se `singlePlacementGroup` é false,- *não é possível* ser modificado como true.
- `subnet`: Pode ser modificada a sub-rede de um conjunto de dimensionamento, desde que a sub-rede original e nova sub-rede estão na mesma rede virtual.

### <a name="properties-that-require-deallocation-to-change"></a>Propriedades que necessitam de Desalocação para alterar

Algumas propriedades podem ser alteradas para determinados valores apenas se as VMs no conjunto de dimensionamento são desalocadas. Estas propriedades incluem:

- `sku name`: Se o novo SKU de VM não é suportado no hardware que o conjunto de dimensionamento está atualmente no, terá de Desalocação as VMs na escala definido antes de modificar `sku name`. Para obter mais informações sobre o redimensionamento de VMs, consulte [esta publicação no blogue do Azure](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>Atualizações específicas da VM

Determinados modificações podem ser aplicadas às VMs específicas em vez das propriedades do conjunto de dimensionamento global. Atualmente, a única atualização VM específico que seja suportada é anexar/desanexar os discos de dados da VMs no conjunto de dimensionamento. Esta funcionalidade está em pré-visualização. Para obter mais informações, consulte o [pré-visualizar documentação](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios"></a>Cenários

### <a name="application-updates"></a>Atualizações da aplicação

Se uma aplicação for implementada para uma escala definido através de extensões, a atualização da configuração de extensão faz com que a aplicação a ser atualizados de acordo com a política de atualização. Por exemplo, se tiver uma nova versão de um script para executar uma extensão de script personalizado, pode atualizar o `fileUris` propriedade para apontar para o script de novo. 

Em alguns casos, pode querer forçar uma atualização, apesar da configuração de extensão é alterada. (Por exemplo, pode atualizar o script sem alterar o URI do script.) Nestes casos, pode modificar `forceUpdateTag` para forçar uma atualização. A plataforma do Azure não interpretar esta propriedade para que alterar o respetivo valor não tem efeito na forma como a extensão é executado. Basta modificar força a extensão para voltar a executar. 

Para mais informações sobre `forceUpdateTag`, consulte o [documentação da REST API para extensões](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

É também comum as aplicações implementadas através de uma imagem personalizada. Este cenário é descrito na secção seguinte.

### <a name="os-updates"></a>Atualizações do SO

Se estiver a utilizar imagens da plataforma, pode atualizar as imagens modificando `imageReference`. Para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).

>[!NOTE]
> Com imagens da plataforma, é comum para especificar "mais recente" para a versão de referência da imagem. Isto significa que quando são criados os conjuntos de dimensionamento, expandido terminar e reprocessamento de imagem, as VMs são criadas com a versão mais recente disponível. No entanto, este *não* significa que a imagem do SO será automaticamente atualizada ao longo do tempo, são lançadas novas versões de imagem. Esta é uma funcionalidade à parte, atualmente em pré-visualização. Para obter mais informações, consulte [SO automática atualiza](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Se estiver a utilizar imagens personalizadas, pode atualizar as imagens, atualizando o `imageReference` ID. Para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="examples"></a>Exemplos

### <a name="update-the-os-image-for-your-scale-set"></a>Atualizar a imagem do SO para o conjunto de dimensionamento

Imaginemos que tem um conjunto com uma versão antiga do Ubuntu LTS 16.04 de dimensionamento. Pretende atualizar para uma versão mais recente do Ubuntu LTS 16.04 (por exemplo, a versão 16.04.201801090). A propriedade de versão de referência de imagem não faz parte de uma lista, pelo que pode modificar estas propriedades diretamente utilizando estes comandos:

* PowerShell: 

  `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

* CLI do Azure: 

  `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="update-the-load-balancer-for-your-scale-set"></a>Atualizar o Balanceador de carga para o conjunto de dimensionamento

Imaginemos que tem uma escala definida com um balanceador de carga do Azure e que pretende substituir o Balanceador de carga com um gateway de aplicação do Azure. As propriedades de gateway de carga balanceador e aplicação, para um conjunto de dimensionamento fazem parte de uma lista. Por isso, pode utilizar os comandos para a remoção e adição de elementos de lista em vez de modificar as propriedades diretamente.

PowerShell:
```
# Get the current model of the scale set and store it in a local PowerShell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# Create a local PowerShell object for the new desired IP configuration, which includes the reference to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# Replace the existing IP configuration in the local PowerShell object (which contains the references to the current Azure load balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local PowerShell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

CLI do Azure:
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # Remove the load balancer back-end pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # Remove the load balancer back-end pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # Add the application gateway back-end pool to the scale set model
```

>[!NOTE]
> Estes comandos pressupõem que haja apenas um balanceador de configuração e a carga IP num conjunto de dimensionamento. Se existirem vários, poderá ter de utilizar um índice de lista que não seja 0.