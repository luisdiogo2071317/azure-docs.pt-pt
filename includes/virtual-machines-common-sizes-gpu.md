---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 03/28/2018
ms.author: danlep;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 54ab118ca9a8dcc591b275f87be2894d2bfad54d
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2018
---
GPU com otimização de VM são especializadas máquinas de virtuais disponíveis com único ou vários GPUs NVIDIA. Estes tamanhos foram concebidos para cargas de trabalho de computação intensiva, intensivas de gráficos e visualização. Este artigo fornece informações sobre o número e tipo de GPUs, vCPUs, discos de dados e NICs, bem como armazenamento débito e a rede de largura de banda para cada tamanho neste agrupamento. 

* **NC, NCv2, NCv3 e ND** tamanhos estão otimizados para aplicações intensivas de computação e rede intensivas e algoritmos, incluindo aplicações baseadas em CUDA e OpenCL e simulações, AI e Learning profunda. 
* **NV** tamanhos são otimizados e foi concebidos para visualização remota, transmissão em fluxo, jogos, codificação e cenários VDI através das estruturas, tais como tendo sido igualmente e DirectX.  


## <a name="nc-series"></a>Série NC

VMs de série de NC são com tecnologia de [NVIDIA Tesla K80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) cartão. Os utilizadores podem crunch através de dados mais rápidos através do aproveitamento CUDA para aplicações de exploração de energia, simulações de falhas, ray rastreada composição, learning avançada e muito mais. A configuração de NC24r fornece uma baixa latência, a interface de rede de alto débito otimizado para paralelo fortemente conjugado cargas de trabalho de computação.


| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | GPU | Discos de dados máximos | NICs máximos |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 380 | 1 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 64 | 4 |

1 GPU = metade de uma placa K80.

*Com capacidade RDMA

## <a name="ncv2-series"></a>Série NCv2

VMs de série NCv2 são utiliza a tecnologia de [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPUs. Estes GPUs podem fornecer 2x mais do que o desempenho de capacidade de cálculo da série de NC. Os clientes podem tirar partido destes GPUs atualizados para tradicionais cargas de trabalho HPC, tais como a modelação de reservatório, DNA sequenciação, protein análise, simulações Monte Carlo e outras pessoas. A configuração de v2 NC24rs fornece uma baixa latência, a interface de rede de alto débito otimizado para paralelo fortemente conjugado cargas de trabalho de computação.

> [!IMPORTANT]
> Para esta família de tamanho, a quota de vCPU (principal) na sua subscrição inicialmente está definida para 0 em cada região. [Pedir um aumento de quota vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) para esta família num [região disponível](https://azure.microsoft.com/regions/services/).
>

| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | GPU | Discos de dados máximos | NICs máximos |
| --- | --- | --- | --- | --- | --- | ---  |
| Standard_NC6s_v2 |6 |112 | 336 | 1 | 12 | 4 |
| Standard_NC12s_v2 |12 |224 | 672 | 2 | 24 | 8 |
| Standard_NC24s_v2 |24 |448 | 1344 | 4 | 32 | 8 |
| Standard_NC24rs_v2* |24 |448 | 1344 | 4 | 32 | 8 |

1 GPU = um cartão de P100.

*Com capacidade RDMA

## <a name="ncv3-series"></a>Série NCv3

VMs de série NCv3 são utiliza a tecnologia de [NVIDIA Tesla V100](http://www.nvidia.com/content/PDF/Volta-Datasheet.pdf) GPUs. Estes GPUs podem fornecer 1,5 x o desempenho de capacidade de cálculo da série de NCv2. Os clientes podem tirar partido destes GPUs atualizados para tradicionais cargas de trabalho HPC, tais como a modelação de reservatório, DNA sequenciação, protein análise, simulações Monte Carlo e outras pessoas. A configuração de v3 NC24rs fornece uma baixa latência, a interface de rede de alto débito otimizado para paralelo fortemente conjugado cargas de trabalho de computação.

> [!IMPORTANT]
> Para esta família de tamanho, a quota de vCPU (principal) na sua subscrição inicialmente está definida para 0 em cada região. [Pedir um aumento de quota vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) para esta família num [região disponível](https://azure.microsoft.com/regions/services/).
>

| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | GPU | Discos de dados máximos | NICs máximos |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 |6 |112 | 336 | 1 | 12 | 4 |
| Standard_NC12s_v3 |12 |224 | 672 | 2 | 24 | 8 |
| Standard_NC24s_v3 |24 |448 | 1344 | 4 | 32 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 1344 | 4 | 32 | 8 |

1 GPU = um cartão de V100.

*Com capacidade RDMA

## <a name="nd-series"></a>Série ND

As máquinas virtuais de série ND são uma nova adição para a família GPU concebida para cargas de trabalho AI e aprendizagem profunda. Oferecem um excelente desempenho para formação e inferência. Instâncias de ND são utiliza a tecnologia de [NVIDIA Tesla P40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPUs. Estas instâncias fornecem excelente desempenho de precisão simples flutuante ponto operações, para cargas de trabalho de AI saber como utilizar o Toolkit de cognitivos, TensorFlow, Caffe e outras estruturas. A série ND também oferece um tamanho de memória GPU muito maior (24 GB), que permite adaptar modelos de rede neuronal muito maiores. Como a série de NC, a série ND oferece uma configuração com uma rede de latência baixa e alta débito secundária através de RDMA e a conectividade de InfiniBand para poder executar tarefas de formação em grande escala GPUs muitos de expansão.

> [!IMPORTANT]
> Para esta família de tamanho, a quota de vCPU (principal) por região na sua subscrição inicialmente está definida para 0. [Pedir um aumento de quota vCPU](../articles/azure-supportability/resource-manager-core-quotas-request.md) para esta família num [região disponível](https://azure.microsoft.com/regions/services/).
>

| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | GPU | Discos de dados máximos | NICs máximos |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s |6 |112 | 336 | 1 | 12 | 4 |
| Standard_ND12s |12 |224 | 672 | 2 | 24 | 8 | 
| Standard_ND24s |24 |448 | 1344 | 4 | 32 | 8 |
| Standard_ND24rs* |24 |448 | 1344 | 4 | 32 | 8 |

1 GPU = um cartão de P40.

*Com capacidade RDMA

## <a name="nv-series"></a>Série NV

As máquinas virtuais de série NV são utiliza a tecnologia de [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPUs e NVIDIA grelha a tecnologia de ambiente de trabalho acelerados aplicações e ambientes de trabalho virtuais onde os clientes são capazes de visualizar os seus dados ou simulações. Os utilizadores têm capacidade para visualizar os seus fluxos de trabalho intensivas de gráficos nas instâncias do NV para obter a capacidade de gráficos superior e adicionalmente executar única precisão cargas de trabalho, tais como a codificação e composição. 

Cada GPU em instâncias de NV vem com uma licença de grelha. Esta licença dá-lhe a flexibilidade para utilizar uma instância de NV como uma estação de trabalho virtual para um único utilizador ou de 25 utilizadores simultâneos podem ligar para a VM para um cenário de aplicação virtual.

| Tamanho | vCPU | Memória: GiB | Armazenamento (SSD) temporário GiB | GPU | Discos de dados máximos | NICs máximos | Estações de trabalho virtuais | Aplicações virtuais | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 64 | 4 | 4 | 100 |

1 GPU = metade de uma placa M60.

 
