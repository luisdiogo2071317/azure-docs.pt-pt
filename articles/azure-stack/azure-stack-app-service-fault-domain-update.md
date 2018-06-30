---
title: 'Serviço de aplicações na pilha do Azure: a falhas de atualização de domínio | Microsoft Docs'
description: Como redistribuir o serviço de aplicações do Azure na pilha do Azure em vários domínios de falhas
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2018
ms.author: anwestg
ms.openlocfilehash: ce57e153dcab6a386150ebefe1ecb4a018514247
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130375"
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Como redistribuir o serviço de aplicações do Azure na pilha do Azure em vários domínios de falhas

*Aplica-se a: Azure pilha integrado sistemas*

Com a atualização de 1802 pilha do Azure suporta agora a distribuição de cargas de trabalho entre domínios de falhas, uma funcionalidade que é fundamental para uma elevada disponibilidade.

>[!IMPORTANT]
>Para tirar partido do suporte de domínio de falhas, tem de atualizar o sistema de pilha do Azure integrado 1802. Este documento só se aplica às implementações de fornecedor de recursos do serviço de aplicações que foram terminou antes da atualização da 1802. Se implementou o serviço de aplicações na pilha do Azure após a atualização de 1802 foi aplicada com pilha do Azure, o fornecedor de recursos já tenham sido distribuído em vários domínios de falhas.

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Rebalancear um fornecedor de recursos do serviço de aplicações em vários domínios de falhas

Para redistribuir os conjuntos de dimensionamento implementados para o fornecedor de recursos do serviço de aplicações, tem de efetuar os passos neste artigo para cada conjunto de dimensionamento. Por predefinição, os nomes de scaleset são:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

>[!NOTE]
> Se não tiver implementadas em alguns dos conjuntos de dimensionamento de camada de trabalho de instâncias, não terá de reequilibrar os conjuntos de dimensionamento. Os conjuntos de dimensionamento irão ser balanceados corretamente quando dimensiona-los no futuro.

Para ampliar os conjuntos de dimensionamento, siga estes passos:

1. Inicie sessão no Portal de administrador de pilha do Azure.
2. Selecione **mais serviços**.
3. Em COMPUTAÇÃO, selecione **conjuntos de dimensionamento de Máquina Virtual**. Conjuntos de dimensionamento existentes implementados como parte da implementação do serviço de aplicações serão listados com as informações de contagem de instâncias. A seguinte captura de ecrã mostra um exemplo de conjuntos de dimensionamento.

      ![Conjuntos de dimensionamento de serviço de aplicações do Azure listado no UX de conjuntos de dimensionamento de Máquina Virtual][1]

4. Aumentar horizontalmente cada conjunto. Por exemplo, se tiver três instâncias existentes no conjunto de dimensionamento, deve aumentar horizontalmente 6 para que as três novas instâncias são implementadas em vários domínios de falhas. Mostra o exemplo do PowerShell seguinte terminar para aumentar horizontalmente o conjunto de dimensionamento.

   ```powershell
   Add-AzureRmAccount -EnvironmentName AzureStackAdmin 

   # Get current scale set
   $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

   # Set and update the capacity of your scale set
   $vmss.sku.capacity = 6
   Update-AzureRmVmss -ResourceGroupName AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
   ```

   >[!NOTE]
   >Este passo pode demorar várias horas a concluir, dependendo do tipo de função e o número de instâncias.

5. No **funções de administração do serviço de aplicações**, monitorizar o estado das novas instâncias de função. Para verificar o estado de uma instância de função, selecione o tipo de função na lista

    ![App Service do Azure, as funções de pilha do Azure][2]

6. Quando o estado de novas instâncias de função é **pronto**, volte ao **conjunto de dimensionamento da Máquina Virtual** e **eliminar** as instâncias da função antigo.

7. Repita estes passos para **cada** conjunto de dimensionamento da máquina virtual.

## <a name="next-steps"></a>Passos Seguintes

Também pode experimentar o outro [plataforma como dos serviços de serviço (PaaS)](azure-stack-tools-paas-services.md).

* [Fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md)
* [Fornecedor de recursos de MySQL](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
