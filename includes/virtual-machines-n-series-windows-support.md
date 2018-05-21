---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/16/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 9b36a8e90c20c8f0d08ca241e48e9306fb5b10f8
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos e controladores suportados

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-tesla-cuda-drivers"></a>NC, NCv2, NCv3 e série ND - controladores NVIDIA Tesla (CUDA)

Ligações de transferência do controlador na tabela seguinte são actual no momento da publicação. Para os controladores mais recentes, visite o Web site da [NVIDIA](http://www.nvidia.com/).

> [!TIP]
> Como alternativa à instalação manual do controlador CUDA na VM do Windows Server, pode implementar um Azure [máquinas de virtuais de ciência de dados](../articles/machine-learning/data-science-virtual-machine/overview.md) imagem. As edições de DSVM para o Windows Server 2016 pré-instalar controladores NVIDIA CUDA, CUDA profunda neuronal biblioteca de rede e outras ferramentas.


| SO | Controlador |
| -------- |------------- |
| Windows Server 2016 | [391.29](http://us.download.nvidia.com/Windows/Quadro_Certified/391.29/391.29-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [391.29](http://us.download.nvidia.com/Windows/Quadro_Certified/391.29/391.29-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nv-series---nvidia-grid-drivers"></a>Série NV - NVIDIA grelha controladores

Microsoft redistribui com os programas de instalação de controlador de grelha NVIDIA para NV VMs. Instale apenas estes controladores de grelha em VMs do Azure NV. Estes incluem de licenciamento para Software para a GPU Virtual grelha no Azure.

| SO | Controlador |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [GRELHA 6.0 (391.03)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [GRELHA 6.0 (391.03)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |