---
title: Tutorial de conjuntos de disponibilidade para VMs do Windows no Azure | Microsoft Docs
description: Saiba mais sobre os Conjuntos de Disponibilidade para VMs do Windows no Azure.
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: tutorial
ms.date: 02/09/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 247f86dafe35d69dd742583d246862b739d9fe90
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-use-availability-sets"></a>Como utilizar os conjuntos de disponibilidade

Neste tutorial, irá aprender a aumentar a disponibilidade e a fiabilidade das suas soluções de Máquina Virtual no Azure através de uma função chamada Conjuntos de Disponibilidade. Os conjuntos de disponibilidade garantem que as VMs que implementa no Azure são distribuídas por vários nós de hardware isolados num cluster. Fazer isto garante que, se ocorrer uma falha de hardware ou software no Azure, apenas um subconjunto das suas VMs é afetado e que a solução global permanece disponível e operacional. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um conjunto de disponibilidade
> * Criar uma VM num conjunto de disponibilidade
> * Verificar os tamanhos de VM disponíveis
> * Verificar o Assistente do Azure

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer a versão 5.3 ou posterior do módulo Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure. 

## <a name="availability-set-overview"></a>Descrição geral do conjunto de disponibilidade

Um Conjunto de Disponibilidade é uma função de agrupamento lógico que pode utilizar no Azure para garantir que os recursos de VM que nele colocar estão isolados uns dos outros quando são implementados num datacenter do Azure. O Azure garante que as VMs que colocar num Conjunto de Disponibilidade são executadas em vários servidores físicos, suportes de computação, unidades de armazenamento e comutadores de rede. Se ocorrer uma falha de software do Azure ou de hardware, apenas um subconjunto das suas VMs será afetado, e a aplicação global mantém-se e continua disponível para os seus clientes. Os Conjuntos de Disponibilidade são uma função essencial quando pretende criar soluções cloud fiáveis.

Consideremos uma solução típica baseada em VM em que poderá ter quatro servidores Web de front-end e utilizar duas VMs de back-end que alojam uma base de dados. Com o Azure, irá definir dois conjuntos de disponibilidade antes de implementar as suas VMs: um conjunto de disponibilidade para a camada Web e um conjunto de disponibilidade para a camada de base de dados. Quando cria uma nova VM, pode especificar o conjunto de disponibilidade como um parâmetro para o comando az vm create, e o Azure garante automaticamente que as VMs que criar no conjunto disponível ficam isoladas em vários recursos de hardware físico. Se o hardware físico em execução por uma das VMs do Servidor Web ou Servidor de Base de Dados tiver um problema, sabe que as outras instâncias das VMs do Servidor Web e de Base de Dados permanecem em execução porque estão em hardware diferente.

Utilize Conjuntos de Disponibilidade quando pretender implementar soluções fiáveis baseadas em VM no Azure.

## <a name="create-an-availability-set"></a>Criar um conjunto de disponibilidade

Pode criar um conjunto de disponibilidade com [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Neste exemplo, define o número de domínios de atualização e com falha como *2* para o conjunto de disponibilidade designado *myAvailabilitySet* no grupo de recursos *myResourceGroupAvailability*.

Crie um grupo de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```

Crie um conjunto de disponibilidade gerido com [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) com o parâmetro `-sku aligned`.

```azurepowershell-interactive
New-AzureRmAvailabilitySet `
   -Location "EastUS" `
   -Name "myAvailabilitySet" `
   -ResourceGroupName "myResourceGroupAvailability" `
   -Sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Criar VMs num conjunto de disponibilidade
As VMs têm de ser criadas no conjunto de disponibilidade para garantir que são distribuídas corretamente pelo hardware. Não é possível adicionar uma VM existente a um conjunto de disponibilidade depois de ter sido criado. 

O hardware numa localização está dividido em vários domínios de atualização e domínios de falhas. Um **domínio de atualização** é um grupo de VMs e hardware físico subjacente que pode ser reiniciado ao mesmo tempo. As VMs no mesmo **domínio de falha** partilham o mesmo armazenamento, bem como a fonte de energia e o comutador de rede. 

Quando cria uma VM com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm), utilize o parâmetro `-AvailabilitySetName` para especificar o nome do conjunto de disponibilidade.

Primeiro, defina um nome de utilizador e palavra-passe para a VM com [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Agora crie duas VMs com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) no conjunto de disponibilidade.

```azurepowershell-interactive
for ($i=1; $i -le 2; $i++)
{
    New-AzureRmVm `
        -ResourceGroupName "myResourceGroupAvailability" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -PublicIpAddressName "myPublicIpAddress$i" `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred
}
```

O parâmetro `-AsJob` cria a VM como uma tarefa em segundo plano, para que os pedidos do PowerShell voltem para si. Pode ver os detalhes das tarefas em segundo plano com o cmdlet `Job`. Demora alguns minutos para criar e configurar ambas as VMs. Quando terminar, tem duas máquinas virtuais distribuídas no hardware subjacente. 

Se observar o conjunto de disponibilidade no portal, acedendo a Grupos de Recursos > myResourceGroupAvailability > myAvailabilitySet, deverá ver como as VMs estão distribuídas pelos dois domínios com falha e de atualização.

![Conjunto de disponibilidade no portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Verificar os tamanhos de VM disponíveis 

Pode adicionar mais VMs ao conjunto de disponibilidade posteriormente, mas tem de conhecer os tamanhos de VM que estão disponíveis no hardware. Utilize [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) para listar todos os tamanhos disponíveis no hardware de cluster para o conjunto de disponibilidade.

```azurepowershell-interactive
Get-AzureRmVMSize `
   -ResourceGroupName "myResourceGroupAvailability" `
   -AvailabilitySetName "myAvailabilitySet"
```

## <a name="check-azure-advisor"></a>Verificar o Assistente do Azure 

Também pode utilizar o Assistente do Azure para obter mais informações sobre formas de melhorar a disponibilidade das suas VMs. O Assistente do Azure ajuda a seguir as melhores práticas para otimizar as suas implementações do Azure. Ele analisa a sua telemetria de configuração e utilização de recursos e, em seguida, recomenda soluções que o podem ajudar a melhorar a rentabilidade, o desempenho, a elevada disponibilidade e a segurança dos seus recursos do Azure.

Inicie sessão no [portal do Azure](https://portal.azure.com), selecione **Todos os serviços** e escreva **Assistente**. O dashboard do Assistente apresenta recomendações personalizadas para a subscrição selecionada. Para obter mais informações, veja [Introdução ao Assistente do Azure](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um conjunto de disponibilidade
> * Criar uma VM num conjunto de disponibilidade
> * Verificar os tamanhos de VM disponíveis
> * Verificar o Assistente do Azure

Avance para o tutorial seguinte para saber mais sobre os conjuntos de dimensionamento de máquinas virtuais.

> [!div class="nextstepaction"]
> [Criar um conjunto de dimensionamento de VM](tutorial-create-vmss.md)


