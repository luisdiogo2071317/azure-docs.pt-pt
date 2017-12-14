---
title: Incorporar Shell em nuvem do Azure | Microsoft Docs
description: Saiba como incorporar a Shell de nuvem do Azure.
services: cloud-shell
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: juluk
ms.openlocfilehash: 7c06a51e7f9f402b2ec10e440ca98125a7f2a7cf
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="embed-azure-cloud-shell"></a>Incorporar Shell em nuvem do Azure

Ao incorporar Shell da nuvem permite aos programadores e escritores conteúdos abrir diretamente Shell de nuvem a partir de um URL dedicado, [shell.azure.com](https://shell.azure.com). Isto desbloqueia o poder do autenticação da Shell de nuvem, ferramentas e atualizadas ferramentas do Azure CLI/Azure PowerShell.

## <a name="how-to"></a>Procedimentos

Integre no botão de início da nuvem Shell ficheiros markdown copiando o seguinte:

```markdown
[![Launch Cloud Shell](https:shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

HTML para incorporar um pop-up que shell de nuvem é abaixo:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="next-steps"></a>Passos seguintes
[Bash no início rápido da Shell de nuvem](quickstart.md)<br>
[PowerShell no início rápido da Shell de nuvem](quickstart-powershell.md)