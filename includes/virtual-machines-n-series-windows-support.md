---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 03/20/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 426340a5b452124dcba4f51f2ec2c1e2ec0f54b2
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos e controladores suportados

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-tesla-cuda-drivers"></a>NC, NCv2, NCv3 e série ND - controladores NVIDIA Tesla (CUDA)

Ligações de transferência do controlador na tabela seguinte são actual no momento da publicação. Para os controladores mais recentes, visite o Web site da [NVIDIA](http://www.nvidia.com/).

> [!TIP]
> Como alternativa à instalação manual do controlador CUDA na VM do Windows Server, pode implementar um Azure [máquinas de virtuais de ciência de dados](../articles/machine-learning/data-science-virtual-machine/overview.md) imagem. As edições de DSVM para o Windows Server 2016 pré-instalar controladores NVIDIA CUDA, CUDA profunda neuronal biblioteca de rede e outras ferramentas.


| SO | Controlador |
| -------- |------------- |
| Windows Server 2016 | [390.85](http://us.download.nvidia.com/Windows/Quadro_Certified/390.85/390.85-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [390.85](http://us.download.nvidia.com/Windows/Quadro_Certified/390.85/390.85-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nv-series---nvidia-grid-drivers"></a>Série NV - NVIDIA grelha controladores

Microsoft redistribui com os programas de instalação de controlador de grelha NVIDIA para NV VMs. Instale apenas estes controladores de grelha em VMs do Azure NV. Estes incluem de licenciamento para Software para a GPU Virtual grelha no Azure.

| SO | Controlador |
| -------- |------------- |
| Windows Server 2016 | [Grelha 5.2 (386.09)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [Grelha 5.2 (386.09)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |