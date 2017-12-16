---
title: Tamanhos de VM Linux do Azure - GPU | Microsoft Docs
description: "Apresenta uma lista na GPU diferentes com otimização de tamanhos disponíveis para computadores virtuais Linux no Azure. Apresenta informações sobre o número de vCPUs, discos de dados e NICs, bem como armazenamento débito e a rede de largura de banda para tamanhos de nesta série."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/14/2017
ms.author: jonbeck
ms.openlocfilehash: 9e8b682d8d7e409f97144d344ec58e710327df7a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/16/2017
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU com otimização de tamanhos de máquinas virtuais

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Para passos de instalação e a verificação de controladores, consulte [a configuração do controlador de série N para Linux](n-series-driver-setup.md).

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Não deve instalar X servidor ou a outros sistemas que utilizam o `Nouveau` controladores em Ubuntu NC VMs. Antes de instalar os controladores de NVIDIA GPU, terá de desativar o `Nouveau` controlador.  

## <a name="other-sizes"></a>Outros tamanhos de
- [Fins gerais](sizes-general.md)
- [Com otimização de computação](sizes-compute.md)
- [Com otimização de memória](sizes-memory.md)
- [Com otimização de armazenamento](sizes-storage.md)
- [Computação de elevado desempenho](sizes-hpc.md)

## <a name="next-steps"></a>Passos seguintes
Saiba mais sobre como [unidades (ACU) de computação do Azure](acu.md) podem ajudar a comparar o desempenho de computação em SKUs do Azure.