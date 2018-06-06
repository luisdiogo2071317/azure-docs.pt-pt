---
title: vCPU quotas do Azure | Microsoft Docs
description: Saiba mais sobre vCPU quotas do Azure.
keywords: ''
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: a880ee18bb13b2cd8471cc58157469555397b872
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34716521"
---
# <a name="virtual-machine-vcpu-quotas"></a>Quotas de vCPU de máquina virtual

As quotas de vCPU para máquinas virtuais e os conjuntos de dimensionamento de máquina virtual são dispostas numa duas camadas para cada subscrição, em cada região. Primeira camada é o Total Regional vCPUs e a segunda camada é os várias VM tamanho famílias núcleos, tais como o vCPUs série D. Sempre que é uma nova VM implementadas a vCPUs para a VM não pode exceder a quota de vCPU para a família de tamanho VM ou a quota do total regional vCPU. Se qualquer um desses quotas for excedido, a implementação de VM não será permitida. Também é uma quota para o número global de máquinas virtuais na região. Os detalhes sobre cada uma destas quotas podem ser vistos no **utilização + quotas** secção o **subscrição** página no [portal do Azure](https://portal.azure.com), ou pode consultar os valores de utilização do Azure CLI.


## <a name="check-usage"></a>Utilização de verificação

Pode verificar a sua utilização de quota utilizando [lista-utilização az vm](/cli/azure/vm#az_vm_list_usage).

```azurecli-interactive
az vm list-usage --location "East US" -o table
```

O resultado deverá ter um aspeto semelhante ao seguinte:


```
Name                                CurrentValue    Limit
--------------------------------  --------------  -------
Availability Sets                              0     2000
Total Regional vCPUs                          29      100
Virtual Machines                               7    10000
Virtual Machine Scale Sets                     0     2000
Standard DSv3 Family vCPUs                     8      100
Standard DSv2 Family vCPUs                     3      100
Standard Dv3 Family vCPUs                      2      100
Standard D Family vCPUs                        8      100
Standard Dv2 Family vCPUs                      8      100
Basic A Family vCPUs                           0      100
Standard A0-A7 Family vCPUs                    0      100
Standard A8-A11 Family vCPUs                   0      100
Standard DS Family vCPUs                       0      100
Standard G Family vCPUs                        0      100
Standard GS Family vCPUs                       0      100
Standard F Family vCPUs                        0      100
Standard FS Family vCPUs                       0      100
Standard Storage Managed Disks                 5    10000
Premium Storage Managed Disks                  5    10000
```

## <a name="reserved-vm-instances"></a>Instâncias de VM Reservada
Reservado instâncias de VM, que estão no âmbito de uma única subscrição, irá adicionar um aspeto novo para as quotas de vCPU. Estes valores descrevem o número de instâncias do tamanho declarado que tem de ser implementável na subscrição. Que funcionam como um marcador de posição no sistema de quota para garantir que esse quota está reservado para garantir que instâncias reservadas são implementáveis na subscrição. Por exemplo, se uma subscrição específica tem 10 Standard_D1 reservado instâncias limitam as utilizações para instâncias reservado Standard_D1 será 10. Isto fará com que o Azure garantir que estão sempre, pelo menos, 10 vCPUs disponíveis na quota de vCPUs Total Regional para ser utilizado para Standard_D1 instâncias e existem, pelo menos, 10 vCPUs da quota de vCPU padrão D família a ser utilizada para Standard_D1 instâncias.

Se um aumento de quota não é necessário para comprar ou um único RI de subscrição, pode [pedir um aumento de quota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) na sua subscrição.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre faturação e de quotas, consulte [subscrição do Azure e limites de serviço, quotas e restrições](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
