---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 05/29/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0264f92fa10bd503a2811ce40ee0b8d4edd5f3b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34669837"
---
## <a name="supported-distributions-and-drivers"></a>Distribuições e controladores suportados

### <a name="nvidia-cuda-drivers"></a>Controladores NVIDIA CUDA

Controladores NVIDIA CUDA NC, NCv2, NCv3 e VMs de série ND (opcionais para a série NV) são suportados apenas nas distribuições de Linux listadas na seguinte tabela. Informações de controlador CUDA são actual no momento da publicação. Para os controladores mais recentes do CUDA, visite o [NVIDIA](https://developer.nvidia.com/cuda-zone) Web site. Certifique-se de que instalar ou atualizar para os controladores mais recentes do CUDA para a distribuição. 

> [!TIP]
> Como alternativa à instalação de controlador CUDA manual uma VM com Linux, pode implementar um Azure [máquinas de virtuais de ciência de dados](../articles/machine-learning/data-science-virtual-machine/overview.md) imagem. As edições de DSVM para Ubuntu 16.04 LTS ou CentOS 7.4 de pré-instalação controladores NVIDIA CUDA, CUDA profunda neuronal biblioteca de rede e outras ferramentas.

| Distribuição | Controlador |
| --- | -- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 ou 7.4<br/><br/> Com base em centOS 7.3 ou 7.4, com base em CentOS 7.4 HPC | NVIDIA CUDA 9.1, ramo controlador R390 |

### <a name="nvidia-grid-drivers"></a>Controladores NVIDIA grelha

Microsoft redistribui com os programas de instalação de controlador de grelha NVIDIA para VMs NV série utilizadas como estações de trabalho virtuais ou para aplicações virtuais. Instale apenas estes controladores de grelha em VMs do Azure NV, apenas nas distribuições listadas na seguinte tabela. Estes incluem de licenciamento para Software para a GPU Virtual grelha no Azure.

| Distribuição | Controlador |
| --- | -- |
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 ou 7.4<br/><br/>7.3 ou 7.4 baseada em centOS | NVIDIA grelha 6.0, o ramo de controlador R390|



> [!WARNING] 
> A instalação de software de terceiros em produtos Red Hat pode afetar os termos de suporte do Red Hat. Veja o [Red Hat Knowledgebase](https://access.redhat.com/articles/1067) (Base de Dados de Conhecimento do Red Hat).
>
