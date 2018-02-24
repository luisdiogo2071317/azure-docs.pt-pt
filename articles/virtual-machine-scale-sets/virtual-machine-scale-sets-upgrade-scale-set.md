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
ms.openlocfilehash: 836d56012afa9e5d5bdec35d85c37dd4b0b788ce
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="modify-a-virtual-machine-scale-set"></a>Modificar um conjunto de dimensionamento de máquina virtual
Este artigo descreve como modificar um conjunto de dimensionamento existente. Isto inclui como alterar a configuração da escala definido, como alterar a configuração das aplicações em execução em escala definido, como gerir a disponibilidade e muito mais.

## <a name="fundamental-concepts"></a>conceitos fundamentais

### <a name="the-scale-set-model"></a>O modelo de conjunto de dimensionamento

Um conjunto de dimensionamento tem um "escala conjunto modelo" que capture o *pretendido* estado da escala definido como um todo. Para consultar o modelo para um conjunto de dimensionamento, pode utilizar:

API de REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/get))

PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (para obter mais informações, consulte o [Powershell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss))

CLI: `az vmss show -g {resourceGroupName} -n {vmSaleSetName}` (para obter mais informações, consulte o [documentação CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show))

Também pode utilizar [resources.azure.com](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) para consultar o modelo para um conjunto de dimensionamento.

A apresentação de saída exata depende as opções de que fornecer ao comando, mas eis algumas saídas de exemplo a partir da CLI do:

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



### <a name="the-scale-set-instance-view"></a>Vista de instância de conjunto de escala

Um conjunto também de dimensionamento tem uma "escala conjunto instância ver" que capture atual *runtime* estado da escala definido como um todo. Para consultar a vista de instância de um conjunto de dimensionamento, pode utilizar:

API de REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/instanceView?api-version={apiVersion}` (para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getinstanceview))

PowerShell: `Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceView` (para obter mais informações, consulte o [Powershell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss))

CLI: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName}` (para obter mais informações, consulte o [documentação CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view))

Também pode utilizar [resources.azure.com](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) para consultar a vista de instância de um conjunto de dimensionamento.

A apresentação de saída exata depende as opções de que fornecer ao comando, mas um resultado de exemplo a partir da CLI do é:

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

Como pode ver, fornecem estas propriedades a definir um resumo do Estado de runtime atual das VMs em escala, incluindo inclui o estado das extensões aplicadas ao conjunto de dimensionamento (omitido de uma forma abreviada).



### <a name="the-scale-set-vm-model-view"></a>Vista de modelo VM de conjunto de escala

Semelhante à forma como um conjunto de dimensionamento tem uma vista de modelo, cada VM no conjunto de dimensionamento tem a suas próprias vista do modelo. Para consultar a vista de modelo para um conjunto de dimensionamento, pode utilizar:

API de REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}?api-version={apiVersion}` (para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/get))

PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (para obter mais informações, consulte o [Powershell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

CLI: `az vmss show -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (para obter mais informações, consulte o [documentação CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_show))

Também pode utilizar [resources.azure.com](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) para consultar o modelo para uma VM num conjunto de dimensionamento.

A apresentação de saída exata depende as opções de que fornecer ao comando, mas eis algumas saídas de exemplo a partir da CLI do:

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

