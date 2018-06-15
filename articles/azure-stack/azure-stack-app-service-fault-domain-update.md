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
ms.date: 03/09/2018
ms.author: anwestg
ms.openlocfilehash: 42adef66fb1b1141ab44aab3a1ccdaae022202b5
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32150979"
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Como redistribuir o serviço de aplicações do Azure na pilha do Azure em vários domínios de falhas

*Aplica-se a: Azure pilha integrado sistemas*

Com a atualização de 1802 pilha do Azure suporta agora a distribuição de cargas de trabalho entre domínios de falhas, uma funcionalidade, o que é fundamental para uma elevada disponibilidade.

> [!IMPORTANT]
> Tem atualizou o sistema de pilha do Azure integrado para 1802 ser capazes de tirar proveito do suporte de domínio de falhas.  Este documento só se aplica às implementações de fornecedor de recursos do serviço de aplicações que foram concluídas antes da atualização 1802.  Se tiver implementado o serviço de aplicações na pilha do Azure após a atualização de 1802 foi aplicada com pilha do Azure, o fornecedor de recursos já tenham sido distribuído em vários domínios de falhas.
>
>

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Rebalancear um fornecedor de recursos do serviço de aplicações em vários domínios de falhas

Para redistribuir os conjuntos de dimensionamento implementados para o fornecedor de recursos do serviço de aplicações, tem de efetuar os seguintes passos para cada conjunto de dimensionamento.  Por predefinição, os nomes de scaleset são:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

> [!NOTE]
> Não se tiver nenhuma instância implementada em alguns dos conjuntos de dimensionamento de camada de trabalho, não terá de reequilibrar os conjuntos de dimensionamento.  Os conjuntos de dimensionamento irão ser balanceados corretamente quando dimensiona-los no futuro.
>
>

1. Navegue para os conjuntos de dimensionamento de Máquina Virtual no Portal de administrador de pilha do Azure.  Conjuntos de dimensionamento existentes implementados como parte da implementação do serviço de aplicações serão listados com as informações de contagem de instâncias.

    ![Conjuntos de dimensionamento de serviço de aplicações do Azure listado no UX de conjuntos de dimensionamento de Máquina Virtual][1]

2. Ampliar junto a cada conjunto.  Por exemplo, se tiver três instâncias existentes no conjunto de dimensionamento, deve aumentar horizontalmente a 6 para que as novas instâncias de três serão aprovisionadas entre domínios de falhas.
    a. [Configurar o ambiente de administrador de pilha do Azure no PowerShell](azure-stack-powershell-configure-admin.md) b. Utilize este exemplo para aumentar horizontalmente o conjunto de dimensionamento:
        ```powershell
                Add-AzureRmAccount -EnvironmentName AzureStackAdmin 

                # Get current scale set
                $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

                # Set and update the capacity of your scale set
                $vmss.sku.capacity = 6
                Update-AzureRmVmss -ResourceGroupName "AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
        '''
> [!NOTE]
> Este passo pode demorar várias horas a concluir, dependendo do tipo de função e o número de instâncias.
>
>

3. Monitorize o estado das novas instâncias de função no painel de funções de administração do serviço de aplicações.  Verificar o estado de uma instância de função individual clicando o tipo de função na lista

    ![App Service do Azure, as funções de pilha do Azure][2]

4. Um novas instâncias estão num **pronto** Estado, volte ao painel do conjunto de dimensionamento da Máquina Virtual e **eliminar** as instâncias antigas.

5. Repita estes passos para **cada** conjunto de dimensionamento da máquina virtual.

## <a name="next-steps"></a>Passos Seguintes

Também pode experimentar o outro [plataforma como dos serviços de serviço (PaaS)](azure-stack-tools-paas-services.md).

* [Fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md)
* [Fornecedor de recursos de MySQL](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
