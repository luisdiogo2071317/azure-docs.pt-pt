---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 05/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 7139ec67536a1c0e41c991db6d867b956f995c11
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
## <a name="supported-distributions-and-drivers"></a>Distribuições e controladores suportados

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-cuda-drivers"></a>NC, NCv2, NCv3 e série ND - NVIDIA CUDA controladores

Informações de controlador CUDA na tabela seguinte são actual no momento da publicação. Para os controladores mais recentes do CUDA, visite o [NVIDIA](https://developer.nvidia.com/cuda-zone) Web site. Certifique-se de que instalar ou atualizar para os controladores mais recentes do CUDA para a distribuição. 

> [!TIP]
> Como alternativa à instalação de controlador CUDA manual uma VM com Linux, pode implementar um Azure [máquinas de virtuais de ciência de dados](../articles/machine-learning/data-science-virtual-machine/overview.md) imagem. As edições de DSVM para Ubuntu 16.04 LTS ou CentOS 7.4 de pré-instalação controladores NVIDIA CUDA, CUDA profunda neuronal biblioteca de rede e outras ferramentas.

| Distribuição | Controlador |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 ou 7.4<br/><br/> Com base em centOS 7.3 ou 7.4, com base em CentOS 7.4 HPC | NVIDIA CUDA 9.1, ramo controlador R390 |

### <a name="nv-series---nvidia-grid-drivers"></a>Série NV - NVIDIA grelha controladores

Microsoft redistribui com os programas de instalação de controlador de grelha NVIDIA para NV VMs. Instale apenas estes controladores de grelha em VMs do Azure NV. Estes incluem de licenciamento para Software para a GPU Virtual grelha no Azure.

| Distribuição | Controlador |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 ou 7.4<br/><br/>7.3 ou 7.4 baseada em centOS | NVIDIA grelha 6.0, o ramo de controlador R390|



> [!WARNING] 
> A instalação de software de terceiros em produtos Red Hat pode afetar os termos de suporte do Red Hat. Veja o [Red Hat Knowledgebase](https://access.redhat.com/articles/1067) (Base de Dados de Conhecimento do Red Hat).
>
