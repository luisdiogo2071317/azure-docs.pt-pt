---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/29/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 8f3d57f8f1f3631421618e31e943606a16e6bf5b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34670024"
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos e controladores suportados

### <a name="nvidia-tesla-cuda-drivers"></a>Controladores NVIDIA Tesla (CUDA)

Controladores de NVIDIA Tesla (CUDA) para NC, NCv2, NCv3 e VMs de série ND (opcionais para a série NV) são suportadas apenas em sistemas operativos listados na seguinte tabela. Ligações de transferência de controladores são actual no momento da publicação. Para os controladores mais recentes, visite o Web site da [NVIDIA](http://www.nvidia.com/).

> [!TIP]
> Como alternativa à instalação manual do controlador CUDA na VM do Windows Server, pode implementar um Azure [máquinas de virtuais de ciência de dados](../articles/machine-learning/data-science-virtual-machine/overview.md) imagem. As edições de DSVM para o Windows Server 2016 pré-instalar controladores NVIDIA CUDA, CUDA profunda neuronal biblioteca de rede e outras ferramentas.


| SO | Controlador |
| -------- |------------- |
| Windows Server 2016 | [397.44](http://us.download.nvidia.com/Windows/Quadro_Certified/397.44/397.44-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [397.44](http://us.download.nvidia.com/Windows/Quadro_Certified/397.44/397.44-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |


### <a name="nvidia-grid-drivers"></a>Controladores NVIDIA grelha

Microsoft redistribui com os programas de instalação de controlador de grelha NVIDIA para VMs NV série utilizadas como estações de trabalho virtuais ou para aplicações virtuais. Instale apenas estes controladores de grelha em VMs do Azure NV, apenas nos sistemas operativos listados na seguinte tabela. Estes incluem de licenciamento para Software para a GPU Virtual grelha no Azure.

| SO | Controlador |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [GRELHA 6.1 (391.58)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [GRELHA 6.1 (391.58)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |