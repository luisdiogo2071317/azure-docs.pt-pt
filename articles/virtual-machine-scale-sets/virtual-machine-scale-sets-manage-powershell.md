---
title: Gerir conjuntos de dimensionamento de Máquina Virtual com o Azure PowerShell | Microsoft Docs
description: Cmdlets do Azure PowerShell comuns para gerir conjuntos de dimensionamento de Máquina Virtual, tais como iniciar e parar uma instância ou altere a escala definir capacidade.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: 39cd7fa2232329716ba16abf92ba4a5f2cc15487
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652785"
---
# <a name="manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Gerir um conjunto com o Azure PowerShell de dimensionamento de máquina virtual
Ao longo do ciclo de vida dos conjuntos de dimensionamento de máquinas virtuais, poderá ter de executar uma ou mais tarefas de gestão. Além disso, pode querer criar scripts que automatizam várias tarefas do ciclo de vida. Este artigo fornece detalhes sobre alguns dos cmdlets Azure PowerShell comuns que permitem-lhe efetuar estas tarefas.

Para concluir estas tarefas de gestão, terá do módulo Azure PowerShell mais recente. Para informações, consulte [introdução ao Azure PowerShell](/powershell/azure/get-started-azureps). Se precisar de criar um conjunto de dimensionamento de máquina virtual, pode [criar um conjunto com o Azure PowerShell de dimensionamento](quick-create-powershell.md).


## <a name="view-information-about-a-scale-set"></a>Ver informações sobre um conjunto de dimensionamento
Para ver a informação geral sobre um conjunto de dimensionamento, utilize [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss). O exemplo seguinte obtém informações sobre o conjunto nomeado de dimensionamento *myScaleSet* no *myResourceGroup* grupo de recursos. Introduza os seus próprios nomes da seguinte forma:

```powershell
Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="view-vms-in-a-scale-set"></a>Ver VMs num conjunto de dimensionamento
Para ver uma lista de instância de VM num conjunto de dimensionamento, utilize [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm). O exemplo seguinte apresenta uma lista de todas as instâncias de VM o conjunto nomeado de dimensionamento *myScaleSet* e no *myResourceGroup* grupo de recursos. Forneça os seus próprios valores para estes nomes de:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Para ver informações adicionais sobre uma instância VM específica, adicione o `-InstanceId` parâmetro [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) e especifique uma instância para ver. O exemplo seguinte visualiza informações sobre a instância de VM *0* no conjunto nomeado de dimensionamento *myScaleSet* e *myResourceGroup* grupo de recursos. Introduza os seus próprios nomes da seguinte forma:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="change-the-capacity-of-a-scale-set"></a>Alterar a capacidade de um conjunto de dimensionamento
Os comandos anteriores mostrou informações sobre o conjunto de dimensionamento e as instâncias de VM. Para aumentar ou reduzir o número de instâncias no conjunto de dimensionamento, pode alterar a capacidade. A escala definida automaticamente cria ou remove o número necessário de VMs, em seguida, configura as VMs para receber tráfego de aplicações.

Em primeiro lugar, crie um objeto de conjunto de dimensionamento com [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) e especifique um valor novo para `sku.capacity`. Para aplicar a alteração de capacidade, utilize [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). As seguintes atualizações de exemplo *myScaleSet* no *myResourceGroup* grupo de recursos para uma capacidade de *5* instâncias. Fornece os seus próprios valores da seguinte forma:

```powershell
# Get current scale set
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss
```

São necessários alguns minutos para atualizar a capacidade do seu conjunto de dimensionamento. Se diminuir a capacidade de uma escala definido, as VMs com a instância mais elevada IDs são removidos pela primeira vez.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Parar e iniciar as VMs num conjunto de dimensionamento
Para parar uma ou mais VMs num conjunto de dimensionamento, utilize [Stop-AzureRmVmss](/powershell/module/azurerm.compute/stop-azurermvmss). O parâmetro `-InstanceId` permite-lhe especificar uma ou mais VMs que deverão ser paradas. Se não especificar um ID de instância, todas as VMs no conjunto de dimensionamento são paradas. Para deixar de várias VMs, separe cada ID de instância com uma vírgula.

O exemplo seguinte deixa de instância *0* no conjunto nomeado de dimensionamento *myScaleSet* e *myResourceGroup* grupo de recursos. Fornece os seus próprios valores da seguinte forma:

```powershell
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

Por predefinição, as VMs paradas são desalocadas e não implicam custos de computação. Se quiser que a VM permaneça no estado aprovisionado quando for parada, adicione o parâmetro `-StayProvisioned` ao comando anterior. As VMs paradas que continuem aprovisionadas incorrem em custos de computação regulares.


### <a name="start-vms-in-a-scale-set"></a>Iniciar VMs num conjunto de dimensionamento
Para iniciar uma ou mais VMs num conjunto de dimensionamento, utilize [Start-AzureRmVmss](/powershell/module/azurerm.compute/start-azurermvmss). O parâmetro `-InstanceId` permite-lhe especificar uma ou mais VMs que deverão ser iniciadas. Se não especificar um ID de instância, todas as VMs no conjunto de dimensionamento são iniciadas. Para começar a várias VMs, separe cada ID de instância com uma vírgula.

O exemplo seguinte inicia a instância *0* no conjunto nomeado de dimensionamento *myScaleSet* e *myResourceGroup* grupo de recursos. Fornece os seus próprios valores da seguinte forma:

```powershell
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="restart-vms-in-a-scale-set"></a>Reinicie as VMs num conjunto de dimensionamento
Para reiniciar uma ou mais VMs num conjunto de dimensionamento, utilize [Retart-AzureRmVmss](/powershell/module/azurerm.compute/restart-azurermvmss). O parâmetro `-InstanceId` permite-lhe especificar uma ou mais VMs que deverão ser reinciadas. Se não especificar um ID de instância, todas as VMs no conjunto de dimensionamento são reiniciadas. Para reiniciar várias VMs, separe cada ID de instância com uma vírgula.

O exemplo seguinte reinicia instância *0* no conjunto nomeado de dimensionamento *myScaleSet* e *myResourceGroup* grupo de recursos. Fornece os seus próprios valores da seguinte forma:

```powershell
Restart-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="remove-vms-from-a-scale-set"></a>Remover VMs a partir de um conjunto de dimensionamento
Para remover uma ou mais VMs num conjunto de dimensionamento, utilize [remover AzureRmVmss](/powershell/module/azurerm.compute/remove-azurermvmss). O `-InstanceId` parâmetro permite-lhe especificar uma ou mais VMs para remover. Se não especificar um ID de instância, são removidas todas as VMs no conjunto de dimensionamento. Para remover várias VMs, separe cada ID de instância com uma vírgula.

O exemplo a seguir remove instância *0* no conjunto nomeado de dimensionamento *myScaleSet* e *myResourceGroup* grupo de recursos. Fornece os seus próprios valores da seguinte forma:

```powershell
Remove-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="next-steps"></a>Passos Seguintes
Outras tarefas comuns para conjuntos de dimensionamento incluem como [implementar uma aplicação](virtual-machine-scale-sets-deploy-app.md), e [atualizar instâncias de VM](virtual-machine-scale-sets-upgrade-scale-set.md). Também pode utilizar o Azure PowerShell para [configurar regras de dimensionamento automático](virtual-machine-scale-sets-autoscale-overview.md).
