---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 09/24/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 2fefe23a57668ce20ed1a6afaf3514750e3c72da
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47043885"
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos e controladores suportados

### <a name="nvidia-tesla-cuda-drivers"></a>Controladores de NVIDIA Tesla (CUDA)

Controladores de NVIDIA Tesla (CUDA) para NC, a NCv2, NCv3 e VMs de série ND (opcionais para a série NV) são suportados apenas em sistemas operativos listados na tabela seguinte. Ligações de transferência do controlador são atualizadas no momento da publicação. Para os controladores mais recentes, visite o Web site da [NVIDIA](http://www.nvidia.com/).

> [!TIP]
> Como alternativa à instalação de controlador CUDA manual numa VM do Windows Server, pode implementar do Azure [máquina de Virtual de ciência de dados](../articles/machine-learning/data-science-virtual-machine/overview.md) imagem. As edições DSVM do Windows Server 2016 previamente instalar controladores de NVIDIA CUDA, CUDA Deep Neural biblioteca de rede e outras ferramentas.


| SO | Controlador |
| -------- |------------- |
| Windows Server 2016 | [398.75](http://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [398.75](http://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |


### <a name="nvidia-grid-drivers"></a>Controladores de GRID da NVIDIA

Microsoft redistribui instaladores de drivers de NVIDIA GRID para NV amd VMs da série NVv2 utilizadas como estações de trabalho virtuais ou para aplicações virtuais. Instale apenas estes controladores GRID em VMs do Azure NV, apenas em sistemas operativos listados na tabela seguinte. Estes controladores incluem licenças de Software GPU Virtual GRADE no Azure.

| SO | Controlador |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [GRELHA 6.2 (391.81)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [GRELHA 6.2 (391.81)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |