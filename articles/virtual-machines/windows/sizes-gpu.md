---
title: Tamanhos de VM do Windows Azure - GPU | Documentos da Microsoft
description: Listas de GPU diferente com otimização de tamanhos disponíveis para máquinas de virtuais do Windows no Azure. Lista as informações sobre o número de vCPUs, discos de dados e NICs, bem como armazenamento e débito de rede largura de banda para tamanhos nesta série.
services: virtual-machines-windows
documentationcenter: ''
author: jonbeck7
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: jonbeck
ms.openlocfilehash: ab3ccd981514225a003b07bafbb73db11aaa21e2
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903735"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>Tamanhos de máquinas virtuais com otimização de GPU

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos e controladores suportados

Para tirar partido das capacidades de GPU de VMs de série N do Azure a executar o Windows, tem de estar instalados controladores NVIDIA GPU. O [extensão de controladores de GPU NVIDIA](../extensions/hpccompute-gpu-windows.md) instala os controladores adequados NVIDIA CUDA ou GRADE numa VM de série N. Instalar ou gerir a extensão com o portal do Azure ou ferramentas, tais como modelos do Azure PowerShell ou do Azure Resource Manager. Consulte a [documentação da extensão de controladores de GPU NVIDIA](../extensions/hpccompute-gpu-windows.md) para sistemas operativos suportados e os passos de implementação. Para obter informações gerais sobre as extensões de VM, consulte [extensões de máquina virtual do Azure e funcionalidades](../extensions/overview.md).

Se optar por instalar controladores NVIDIA GPU manualmente, consulte [configuração de controladores GPU da série N para Windows](n-series-driver-setup.md) para sistemas operativos suportados, drivers e passos de instalação e verificação.

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

## <a name="other-sizes"></a>Outros tamanhos
- [Fins gerais](sizes-general.md)
- [Com otimização de computação](sizes-compute.md)
- [Computação de elevado desempenho](sizes-hpc.md)
- [Com otimização de memória](sizes-memory.md)
- [Com otimização de armazenamento](sizes-storage.md)
- [Gerações anteriores](sizes-previous-gen.md)

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre como [computação do Azure (ACU) de unidades](acu.md) pode ajudá-lo a comparar o desempenho de computação nos SKUs do Azure.

