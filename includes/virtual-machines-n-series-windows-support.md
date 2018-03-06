---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 03/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 506c2a4cf675a347dc4c45c9ccf8bce95de2f6fc
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operativos e controladores suportados

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-tesla-drivers"></a>NC, NCv2, NCv3 e série ND - NVIDIA Tesla controladores

| SO | Controlador |
| -------- |------------- |
| Windows Server 2016 | [385.54](http://us.download.nvidia.com/Windows/Quadro_Certified/385.54/385.54-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [385.54](http://us.download.nvidia.com/Windows/Quadro_Certified/385.54/385.54-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

> [!NOTE]
> As ligações de transferência do controlador Tesla estão atualizadas no momento da publicação. Para os controladores mais recentes, visite o Web site da [NVIDIA](http://www.nvidia.com/).
>

### <a name="nv-series---nvidia-grid-drivers"></a>Série NV - NVIDIA grelha controladores

| SO | Controlador |
| -------- |------------- |
| Windows Server 2016 | [Grelha 5.2 (386.09)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [Grelha 5.2 (386.09)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |

> [!NOTE]
> Microsoft redistribui com os programas de instalação de controlador de grelha NVIDIA para NV VMs. Instale apenas estes controladores de grelha em VMs do Azure NV. Estes incluem de licenciamento para Software para a GPU Virtual grelha no Azure.
>