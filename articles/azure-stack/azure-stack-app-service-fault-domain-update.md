---
title: 'Serviço de aplicações no Azure Stack: atualização de domínio de falha | Documentos da Microsoft'
description: Como redistribuir o serviço de aplicações do Azure no Azure Stack em domínios de falha
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
ms.date: 09/05/2018
ms.author: anwestg
ms.openlocfilehash: acadd1adec93d10d64712a2fbedb89e098998294
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44025967"
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Como redistribuir o serviço de aplicações do Azure no Azure Stack em domínios de falha

*Aplica-se a: sistemas integrados do Azure Stack*

Com a atualização 1802, Azure Stack agora suporta a distribuição de cargas de trabalho entre domínios de falha, uma funcionalidade que é fundamental para elevada disponibilidade.

>[!IMPORTANT]
>Para tirar partido do suporte de domínio de falhas, tem de atualizar o seu sistema integrado do Azure Stack para 1802. Este documento aplica-se apenas para implementações de fornecedor de recursos do serviço de aplicações que foram concluídas antes da atualização 1802. Se tiver implementado o serviço de aplicações no Azure Stack depois da atualização 1802 foi aplicada ao Azure Stack, o fornecedor de recursos já é distribuído por domínios de falha.

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Reequilibrar um fornecedor de recursos do serviço de aplicações em domínios de falha

Para redistribuir os conjuntos de dimensionamento implementados para o fornecedor de recursos do serviço de aplicações, tem de efetuar os passos neste artigo para cada conjunto de dimensionamento. Por predefinição, os nomes de conjunto de dimensionamento são:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

>[!NOTE]
> Se não tiver instâncias implementadas no alguns dos conjuntos de dimensionamento do escalão de worker, não tem de reequilibrar as esses conjuntos de dimensionamento. Os conjuntos de dimensionamento ser balanceados corretamente, quando dimensione-os no futuro.

Para aumentar horizontalmente os conjuntos de dimensionamento, siga estes passos:

1. Inicie sessão no Portal do Azure Stack administrador.
1. Selecione **Todos os serviços**.
2. Na **COMPUTAÇÃO** categoria, selecione **conjuntos de dimensionamento de máquinas virtuais**. Conjuntos de dimensionamento existente implementados como parte da implementação do serviço de aplicações serão listados com informações de contagem de instância. Captura de ecrã seguinte mostra um exemplo de conjuntos de dimensionamento.

      ![Conjuntos de dimensionamento de serviço de aplicações do Azure listados na UX de conjuntos de dimensionamento de Máquina Virtual][1]

1. Aumentar horizontalmente a cada conjunto. Por exemplo, se tiver três instâncias existentes no conjunto de dimensionamento deve aumentar horizontalmente a 6 para que as três novas instâncias são implementadas em domínios de falha. O exemplo de PowerShell seguinte mostra os para aumentar horizontalmente o conjunto de dimensionamento.

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

1. Na **funções de administração do serviço de aplicações**, monitorizar o estado das instâncias de função nova. Para verificar o estado de uma instância de função, selecione o tipo de função na lista

    ![Serviço de aplicações do Azure nas funções do Azure Stack][2]

1. Quando o estado de novas instâncias de função é **pronto**, volte ao **conjunto de dimensionamento de máquinas virtuais** e **eliminar** as instâncias de função antigo.

1. Repita estes passos para **cada** conjunto de dimensionamento de máquina virtual.

## <a name="next-steps"></a>Passos Seguintes

Também pode tentar outros [plataforma como um serviço de serviço (PaaS)](azure-stack-tools-paas-services.md).

* [Fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md)
* [Fornecedor de recursos MySQL](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
