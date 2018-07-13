---
title: Gerir conjuntos de dimensionamento de máquinas virtuais com o Azure PowerShell | Documentos da Microsoft
description: Capacidade do conjunto de cmdlets do Azure PowerShell comuns para gerir conjuntos de dimensionamento de máquinas virtuais, tais como iniciar e parar uma instância ou alterar a escala.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: cynthn
ms.openlocfilehash: a300e2f2febab8436f8d52b71955b3614bd10605
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38707447"
---
# <a name="manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Gerir um conjunto de dimensionamento com o Azure PowerShell
Ao longo do ciclo de vida dos conjuntos de dimensionamento de máquinas virtuais, poderá ter de executar uma ou mais tarefas de gestão. Além disso, pode querer criar scripts que automatizam várias tarefas do ciclo de vida. Este artigo fornece detalhes sobre alguns dos cmdlets Azure PowerShell comuns que permitem-lhe realizar estas tarefas.

Para concluir estas tarefas de gestão, terá do módulo Azure PowerShell mais recente. Para obter informações, consulte [introdução ao Azure PowerShell](/powershell/azure/get-started-azureps). Se precisar de criar um conjunto de dimensionamento de máquina virtual, pode [criar conjunto de dimensionamento com o Azure PowerShell](quick-create-powershell.md).


## <a name="view-information-about-a-scale-set"></a>Ver informações sobre um conjunto de dimensionamento
Para ver as informações gerais sobre um conjunto de dimensionamento, utilize [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss). O exemplo seguinte obtém informações sobre o conjunto nomeado de dimensionamento *myScaleSet* no *myResourceGroup* grupo de recursos. Introduza os seus próprios nomes da seguinte forma:

```powershell
Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="view-vms-in-a-scale-set"></a>Ver VMs num conjunto de dimensionamento
Para ver uma lista de instância de VM num conjunto de dimensionamento, utilize [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm). O exemplo seguinte lista todas as instâncias VM no conjunto nomeado de dimensionamento *myScaleSet* e, no *myResourceGroup* grupo de recursos. Fornece seus próprios valores para estes nomes:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Para ver informações adicionais sobre uma instância VM específica, adicione a `-InstanceId` parâmetro [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) e especifique uma instância para ver. O exemplo seguinte mostra informações sobre a instância de VM *0* no conjunto nomeado de dimensionamento *myScaleSet* e o *myResourceGroup* grupo de recursos. Introduza os seus próprios nomes da seguinte forma:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="change-the-capacity-of-a-scale-set"></a>Alterar a capacidade de um conjunto de dimensionamento
Os comandos anteriores mostraram informações sobre o conjunto de dimensionamento e as instâncias de VM. Para aumentar ou diminuir o número de instâncias no conjunto de dimensionamento, pode alterar a capacidade. A escala definida automaticamente cria ou remove o número necessário de VMs, e configura as VMs para receber o tráfego de aplicativo.

Em primeiro lugar, crie um objeto de conjunto de dimensionamento com [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) e especifique um valor novo para `sku.capacity`. Para aplicar a alteração de capacidade, utilize [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). As seguintes atualizações de exemplo *myScaleSet* no *myResourceGroup* grupo de recursos com uma capacidade de *5* instâncias. Fornece seus próprios valores da seguinte forma:

```powershell
# Get current scale set
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss
```

São necessários alguns minutos para atualizar a capacidade do seu conjunto de dimensionamento. Se diminuir a capacidade de uma escala definido, as VMs com a instância mais alta IDs são removidas primeiro.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Parar e iniciar VMs num conjunto de dimensionamento
Para parar uma ou mais VMs num conjunto de dimensionamento, utilize [Stop-AzureRmVmss](/powershell/module/azurerm.compute/stop-azurermvmss). O parâmetro `-InstanceId` permite-lhe especificar uma ou mais VMs que deverão ser paradas. Se não especificar um ID de instância, todas as VMs no conjunto de dimensionamento são paradas. Para parar de várias VMs, separe cada ID de instância com uma vírgula.

O exemplo seguinte para a instância *0* no conjunto nomeado de dimensionamento *myScaleSet* e o *myResourceGroup* grupo de recursos. Fornece seus próprios valores da seguinte forma:

```powershell
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

Por predefinição, as VMs paradas são desalocadas e não implicam custos de computação. Se quiser que a VM permaneça no estado aprovisionado quando for parada, adicione o parâmetro `-StayProvisioned` ao comando anterior. As VMs paradas que continuem aprovisionadas incorrem em custos de computação regulares.


### <a name="start-vms-in-a-scale-set"></a>Iniciar VMs num conjunto de dimensionamento
Para iniciar uma ou mais VMs num conjunto de dimensionamento, utilize [Start-AzureRmVmss](/powershell/module/azurerm.compute/start-azurermvmss). O parâmetro `-InstanceId` permite-lhe especificar uma ou mais VMs que deverão ser iniciadas. Se não especificar um ID de instância, todas as VMs no conjunto de dimensionamento são iniciadas. Para começar a várias VMs, separe cada ID de instância com uma vírgula.

O exemplo seguinte inicia a instância *0* no conjunto nomeado de dimensionamento *myScaleSet* e o *myResourceGroup* grupo de recursos. Fornece seus próprios valores da seguinte forma:

```powershell
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="restart-vms-in-a-scale-set"></a>Reiniciar VMs num conjunto de dimensionamento
Para reiniciar uma ou mais VMs num conjunto de dimensionamento, utilize [Retart-AzureRmVmss](/powershell/module/azurerm.compute/restart-azurermvmss). O parâmetro `-InstanceId` permite-lhe especificar uma ou mais VMs que deverão ser reinciadas. Se não especificar um ID de instância, todas as VMs no conjunto de dimensionamento são reiniciadas. Para reiniciar várias VMs, separe cada ID de instância com uma vírgula.

O seguinte exemplo reinicia a instância *0* no conjunto nomeado de dimensionamento *myScaleSet* e o *myResourceGroup* grupo de recursos. Fornece seus próprios valores da seguinte forma:

```powershell
Restart-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="remove-vms-from-a-scale-set"></a>Remover as VMs a partir de um conjunto de dimensionamento
Para remover uma ou mais VMs num conjunto de dimensionamento, utilize [Remove-AzureRmVmss](/powershell/module/azurerm.compute/remove-azurermvmss). O `-InstanceId` parâmetro permite-lhe especificar uma ou mais VMs para remover. Se não especificar um ID de instância, todas as VMs no conjunto de dimensionamento são removidas. Para remover várias VMs, separe cada ID de instância com uma vírgula.

O exemplo seguinte remove a instância *0* no conjunto nomeado de dimensionamento *myScaleSet* e o *myResourceGroup* grupo de recursos. Fornece seus próprios valores da seguinte forma:

```powershell
Remove-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="next-steps"></a>Passos Seguintes
Incluem outras tarefas comuns para conjuntos de dimensionamento como [implementar uma aplicação](virtual-machine-scale-sets-deploy-app.md), e [atualizar instâncias VM](virtual-machine-scale-sets-upgrade-scale-set.md). Também pode utilizar o Azure PowerShell para [configurar regras de dimensionamento automático](virtual-machine-scale-sets-autoscale-overview.md).
