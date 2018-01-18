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
ms.openlocfilehash: 3ceddb94336fc2703e6f916f05ab1ec3676cb50d
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="embed-azure-cloud-shell"></a>Incorporar Shell em nuvem do Azure

Ao incorporar Shell da nuvem permite aos programadores e escritores conteúdos abrir diretamente Shell de nuvem a partir de um URL dedicado, [shell.azure.com](https://shell.azure.com). Isto coloca imediatamente a capacidade total de autenticação da Shell de nuvem, ferramentas, e atualizada do Azure CLI/Azure PowerShell ferramentas aos seus utilizadores.

Botão de tamanho normal

[![](https://shell.azure.com/images/launchcloudshell.png "Inicie o Shell de nuvem do Azure")](https://shell.azure.com)

Botão de tamanho grande

[![](https://shell.azure.com/images/launchcloudshell@2x.png "Inicie o Shell de nuvem do Azure")](https://shell.azure.com)

## <a name="how-to"></a>Procedimentos

Integre no botão de início da nuvem Shell ficheiros markdown copiando o seguinte:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

HTML para incorporar um pop-up que shell de nuvem é abaixo:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Personalizar a experiência

Defina uma experiência de shell específica, aumentar o seu URL.
|Experiência   |URL   |
|---|---|
|A maioria utilizados recentemente shell   |shell.azure.com           |
|Bash                       |shell.azure.com/bash       |
|PowerShell                 |shell.azure.com/powershell |

## <a name="next-steps"></a>Passos Seguintes
[Bash no início rápido da Shell de nuvem](quickstart.md)<br>
[PowerShell no início rápido da Shell de nuvem](quickstart-powershell.md)
