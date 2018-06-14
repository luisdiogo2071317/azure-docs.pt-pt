---
title: Escolha os tamanhos de VM para conjuntos do Azure Batch | Microsoft Docs
description: Como escolher entre os tamanhos VM disponíveis para nós de computação em conjuntos do Azure Batch
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2018
ms.author: danlep
ms.openlocfilehash: addd1e9314a754b40cc5d49c0299f007580f512f
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
ms.locfileid: "29765610"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Escolher um tamanho VM para nós de computação num conjunto do Azure Batch

Quando seleciona um tamanho de nó de um conjunto do Azure Batch, pode escolher entre quase todas as VM tamanhos disponíveis no Azure. Para cargas de trabalho diferentes, o Azure oferece uma variedade de tamanhos para Linux e VMs do Windows. 

Existem algumas exceções e limitações para escolher um tamanho VM:
* Algumas famílias VM ou tamanhos de VM não são suportados no Batch. 
* Alguns dos tamanhos da VM estão limitados e têm de ser ativadas especificamente antes que podem ser atribuídas.


## <a name="supported-vm-families-and-sizes"></a>Famílias VM e os tamanhos suportados

### <a name="pools-in-virtual-machine-configuration"></a>Conjuntos de configuração da Máquina Virtual

Conjuntos do batch na configuração da Máquina Virtual suportam todos os tamanhos de VM ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)) *exceto* para o seguinte:

| Família  | Tamanhos não suportados  |
|---------|---------|
| A série básico | Basic_A0 (A0) |
| Série A | Standard_A0 |
| Série B | Todos |
| Fsv2-series<sup>*</sup> | Todos |

<sup>*</sup>Os tamanhos nesta série estão no plano de suporte futuro.

### <a name="pools-in-cloud-service-configuration"></a>Conjuntos de configuração do serviço em nuvem

Conjuntos do batch na configuração do serviço em nuvem suportam todos os [tamanhos de VM para serviços em nuvem](../cloud-services/cloud-services-sizes-specs.md) *exceto* para o seguinte:

| Família  | Tamanhos não suportados  |
|---------|---------|
| Série A | ExtraSmall |
| Série Av2 | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="restricted-vm-families"></a>Restritas famílias VM
As famílias VM seguintes podem ser atribuídas em conjuntos do Batch, mas tem de pedir um aumento de quota específico (consulte [neste artigo](batch-quota-limit.md#increase-a-quota)):
* Série NCv2
* Série NCv3
* Série ND

Estes tamanhos só podem ser utilizados em conjuntos na configuração da Máquina Virtual.

## <a name="size-considerations"></a>Considerações de tamanho

* **Os requisitos da aplicação** -considere as características e os requisitos da aplicação, irá executar em nós. Alguns aspetos, como se a aplicação tem vários threads e a quantidade de memória que consome, podem ajudar a determinar o tamanho de nó mais adequado e económico. Para várias instâncias [cargas de trabalho MPI](batch-mpi.md) ou aplicações de CUDA, considere especializada [HPC](../virtual-machines/linux/sizes-hpc.md) ou [preparados para a GPU](../virtual-machines/linux/sizes-gpu.md) tamanhos de VM, respetivamente. (Consulte [utilização com capacidade RDMA ou preparados para a GPU instâncias em conjuntos do Batch](batch-pool-compute-intensive-sizes.md).) 

* **Tarefas por nó** -é típica para selecionar um tamanho de nó pressupondo que uma tarefa é executada num nó de cada vez. No entanto, pode ser vantajoso ter várias tarefas (e, por conseguinte, as várias instâncias de aplicações) [executadas em paralelo](batch-parallel-node-tasks.md) em nós de computação durante a execução da tarefa. Neste caso, é comum para escolher um tamanho de nó especializados para contemplar a necessidade de aumento de execução da tarefa paralela.

* **Carregar níveis para diferentes tarefas** -todos os nós num conjunto são o mesmo tamanho. Se quiser executar aplicações com requisitos de sistema e/ou níveis de carga diferentes, recomendamos utilizar conjuntos separados. 

* **Disponibilidade de região** -família de uma VM ou tamanho poderão não estar disponível nas regiões onde criar as contas do Batch. Para verificar se está disponível um tamanho, consulte o artigo [produtos disponíveis por região](https://azure.microsoft.com/regions/services/).

* **Quotas** - [núcleos quotas](batch-quota-limit.md#resource-quotas) no seu Batch conta pode limitar o número de nós de um tamanho específico, pode adicionar a um conjunto do Batch. Para pedir um aumento de quota, consulte [neste artigo](batch-quota-limit.md#increase-a-quota). 

* **Configuração do conjunto aplicacional** -em geral, dispõe de mais opções de tamanho VM ao criar um agrupamento na configuração da Máquina Virtual, comparada com a configuração de serviço em nuvem.

## <a name="next-steps"></a>Passos Seguintes

* Para uma descrição geral aprofundada do Batch, consulte [paralelo em grande escala desenvolver soluções com o Batch de computação](batch-api-basics.md).
* Para obter informações sobre como utilizar os tamanhos de VM de computação intensiva, consulte [utilização com capacidade RDMA ou preparados para a GPU instâncias em conjuntos do Batch](batch-pool-compute-intensive-sizes.md). 


