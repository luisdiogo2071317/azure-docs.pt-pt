---
title: Gerir domínios de falha em conjuntos de dimensionamento de máquina virtual do Azure | Documentos da Microsoft
description: Saiba como escolher o número certo de domínios de falha ao conjunto de dimensionamento de máquinas virtuais a criar.
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: drewm
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/18
ms.author: rajraj
ms.openlocfilehash: 3b5568e0eed06684667b8aeefa389b6d0aab2a5f
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53636179"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Escolher o número certo de domínios de falha para o conjunto de dimensionamento de máquina virtual
Os conjuntos de dimensionamento de máquinas virtuais são criados com cinco domínios de falha, por predefinição em regiões do Azure com nenhuma zona. Para as regiões que suportam a implementação de zonal dos conjuntos de dimensionamento de máquina virtual, o valor predefinido da contagem de domínios de falhas é 1 para cada uma das zonas. FD = 1 implica, neste caso, que as instâncias VM que pertença ao conjunto de dimensionamento se espalham pelos muitos racks na base de melhor esforço.

Também pode considerar a alinhar o número de domínios de falha de conjunto de dimensionamento com o número de domínios de falha de Managed Disks. Este alinhamento pode ajudar a evitar a perda de quórum se falhar um domínio de falhas de Managed Disks completo. A contagem de FD pode ser definida para menor ou igual ao número de discos geridos em cada uma das regiões de domínios de falha. Consulte este [documento](../virtual-machines/windows/manage-availability.md) para saber mais sobre o número de domínios de falha de Managed Disks por região.

## <a name="rest-api"></a>API REST
Pode definir a propriedade `properties.platformFaultDomainCount` para 1, 2 ou 3 (predefinição de 5 se não for especificado). Consulte a documentação para a REST API [aqui](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="azure-cli"></a>CLI do Azure
Pode definir o parâmetro `--platform-fault-domain-count` para 1, 2 ou 3 (predefinição de 5 se não for especificado). Consulte a documentação para a CLI do Azure [aqui](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az-vmss-create).

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --platform-fault-domain-count 3\
  --generate-ssh-keys
```

A criação e configuração de todas as VMs e recursos do conjunto de dimensionamento demora alguns minutos.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [funcionalidades de disponibilidade e redundância](../virtual-machines/windows/regions-and-availability.md) para ambientes do Azure.
