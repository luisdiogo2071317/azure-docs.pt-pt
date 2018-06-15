---
title: Aumentar horizontalmente as funções de trabalho nos serviços de aplicações - pilha do Azure | Microsoft Docs
description: Orientação detalhada para o dimensionamento dos serviços de aplicação de pilha do Azure
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 04a93bc841d553296dca7635151c14892970121c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34357797"
---
# <a name="app-service-on-azure-stack-add-more-infrastructure-or-worker-roles"></a>Serviço de aplicações na pilha do Azure: adicionar várias funções de infraestrutura ou de trabalho

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*  

Este documento fornece instruções sobre como dimensionar o serviço de aplicações nas funções de infraestrutura e de trabalho de pilha do Azure. Contém passos para criar funções de trabalho adicionais para suportar aplicações de qualquer dimensão.

> [!NOTE]
> Se o seu ambiente de pilha do Azure tem mais do que 96 GB de RAM, poderá ter dificuldades adicionar capacidade adicional.

Serviço de aplicações na pilha do Azure, por predefinição, suporta escalões gratuito e partilhado de trabalho. Para adicionar outros escalões de trabalho, terá de adicionar mais funções de trabalho.

Se não tem a certeza de que foi implementado com a predefinição do serviço de aplicações numa instalação de pilha do Azure, pode rever informações adicionais a [do serviço de aplicações na descrição geral do Azure pilha](azure-stack-app-service-overview.md).

App Service do Azure na pilha do Azure implementa todas as funções utilizando conjuntos de dimensionamento de Máquina Virtual e como tal tira partido das capacidades de dimensionamento desta carga de trabalho. Por conseguinte, todas as dimensionamento das camadas de trabalho é feita através do administrador de serviço de aplicações.

> [!IMPORTANT]
> Atualmente não é possível dimensionar conjuntos de dimensionamento de máquina virtual no portal, conforme identificado nas notas de versão de pilha do Azure, por conseguinte, utilize o exemplo do PowerShell para aumentar horizontalmente.
>
>

## <a name="add-additional-workers-with-powershell"></a>Adicionar técnicos adicionais com o PowerShell

1. [Configurar o ambiente de administrador de pilha do Azure no PowerShell](azure-stack-powershell-configure-admin.md)

2. Utilize este exemplo para aumentar horizontalmente o conjunto de dimensionamento:
   ```powershell
   
    ##### Scale out the AppService Role instances ######
   
    # Set context to AzureStack admin.
    Login-AzureRmAccount -EnvironmentName AzureStackAdmin
                                                 
    ## Name of the Resource group where AppService is deployed.
    $AppServiceResourceGroupName = "AppService.local"

    ## Name of the ScaleSet : e.g. FrontEndsScaleSet, ManagementServersScaleSet, PublishersScaleSet , LargeWorkerTierScaleSet,      MediumWorkerTierScaleSet, SmallWorkerTierScaleSet, SharedWorkerTierScaleSet
    $ScaleSetName = "SharedWorkerTierScaleSet"

    ## TotalCapacity is sum of the instances needed at the end of operation. 
    ## e.g. if your VMSS has 1 instance(s) currently and you need 1 more the TotalCapacity should be set to 2
    $TotalCapacity = 2  

    # Get current scale set
    $vmss = Get-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -VMScaleSetName $ScaleSetName

    # Set and update the capacity
    $vmss.sku.capacity = $TotalCapacity
    Update-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -Name $ScaleSetName -VirtualMachineScaleSet $vmss 
   ```    

   > [!NOTE]
   > Este passo pode demorar várias horas a concluir, dependendo do tipo de função e o número de instâncias.
   >
   >

3. Monitorizar o estado das novas instâncias de função na administração de serviço de aplicações, para verificar o estado de uma instância de função individual clique no tipo de função na lista.

## <a name="add-additional-workers-directly-within-the-app-service-resource-provider-admin"></a>Adicionar técnicos adicionais diretamente no Admin de fornecedor de recursos de serviço de aplicações

1. Inicie sessão no portal de administração do Azure pilha como o administrador de serviço.

2. Navegue até à **serviços aplicacionais**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)

3. Clique em **funções**. Aqui pode ver a repartição de todas as funções do serviço de aplicações implementadas.

4. Clique na linha do tipo pretende dimensionar e, em seguida, clique em **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)

5. Clique em **dimensionamento**, selecione o número de instâncias que pretende dimensionar e, em seguida, clique em **guardar**.

    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. Serviço de aplicações na pilha do Azure irá agora adição de mais VMs, configurá-los, instalar o software necessário e marcá-los como preparada quando este processo esteja concluído. Este processo pode demorar cerca de 80 minutos.

7. Pode monitorizar o progresso da disponibilidade das novas funções ao visualizar os trabalhadores no **funções** painel.

## <a name="result"></a>Resultado

Depois de estarem totalmente implementado e pronto, os trabalhadores ficam disponíveis para os utilizadores implementar a sua carga de trabalho no-los. O seguinte mostra um exemplo dos vários escalões de preços disponíveis por predefinição. Se não houver nenhuma trabalhadores disponíveis para uma camada de trabalho específica, a opção de escolher o escalão de preço correspondente não está disponível.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Para ampliar a gestão, funções de Front-End ou publicador adicionar que tem ampliar o tipo de função correspondente. 
>
>

Para ampliar a gestão, Front-End ou funções do publicador, siga os mesmos passos selecionar o tipo de função adequados. Os controladores não são implementados como conjuntos de dimensionamento e, por conseguinte, duas devem ser implementadas no momento de instalação para todas as implementações de produção.

### <a name="next-steps"></a>Passos Seguintes

[Configurar origens de implementação](azure-stack-app-service-configure-deployment-sources.md)
