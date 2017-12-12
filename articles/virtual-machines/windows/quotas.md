---
title: vCPU quotas do Azure | Microsoft Docs
description: Saiba mais sobre vCPU quotas do Azure.
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: drewm
ms.openlocfilehash: b481299b62d452bc306c1f9c1fa2cdccd49b818e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="virtual-machine-vcpu-quotas"></a>Quotas de vCPU de máquina virtual

As quotas de vCPU para máquinas virtuais e os conjuntos de dimensionamento de máquina virtual são dispostas numa duas camadas para cada subscrição, em cada região. Primeira camada é o Total Regional vCPUs e a segunda camada é os várias VM tamanho famílias núcleos, tais como vCPUs padrão D família. Sempre que é uma nova VM implementadas a vCPUs para a VM recentemente implementada não pode exceder a quota de vCPU para a família de tamanho VM específica ou a quota do total regional vCPU. Se qualquer um desses quotas for excedido, em seguida, a implementação de VM não será permitida. Também é uma quota para o número global de máquinas virtuais na região. Os detalhes sobre cada uma destas quotas podem ser vistos no **utilização + quotas** secção o **subscrição** página no [portal do Azure](https://portal.azure.com), ou pode consultar os valores a utilizar PowerShell.

 
## <a name="check-usage"></a>Utilização de verificação

Pode utilizar o [Get-AzureRmVMUsage](/powershell/module/azurerm.compute/get-azurermvmusage) cmdlet para verificar a utilização da quota.

```azurepowershell-interactive
Get-AzureRmVMUsage -Location "East US"
```

O resultado será semelhante a isto:

```
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional vCPUs                         4   260 Count
Virtual Machines                             4 10000 Count
Virtual Machine Scale Sets                   1  2000 Count
Standard B Family vCPUs                      1    10 Count
Standard DSv2 Family vCPUs                   1   100 Count
Standard Dv2 Family vCPUs                    2   100 Count
Basic A Family vCPUs                         0   100 Count
Standard A0-A7 Family vCPUs                  0   250 Count
Standard A8-A11 Family vCPUs                 0   100 Count
Standard D Family vCPUs                      0   100 Count
Standard G Family vCPUs                      0   100 Count
Standard DS Family vCPUs                     0   100 Count
Standard GS Family vCPUs                     0   100 Count
Standard F Family vCPUs                      0   100 Count
Standard FS Family vCPUs                     0   100 Count
Standard NV Family vCPUs                     0    24 Count
Standard NC Family vCPUs                     0    48 Count
Standard H Family vCPUs                      0     8 Count
Standard Av2 Family vCPUs                    0   100 Count
Standard LS Family vCPUs                     0   100 Count
Standard Dv2 Promo Family vCPUs              0   100 Count
Standard DSv2 Promo Family vCPUs             0   100 Count
Standard MS Family vCPUs                     0     0 Count
Standard Dv3 Family vCPUs                    0   100 Count
Standard DSv3 Family vCPUs                   0   100 Count
Standard Ev3 Family vCPUs                    0   100 Count
Standard ESv3 Family vCPUs                   0   100 Count
Standard FSv2 Family vCPUs                   0   100 Count
Standard ND Family vCPUs                     0     0 Count
Standard NCv2 Family vCPUs                   0     0 Count
Standard NCv3 Family vCPUs                   0     0 Count
Standard LSv2 Family vCPUs                   0     0 Count
Standard Storage Managed Disks               2 10000 Count
Premium Storage Managed Disks                1 10000 Count

```


## <a name="reserved-vm-instances"></a>Instâncias de VM Reservada
Reservado instâncias de VM, que estão no âmbito de uma única subscrição, irá adicionar um aspeto novo para as quotas de vCPU. Estes valores descrevem o número de instâncias do tamanho declarado que tem de ser implementável na subscrição. Que funcionam como um marcador de posição no sistema de quota para garantir que esse quota está reservado para garantir que instâncias reservadas são implementáveis na subscrição. Por exemplo, se uma subscrição específica tem 10 Standard_D1 reservado instâncias limitam as utilizações para instâncias reservado Standard_D1 será 10. Isto fará com que o Azure garantir que estão sempre, pelo menos, 10 vCPUs disponíveis na quota de vCPUs Total Regional para ser utilizado para Standard_D1 instâncias e existem, pelo menos, 10 vCPUs da quota de vCPU padrão D família a ser utilizada para Standard_D1 instâncias.

Se um aumento de quota não é necessário para comprar ou um único RI de subscrição, pode [pedir um aumento de quota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) na sua subscrição.

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre faturação e de quotas, consulte [subscrição do Azure e limites de serviço, quotas e restrições](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).