---
title: vCPU quotas do Azure | Microsoft Docs
description: Saiba mais sobre vCPU quotas do Azure.
keywords: 
services: virtual-machines-linux
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: drewm
ms.openlocfilehash: 8b99fd92d9031b7172e698cf5db1f776387bdfb9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="virtual-machine-vcpu-quotas"></a>Quotas de vCPU de máquina virtual

As quotas de vCPU para máquinas virtuais e os conjuntos de dimensionamento de máquina virtual são dispostas numa duas camadas para cada subscrição, em cada região. Primeira camada é o Total Regional vCPUs e a segunda camada é os várias VM tamanho famílias núcleos, tais como vCPUs padrão D família. Sempre que é uma nova VM implementadas a vCPUs para a VM recentemente implementada não pode exceder a quota de vCPU para a família de tamanho VM específica ou a quota do total regional vCPU. Se qualquer um desses quotas for excedido, em seguida, a implementação de VM não será permitida. Também é uma quota para o número global de máquinas virtuais na região. Os detalhes sobre cada uma destas quotas podem ser vistos no **utilização + quotas** secção o **subscrição** página no [portal do Azure](https://portal.azure.com), ou pode consultar os valores utilizando a CLI do Azure .


## <a name="check-usage"></a>Utilização de verificação

Pode verificar a sua utilização de quota utilizando [lista-utilização az vm](/cli/azure/vm#az_vm_list_usage).

```azurecli-interactive
az vm list-usage --location "East US"
[
  …
  {
    "currentValue": 4,
    "limit": 260,
    "name": {
      "localizedValue": "Total Regional vCPUs",
      "value": "cores"
    }
  },
  {
    "currentValue": 4,
    "limit": 10000,
    "name": {
      "localizedValue": "Virtual Machines",
      "value": "virtualMachines"
    }
  },
  {
    "currentValue": 1,
    "limit": 2000,
    "name": {
      "localizedValue": "Virtual Machine Scale Sets",
      "value": "virtualMachineScaleSets"
    }
  },
  {
    "currentValue": 1,
    "limit": 10,
    "name": {
      "localizedValue": "Standard B Family vCPUs",
      "value": "standardBFamily"
    }
  },
```
## <a name="reserved-vm-instances"></a>Instâncias de VM Reservada
Reservado instâncias de VM, que estão no âmbito de uma única subscrição, irá adicionar um aspeto novo para as quotas de vCPU. Estes valores descrevem o número de instâncias do tamanho declarado que tem de ser implementável na subscrição. Que funcionam como um marcador de posição no sistema de quota para garantir que esse quota está reservado para garantir que instâncias reservadas são implementáveis na subscrição. Por exemplo, se uma subscrição específica tem 10 Standard_D1 reservado instâncias limitam as utilizações para instâncias reservado Standard_D1 será 10. Isto fará com que o Azure garantir que estão sempre, pelo menos, 10 vCPUs disponíveis na quota de vCPUs Total Regional para ser utilizado para Standard_D1 instâncias e existem, pelo menos, 10 vCPUs da quota de vCPU padrão D família a ser utilizada para Standard_D1 instâncias.

Se um aumento de quota não é necessário para comprar ou um único RI de subscrição, pode [pedir um aumento de quota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) na sua subscrição.

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre faturação e de quotas, consulte [subscrição do Azure e limites de serviço, quotas e restrições](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=/azure/billing/TOC.json).
