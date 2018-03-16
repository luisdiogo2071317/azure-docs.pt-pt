---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: ee32886ddb74bdbbe0f240310629c8ef26230a68
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
## <a name="deployment-considerations"></a>Considerações sobre implementação
* **Subscrição do Azure** – para implementar mais do que alguns instâncias de computação intensiva, considere uma subscrição pay as you go ou outras opções de compra. Se estiver a utilizar uma [conta gratuita do Azure](https://azure.microsoft.com/free/), pode utilizar apenas um número limitado de núcleos de computação do Azure.

* **Preços e disponibilidade** -tamanhos de VM estas são oferecidos apenas num padrão de escalão de preço. Verifique [produtos disponíveis por região] (https://azure.microsoft.com/regions/services/) de disponibilidade em regiões do Azure. 
* **Quota de núcleos** – poderá ter de aumentar a quota de núcleos na sua subscrição do Azure do valor predefinido. A subscrição também poderá limitar o número de núcleos que pode implementar em determinados famílias de tamanho VM, incluindo a série de H. Para pedir um aumento de quota [abrir um pedido de suporte ao cliente online](../articles/azure-supportability/how-to-create-azure-support-request.md) , sem encargos. (Predefinição limites podem variar consoante a categoria de subscrição.)
  
  > [!NOTE]
  > Se tiver necessidades de capacidade em grande escala, contacte o suporte do Azure. Quotas do Azure são crédito limites, não as garantias de capacidade. Independentemente da sua quota, são-lhe apenas cobrados de núcleos que utiliza.
  > 
  > 
* **Rede virtual** – um Azure [rede virtual](https://azure.microsoft.com/documentation/services/virtual-network/) não é necessário utilizar as instâncias intensivas de computação. No entanto, para muitas implementações precisa de, pelo menos, uma rede de virtual do Azure baseados na nuvem ou uma ligação site a site se precisar de aceder a recursos no local. Quando for necessário, crie uma nova rede virtual para implementar as instâncias. A adição de VMs de computação intensiva para uma rede virtual num grupo de afinidade não é suportada.
* **Redimensionamento** – devido ao seu hardware especializado, apenas pode redimensionar instâncias intensivas de computação dentro da mesma família de tamanho (série de H ou intensivas de computação série A). Por exemplo, apenas pode redimensionar uma VM de série de H a partir de um tamanho de série H para outro. Além disso, a redimensionar a partir de um tamanho de não-intensivas de computação para um tamanho de computação intensiva não é suportada.  

## <a name="rdma-capable-instances"></a>Instâncias com capacidade RDMA
Uma interface de rede para a conectividade de (RDMA) de acesso remoto direto à memória de funcionalidade de um subconjunto das instâncias intensivas de computação (H16r, H16mr, A8 e A9). (Tamanhos de série N selecionado designados com r como NC24r também estão com capacidade RDMA). Esta interface está além da interface de rede do Azure padrão disponível para outros tamanhos de VM. 
  
Esta interface que permite que as instâncias comunicar através de uma rede InfiniBand (IB), a funcionar em FDR as taxas de máquinas virtuais H16r, H16mr e série N com capacidade RDMA e QDR as taxas de máquinas virtuais A8 e A9 com capacidade RDMA. Estas capacidades RDMA podem melhorar a escalabilidade e o desempenho de determinadas aplicações de Interface de passagem de mensagens (MPI).

> [!NOTE]
> No Azure, o IP através de IB não é suportada. Apenas RDMA sobre IB é suportada.
>

Implemente as VMs de HPC com capacidade RDMA no mesmo conjunto de disponibilidade ou conjunto de dimensionamento da VM (quando utiliza o modelo de implementação Azure Resource Manager) ou o mesmo serviço de nuvem (quando utiliza o modelo de implementação clássica). Requisitos adicionais para aceder à rede Azure RDMA as VMs com capacidade RDMA HPC seguem.