Como pode ver, estas propriedades descrevem a configuração da VM e não a configuração da escala definido como um todo. Por exemplo, o modelo de conjunto de dimensionamento tem `overprovision` como uma propriedade, enquanto o modelo para uma VM num conjunto de dimensionamento não. Esta diferença é porque provocam um aprovisionamento é uma propriedade para o conjunto como um todo, não individuais VMs no conjunto de dimensionamento de dimensionamento (para obter mais informações sobre provocam um aprovisionamento, consulte [esta documentação](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview#overprovisioning)).



### <a name="the-scale-set-vm-instance-view"></a>Vista de instância VM de conjunto de escala

Semelhante à forma como um conjunto de dimensionamento tem uma vista de instância, cada VM no conjunto de dimensionamento tem a suas próprias vista de instância. Para consultar a vista de instância de um conjunto de dimensionamento, pode utilizar:

API de REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualmachines/{instanceId}/instanceView?api-version={apiVersion}` (para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/getinstanceview))

PowerShell: `Get-AzureRmVmssVm -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -InstanceView` (para obter mais informações, consulte o [Powershell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

CLI: `az vmss get-instance-view -g {resourceGroupName} -n {vmSaleSetName} --instance-id {instanceId}` (para obter mais informações, consulte o [documentação CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_get_instance_view))

Também pode utilizar [resources.azure.com](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) para consultar a vista de instância para uma VM num conjunto de dimensionamento.

A apresentação de saída exata depende as opções de que fornecer ao comando, mas eis algumas saídas de exemplo a partir da CLI do:

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

Como pode ver, estas propriedades descrevem o estado atual do tempo de execução da VM, incluindo quaisquer extensões aplicadas ao conjunto de dimensionamento (omitido de uma forma abreviada).




## <a name="how-to-update-global-scale-set-properties"></a>Como atualizar escala global definir propriedades

Para atualizar uma propriedade de conjunto de dimensionamento global, tem de atualizar a propriedade no modelo de conjunto de dimensionamento. Pode utilizar para esta atualização através de:

API de REST: `PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}?api-version={apiVersion}` (para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate))

Modelos do Resource Manager: em alternativa, pode implementar um modelo do Resource Manager com as propriedades da REST API para atualizar as propriedades do conjunto de dimensionamento global.

PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -VirtualMachineScaleSet {scaleSetConfigPowershellObject}` (para obter mais informações, consulte o [Powershell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss))

CLI. Para modificar uma propriedade: `az vmss update --set {propertyPath}={value}`. Para adicionar um objeto para uma propriedade de lista num conjunto de dimensionamento: `az vmss update --add {propertyPath} {JSONObjectToAdd}`. Para remover um objeto de uma propriedade de lista num conjunto de dimensionamento: `az vmss update --remove {propertyPath} {indexToRemove}`. (para obter mais informações, consulte o [documentação CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update)). Em alternativa, se tiver implementado anteriormente a escala definida utilizando o `az vmss create` comando, pode executar o `az vmss create` comando novamente para atualizar o conjunto de dimensionamento. Para tal, tem de garantir que todas as propriedades de `az vmss create` comando são os mesmos como anteriormente, exceto as propriedades que pretende modificar.



Também pode utilizar [resources.azure.com](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) atualizar a escala de modelo definido.

Assim que o modelo de conjunto de dimensionamento é atualizado, a nova configuração aplica-se para VMs novas criadas no conjunto de dimensionamento. No entanto, os modelos para as VMs existentes no conjunto de dimensionamento tem ainda ser colocados atualizado com o mais recentes geral escala conjunto modelo. O modelo para cada VM é uma propriedade booleana designada `latestModelApplied` que indica se a VM está atualizada com o mais recentes geral escala conjunto modelo (`true` significa que a VM está atualizada com o modelo mais recente).




## <a name="how-to-bring-vms-up-to-date-with-the-latest-scale-set-model"></a>Como colocar as VMs com o modelo de conjunto de dimensionamento mais recente

Conjuntos de dimensionamento de tem uma "política de atualização de" que determine como as VMs estão colocadas atualizados com o modelo de conjunto de dimensionamento mais recente. Os três modos para a política de atualização são:

- Automático: Neste modo, o conjunto de dimensionamento faz com que não garantias sobre a ordem das VMs que está a ser colocada para baixo. O conjunto de dimensionamento pode demorar baixo todas as VMs em simultâneo. 
- A anular: Neste modo, o conjunto de dimensionamento lança a atualização em lotes com um período de tempo de interrupção opcionais entre lotes.
- Manual: Neste modo, quando atualizar o modelo de conjunto de dimensionamento, nada acontece para VMs existentes. Para atualizar as VMs existentes, tem de efetuar uma "atualização manual" de cada VM existente. Pode utilizar para esta atualização manual através de:

API de REST: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/manualupgrade?api-version={apiVersion}` (para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/updateinstances))

PowerShell: `Update-AzureRmVmssInstance -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId}` (para obter mais informações, consulte o [Powershell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance))

CLI: `az vmss update-instances -g {resourceGroupName} -n {vmScaleSetName} --instance-ids {instanceIds}` (para obter mais informações, consulte o [documentação CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_update_instances)).

Também pode utilizar o [Azure SDKs](https://azure.microsoft.com/downloads/) para efetuar uma atualização manual numa VM num conjunto de dimensionamento.

>[!NOTE]
> Clusters de Service Fabric só podem utilizar o modo automático, mas a atualização é processada de forma. Para obter mais informações sobre as atualizações de recursos de infraestrutura de serviço, consulte [a documentação do Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade).

>[!NOTE]
> Não há um tipo de modificação para propriedades do conjunto de dimensionamento global que segue a política de atualização. Estas são as alterações à escala definir perfil do SO (por exemplo, admin nome de utilizador e palavra-passe). Estas alterações só se aplicam a VMs criados após a alteração na escala de modelo. Para colocar as VMs existentes atualizadas, tem de fazer uma "recriação de imagem" de cada VM existente. Pode efetuar esta recriação de imagem através de:

API de REST: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage))

PowerShell: `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (para obter mais informações, consulte o [Powershell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm))

