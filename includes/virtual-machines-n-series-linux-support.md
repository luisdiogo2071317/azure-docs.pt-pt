---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 22d37ca30f1319f46a52b96be1c527f6f56719ab
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
## <a name="supported-distributions-and-drivers"></a>Distribuições e controladores suportados

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-cuda-drivers"></a>NC, NCv2, NCv3 e série ND - NVIDIA CUDA controladores
| Distribuição | Controlador |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 ou 7.4<br/><br/> CentOS 7.3 ou 7.4 | NVIDIA CUDA 9.1, ramo controlador R390 |

> [!IMPORTANT]
> Certifique-se de que instalar ou atualizar para os controladores mais recentes do CUDA para a distribuição. Controladores anteriores versão R390 que poderão ter problemas com kernels atualizados do Linux.
>

### <a name="nv-series---nvidia-grid-drivers"></a>Série NV - NVIDIA grelha controladores

| Distribuição | Controlador |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3<br/><br/>CentOS-based 7.3 | 5.2 de grelha NVIDIA, ramo controlador R384|

> [!NOTE]
> Microsoft redistribui com os programas de instalação de controlador de grelha NVIDIA para NV VMs. Instale apenas estes controladores de grelha em VMs do Azure NV. Estes incluem de licenciamento para Software para a GPU Virtual grelha no Azure.
>

> [!WARNING] 
> A instalação de software de terceiros em produtos Red Hat pode afetar os termos de suporte do Red Hat. Veja o [Red Hat Knowledgebase](https://access.redhat.com/articles/1067) (Base de Dados de Conhecimento do Red Hat).
>
