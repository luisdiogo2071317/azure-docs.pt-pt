---
title: Tamanhos de VM do Linux do Azure - GPU | Documentos da Microsoft
description: Listas de GPU diferente com otimização de tamanhos disponíveis para máquinas virtuais do Linux no Azure. Lista as informações sobre o número de vCPUs, discos de dados e NICs, bem como armazenamento e débito de rede largura de banda para tamanhos nesta série.
services: virtual-machines-linux
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: jonbeck
ms.openlocfilehash: 8f50f090fe38382b8bc3cb7f669ab4025d36ff76
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036631"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Tamanhos de máquinas virtuais com otimização de GPU

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-distributions-and-drivers"></a>Distribuições e controladores suportados

Para tirar partido das capacidades GPU do Azure VMs de série N que executem o Linux, tem de estar instalados controladores NVIDIA GPU. O [extensão de controladores de GPU NVIDIA](../extensions/hpccompute-gpu-linux.md) instala os controladores adequados NVIDIA CUDA ou GRADE numa VM de série N. Instalar ou gerir a extensão com o portal do Azure ou ferramentas, como os modelos da CLI do Azure ou Azure Resource Manager. Consulte a [documentação da extensão de controladores de GPU NVIDIA](../extensions/hpccompute-gpu-linux.md) para as distribuições suportadas e passos de implementação. Para obter informações gerais sobre as extensões de VM, consulte [extensões de máquina virtual do Azure e funcionalidades](../extensions/overview.md).

Se optar por instalar controladores NVIDIA GPU manualmente, consulte [configuração de controladores GPU da série N para Linux](n-series-driver-setup.md) para as distribuições suportadas, drivers e passos de instalação e verificação.


[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Não deve instalar X servidor ou outros sistemas que utilizam o `Nouveau` driver em VMs do Ubuntu NC. Antes de instalar controladores NVIDIA GPU, terá de desativar o `Nouveau` driver.  

## <a name="other-sizes"></a>Outros tamanhos
- [Fins gerais](sizes-general.md)
- [Com otimização de computação](sizes-compute.md)
- [Com otimização de memória](sizes-memory.md)
- [Com otimização de armazenamento](sizes-storage.md)
- [Computação de elevado desempenho](sizes-hpc.md)
- [Gerações anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre como [computação do Azure (ACU) de unidades](acu.md) pode ajudá-lo a comparar o desempenho de computação nos SKUs do Azure.