CLI: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (para obter mais informações, consulte o [documentação CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage)).

Também pode utilizar o [Azure SDKs](https://azure.microsoft.com/downloads/) para recriar uma VM num conjunto de dimensionamento.




## <a name="properties-with-restrictions-on-modification"></a>Propriedades com restrições de modificação

### <a name="create-time-properties"></a>Propriedades do tempo criar

Algumas propriedades só podem ser definidas durante a criação inicialmente a escala definidos. Estas propriedades incluem:

- Zonas
- publicador de referência da imagem
- oferta de referência da imagem

### <a name="properties-that-can-only-be-changed-based-on-the-current-value"></a>Propriedades que só podem ser alteradas com base no valor atual

Algumas propriedades podem ser alteradas com exceções dependendo do valor atual. Estas propriedades incluem:

- singlePlacementGroup: singlePlacementGroup for VERDADEIRO, poderão ser alterado como false. No entanto, se for FALSO, singlePlacementGroup- **não pode** ser modificado como true.
- sub-rede: a sub-rede de um conjunto de dimensionamento pode ser modificada, desde que a sub-rede original e nova sub-rede estão na mesma rede virtual.

### <a name="properties-that-require-deallocation-to-change"></a>Propriedades que necessitam de Desalocação para alterar

Algumas propriedades só podem ser alteradas para determinados valores se as VMs no conjunto de dimensionamento são desalocadas. Estas propriedades incluem:

- nome do SKU: se o novo SKU de VM não é suportado no hardware de escala conjunto está atualmente ativado, tem de Desalocação as VMs na escala definido antes de modificar o nome do sku. Para obter mais informações sobre o redimensionamento de VMs, consulte [esta publicação no blogue do Azure](https://azure.microsoft.com/blog/resize-virtual-machines/).


## <a name="vm-specific-updates"></a>Atualizações específicas da VM

Determinados modificações podem ser aplicadas às VMs específicas em vez das propriedades do conjunto de dimensionamento global. Atualmente, a única atualização VM específico que seja suportada é anexar/desanexar os discos de dados da VMs no conjunto de dimensionamento. Esta funcionalidade está em pré-visualização. Para obter mais informações, consulte o [pré-visualizar documentação](https://github.com/Azure/vm-scale-sets/tree/master/preview/disk).

## <a name="scenarios-application-updates-os-updates-etc"></a>Cenários: Atualizações de aplicações, atualizações do SO, etc.

### <a name="application-updates"></a>Atualizações da aplicação

Se uma aplicação for implementada para uma escala definido através de extensões, a atualização da configuração de extensão faz com que a aplicação de atualização de acordo com a política de atualização. Por exemplo, se tiver uma nova versão de um script para executar uma extensão de script personalizado, foi possível atualizar a propriedade fileUris para apontar para o script de novo. Em alguns casos, no entanto, pode pretender forçar uma atualização, apesar da configuração de extensão é alterada (por exemplo, atualizar o script sem alterar o URI do script). Nestes casos, pode modificar o forceUpdateTag para forçar uma atualização. A plataforma do Azure não interpretar esta propriedade para que alterar o respetivo valor não tem efeito na forma como a extensão é executado. Basta modificar força a extensão para voltar a executar. Para obter mais informações sobre o forceUpdateTag, consulte o [documentação da REST API para extensões](https://docs.microsoft.com/rest/api/compute/virtualmachineextensions/createorupdate).

É também comum as aplicações implementadas através de uma imagem personalizada. Este cenário é abrangido na secção seguinte "Atualizações do SO"

### <a name="os-updates"></a>Atualizações do SO

Se estiver a utilizar imagens da plataforma, pode atualizar a imagem, modificando o atributo imageReference (mais informações no [documentação da REST API](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

>[!NOTE]
> Com imagens da plataforma, é comum para especificar "mais recente" para a versão de referência da imagem. Isto significa que durante a escala conjunto criar, escalamento horizontal e recriação de imagem, as VMs são criadas com a versão mais recente disponível. No entanto, este **não** significa que a imagem do SO será automaticamente atualizada ao longo do tempo, são lançadas novas versões de imagem. Esta é uma funcionalidade à parte, atualmente em pré-visualização. Para obter mais informações, consulte o [documentação de atualizações automáticas de SO](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade).

Se estiver a utilizar imagens personalizadas, pode atualizar a imagem, atualizando o ID de atributo imageReference (mais informações no [documentação da REST API](https://docs.microsoft.com/en-us/rest/api/compute/virtualmachinescalesets/createorupdate)).

## <a name="examples"></a>Exemplos

### <a name="updating-the-os-image-for-your-scale-set"></a>A atualização da imagem de SO para o conjunto de dimensionamento

Imaginemos que tem um conjunto com uma versão antiga do Ubuntu LTS 16.04 de dimensionamento e pretende atualizar para uma versão mais recente do Ubuntu LTS 16.04 (por exemplo, a versão 16.04.201801090). A propriedade de versão de referência de imagem não faz parte de uma lista, pelo que pode modificar diretamente estas propriedades com estes comandos:

PowerShell: `Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -ImageReferenceVersion 16.04.201801090`

CLI: `az vmss update -g {resourceGroupName} -n {vmScaleSetName} --set virtualMachineProfile.storageProfile.imageReference.version=16.04.201801090`


### <a name="updating-the-load-balancer-for-your-scale-set"></a>Atualizar o Balanceador de carga para o conjunto de dimensionamento

Imaginemos que tem uma escala definida com um balanceador de carga do Azure e que pretende substituir o Balanceador de carga do Azure com um Gateway de aplicação do Azure. As propriedades de gateway de carga balanceador e aplicação, para um conjunto de dimensionamento fazem parte de uma lista, pelo que pode utilizar os comandos para a remoção e adição de elementos de lista em vez de modificar as propriedades diretamente:

PowerShell: 
```
# get the current model of the scale set and store it in a local powershell object named $vmss
> $vmss=Get-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName}

# create a local powershell object for the new desired IP configuration, which includes the referencerence to the application gateway
> $ipconf = New-AzureRmVmssIPConfig myNic -ApplicationGatewayBackendAddressPoolsId /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendAddressPoolName} -SubnetId $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Subnet.Id –Name $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0].Name

# replace the existing IP configuration in the local powershell object (which contains the references to the current Azure Load Balancer) with the new IP configuration
> $vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].IpConfigurations[0] = $ipconf

# Update the model of the scale set with the new configuration in the local powershell object
> Update-AzureRmVmss -ResourceGroupName {resourceGroupName} -Name {vmScaleSetName} -virtualMachineScaleSet $vmss

```

CLI:
```
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerBackendAddressPools 0 # remove the load balancer backend pool from the scale set model
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --remove virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].loadBalancerInboundNatPools 0 # remove the load balancer backend pool from the scale set model; only necessary if you have NAT pools configured on the scale set
az vmss update -g {resourceGroupName} -n {vmScaleSetName} --add virtualMachineProfile.networkProfile.networkInterfaceConfigurations[0].ipConfigurations[0].ApplicationGatewayBackendAddressPools '{"id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/applicationGateways/{applicationGatewayName}/backendAddressPools/{applicationGatewayBackendPoolName}"}' # add the application gateway backend pool to the scale set model
```

>[!NOTE]
> Estes comandos pressupõem que haja apenas um balanceador de configuração e a carga IP num conjunto de dimensionamento. Se existirem vários, poderá ter de utilizar um índice de lista que não seja 0.